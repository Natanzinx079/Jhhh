-- NatanHub | Dead Rails Completo

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

-- UI Setup (sua base original aprimorada)

local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

local darkColor = Color3.fromRGB(25, 25, 25)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)
local highlightColor = Color3.fromRGB(35, 35, 35)

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 520, 0, 320)
mainFrame.Position = UDim2.new(0.5, -260, 0.5, -160)
mainFrame.BackgroundColor3 = darkColor
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui
mainFrame.Visible = true
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.ClipsDescendants = true
mainFrame.BackgroundTransparency = 0

local UICornerMain = Instance.new("UICorner", mainFrame)
UICornerMain.CornerRadius = UDim.new(0, 10)

local shadow = Instance.new("ImageLabel", mainFrame)
shadow.Image = "rbxassetid://1316045217"
shadow.ImageTransparency = 0.5
shadow.Size = UDim2.new(1, 30, 1, 30)
shadow.Position = UDim2.new(0, -15, 0, -15)
shadow.BackgroundTransparency = 1
shadow.ZIndex = 0

local titleBar = Instance.new("TextLabel")
titleBar.Parent = mainFrame
titleBar.Size = UDim2.new(1, 0, 0, 32)
titleBar.BackgroundColor3 = highlightColor
titleBar.Text = "NATAN DEAD RAILS 1.0"
titleBar.TextColor3 = Color3.fromRGB(255, 0, 0) -- vermelho
titleBar.Font = Enum.Font.GothamBold
titleBar.TextSize = 16
titleBar.BorderSizePixel = 0

local UICornerTitle = Instance.new("UICorner", titleBar)
UICornerTitle.CornerRadius = UDim.new(0, 6)

local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Size = UDim2.new(0, 130, 1, -32)
sideMenu.Position = UDim2.new(0, 0, 0, 32)
sideMenu.BackgroundColor3 = highlightColor
sideMenu.Parent = mainFrame

local UICornerSide = Instance.new("UICorner", sideMenu)
UICornerSide.CornerRadius = UDim.new(0, 6)

local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Config"}
local tabFrames = {}
local activeTab

for i, tabName in ipairs(tabs) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, 0, 0, 35)
    tabBtn.Position = UDim2.new(0, 0, 0, (i - 1) * 35)
    tabBtn.Text = tabName
    tabBtn.BackgroundColor3 = darkColor
    tabBtn.TextColor3 = textColor
    tabBtn.Font = Enum.Font.Gotham
    tabBtn.TextSize = 14
    tabBtn.Parent = sideMenu

    local UICornerBtn = Instance.new("UICorner", tabBtn)
    UICornerBtn.CornerRadius = UDim.new(0, 6)

    local tabFrame = Instance.new("Frame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -32)
    tabFrame.Position = UDim2.new(0, 130, 0, 32)
    tabFrame.BackgroundColor3 = darkColor
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab then activeTab.Visible = false end
        tabFrame.Visible = true
        activeTab = tabFrame
    end)
end

-- Botões de fechar e minimizar

local closeBtn = Instance.new("TextButton")
closeBtn.Parent = mainFrame
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
closeBtn.TextColor3 = textColor
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 16

local minBtn = Instance.new("TextButton")
minBtn.Parent = mainFrame
minBtn.Text = "-"
minBtn.Size = UDim2.new(0, 30, 0, 30)
minBtn.Position = UDim2.new(1, -60, 0, 0)
minBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
minBtn.TextColor3 = textColor
minBtn.Font = Enum.Font.GothamBold
minBtn.TextSize = 16

local closeCorner = Instance.new("UICorner", closeBtn)
closeCorner.CornerRadius = UDim.new(0, 6)
local minCorner = Instance.new("UICorner", minBtn)
minCorner.CornerRadius = UDim.new(0, 6)

closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

