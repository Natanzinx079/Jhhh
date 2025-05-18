-- Natan Dead Reails Hub para Dead Rails (Delta Android)
-- Desenvolvido para uso educativo

-- Serviços
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

-- GUI
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatanHub"
ScreenGui.ResetOnSpawn = false

-- Janela Principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Name = "MainFrame"
MainFrame.Position = UDim2.new(0.3, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 200)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

-- Título
local Title = Instance.new("TextLabel", MainFrame)
Title.Text = "NatanHub | Dead Reails"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 16

-- Botões de Controle
local CloseButton = Instance.new("TextButton", MainFrame)
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 16

local MinimizeButton = Instance.new("TextButton", MainFrame)
MinimizeButton.Text = "-"
MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
MinimizeButton.Position = UDim2.new(1, -60, 0, 0)
MinimizeButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
MinimizeButton.TextColor3 = Color3.new(1, 1, 1)
MinimizeButton.Font = Enum.Font.SourceSansBold
MinimizeButton.TextSize = 16

-- Área Interna
local ContentFrame = Instance.new("Frame", MainFrame)
ContentFrame.Size = UDim2.new(1, 0, 1, -30)
ContentFrame.Position = UDim2.new(0, 0, 0, 30)
ContentFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)

-- Sliders (Simples usando TextBox)
local SpeedLabel = Instance.new("TextLabel", ContentFrame)
SpeedLabel.Text = "Walk Speed"
SpeedLabel.Size = UDim2.new(1, -20, 0, 20)
SpeedLabel.Position = UDim2.new(0, 10, 0, 10)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.TextColor3 = Color3.new(1, 1, 1)
SpeedLabel.TextSize = 14
SpeedLabel.Font = Enum.Font.SourceSans

local SpeedBox = Instance.new("TextBox", ContentFrame)
SpeedBox.Text = "16"
SpeedBox.Size = UDim2.new(1, -20, 0, 25)
SpeedBox.Position = UDim2.new(0, 10, 0, 30)
SpeedBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SpeedBox.TextColor3 = Color3.new(1, 1, 1)
SpeedBox.Font = Enum.Font.SourceSans
SpeedBox.TextSize = 14
SpeedBox.ClearTextOnFocus = false

local JumpLabel = Instance.new("TextLabel", ContentFrame)
JumpLabel.Text = "Jump Power"
JumpLabel.Size = UDim2.new(1, -20, 0, 20)
JumpLabel.Position = UDim2.new(0, 10, 0, 60)
JumpLabel.BackgroundTransparency = 1
JumpLabel.TextColor3 = Color3.new(1, 1, 1)
JumpLabel.TextSize = 14
JumpLabel.Font = Enum.Font.SourceSans

local JumpBox = Instance.new("TextBox", ContentFrame)
JumpBox.Text = "50"
JumpBox.Size = UDim2.new(1, -20, 0, 25)
JumpBox.Position = UDim2.new(0, 10, 0, 80)
JumpBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
JumpBox.TextColor3 = Color3.new(1, 1, 1)
JumpBox.Font = Enum.Font.SourceSans
JumpBox.TextSize = 14
JumpBox.ClearTextOnFocus = false

-- Funções
SpeedBox.FocusLost:Connect(function()
	local speed = tonumber(SpeedBox.Text)
	if speed then
		Humanoid.WalkSpeed = speed
	end
end)

JumpBox.FocusLost:Connect(function()
	local power = tonumber(JumpBox.Text)
	if power then
		Humanoid.JumpPower = power
	end
end)

CloseButton.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

MinimizeButton.MouseButton1Click:Connect(function()
	ContentFrame.Visible = not ContentFrame.Visible
end)
