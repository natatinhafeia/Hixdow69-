-- Definir variáveis essenciais
local UIS = game:GetService("UserInputService")
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local Camera = game:GetService("Workspace").CurrentCamera

-- Criar a UI personalizada
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = Player.PlayerGui
ScreenGui.Name = "CustomBloxUI"

-- Função para criar um botão
local function createButton(text, position, size, callback)
    local button = Instance.new("TextButton")
    button.Text = text
    button.Size = UDim2.new(0, size.X, 0, size.Y)
    button.Position = UDim2.new(0, position.X, 0, position.Y)
    button.BackgroundColor3 = Color3.fromRGB(0, 102, 255)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 20
    button.Parent = ScreenGui

    -- Captura de toque (touch) no Delta Executor
    button.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            callback()
        end
    end)
    return button
end

-- Função para teleporte
local function teleportTo(target)
    if target and target:FindFirstChild("HumanoidRootPart") then
        Player.Character.HumanoidRootPart.CFrame = target.CFrame
    end
end

-- Função Auto Farm
local function autoFarm(target)
    while target and target:FindFirstChild("Humanoid") and target.Humanoid.Health > 0 do
        -- Atacar o inimigo
        Player.Character:FindFirstChildOfClass("Humanoid"):MoveTo(target.HumanoidRootPart.Position)
        wait(1) -- Aguarda 1 segundo para dar tempo de atacar
    end
end

-- Função Auto Girar Fruta
local function autoSpinFruit()
    while true do
        local fruitSpinButton = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("FruitSpinButton")  -- Nome correto do botão de girar fruta
        if fruitSpinButton then
            fruitSpinButton.MouseButton1Click:Fire()  -- Simula o clique para girar a fruta
            wait(10) -- Aguarda 10 segundos antes de girar novamente
        end
        wait(1)
    end
end

-- Função Auto Girar Raça
local function autoSpinRace()
    while true do
        local raceSpinButton = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("RaceSpinButton")  -- Nome correto do botão de girar raça
        if raceSpinButton then
            raceSpinButton.MouseButton1Click:Fire()  -- Simula o clique para girar a raça
            wait(10) -- Aguarda 10 segundos antes de girar novamente
        end
        wait(1)
    end
end

-- Função para ESP (marcação de inimigos)
local function createESP(target)
    if target and target:FindFirstChild("HumanoidRootPart") then
        local box = Instance.new("BillboardGui")
        box.Adornee = target.HumanoidRootPart
        box.Size = UDim2.new(0, 100, 0, 100)
        box.StudsOffset = Vector3.new(0, 3, 0)
        box.Parent = target.HumanoidRootPart
        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(1, 0, 1, 0)
        frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        frame.BackgroundTransparency = 0.5
        frame.Parent = box
    end
end

-- Função Auto Sea Beast (Ativar Auto Farm no Sea Beast)
local function autoSeaBeast()
    while true do
        local seaBeast = workspace:FindFirstChild("SeaBeast")  -- Substitua com o nome exato do Sea Beast
        if seaBeast and seaBeast:FindFirstChild("Humanoid") then
            -- Iniciar Auto Farm no Sea Beast
            autoFarm(seaBeast)
        end
        wait(5)  -- Verifica a cada 5 segundos
    end
end

-- Função para aceitar missões automaticamente de NPCs
local function autoAcceptQuests()
    while true do
        for _, npc in pairs(workspace:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("QuestGiver") then
                local questGiver = npc.QuestGiver
                if questGiver and questGiver:FindFirstChild("ClickDetector") then
                    -- Simula o clique no NPC para aceitar a missão
                    fireclickdetector(questGiver.ClickDetector)
                    wait(2) -- Espera 2 segundos antes de aceitar outra missão
                end
            end
        end
        wait(5) -- Verifica a cada 5 segundos
    end
end

-- Função para pegar todos os estilos de luta
local function autoGetFightingStyles()
    while true do
        for _, npc in pairs(workspace:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("FightingStyleGiver") then
                local styleGiver = npc.FightingStyleGiver
                if styleGiver and styleGiver:FindFirstChild("ClickDetector") then
                    -- Simula o clique no NPC para pegar o estilo de luta
                    fireclickdetector(styleGiver.ClickDetector)
                    wait(2) -- Espera 2 segundos antes de pegar outro estilo
                end
            end
        end
        wait(5) -- Verifica a cada 5 segundos
    end
end

-- Função para pegar todos os Haki
local function autoGetHaki()
    while true do
        for _, npc in pairs(workspace:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("HakiGiver") then
                local hakiGiver = npc.HakiGiver
                if hakiGiver and hakiGiver:FindFirstChild("ClickDetector") then
                    -- Simula o clique no NPC para pegar o Haki
                    fireclickdetector(hakiGiver.ClickDetector)
                    wait(2) -- Espera 2 segundos antes de pegar o próximo Haki
                end
            end
        end
        wait(5) -- Verifica a cada 5 segundos
    end
end

-- Criar a UI com botões
local toggleFarmButton = createButton("Ativar Auto Farm", Vector2.new(20, 20), Vector2.new(200, 50), function()
    local target = workspace:WaitForChild("BossName")  -- Substitua com o nome do boss ou inimigo
    autoFarm(target)
end)

local teleportButton = createButton("Teleporte para o Boss", Vector2.new(20, 80), Vector2.new(200, 50), function()
    local boss = workspace:WaitForChild("BossName")  -- Substitua com o nome do boss
    teleportTo(boss)
end)

local autoSpinFruitButton = createButton("Auto Girar Fruta", Vector2.new(20, 140), Vector2.new(200, 50), function()
    autoSpinFruit()
end)

local autoSpinRaceButton = createButton("Auto Girar Raça", Vector2.new(20, 200), Vector2.new(200, 50), function()
    autoSpinRace()
end)

local autoSeaBeastButton = createButton("Auto Sea Beast", Vector2.new(20, 260), Vector2.new(200, 50), function()
    autoSeaBeast()
end)

local autoAcceptQuestButton = createButton("Auto Aceitar Missões", Vector2.new(20, 320), Vector2.new(200, 50), function()
    autoAcceptQuests()
end)

local autoGetFightingStylesButton = createButton("Auto Pegar Estilos de Luta", Vector2.new(20, 380), Vector2.new(200, 50), function()
    autoGetFightingStyles()
end)

local autoGetHakiButton = createButton("Auto Pegar Haki", Vector2.new(20, 440), Vector2.new(200, 50), function()
    autoGetHaki()
end)

-- Função para habilitar ESP para inimigos no mundo
local function enableESPForEnemies()
    for _, enemy in pairs(workspace:GetChildren()) do
        if enemy:FindFirstChild("Humanoid") then
            createESP(enemy)
        end
    end
end

-- Rodar a função ESP em paralelo
spawn(function()
    enableESPForEnemies()
end)

-- Garante que a GUI seja removida ao sair do jogo
Player.PlayerRemoving:Connect(function()
    if ScreenGui and ScreenGui.Parent then
        ScreenGui:Destroy()
    end
end)