local floatBtn = Instance.new("TextButton")
floatBtn.Parent = gui
floatBtn.Text = "NatanHub"
floatBtn.Size = UDim2.new(0, 120, 0, 40)
floatBtn.Position = UDim2.new(0, 10, 0, 10)
floatBtn.BackgroundColor3 = accentColor
floatBtn.TextColor3 = textColor
floatBtn.Font = Enum.Font.GothamBold
floatBtn.TextSize = 16
floatBtn.Visible = false
floatBtn.Active = true
floatBtn.Draggable = true

local floatCorner = Instance.new("UICorner", floatBtn)
floatCorner.CornerRadius = UDim.new(0, 8)

minBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    floatBtn.Visible = true
end)

floatBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    floatBtn.Visible = false
end)

-- Abre aba Character por padrão
task.wait()
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]

-- Funções auxiliares UI
local function createLabel(text, posY, parent)
    local label = Instance.new("TextLabel")
    label.Text = text
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Size = UDim2.new(0, 200, 0, 25)
    label.BackgroundTransparency = 1
    label.TextColor3 = textColor
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.Parent = parent
    return label
end

local function createInputBox(value, posY, parent, callback)
    local box = Instance.new("TextBox")
    box.Text = tostring(value)
    box.Size = UDim2.new(0, 100, 0, 30)
    box.Position = UDim2.new(0, 10, 0, posY)
    box.BackgroundColor3 = highlightColor
    box.TextColor3 = textColor
    box.Font = Enum.Font.Gotham
    box.TextSize = 14
    box.Parent = parent

    local corner = Instance.new("UICorner", box)
    corner.CornerRadius = UDim.new(0, 6)

    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then pcall(callback, val) end
    end)

    return box
end

-- === Character Tab ===
local charTab = tabFrames["Character"]

createLabel("Walk Speed", 20, charTab)
createInputBox(Humanoid.WalkSpeed, 50, charTab, function(val)
    if val >= 8 and val <= 100 then
        Humanoid.WalkSpeed = val
    end
end)

createLabel("Jump Power", 90, charTab)
createInputBox(Humanoid.JumpPower, 120, charTab, function(val)
    if val >= 10 and val <= 150 then
        Humanoid.JumpPower = val
    end
end)

-- === Teleport Tab ===
local teleportTab = tabFrames["Teleport"]

-- Função para teleportar
local function teleportTo(pos)
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(pos)
    end
end

createLabel("Teleport to:", 10, teleportTab)

-- Exemplos de pontos, ajusta conforme mapa Dead Rails
local teleportPoints = {
    {Name = "Trem", Position = Vector3.new(0, 10, 0)},
    {Name = "Base 1", Position = Vector3.new(100, 10, 0)},
    {Name = "Base 2", Position = Vector3.new(200, 10, 0)},
    {Name = "Final", Position = Vector3.new(300, 10, 0)},
}

local buttonY = 40
for _, point in ipairs(teleportPoints) do
    local btn = Instance.new("TextButton")
    btn.Text = point.Name
    btn.Size = UDim2.new(0, 120, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, buttonY)
    btn.BackgroundColor3 = accentColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.Parent = teleportTab

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    btn.MouseButton1Click:Connect(function()
        teleportTo(point.Position)
    end)

    buttonY = buttonY + 40
end

-- === Visual Tab ===
local visualTab = tabFrames["Visual"]

local fullbrightEnabled = false
local fogEnabled = true

local fullbrightBtn = Instance.new("TextButton")
fullbrightBtn.Text = "Toggle FullBright"
fullbrightBtn.Size = UDim2.new(0, 150, 0, 30)
fullbrightBtn.Position = UDim2.new(0, 10, 0, 10)
fullbrightBtn.BackgroundColor3 = accentColor
fullbrightBtn.TextColor3 = textColor
fullbrightBtn.Font = Enum.Font.GothamBold
fullbrightBtn.TextSize = 14
fullbrightBtn.Parent = visualTab

