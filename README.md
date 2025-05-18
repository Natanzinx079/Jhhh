local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatanHub"
ScreenGui.Parent = game.CoreGui

-- Janela Principal
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Parent = ScreenGui
Main.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Main.BorderSizePixel = 0
Main.Position = UDim2.new(0.5, -150, 0.5, -100)
Main.Size = UDim2.new(0, 300, 0, 200)
Main.Active = true
Main.Draggable = true

-- Título
local Title = Instance.new("TextLabel")
Title.Parent = Main
Title.Text = "NatanHub | Dead Reails"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 16
Title.Name = "Title"

-- Botão Minimizar
local Minimize = Instance.new("TextButton")
Minimize.Parent = Main
Minimize.Text = "-"
Minimize.Size = UDim2.new(0, 30, 0, 30)
Minimize.Position = UDim2.new(1, -60, 0, 0)
Minimize.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
Minimize.Font = Enum.Font.SourceSansBold
Minimize.TextSize = 18
Minimize.Name = "Minimize"

-- Botão Fechar
local Close = Instance.new("TextButton")
Close.Parent = Main
Close.Text = "X"
Close.Size = UDim2.new(0, 30, 0, 30)
Close.Position = UDim2.new(1, -30, 0, 0)
Close.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Close.TextColor3 = Color3.fromRGB(255, 255, 255)
Close.Font = Enum.Font.SourceSansBold
Close.TextSize = 18
Close.Name = "Close"

-- Seção: WalkSpeed
local WalkSpeedLabel = Instance.new("TextLabel")
WalkSpeedLabel.Parent = Main
WalkSpeedLabel.Text = "Walk Speed"
WalkSpeedLabel.Size = UDim2.new(1, -20, 0, 20)
WalkSpeedLabel.Position = UDim2.new(0, 10, 0, 50)
WalkSpeedLabel.BackgroundTransparency = 1
WalkSpeedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
WalkSpeedLabel.Font = Enum.Font.SourceSans
WalkSpeedLabel.TextSize = 14

local WalkSpeedBox = Instance.new("TextBox")
WalkSpeedBox.Parent = Main
WalkSpeedBox.Text = "50"
WalkSpeedBox.Size = UDim2.new(1, -20, 0, 30)
WalkSpeedBox.Position = UDim2.new(0, 10, 0, 70)
WalkSpeedBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
WalkSpeedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
WalkSpeedBox.Font = Enum.Font.SourceSans
WalkSpeedBox.TextSize = 14

-- Seção: JumpPower
local JumpPowerLabel = Instance.new("TextLabel")
JumpPowerLabel.Parent = Main
JumpPowerLabel.Text = "Jump Power"
JumpPowerLabel.Size = UDim2.new(1, -20, 0, 20)
JumpPowerLabel.Position = UDim2.new(0, 10, 0, 110)
JumpPowerLabel.BackgroundTransparency = 1
JumpPowerLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
JumpPowerLabel.Font = Enum.Font.SourceSans
JumpPowerLabel.TextSize = 14

local JumpPowerBox = Instance.new("TextBox")
JumpPowerBox.Parent = Main
JumpPowerBox.Text = "50"
JumpPowerBox.Size = UDim2.new(1, -20, 0, 30)
JumpPowerBox.Position = UDim2.new(0, 10, 0, 130)
JumpPowerBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
JumpPowerBox.TextColor3 = Color3.fromRGB(255, 255, 255)
JumpPowerBox.Font = Enum.Font.SourceSans
JumpPowerBox.TextSize = 14

-- Botão Flutuante (só aparece ao minimizar)
local FloatBtn = Instance.new("TextButton")
FloatBtn.Parent = ScreenGui
FloatBtn.Text = "NatanHub"
FloatBtn.Size = UDim2.new(0, 100, 0, 30)
FloatBtn.Position = UDim2.new(0, 10, 0, 10)
FloatBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FloatBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
FloatBtn.Font = Enum.Font.SourceSansBold
FloatBtn.TextSize = 14
FloatBtn.Visible = false
FloatBtn.Active = true
FloatBtn.Draggable = true

-- Ações dos botões
Minimize.MouseButton1Click:Connect(function()
	Main.Visible = false
	FloatBtn.Visible = true
end)

FloatBtn.MouseButton1Click:Connect(function()
	Main.Visible = true
	FloatBtn.Visible = false
end)

Close.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)
