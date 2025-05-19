-- Natan Dead - Script Profissional para Dead Rails
-- Compatível Delta Executor e Android
-- Menu com funções: Teleporte, Auto Farm, ESP, Noclip, Kill Aura, Minimize e Close

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Configurações visuais
local colors = {
    main = Color3.fromRGB(255, 0, 0),
    background = Color3.fromRGB(20, 20, 20),
    accent = Color3.fromRGB(45, 45, 45),
    text = Color3.fromRGB(230, 230, 230),
    enabled = Color3.fromRGB(0, 180, 0),
    disabled = Color3.fromRGB(180, 0, 0)
}

-- Criar GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "NatanDeadUI"
screenGui.Parent = game:GetService("CoreGui")
screenGui.ResetOnSpawn = false

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 370, 0, 530)
mainFrame.Position = UDim2.new(0, 20, 0, 70)
mainFrame.BackgroundColor3 = colors.background
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.Active = true
mainFrame.Draggable = true

local mainCorner = Instance.new("UICorner", mainFrame)
mainCorner.CornerRadius = UDim.new(0, 10)

-- Top bar
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 50)
topBar.BackgroundColor3 = colors.accent
topBar.Parent = mainFrame

local topBarCorner = Instance.new("UICorner", topBar)
topBarCorner.CornerRadius = UDim.new(0, 10)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -90, 1, 0)
title.Position = UDim2.new(0, 15, 0, 0)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.TextColor3 = colors.main
title.Text = "Natan Dead"
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = topBar

-- Close Button
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 40, 0, 40)
closeBtn.Position = UDim2.new(1, -45, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 22
closeBtn.TextColor3 = colors.text
closeBtn.Text = "X"
closeBtn.Parent = topBar
local closeCorner = Instance.new("UICorner", closeBtn)
closeCorner.CornerRadius = UDim.new(0, 6)

closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Minimize Button
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 40, 0, 40)
minimizeBtn.Position = UDim2.new(1, -90, 0, 5)
minimizeBtn.BackgroundColor3 = colors.accent
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.TextColor3 = colors.text
minimizeBtn.Text = "-"
minimizeBtn.Parent = topBar
local minimizeCorner = Instance.new("UICorner", minimizeBtn)
minimizeCorner.CornerRadius = UDim.new(0, 6)

local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        mainFrame.Size = UDim2.new(0, 370, 0, 50)
        for _, v in pairs(mainFrame:GetChildren()) do
            if v ~= topBar then
                v.Visible = false
            end
        end
    else
        mainFrame.Size = UDim2.new(0, 370, 0, 530)
        for _, v in pairs(mainFrame:GetChildren()) do
            v.Visible = true
        end
    end
end)

-- Container para botões
local buttonsFrame = Instance.new("ScrollingFrame")
buttonsFrame.Size = UDim2.new(1, -20, 1, -60)
buttonsFrame.Position = UDim2.new(0, 10, 0, 55)
buttonsFrame.BackgroundTransparency = 1
buttonsFrame.BorderSizePixel = 0
buttonsFrame.ScrollBarThickness = 6
buttonsFrame.Parent = mainFrame
buttonsFrame.CanvasSize = UDim2.new(0, 0, 3, 0) -- Para rolar, aumenta se precisar

local layout = Instance.new("UIListLayout")
layout.Parent = buttonsFrame
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 10)

-- Função para criar botões padrão
local function createButton(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.BackgroundColor3 = colors.accent
    btn.TextColor3 = colors.text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.Text = name
    btn.AutoButtonColor = false
    btn.Parent = buttonsFrame
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 8)
    return btn
end

-- Teleport para bases (lista com nomes reais das bases e torretas)
local bases = {
    ["Base 1"] = "Base1Turret",
    ["Base 2"] = "Base2Turret",
    ["Base 3"] = "Base3Turret",
    ["Base 4"] = "Base4Turret",
    ["Base Final"] = "EndTurret"
}

local function getBasePart(baseName)
    -- Tenta encontrar o objeto no workspace
    local turretName = bases[baseName]
    if not turretName then return nil end
    return workspace:FindFirstChild(turretName)
end

local teleportBtns = {}

for baseName, turretName in pairs(bases) do
    local btn = createButton("TP para "..baseName)
    btn.LayoutOrder = #teleportBtns + 1
    btn.MouseButton1Click:Connect(function()
        local part = getBasePart(baseName)
        if part and part:IsA("BasePart") then
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                char.HumanoidRootPart.CFrame = part.CFrame + Vector3.new(0, 5, 0)
            else
                warn("Personagem ou HumanoidRootPart não encontrado")
            end
        else
            warn("Base '"..baseName.."' não encontrada")
        end
    end)
    table.insert(teleportBtns, btn)
end