local cornerFB = Instance.new("UICorner", fullbrightBtn)
cornerFB.CornerRadius = UDim.new(0, 6)

fullbrightBtn.MouseButton1Click:Connect(function()
    if fullbrightEnabled then
        Lighting.Brightness = 1
        Lighting.FogEnd = 100000
        fullbrightEnabled = false
    else
        Lighting.Brightness = 2
        Lighting.FogEnd = 100000
        fullbrightEnabled = true
    end
end)

local fogBtn = Instance.new("TextButton")
fogBtn.Text = "Toggle Fog"
fogBtn.Size = UDim2.new(0, 150, 0, 30)
fogBtn.Position = UDim2.new(0, 10, 0, 50)
fogBtn.BackgroundColor3 = accentColor
fogBtn.TextColor3 = textColor
fogBtn.Font = Enum.Font.GothamBold
fogBtn.TextSize = 14
fogBtn.Parent = visualTab

local cornerFog = Instance.new("UICorner", fogBtn)
cornerFog.CornerRadius = UDim.new(0, 6)

fogBtn.MouseButton1Click:Connect(function()
    if fogEnabled then
        Lighting.FogEnd = 0
        fogEnabled = false
    else
        Lighting.FogEnd = 100000
        fogEnabled = true
    end
end)

-- === Combat Tab ===
local combatTab = tabFrames["Combat"]

local killAuraEnabled = false
local killAuraRange = 15

local killAuraBtn = Instance.new("TextButton")
killAuraBtn.Text = "Toggle Kill Aura"
killAuraBtn.Size = UDim2.new(0, 150, 0, 30)
killAuraBtn.Position = UDim2.new(0, 10, 0, 10)
killAuraBtn.BackgroundColor3 = accentColor
killAuraBtn.TextColor3 = textColor
killAuraBtn.Font = Enum.Font.GothamBold
killAuraBtn.TextSize = 14
killAuraBtn.Parent = combatTab

local cornerKA = Instance.new("UICorner", killAuraBtn)
cornerKA.CornerRadius = UDim.new(0, 6)

killAuraBtn.MouseButton1Click:Connect(function()
    killAuraEnabled = not killAuraEnabled
end)

-- Kill Aura Loop
RunService.Heartbeat:Connect(function()
    if killAuraEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                if dist <= killAuraRange then
                    -- Atacar NPC (ajuste conforme dano do jogo)
                    npc.Humanoid:TakeDamage(10)
                end
            end
        end
    end
end)

-- === Config Tab ===
local configTab = tabFrames["Config"]

-- Noclip toggle
local noclipEnabled = false

local noclipBtn = Instance.new("TextButton")
noclipBtn.Text = "Toggle Noclip"
noclipBtn.Size = UDim2.new(0, 150, 0, 30)
noclipBtn.Position = UDim2.new(0, 10, 0, 10)
noclipBtn.BackgroundColor3 = accentColor
noclipBtn.TextColor3 = textColor
noclipBtn.Font = Enum.Font.GothamBold
noclipBtn.TextSize = 14
noclipBtn.Parent = configTab

local cornerNC = Instance.new("UICorner", noclipBtn)
cornerNC.CornerRadius = UDim.new(0, 6)

noclipBtn.MouseButton1Click:Connect(function()
    noclipEnabled = not noclipEnabled
    if noclipEnabled then
        LocalPlayer.Character.Humanoid.PlatformStand = true
    else
        LocalPlayer.Character.Humanoid.PlatformStand = false
    end
end)

