local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

-- GUI Principal
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatanHub"

-- Função para criar botões laterais
local function CreateSideButton(name, parent, posY)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, 0, 0, 30)
	btn.Position = UDim2.new(0, 0, 0, posY)
	btn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
	btn.Text = name
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 14
	btn.Parent = parent
	return btn
end

-- Janela Principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Name = "MainFrame"
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Size = UDim2.new(0, 400, 0, 250)
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -125)
MainFrame.Active = true
MainFrame.Draggable = true

-- Barra lateral
local Sidebar = Instance.new("Frame", MainFrame)
Sidebar.Size = UDim2.new(0, 100, 1, 0)
Sidebar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)

-- Área de conteúdo
local Content = Instance.new("Frame", MainFrame)
Content.Size = UDim2.new(1, -100, 1, 0)
Content.Position = UDim2.new(0, 100, 0, 0)
Content.BackgroundColor3 = Color3.fromRGB(35, 35, 35)

-- Botão Minimizar
local Minimize = Instance.new("TextButton", MainFrame)
Minimize.Text = "-"
Minimize.Size = UDim2.new(0, 30, 0, 30)
Minimize.Position = UDim2.new(1, -60, 0, 0)
Minimize.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
Minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
Minimize.Font = Enum.Font.SourceSansBold
Minimize.TextSize = 18

-- Botão Fechar
local Close = Instance.new("TextButton", MainFrame)
Close.Text = "X"
Close.Size = UDim2.new(0, 30, 0, 30)
Close.Position = UDim2.new(1, -30, 0, 0)
Close.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Close.TextColor3 = Color3.fromRGB(255, 255, 255)
Close.Font = Enum.Font.SourceSansBold
Close.TextSize = 18

-- Botão Flutuante
local FloatBtn = Instance.new("TextButton", ScreenGui)
FloatBtn.Text = "NatHub"
FloatBtn.Size = UDim2.new(0, 100, 0, 30)
FloatBtn.Position = UDim2.new(0, 10, 0, 10)
FloatBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FloatBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
FloatBtn.Font = Enum.Font.SourceSansBold
FloatBtn.TextSize = 14
FloatBtn.Visible = false
FloatBtn.Draggable = true

-- Página Character
local CharacterPage = Instance.new("Frame", Content)
CharacterPage.Size = UDim2.new(1, 0, 1, 0)
CharacterPage.Visible = false
CharacterPage.BackgroundTransparency = 1

local function CreateSlider(name, parent, min, max, default, posY, callback)
	local label = Instance.new("TextLabel", parent)
	label.Text = name
	label.Position = UDim2.new(0, 10, 0, posY)
	label.Size = UDim2.new(0, 200, 0, 20)
	label.TextColor3 = Color3.fromRGB(255, 255, 255)
	label.BackgroundTransparency = 1
	label.TextSize = 14
	label.Font = Enum.Font.SourceSans

	local slider = Instance.new("TextBox", parent)
	slider.Position = UDim2.new(0, 10, 0, posY + 20)
	slider.Size = UDim2.new(0, 100, 0, 25)
	slider.Text = tostring(default)
	slider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	slider.TextColor3 = Color3.fromRGB(255, 255, 255)
	slider.Font = Enum.Font.SourceSans
	slider.TextSize = 14

	slider.FocusLost:Connect(function()
		local value = tonumber(slider.Text)
		if value then
			value = math.clamp(value, min, max)
			callback(value)
		end
	end)
end

-- WalkSpeed e JumpPower sliders
CreateSlider("Walk Speed", CharacterPage, 16, 500, 16, 10, function(val)
	pcall(function()
		LocalPlayer.Character.Humanoid.WalkSpeed = val
	end)
end)

CreateSlider("Jump Power", CharacterPage, 50, 500, 50, 60, function(val)
	pcall(function()
		LocalPlayer.Character.Humanoid.JumpPower = val
	end)
end)

-- Botões laterais
local pages = {
	Main = Instance.new("Frame", Content),
	Character = CharacterPage,
	Teleport = Instance.new("Frame", Content),
	Visual = Instance.new("Frame", Content),
	Combat = Instance.new("Frame", Content),
	Configuration = Instance.new("Frame", Content)
}

for _, frame in pairs(pages) do
	frame.Size = UDim2.new(1, 0, 1, 0)
	frame.Visible = false
	frame.BackgroundTransparency = 1
end

local y = 0
for name, page in pairs(pages) do
	local btn = CreateSideButton(name, Sidebar, y)
	y = y + 35

	btn.MouseButton1Click:Connect(function()
		for _, f in pairs(pages) do
			f.Visible = false
		end
		page.Visible = true
	end)
end

pages.Main.Visible = true -- Mostrar Main como padrão

-- Eventos dos botões
Minimize.MouseButton1Click:Connect(function()
	MainFrame.Visible = false
	FloatBtn.Visible = true
end)

FloatBtn.MouseButton1Click:Connect(function()
	MainFrame.Visible = true
	FloatBtn.Visible = false
end)

Close.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)
