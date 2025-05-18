-- GUI principal
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "NatHub"

-- Janela principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 500, 0, 300)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui
MainFrame.Visible = true

-- Cantos arredondados
local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 6)

-- Botão de minimizar
local Minimize = Instance.new("TextButton")
Minimize.Size = UDim2.new(0, 20, 0, 20)
Minimize.Position = UDim2.new(1, -45, 0, 5)
Minimize.Text = "-"
Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
Minimize.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Minimize.BorderSizePixel = 0
Minimize.Parent = MainFrame

-- Botão de fechar
local Close = Instance.new("TextButton")
Close.Size = UDim2.new(0, 20, 0, 20)
Close.Position = UDim2.new(1, -20, 0, 5)
Close.Text = "X"
Close.TextColor3 = Color3.fromRGB(255, 70, 70)
Close.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Close.BorderSizePixel = 0
Close.Parent = MainFrame

-- Menu lateral
local SideMenu = Instance.new("Frame")
SideMenu.Size = UDim2.new(0, 130, 1, 0)
SideMenu.Position = UDim2.new(0, 0, 0, 0)
SideMenu.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
SideMenu.BorderSizePixel = 0
SideMenu.Parent = MainFrame

-- Abas laterais
local Tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local Buttons = {}

for i, name in ipairs(Tabs) do
	local TabButton = Instance.new("TextButton")
	TabButton.Size = UDim2.new(1, 0, 0, 35)
	TabButton.Position = UDim2.new(0, 0, 0, (i - 1) * 36 + 10)
	TabButton.Text = name
	TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
	TabButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	TabButton.BorderSizePixel = 0
	TabButton.Font = Enum.Font.GothamSemibold
	TabButton.TextSize = 14
	TabButton.Parent = SideMenu
	Buttons[name] = TabButton
end

-- Área de conteúdo
local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -130, 1, -30)
Content.Position = UDim2.new(0, 130, 0, 30)
Content.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Content.BorderSizePixel = 0
Content.Parent = MainFrame

-- Aba Character com sliders
local function CreateSlider(name, min, max, default, yPos, onChange)
	local label = Instance.new("TextLabel", Content)
	label.Position = UDim2.new(0, 20, 0, yPos)
	label.Size = UDim2.new(0, 150, 0, 20)
	label.Text = name
	label.TextColor3 = Color3.fromRGB(255, 255, 255)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.Gotham
	label.TextSize = 14
	
	local slider = Instance.new("TextButton", Content)
	slider.Position = UDim2.new(0, 180, 0, yPos)
	slider.Size = UDim2.new(0, 200, 0, 20)
	slider.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
	slider.Text = tostring(default)
	slider.TextColor3 = Color3.fromRGB(255, 255, 255)
	slider.Font = Enum.Font.Gotham
	slider.TextSize = 12
	
	slider.MouseButton1Click:Connect(function()
		local new = tonumber(game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"):WaitForChild("NatHub"):WaitForChild("Input"):GetText())
		if new and new >= min and new <= max then
			slider.Text = tostring(new)
			onChange(new)
		end
	end)
end

-- Character Sliders
CreateSlider("Walk Speed", 0, 200, 16, 10, function(value)
	game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)

CreateSlider("Jump Power", 0, 200, 50, 40, function(value)
	game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
end)

-- Ocultar Menu
Minimize.MouseButton1Click:Connect(function()
	Content.Visible = not Content.Visible
	SideMenu.Visible = not SideMenu.Visible
end)

-- Fechar GUI
Close.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)