-- Noclip loop
RunService.Stepped:Connect(function()
    if noclipEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
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

-- Unlock mouse and camera
local unlockMouseBtn = Instance.new("TextButton")
unlockMouseBtn.Text = "Unlock Mouse"
unlockMouseBtn.Size = UDim2.new(0, 150, 0, 30)
unlockMouseBtn.Position = UDim2.new(0, 10, 0, 50)
unlockMouseBtn.BackgroundColor3 = accentColor
unlockMouseBtn.TextColor3 = textColor
unlockMouseBtn.Font = Enum.Font.GothamBold
unlockMouseBtn.TextSize = 14
unlockMouseBtn.Parent = configTab

local cornerUM = Instance.new("UICorner", unlockMouseBtn)
cornerUM.CornerRadius = UDim.new(0, 6)

unlockMouseBtn.MouseButton1Click:Connect(function()
    UserInputService.MouseBehavior = Enum.MouseBehavior.Default
    UserInputService.MouseIconEnabled = true
end)

-- Auto farm toggle
local autoFarmEnabled = false
local autoFarmBtn = Instance.new("TextButton")
autoFarmBtn.Text = "Toggle Auto Farm"
autoFarmBtn.Size = UDim2.new(0, 150, 0, 30)
autoFarmBtn.Position = UDim2.new(0, 10, 0, 90)
autoFarmBtn.BackgroundColor3 = accentColor
autoFarmBtn.TextColor3 = textColor
autoFarmBtn.Font = Enum.Font.GothamBold
autoFarmBtn.TextSize = 14
autoFarmBtn.Parent = configTab

local cornerAF = Instance.new("UICorner", autoFarmBtn)
cornerAF.CornerRadius = UDim.new(0, 6)

autoFarmBtn.MouseButton1Click:Connect(function()
    autoFarmEnabled = not autoFarmEnabled
end)

-- Auto farm loop
RunService.Heartbeat:Connect(function()
    if autoFarmEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character.HumanoidRootPart
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                if dist <= 30 then
                    -- Teleport para perto do NPC e ataca
                    LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
                    npc.Humanoid:TakeDamage(15)
                    task.wait(0.3)
                end
            end
        end
    end
end)

-- ESP simples para NPCs e objetos importantes

local espEnabled = false

local espBtn = Instance.new("TextButton")
espBtn.Text = "Toggle ESP"
espBtn.Size = UDim2.new(0, 150, 0, 30)
espBtn.Position = UDim2.new(0, 10, 0, 130)
espBtn.BackgroundColor3 = accentColor
espBtn.TextColor3 = textColor
espBtn.Font = Enum.Font.GothamBold
espBtn.TextSize = 14
espBtn.Parent = configTab

local cornerESP = Instance.new("UICorner", espBtn)
cornerESP.CornerRadius = UDim.new(0, 6)

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
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.Font = Enum.Font.GothamBold
    label.TextSize = 14
    label.TextStrokeTransparency = 0
    label.Text = target.Parent.Name or "NPC"

    return billboard
end

local espTags = {}

espBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if not espEnabled then
        for _, tag in pairs(espTags) do
            tag:Destroy()
        end
        espTags = {}
    end
end)

RunService.Heartbeat:Connect(function()
    if espEnabled then
        for _, npc in pairs(workspace.Enemies:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") and not espTags[npc.HumanoidRootPart] then
                local tag = createESPTag(npc.HumanoidRootPart)
                tag.Parent = workspace.CurrentCamera
                espTags[npc.HumanoidRootPart] = tag
            end
        end
    end
end)

-- Ativar aba Main para o usuário

local mainTab = tabFrames["Main"]
mainTab.Visible = true
if activeTab then activeTab.Visible = false end
activeTab = mainTab

-- Exemplo de conteúdo na aba Main

local mainLabel = Instance.new("TextLabel")
mainLabel.Text = "Bem-vindo ao Natan Dead Rails 1.0"
mainLabel.Size = UDim2.new(1, -20, 0, 40)
mainLabel.Position = UDim2.new(0, 10, 0, 10)
mainLabel.BackgroundTransparency = 1
mainLabel.TextColor3 = textColor
mainLabel.Font = Enum.Font.GothamBold
mainLabel.TextSize = 18
mainLabel.Parent = mainTab

return gui
