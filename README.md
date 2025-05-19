-- NATAN DEAD REAILS - SCRIPT PROFISSIONAL PARA DEAD RAILS (ROBLOX)
-- Compatível com Delta Executor e Android
-- Inspirado em NatHub, KiciaHook e Lunor

local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local ToggleButton = Instance.new("TextButton")
local CloseButton = Instance.new("TextButton")
local MinimizeButton = Instance.new("TextButton")

-- UI CONFIGURAÇÃO INICIAL
ScreenGui.Name = "NatanDeadReailsUI"
ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.2, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 400, 0, 350)
MainFrame.Visible = true
MainFrame.Active = true
MainFrame.Draggable = true

Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Font = Enum.Font.GothamBold
Title.Text = "NATAN DEAD REAILS 1.0"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 22

CloseButton.Name = "CloseButton"
CloseButton.Parent = MainFrame
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 18
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Parent = MainFrame
MinimizeButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
MinimizeButton.Position = UDim2.new(1, -70, 0, 5)
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.Text = "-"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeButton.TextSize = 18

local minimized = false
MinimizeButton.MouseButton1Click:Connect(function()
    minimized = not minimized
    for _, child in pairs(MainFrame:GetChildren()) do
        if child ~= Title and child ~= CloseButton and child ~= MinimizeButton then
            child.Visible = not minimized
        end
    end
end)

-- EXEMPLO DE BOTÃO: FULL BRIGHT
local FullBrightButton = Instance.new("TextButton")
FullBrightButton.Name = "FullBrightButton"
FullBrightButton.Parent = MainFrame
FullBrightButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
FullBrightButton.Position = UDim2.new(0, 20, 0, 60)
FullBrightButton.Size = UDim2.new(0, 150, 0, 40)
FullBrightButton.Font = Enum.Font.GothamBold
FullBrightButton.Text = "Full Bright"
FullBrightButton.TextColor3 = Color3.fromRGB(255, 255, 255)
FullBrightButton.TextSize = 18

local fullbrightEnabled = false
FullBrightButton.MouseButton1Click:Connect(function()
    fullbrightEnabled = not fullbrightEnabled
    if fullbrightEnabled then
        game:GetService("Lighting").Brightness = 2
        game:GetService("Lighting").Ambient = Color3.new(1, 1, 1)
        game:GetService("Lighting").OutdoorAmbient = Color3.new(1, 1, 1)
        FullBrightButton.Text = "Full Bright: ON"
    else
        game:GetService("Lighting").Brightness = 1
        game:GetService("Lighting").Ambient = Color3.new(0.5, 0.5, 0.5)
        game:GetService("Lighting").OutdoorAmbient = Color3.new(0.5, 0.5, 0.5)
        FullBrightButton.Text = "Full Bright"
    end
end)
