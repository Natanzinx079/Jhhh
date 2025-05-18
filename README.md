b = tabFrames["Character"]
-- NatanHub completo para Dead Rails (0.3.1) - UI + funcionalidades

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Mouse = LocalPlayer:GetMouse()

local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

-- Cores
local darkColor = Color3.fromRGB(25, 25, 25)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)
local highlightColor = Color3.fromRGB(35, 35, 35)
local redColor = Color3.fromRGB(255, 50, 50)

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 520, 0, 350)
mainFrame.Position = UDim2.new(0.5, -260, 0.5, -175)
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

-- Title Bar
local titleBar = Instance.new("TextLabel")
titleBar.Parent = mainFrame
titleBar.Size = UDim2.new(1, 0, 0, 32)
titleBar.BackgroundColor3 = highlightColor
titleBar.Text = "NATAN DEAD RAILS 1.0"
titleBar.TextColor3 = redColor
titleBar.Font = Enum.Font.GothamBold
titleBar.TextSize = 18
titleBar.BorderSizePixel = 0

local UICornerTitle = Instance.new("UICorner", titleBar)
UICornerTitle.CornerRadius = UDim.new(0, 6)

-- Side Menu
local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Size = UDim2.new(0, 130, 1, -32)
sideMenu.Position = UDim2.new(0, 0, 0, 32)
sideMenu.BackgroundColor3 = highlightColor
sideMenu.Parent = mainFrame

local UICornerSide = Instance.new("UICorner", sideMenu)
UICornerSide.CornerRadius = UDim.new(0, 6)

-- Tabs
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

-- Helper para criar Labels
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

-- Helper para criar TextBoxes
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

---------------------
-- Aba Character --
---------------------
local charTab = tabFrames["Character"]

createLabel("Walk Speed", 20, charTab)
createInputBox(Humanoid.WalkSpeed, 50, charTab, function(val)
    if Humanoid and Humanoid.Parent then
        Humanoid.WalkSpeed = val
    end
end)

createLabel("Jump Power", 90, charTab)
createInputBox(Humanoid.JumpPower, 120, charTab, function(val)
    if Humanoid and Humanoid.Parent then
        Humanoid.JumpPower = val
    end
end)

----------------------
-- Aba Teleport --
----------------------
local teleportTab = tabFrames["Teleport"]

createLabel("Teleport to Train", 10, teleportTab)
local tpTrainBtn = Instance.new("TextButton")
tpTrainBtn.Text = "Teleport Train"
tpTrainBtn.Size = UDim2.new(0, 150, 0, 30)
tpTrainBtn.Position = UDim2.new(0, 10, 0, 40)
tpTrainBtn.BackgroundColor3 = accentColor
tpTrainBtn.TextColor3 = textColor
tpTrainBtn.Font = Enum.Font.GothamBold
tpTrainBtn.TextSize = 14
tpTrainBtn.Parent = teleportTab
local cornerTpTrain = Instance.new("UICorner", tpTrainBtn)
cornerTpTrain.CornerRadius = UDim.new(0, 6)

tpTrainBtn.MouseButton1Click:Connect(function()
    local train = workspace:FindFirstChild("Train") or workspace:FindFirstChild("TrainModel")
    if train and Character and Character:FindFirstChild("HumanoidRootPart") then
        Character.HumanoidRootPart.CFrame = train:GetModelCFrame() or train.CFrame or train.PrimaryPart.CFrame
    else
        warn("Train not found")
    end
end)

createLabel("Teleport to Bases", 80, teleportTab)

-- Detectar todas bases no workspace (supondo que ficam em workspace.Bases)
local basesFolder = workspace:FindFirstChild("Bases") or workspace:FindFirstChild("BasesFolder")

local baseButtons = {}
local baseY = 110

if basesFolder then
    for _, base in ipairs(basesFolder:GetChildren()) do
        if base:IsA("BasePart") or base.PrimaryPart then
            local baseBtn = Instance.new("TextButton")
            baseBtn.Text = base.Name
            baseBtn.Size = UDim2.new(0, 150, 0, 30)
            baseBtn.Position = UDim2.new(0, 10, 0, baseY)
            baseBtn.BackgroundColor3 = accentColor
            baseBtn.TextColor3 = textColor
            baseBtn.Font = Enum.Font.GothamBold
            baseBtn.TextSize = 14
            baseBtn.Parent = teleportTab
            local corner = Instance.new("UICorner", baseBtn)
            corner.CornerRadius = UDim.new(0, 6)

            baseBtn.MouseButton1Click:Connect(function()
                local targetCFrame
                if base.PrimaryPart then
                    targetCFrame = base.PrimaryPart.CFrame + Vector3.new(0, 3, 0)
                elseif base:IsA("BasePart") then
                    targetCFrame = base.CFrame + Vector3.new(0, 3, 0)
                end

                if targetCFrame and Character and Character:FindFirstChild("HumanoidRootPart") then
                    Character.HumanoidRootPart.CFrame = targetCFrame
                end
            end)
            baseY = baseY + 40
        end
    end
