Frames["Character"]
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui
gui.IgnoreGuiInset = true
gui.ResetOnSpawn = false

-- UI Styles
local darkColor = Color3.fromRGB(20, 20, 20)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)
local borderColor = Color3.fromRGB(60, 60, 60)

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 520, 0, 320)
mainFrame.Position = UDim2.new(0.5, -260, 0.5, -160)
mainFrame.BackgroundColor3 = darkColor
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui
mainFrame.Active = true
mainFrame.Draggable = true

local corner = Instance.new("UICorner", mainFrame)
corner.CornerRadius = UDim.new(0, 10)

-- Title Bar
local titleBar = Instance.new("TextLabel")
titleBar.Parent = mainFrame
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleBar.Text = "NatHub | Dead Rails (0.3.1)"
titleBar.TextColor3 = textColor
titleBar.Font = Enum.Font.SourceSansBold
titleBar.TextSize = 16

-- Side Menu
local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Size = UDim2.new(0, 130, 1, -30)
sideMenu.Position = UDim2.new(0, 0, 0, 30)
sideMenu.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
sideMenu.Parent = mainFrame

local sideLayout = Instance.new("UIListLayout", sideMenu)
sideLayout.Padding = UDim.new(0, 5)

-- Tab Setup
local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local tabFrames = {}
local activeTab

for i, tabName in ipairs(tabs) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, -10, 0, 35)
    tabBtn.Text = tabName
    tabBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabBtn.TextColor3 = textColor
    tabBtn.Font = Enum.Font.SourceSansBold
    tabBtn.TextSize = 14
    tabBtn.Parent = sideMenu

    local corner = Instance.new("UICorner", tabBtn)
    corner.CornerRadius = UDim.new(0, 6)

    local tabFrame = Instance.new("Frame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -30)
    tabFrame.Position = UDim2.new(0, 130, 0, 30)
    tabFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab then activeTab.Visible = false end
        tabFrame.Visible = true
        activeTab = tabFrame
    end)
end

-- Character Tab
local charTab = tabFrames["Character"]

local container = Instance.new("Frame", charTab)
container.Size = UDim2.new(1, 0, 1, 0)
container.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", container)
layout.Padding = UDim.new(0, 10)
layout.SortOrder = Enum.SortOrder.LayoutOrder

local padding = Instance.new("UIPadding", container)
padding.PaddingLeft = UDim.new(0, 15)
padding.PaddingTop = UDim.new(0, 15)

local function createSlider(labelText, minValue, maxValue, defaultValue, onChange)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 300, 0, 60)
    frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    frame.LayoutOrder = 1

    local corner = Instance.new("UICorner", frame)
    corner.CornerRadius = UDim.new(0, 8)

    local stroke = Instance.new("UIStroke", frame)
    stroke.Color = borderColor
    stroke.Thickness = 1

    local label = Instance.new("TextLabel", frame)
    label.Text = labelText
    label.Size = UDim2.new(1, -20, 0, 20)
    label.Position = UDim2.new(0, 10, 0, 5)
    label.BackgroundTransparency = 1
    label.TextColor3 = textColor
    label.Font = Enum.Font.SourceSansBold
    label.TextSize = 14
    label.TextXAlignment = Enum.TextXAlignment.Left

    local slider = Instance.new("TextBox", frame)
    slider.Text = tostring(defaultValue)
    slider.Size = UDim2.new(1, -20, 0, 25)
    slider.Position = UDim2.new(0, 10, 0, 30)
    slider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    slider.TextColor3 = textColor
    slider.Font = Enum.Font.SourceSans
    slider.TextSize = 14

    local corner2 = Instance.new("UICorner", slider)
    corner2.CornerRadius = UDim.new(0, 6)

    slider.FocusLost:Connect(function()
        local val = tonumber(slider.Text)
        if val and val >= minValue and val <= maxValue then
            onChange(val)
        else
            slider.Text = tostring(defaultValue)
        end
    end)

    frame.Parent = container
end

createSlider("Walk Speed", 1, 100, Character:WaitForChild("Humanoid").WalkSpeed, function(val)
    pcall(function()
        Character.Humanoid.WalkSpeed = val
    end)
end)

createSlider("Jump Power", 1, 200, Character.Humanoid.JumpPower, function(val)
    pcall(function()
        Character.Humanoid.JumpPower = val
    end)
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

local cornerFloat = Instance.new("UICorner", floatBtn)
cornerFloat.CornerRadius = UDim.new(0, 8)

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

-- Abrir aba Character como padrão
task.wait()
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]
