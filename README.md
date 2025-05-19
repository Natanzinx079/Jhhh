-- Natan Dead Rails 1.0 - Script melhorado e corrigido
-- Compatível Delta Executor / Android

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local TweenService = game:GetService("TweenService")

-- Variáveis e configurações de cores
local guiOpen = true
local mainColor = Color3.fromRGB(255, 0, 0) -- vermelho para título
local bgColor = Color3.fromRGB(25, 25, 25)
local accentColor = Color3.fromRGB(40, 40, 40)
local textColor = Color3.fromRGB(255, 255, 255)
local espColor = Color3.fromRGB(255, 50, 50)

-- Criando GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "NatanDeadRailsUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = game:GetService("CoreGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 350, 0, 500)
mainFrame.Position = UDim2.new(0, 20, 0, 50)
mainFrame.BackgroundColor3 = bgColor
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.Active = true
mainFrame.Draggable = true

local uiCorner = Instance.new("UICorner", mainFrame)
uiCorner.CornerRadius = UDim.new(0, 10)

-- Título
local title = Instance.new("TextLabel")
title.Text = "NATAN DEAD RAILS 1.0"
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.TextColor3 = mainColor
title.BackgroundTransparency = 1
title.Size = UDim2.new(1, 0, 0, 40)
title.Parent = mainFrame

-- Botão fechar/abrir
local toggleBtn = Instance.new("TextButton")
toggleBtn.Text = "X"
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 20
toggleBtn.TextColor3 = textColor
toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
toggleBtn.Size = UDim2.new(0, 40, 0, 40)
toggleBtn.Position = UDim2.new(1, -45, 0, 0)
toggleBtn.Parent = mainFrame
local cornerToggle = Instance.new("UICorner", toggleBtn)
cornerToggle.CornerRadius = UDim.new(0, 6)

toggleBtn.MouseButton1Click:Connect(function()
    guiOpen = not guiOpen
    if guiOpen then
        mainFrame:TweenSize(UDim2.new(0, 350, 0, 500), "Out", "Quad", 0.3, true)
        toggleBtn.Text = "X"
    else
        mainFrame:TweenSize(UDim2.new(0, 40, 0, 40), "Out", "Quad", 0.3, true)
        toggleBtn.Text = ">"
    end
end)

-- Container para botões
local buttonsFrame = Instance.new("Frame")
buttonsFrame.Size = UDim2.new(1, -20, 1, -50)
buttonsFrame.Position = UDim2.new(0, 10, 0, 45)
buttonsFrame.BackgroundTransparency = 1
buttonsFrame.Parent = mainFrame
buttonsFrame.ClipsDescendants = true

-- Função para criar botões bonitos
local function createButton(text, posY)
    local btn = Instance.new("TextButton")
    btn.Text = text
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Position = UDim2.new(0, 0, 0, posY)
    btn.BackgroundColor3 = accentColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.Parent = buttonsFrame
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 8)
    return btn
end

