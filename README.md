local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

local darkColor = Color3.fromRGB(20, 20, 20)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)

local iconList = {
    Main = "rbxassetid://7733960981",
    Character = "rbxassetid://7734026636",
    Teleport = "rbxassetid://7734075736",
    Visual = "rbxassetid://7734068321",
    Combat = "rbxassetid://7734032644",
    Configuration = "rbxassetid://7733911829"
}

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

-- Side Menu
local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Size = UDim2.new(0, 130, 1, -30)
sideMenu.Position = UDim2.new(0, 0, 0, 30)
sideMenu.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
sideMenu.Parent = mainFrame

-- Tabs
local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local tabFrames = {}
local activeTab

for i, tabName in ipairs(tabs) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, 0, 0, 35)
    tabBtn.Position = UDim2.new(0, 0, 0, (i - 1) * 35)
    tabBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabBtn.Text = "  " .. tabName
    tabBtn.TextColor3 = textColor
    tabBtn.Font = Enum.Font.SourceSansBold
    tabBtn.TextSize = 14
    tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    tabBtn.Parent = sideMenu

    -- Icon
    local icon = Instance.new("ImageLabel")
    icon.Image = iconList[tabName] or ""
    icon.Size = UDim2.new(0, 20, 0, 20)
    icon.Position = UDim2.new(0, 5, 0.5, -10)
    icon.BackgroundTransparency = 1
    icon.Parent = tabBtn

    -- Tab Content Frame
    local tabFrame = Instance.new("Frame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -30)
    tabFrame.Position = UDim2.new(0, 130, 0, 30)
    tabFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab and activeTab ~= tabFrame then
            local fadeOut = TweenService:Create(activeTab, TweenInfo.new(0.2), {BackgroundTransparency = 1})
            fadeOut:Play()
            fadeOut.Completed:Wait()
            activeTab.Visible = false
            activeTab.BackgroundTransparency = 0
        end

        tabFrame.Visible = true
        tabFrame.BackgroundTransparency = 1
        local fadeIn = TweenService:Create(tabFrame, TweenInfo.new(0.2), {BackgroundTransparency = 0})
        fadeIn:Play()
        activeTab = tabFrame
    end)
end

-- Floating Button
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

-- Close / Minimize
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

-- Ativar a aba "Character" como padrão
wait()
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]