-- Teleporte para banco do trem
local tpTrainBtn = createButton("TP para Banco do Trem")
tpTrainBtn.LayoutOrder = #teleportBtns + 1
tpTrainBtn.MouseButton1Click:Connect(function()
    local trainSeat = workspace:FindFirstChild("TrainSeat") or workspace:FindFirstChild("BancoDoTrem")
    if trainSeat and trainSeat:IsA("BasePart") then
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.CFrame = trainSeat.CFrame + Vector3.new(0, 3, 0)
        else
            warn("Personagem ou HumanoidRootPart não encontrado")
        end
    else
        warn("Banco do trem não encontrado.")
    end
end)

-- Função para criar Toggle Buttons (ligar/desligar)
local function createToggleButton(name)
    local btn = createButton(name.." [OFF]")
    local enabled = false
    btn.BackgroundColor3 = colors.accent

    local function toggle()
        enabled = not enabled
        btn.Text = name.." ["..(enabled and "ON" or "OFF").."]"
        btn.BackgroundColor3 = enabled and colors.enabled or colors.accent
        return enabled
    end

    btn.Toggle = toggle
    btn.IsEnabled = function() return enabled end
    return btn
end

-- Noclip
local noclipEnabled = false
local noclipBtn = createToggleButton("Noclip")
noclipBtn.MouseButton1Click:Connect(function()
    noclipEnabled = noclipBtn.Toggle()
end)

RunService.Stepped:Connect(function()
    if noclipEnabled and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    elseif LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)

-- Auto Farm
local autoFarmEnabled = false
local autoFarmBtn = createToggleButton("Auto Farm")
autoFarmBtn.MouseButton1Click:Connect(function()
    autoFarmEnabled = autoFarmBtn.Toggle()
end)

-- Auto Farm simples: ataque inimigos próximos
RunService.Heartbeat:Connect(function()
    if autoFarmEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        if workspace:FindFirstChild("Enemies") then
            for _, npc in pairs(workspace.Enemies:GetChildren()) do
                if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                    local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                    if dist <= 30 then
                        -- Teleporta perto do NPC e causa dano
                        hrp.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
                        npc.Humanoid:TakeDamage(20)
                        task.wait(0.2)
                    end
                end
            end
        end
    end
end)

-- ESP
local espEnabled = false
local espBtn = createToggleButton("ESP")
espBtn.MouseButton1Click:Connect(function()
    espEnabled = espBtn.Toggle()
    if not espEnabled then
        -- Remove todos os ESPs
        for _, espTag in pairs(workspace:GetChildren()) do
            if espTag.Name == "ESP_Tag" then
                espTag:Destroy()
            end
        end
    end
end)

local function createESPTag(target)
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP_Tag"
    billboard.Adornee = target
    billboard.Size = UDim2.new(0, 100, 0, 30)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true

    local label = Instance.new("TextLabel", billboard)
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 0.6
    label.BackgroundColor3 = Color3.new(0, 0, 0)
    label.TextColor3 = colors.main
    label.Font = Enum.Font.GothamBold
    label.TextSize = 14
    label.TextStrokeTransparency = 0
    label.Text = target.Parent.Name or "NPC"

    return billboard
end

RunService.Heartbeat:Connect(function()
    if espEnabled and workspace:FindFirstChild("Enemies") then
        local camera = workspace.CurrentCamera
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") then
                if not npc.HumanoidRootPart:FindFirstChild("ESP_Tag") then
                    local tag = createESPTag(npc.HumanoidRootPart)
                    tag.Parent = npc.HumanoidRootPart
                end
            end
        end
    end
end)

-- Kill Aura
local killAuraEnabled = false
local killAuraBtn = createToggleButton("Kill Aura")
killAuraBtn.MouseButton1Click:Connect(function()
    killAuraEnabled = killAuraBtn.Toggle()
end)

RunService.Heartbeat:Connect(function()
    if killAuraEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        if workspace:FindFirstChild("Enemies") then
            for _, npc in pairs(workspace.Enemies:GetChildren()) do
                if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                    local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                    if dist <= 10 then
                        npc.Humanoid:TakeDamage(30)
                    end
                end
            end
        end
    end
end)

-- Notificações simples
local function notify(text)
    local notif = Instance.new("TextLabel")
    notif.Size = UDim2.new(0, 200, 0, 40)
    notif.Position = UDim2.new(0.5, -100, 0.8, 0)
    notif.BackgroundColor3 = colors.accent
    notif.TextColor3 = colors.text
    notif.Font = Enum.Font.GothamBold
    notif.TextSize = 18
    notif.Text = text
    notif.TextWrapped = true
    notif.Parent = screenGui
    local corner = Instance.new("UICorner", notif)
    corner.CornerRadius = UDim.new(0, 6)

    task.delay(3, function()
        notif:Destroy()
    end)
end

notify("Natan Dead carregado com sucesso!")

-- Proteção para evitar múltiplas execuções
if _G.NatanDeadLoaded then
    notify("Script já está rodando!")
    return
else
    _G.NatanDeadLoaded = true
end