-- Teleport para banco do trem
local tpTrainBtn = createButton("Teleporte para Banco do Trem", 0)
tpTrainBtn.MouseButton1Click:Connect(function()
    local trainSeat = workspace:FindFirstChild("TrainSeat") or workspace:FindFirstChild("BancoDoTrem") -- ajusta nome do objeto se necessário
    if trainSeat and trainSeat:IsA("BasePart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = trainSeat.CFrame + Vector3.new(0, 3, 0)
    else
        warn("Banco do trem não encontrado.")
    end
end)

-- Teleporte para torre final (torreta)
local tpEndBtn = createButton("Teleporte para Torreta Final", 50)
tpEndBtn.MouseButton1Click:Connect(function()
    local endTurret = workspace:FindFirstChild("EndTurret") or workspace:FindFirstChild("TorretaFinal") -- ajuste nome real do objeto da torreta final no jogo
    if endTurret and endTurret:IsA("BasePart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = endTurret.CFrame + Vector3.new(0, 3, 0)
    else
        warn("Torreta final não encontrada.")
    end
end)

-- Teleporte para o final da linha (exemplo baseado no cenário)
local tpFinalBtn = createButton("Teleporte para Final da Linha", 100)
tpFinalBtn.MouseButton1Click:Connect(function()
    local finalPart = workspace:FindFirstChild("EndPoint") or workspace:FindFirstChild("FinalPoint") -- ajustar nome do objeto que marca o fim da linha
    if finalPart and finalPart:IsA("BasePart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = finalPart.CFrame + Vector3.new(0, 3, 0)
    else
        warn("Final da linha não encontrado.")
    end
end)

-- Noclip toggle
local noclipEnabled = false
local noclipBtn = createButton("Toggle Noclip", 150)
noclipBtn.MouseButton1Click:Connect(function()
    noclipEnabled = not noclipEnabled
    if noclipEnabled then
        noclipBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    else
        noclipBtn.BackgroundColor3 = accentColor
    end
end)

-- Implementação do noclip
RunService.Stepped:Connect(function()
    if noclipEnabled and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") and part.CanCollide == true then
                part.CanCollide = false
            end
        end
    elseif LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") and part.CanCollide == false then
                part.CanCollide = true
            end
        end
    end
end)

-- Auto farm toggle
local autoFarmEnabled = false
local autoFarmBtn = createButton("Toggle Auto Farm", 200)
autoFarmBtn.MouseButton1Click:Connect(function()
    autoFarmEnabled = not autoFarmEnabled
    if autoFarmEnabled then
        autoFarmBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    else
        autoFarmBtn.BackgroundColor3 = accentColor
    end
end)

-- Auto farm loop simples (atacar inimigos próximos)
RunService.Heartbeat:Connect(function()
    if autoFarmEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                if dist <= 30 then
                    -- Teleporta perto do NPC e causa dano
                    LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
                    npc.Humanoid:TakeDamage(15)
                    task.wait(0.3)
                end
            end
        end
    end
end)

-- ESP simples com linha para inimigos
local espEnabled = false
local espBtn = createButton("Toggle ESP", 250)
local espTags = {}

local function createESPTag(target)
    local billboard = Instance.new("BillboardGui")
    billboard.Adornee = target
    billboard.Size = UDim2.new(0, 100, 0, 30)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true

    local label = Instance.new("TextLabel", billboard)
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 0.5
    label.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    label.TextColor3 = espColor
    label.Font = Enum.Font.GothamBold
    label.TextSize = 14
    label.TextStrokeTransparency = 0
    label.Text = target.Parent.Name or "NPC"

    return billboard
end

local function drawLineToTarget(target)
    local camera = workspace.CurrentCamera
    local line = Drawing and Drawing.new("Line") or nil
    if not line then return nil end
    return line
end

espBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if not espEnabled then
        for _, tag in pairs(espTags) do
            if tag.Billboard then tag.Billboard:Destroy() end
            if tag.Line then tag.Line:Remove() end
        end
        espTags = {}
        -- Remove linhas Drawing (se usado)
    end
end)

RunService.Heartbeat:Connect(function()
    if espEnabled then
        local camera = workspace.CurrentCamera
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") then
                if not espTags[npc.HumanoidRootPart] then
                    local tag = createESPTag(npc.HumanoidRootPart)
                    tag.Parent = camera
                    espTags[npc.HumanoidRootPart] = {Billboard = tag}
                end
            end
        end
    end
end)

-- Kill Aura básico (dano em área)
local killAuraEnabled = false
local killAuraBtn = createButton("Toggle Kill Aura", 300)
killAuraBtn.MouseButton1Click:Connect(function()
    killAuraEnabled = not killAuraEnabled
    if killAuraEnabled then
        killAuraBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    else
        killAuraBtn.BackgroundColor3 = accentColor
    end
end)

RunService.Heartbeat:Connect(function()
    if killAuraEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                if dist <= 10 then
                    npc.Humanoid:TakeDamage(20)
                end
            end
        end
    end
end)

-- Aviso inicial
local infoLabel = Instance.new("TextLabel")
infoLabel.Text = "Script criado por Natan"
infoLabel.Font = Enum.Font.GothamBold
infoLabel.TextSize = 16
infoLabel.TextColor3 = mainColor
infoLabel.BackgroundTransparency = 1
infoLabel.Size = UDim2.new(1, 0, 0, 30)
infoLabel.Position = UDim2.new(0, 0, 1, -35)
infoLabel.Parent = mainFrame

print("Natan Dead Rails 1.0 carregado com sucesso.")