else
    createLabel("Bases folder not found", 110, teleportTab)
end

createLabel("Teleport to End", baseY + 10, teleportTab)
local tpEndBtn = Instance.new("TextButton")
tpEndBtn.Text = "Teleport End"
tpEndBtn.Size = UDim2.new(0, 150, 0, 30)
tpEndBtn.Position = UDim2.new(0, 10, 0, baseY + 40)
tpEndBtn.BackgroundColor3 = accentColor
tpEndBtn.TextColor3 = textColor
tpEndBtn.Font = Enum.Font.GothamBold
tpEndBtn.TextSize = 14
tpEndBtn.Parent = teleportTab
local cornerTpEnd = Instance.new("UICorner", tpEndBtn)
cornerTpEnd.CornerRadius = UDim.new(0, 6)

tpEndBtn.MouseButton1Click:Connect(function()
    local finish = workspace:FindFirstChild("Finish") or workspace:FindFirstChild("EndPoint")
    if finish and finish:IsA("BasePart") and Character and Character:FindFirstChild("HumanoidRootPart") then
        Character.HumanoidRootPart.CFrame = finish.CFrame + Vector3.new(0, 3, 0)
    else
        warn("End not found")
    end
end)

----------------------
-- Aba Visual --
----------------------
local visualTab = tabFrames["Visual"]

-- FullBright toggle
local fullBrightBtn = Instance.new("TextButton")
fullBrightBtn.Text = "Toggle Full Bright"
fullBrightBtn.Size = UDim2.new(0, 200, 0, 35)
fullBrightBtn.Position = UDim2.new(0, 10, 0, 20)
fullBrightBtn.BackgroundColor3 = accentColor
fullBrightBtn.TextColor3 = textColor
fullBrightBtn.Font = Enum.Font.GothamBold
fullBrightBtn.TextSize = 14
fullBrightBtn.Parent = visualTab
local cornerFullBright = Instance.new("UICorner", fullBrightBtn)
cornerFullBright.CornerRadius = UDim.new(0, 6)

local originalLightingSettings = {
    Brightness = Lighting.Brightness,
    ClockTime = Lighting.ClockTime,
    FogEnd = Lighting.FogEnd,
    GlobalShadows = Lighting.GlobalShadows,
    Ambient = Lighting.Ambient,
    OutdoorAmbient = Lighting.OutdoorAmbient,
}

local fullBrightOn = false
fullBrightBtn.MouseButton1Click:Connect(function()
    fullBrightOn = not fullBrightOn
    if fullBrightOn then
        Lighting.Brightness = 2
        Lighting.ClockTime = 14
        Lighting.FogEnd = 100000
        Lighting.GlobalShadows = false
        Lighting.Ambient = Color3.new(1, 1, 1)
        Lighting.OutdoorAmbient = Color3.new(1, 1, 1)
    else
        Lighting.Brightness = originalLightingSettings.Brightness
        Lighting.ClockTime = originalLightingSettings.ClockTime
        Lighting.FogEnd = originalLightingSettings.FogEnd
        Lighting.GlobalShadows = originalLightingSettings.GlobalShadows
        Lighting.Ambient = originalLightingSettings.Ambient
        Lighting.OutdoorAmbient = originalLightingSettings.OutdoorAmbient
    end
end)

-- Remove Fog toggle
local removeFogBtn = Instance.new("TextButton")
removeFogBtn.Text = "Toggle Remove Fog"
removeFogBtn.Size = UDim2.new(0, 200, 0, 35)
removeFogBtn.Position = UDim2.new(0, 10, 0, 70)
removeFogBtn.BackgroundColor3 = accentColor
removeFogBtn.TextColor3 = textColor
removeFogBtn.Font = Enum.Font.GothamBold
removeFogBtn.TextSize = 14
removeFogBtn.Parent = visualTab
local cornerRemoveFog = Instance.new("UICorner", removeFogBtn)
cornerRemoveFog.CornerRadius = UDim.new(0, 6)

local fogRemoved = false
removeFogBtn.MouseButton1Click:Connect(function()
    fogRemoved = not fogRemoved
    if fogRemoved then
        Lighting.FogEnd = 100000
    else
        Lighting.FogEnd = originalLightingSettings.FogEnd
    end
end)

----------------------
-- Aba Combat --
----------------------
local combatTab = tabFrames["Combat"]

