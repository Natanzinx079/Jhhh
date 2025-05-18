-- NatanHub com visual e funções aprimorados
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui

gui.ResetOnSpawn = false

-- UI Styles
local darkColor = Color3.fromRGB(20, 20, 20)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 500, 0, 300)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
mainFrame.BackgroundColor3 = darkColor
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui
mainFrame.Visible = true
mainFrame.Active = true
mainFrame.Draggable = true

-- Title Bar
local titleBar = Instance.new("TextLabel")
titleBar.Parent = mainFrame
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleBar.Text = "NatHub | Dead Rails (0.3.1)"
titleBar.TextColor3 = textColor
titleBar.Font = Enum.Font.SourceSansBold
titleBar.TextSize = 16

gui.IgnoreGuiInset = true

-- Side Menu
local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Size = UDim2.new(0, 130, 1, -30)
sideMenu.Position = UDim2.new(0, 0, 0, 30)
sideMenu.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
sideMenu.Parent = mainFrame

-- Tab Buttons
local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local tabFrames = {}
local activeTab

for i, tabName in ipairs(tabs) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, 0, 0, 35)
    tabBtn.Position = UDim2.new(0, 0, 0, (i - 1) * 35)
    tabBtn.Text = tabName
    tabBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabBtn.TextColor3 = textColor
    tabBtn.Font = Enum.Font.SourceSansBold
    tabBtn.TextSize = 14
    tabBtn.Parent = sideMenu

    local tabFrame = Instance.new("Frame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -30)
    tabFrame.Position = UDim2.new(0, 130, 0, 30)
    tabFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab then
            activeTab.Visible = false
        end
        tabFrame.Visible = true
        activeTab = tabFrame
    end)
end

-- Character Tab Content
local charTab = tabFrames["Character"]

local walkSpeedLabel = Instance.new("TextLabel")
walkSpeedLabel.Text = "Walk Speed"
walkSpeedLabel.Position = UDim2.new(0, 10, 0, 20)
walkSpeedLabel.Size = UDim2.new(0, 200, 0, 25)
walkSpeedLabel.BackgroundTransparency = 1
walkSpeedLabel.TextColor3 = textColor
walkSpeedLabel.Font = Enum.Font.SourceSans
walkSpeedLabel.TextSize = 14
walkSpeedLabel.Parent = charTab

local walkSpeedSlider = Instance.new("TextBox")
walkSpeedSlider.Text = tostring(LocalPlayer.Character.Humanoid.WalkSpeed)
walkSpeedSlider.Size = UDim2.new(0, 100, 0, 30)
walkSpeedSlider.Position = UDim2.new(0, 10, 0, 50)
walkSpeedSlider.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
walkSpeedSlider.TextColor3 = textColor
walkSpeedSlider.Font = Enum.Font.SourceSans
walkSpeedSlider.TextSize = 14
walkSpeedSlider.Parent = charTab

walkSpeedSlider.FocusLost:Connect(function()
    local val = tonumber(walkSpeedSlider.Text)
    if val then
        pcall(function()
            LocalPlayer.Character.Humanoid.WalkSpeed = val
        end)
    end
end)

local jumpPowerLabel = Instance.new("TextLabel")
jumpPowerLabel.Text = "Jump Power"
jumpPowerLabel.Position = UDim2.new(0, 10, 0, 90)
jumpPowerLabel.Size = UDim2.new(0, 200, 0, 25)
jumpPowerLabel.BackgroundTransparency = 1
jumpPowerLabel.TextColor3 = textColor
jumpPowerLabel.Font = Enum.Font.SourceSans
jumpPowerLabel.TextSize = 14
jumpPowerLabel.Parent = charTab

local jumpPowerSlider = Instance.new("TextBox")
jumpPowerSlider.Text = tostring(LocalPlayer.Character.Humanoid.JumpPower)
jumpPowerSlider.Size = UDim2.new(0, 100, 0, 30)
jumpPowerSlider.Position = UDim2.new(0, 10, 0, 120)
jumpPowerSlider.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
jumpPowerSlider.TextColor3 = textColor
jumpPowerSlider.Font = Enum.Font.SourceSans
jumpPowerSlider.TextSize = 14
jumpPowerSlider.Parent = charTab

jumpPowerSlider.FocusLost:Connect(function()
    local val = tonumber(jumpPowerSlider.Text)
    if val then
        pcall(function()
            LocalPlayer.Character.Humanoid.JumpPower = val
        end)
    end
end)

-- Floating button
local floatBtn = Instance.new("TextButton")
floatBtn.Parent = gui
floatBtn.Text = "NatHub"
floatBtn.Size = UDim2.new(0, 120, 0, 40)
floatBtn.Position = UDim2.new(0, 10, 0, 10)
floatBtn.BackgroundColor3 = accentColor
floatBtn.TextColor3 = textColor
floatBtn.Font = Enum.Font.SourceSansBold
floatBtn.TextSize = 16
floatBtn.Visible = false
floatBtn.Active = true
floatBtn.Draggable = true

-- Minimize & Close
local closeBtn = Instance.new("TextButton")
closeBtn.Parent = mainFrame
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.TextColor3 = textColor
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextSize = 18

local minBtn = Instance.new("TextButton")
minBtn.Parent = mainFrame
minBtn.Text = "-"
minBtn.Size = UDim2.new(0, 30, 0, 30)
minBtn.Position = UDim2.new(1, -60, 0, 0)
minBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
minBtn.TextColor3 = textColor
minBtn.Font = Enum.Font.SourceSansBold
minBtn.TextSize = 18

closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

minBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    floatBtn.Visible = true
end)

floatBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    floatBtn.Visible = false
end)

-- Abrir aba "Character" como padrão
wait()
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]
