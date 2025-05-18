-- NatHub 0.3.1 (Estilo moderno) - Compatível com Android
-- Desenvolvido para o jogo Dead Rails

local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TopBar = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseBtn = Instance.new("TextButton")
local MinBtn = Instance.new("TextButton")
local TabButtons = Instance.new("Frame")
local Tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local TabFrames = {}
local ToggleGui = Instance.new("TextButton")

-- Gui settings
ScreenGui.Name = "NatHub"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Toggle button
ToggleGui.Name = "ToggleGui"
ToggleGui.Parent = ScreenGui
ToggleGui.Size = UDim2.new(0, 100, 0, 40)
ToggleGui.Position = UDim2.new(0, 20, 1, -60)
ToggleGui.Text = "NatHub"
ToggleGui.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ToggleGui.TextColor3 = Color3.new(1, 1, 1)
ToggleGui.BorderSizePixel = 0
ToggleGui.AutoButtonColor = true
ToggleGui.Visible = true

-- Main UI frame
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0, 500, 0, 300)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Visible = true

-- Top Bar
TopBar.Name = "TopBar"
TopBar.Parent = MainFrame
TopBar.Size = UDim2.new(1, 0, 0, 30)
TopBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
TopBar.BorderSizePixel = 0

-- Title
Title.Name = "Title"
Title.Parent = TopBar
Title.Size = UDim2.new(1, -60, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Font = Enum.Font.GothamBold
Title.TextSize = 14

-- Close and Minimize Buttons
CloseBtn.Name = "CloseBtn"
CloseBtn.Parent = TopBar
CloseBtn.Text = "X"
CloseBtn.Size = UDim2.new(0, 30, 1, 0)
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.BorderSizePixel = 0

MinBtn.Name = "MinBtn"
MinBtn.Parent = TopBar
MinBtn.Text = "–"
MinBtn.Size = UDim2.new(0, 30, 1, 0)
MinBtn.Position = UDim2.new(1, -60, 0, 0)
MinBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MinBtn.TextColor3 = Color3.new(1, 1, 1)
MinBtn.BorderSizePixel = 0

-- Tab Buttons
TabButtons.Name = "TabButtons"
TabButtons.Parent = MainFrame
TabButtons.Size = UDim2.new(0, 120, 1, -30)
TabButtons.Position = UDim2.new(0, 0, 0, 30)
TabButtons.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TabButtons.BorderSizePixel = 0

-- Create tab buttons
for i, tabName in ipairs(Tabs) do
    local btn = Instance.new("TextButton")
    btn.Name = tabName .. "Btn"
    btn.Parent = TabButtons
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Position = UDim2.new(0, 0, 0, (i - 1) * 40)
    btn.Text = tabName
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.BorderSizePixel = 0

    local frame = Instance.new("Frame")
    frame.Name = tabName .. "Frame"
    frame.Parent = MainFrame
    frame.Size = UDim2.new(1, -120, 1, -30)
    frame.Position = UDim2.new(0, 120, 0, 30)
    frame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    frame.Visible = false
    TabFrames[tabName] = frame

    btn.MouseButton1Click:Connect(function()
        for _, fr in pairs(TabFrames) do
            fr.Visible = false
        end
        frame.Visible = true
    end)
end

-- Funções simples no tab "Character"
local wsLabel = Instance.new("TextLabel", TabFrames["Character"])
wsLabel.Text = "Walk Speed"
wsLabel.Size = UDim2.new(0, 100, 0, 20)
wsLabel.Position = UDim2.new(0, 10, 0, 10)
wsLabel.BackgroundTransparency = 1
wsLabel.TextColor3 = Color3.new(1, 1, 1)

local wsSlider = Instance.new("TextBox", TabFrames["Character"])
wsSlider.Size = UDim2.new(0, 60, 0, 25)
wsSlider.Position = UDim2.new(0, 120, 0, 10)
wsSlider.Text = "16"
wsSlider.TextColor3 = Color3.new(1, 1, 1)
wsSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

local jpLabel = Instance.new("TextLabel", TabFrames["Character"])
jpLabel.Text = "Jump Power"
jpLabel.Size = UDim2.new(0, 100, 0, 20)
jpLabel.Position = UDim2.new(0, 10, 0, 40)
jpLabel.BackgroundTransparency = 1
jpLabel.TextColor3 = Color3.new(1, 1, 1)

local jpSlider = Instance.new("TextBox", TabFrames["Character"])
jpSlider.Size = UDim2.new(0, 60, 0, 25)
jpSlider.Position = UDim2.new(0, 120, 0, 40)
jpSlider.Text = "50"
jpSlider.TextColor3 = Color3.new(1, 1, 1)
jpSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

wsSlider.FocusLost:Connect(function()
    local num = tonumber(wsSlider.Text)
    if num then
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = num
    end
end)

jpSlider.FocusLost:Connect(function()
    local num = tonumber(jpSlider.Text)
    if num then
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = num
    end
end)

-- Close and Minimize logic
CloseBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

MinBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    ToggleGui.Visible = true
end)

ToggleGui.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    ToggleGui.Visible = false
end)

-- Ativar aba Main por padrão
TabFrames["Main"].Visible = true