-- Kill Aura toggle
local killAuraBtn = Instance.new("TextButton")
killAuraBtn.Text = "Toggle Kill Aura"
killAuraBtn.Size = UDim2.new(0, 150, 0, 35)
killAuraBtn.Position = UDim2.new(0, 10, 0, 20)
killAuraBtn.BackgroundColor3 = accentColor
killAuraBtn.TextColor3 = textColor
killAuraBtn.Font = Enum.Font.GothamBold
killAuraBtn.TextSize = 14
killAuraBtn.Parent = combatTab
local cornerKillAura = Instance.new("UICorner", killAuraBtn)
cornerKillAura.CornerRadius = UDim.new(0, 6)

local killAuraOn = false
local killRange = 10
local RunService = game:GetService("RunService")

killAuraBtn.MouseButton1Click:Connect(function()
    killAuraOn = not killAuraOn
end)

RunService.Heartbeat:Connect(function()
    if killAuraOn and Character and Character:FindFirstChild("HumanoidRootPart") then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health > 0 then
                local dist = (player.Character.HumanoidRootPart.Position - Character.HumanoidRootPart.Position).Magnitude
                if dist <= killRange then
                    -- Aplica dano (supondo método simples)
                    player.Character.Humanoid.Health = 0
                end
            end
        end
    end
end)

-- Noclip toggle
local noclipBtn = Instance.new("TextButton")
noclipBtn.Text = "Toggle Noclip"
noclipBtn.Size = UDim2.new(0, 150, 0, 35)
noclipBtn.Position = UDim2.new(0, 10, 0, 70)
noclipBtn.BackgroundColor3 = accentColor
noclipBtn.TextColor3 = textColor
noclipBtn.Font = Enum.Font.GothamBold
noclipBtn.TextSize = 14
noclipBtn.Parent = combatTab
local cornerNoclip = Instance.new("UICorner", noclipBtn)
cornerNoclip.CornerRadius = UDim.new(0, 6)

local noclipOn = false
local function noclip(state)
    if not Character then return end
    for _, part in pairs(Character:GetChildren()) do
        if part:IsA("BasePart") then
            part.CanCollide = not state
        end
    end
end

noclipBtn.MouseButton1Click:Connect(function()
    noclipOn = not noclipOn
    noclip(noclipOn)
end)

----------------------
-- Aba Main --
----------------------
local mainTab = tabFrames["Main"]

-- Unlock Mouse
local unlockMouseBtn = Instance.new("TextButton")
unlockMouseBtn.Text = "Toggle Unlock Mouse"
unlockMouseBtn.Size = UDim2.new(0, 180, 0, 35)
unlockMouseBtn.Position = UDim2.new(0, 10, 0, 20)
unlockMouseBtn.BackgroundColor3 = accentColor
unlockMouseBtn.TextColor3 = textColor
unlockMouseBtn.Font = Enum.Font.GothamBold
unlockMouseBtn.TextSize = 14
unlockMouseBtn.Parent = mainTab
local cornerUnlockMouse = Instance.new("UICorner", unlockMouseBtn)
cornerUnlockMouse.CornerRadius = UDim.new(0, 6)

local mouseUnlocked = false
unlockMouseBtn.MouseButton1Click:Connect(function()
    mouseUnlocked = not mouseUnlocked
    UserInputService.MouseBehavior = mouseUnlocked and Enum.MouseBehavior.Default or Enum.MouseBehavior.LockCenter
    UserInputService.MouseIconEnabled = mouseUnlocked
end)

-- Auto Farm Toggle
local autoFarmBtn = Instance.new("TextButton")
autoFarmBtn.Text = "Toggle Auto Farm"
autoFarmBtn.Size = UDim2.new(0, 180, 0, 35)
autoFarmBtn.Position = UDim2.new(0, 10, 0, 70)
autoFarmBtn.BackgroundColor3 = accentColor
autoFarmBtn.TextColor3 = textColor
autoFarmBtn.Font = Enum.Font.GothamBold
autoFarmBtn.TextSize = 14
autoFarmBtn.Parent = mainTab
local cornerAutoFarm = Instance.new("UICorner", autoFarmBtn)
cornerAutoFarm.CornerRadius = UDim.new(0, 6)

local autoFarmOn = false

autoFarmBtn.MouseButton1Click:Connect(function()
    autoFarmOn = not autoFarmOn
end)

-- Auto Farm loop
RunService.Heartbeat:Connect(function()
    if autoFarmOn and Character and Character:FindFirstChild("HumanoidRootPart") then
        -- Exemplo: Teleportar para bases automaticamente e coletar
        if basesFolder then
            for _, base in ipairs(basesFolder:GetChildren()) do
                if base.PrimaryPart then
                    Character.HumanoidRootPart.CFrame = base.PrimaryPart.CFrame + Vector3.new(0, 3, 0)
                    wait(1)
                elseif base:IsA("BasePart") then
                    Character.HumanoidRootPart.CFrame = base.CFrame + Vector3.new(0, 3, 0)
                    wait(1)
                end
            end
        end
    end
end)

-- Abrir Aba Main por padrão
tabFrames["Main"].Visible = true
activeTab = tabFrames["Main"]
