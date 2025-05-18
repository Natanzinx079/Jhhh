-- NatanHub com visual aprimorado (sem alterar funcionalidades)
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

-- Cores
local darkColor = Color3.fromRGB(20, 20, 20)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)

-- Ícones das abas
local tabIcons = {
    Main = "🏠",
    Character = "🧍",
    Teleport = "🗺️",
    Visual = "⭐",
    Combat = "⚔️",
    Configuration = "⚙️"
}

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 500, 0, 300)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
mainFrame.BackgroundColor3 = darkColor
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = gui

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

-- Abas
local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local tabFrames = {}
local activeTab

for i, tabName in ipairs(tabs) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, 0, 0, 35)
    tabBtn.Position = UDim2.new(0, 0, 0, (i - 1) * 35)
    tabBtn.Text = tabIcons[tabName] .. "  " .. tabName
    tabBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    tabBtn.TextColor3 = textColor
    tabBtn.Font = Enum.Font.SourceSansBold
    tabBtn.TextSize = 14
    tabBtn.TextXAlignment = Enum.TextXAlignment.Left
    tabBtn.PaddingLeft = UDim.new(0, 10)
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

-- Aba Character
local charTab = tabFrames["Character"]

local function createLabel(text, posY)
    local lbl = Instance.new("TextLabel")
    lbl.Text = text
    lbl.Position = UDim2.new(0, 10, 0, posY)
    lbl.Size = UDim2.new(0, 200, 0, 25)
    lbl.BackgroundTransparency = 1
    lbl.TextColor3 = textColor
    lbl.Font = Enum.Font.SourceSans
    lbl.TextSize = 14
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = charTab
    return lbl
end

local function createInputBox(default, posY, callback)
    local box = Instance.new("TextBox")
    box.Text = tostring(default)
    box.Size = UDim2.new(0, 100, 0, 30)
    box.Position = UDim2.new(0, 10, 0, posY)
    box.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    box.TextColor3 = textColor
    box.Font = Enum.Font.SourceSans
    box.TextSize = 14
    box.ClearTextOnFocus = false
    box.Parent = charTab
    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then pcall(function() callback(val) end) end
    end)
    return box
end

createLabel("Walk Speed", 20)
createInputBox(LocalPlayer.Character.Humanoid.WalkSpeed, 50, function(val)
    LocalPlayer.Character.Humanoid.WalkSpeed = val
end)

createLabel("Jump Power", 90)
createInputBox(LocalPlayer.Character.Humanoid.JumpPower, 120, function(val)
    LocalPlayer.Character.Humanoid.JumpPower = val
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

-- Botões de minimizar e fechar
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
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]
