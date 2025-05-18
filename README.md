-- // GUI Principal
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "NatHub"

-- // Janela
local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 500, 0, 300)
mainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true

-- // TopBar
local topBar = Instance.new("Frame", mainFrame)
topBar.Size = UDim2.new(1, 0, 0, 25)
topBar.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
topBar.BorderSizePixel = 0

-- // Botão Minimizar
local minimizeBtn = Instance.new("TextButton", topBar)
minimizeBtn.Size = UDim2.new(0, 25, 0, 25)
minimizeBtn.Position = UDim2.new(1, -50, 0, 0)
minimizeBtn.Text = "-"
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
minimizeBtn.BorderSizePixel = 0

-- // Botão Fechar
local closeBtn = Instance.new("TextButton", topBar)
closeBtn.Size = UDim2.new(0, 25, 0, 25)
closeBtn.Position = UDim2.new(1, -25, 0, 0)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
closeBtn.BorderSizePixel = 0

-- // Menu Lateral
local sideBar = Instance.new("Frame", mainFrame)
sideBar.Size = UDim2.new(0, 130, 1, -25)
sideBar.Position = UDim2.new(0, 0, 0, 25)
sideBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
sideBar.BorderSizePixel = 0

-- // Conteúdo
local contentFrame = Instance.new("Frame", mainFrame)
contentFrame.Size = UDim2.new(1, -130, 1, -25)
contentFrame.Position = UDim2.new(0, 130, 0, 25)
contentFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
contentFrame.BorderSizePixel = 0

-- // Função para criar abas
local tabs = {}
local function createTab(name)
	local button = Instance.new("TextButton", sideBar)
	button.Size = UDim2.new(1, 0, 0, 35)
	button.Text = name
	button.TextColor3 = Color3.fromRGB(255, 255, 255)
	button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
	button.BorderSizePixel = 0

	local page = Instance.new("Frame", contentFrame)
	page.Size = UDim2.new(1, 0, 1, 0)
	page.Visible = false
	page.BackgroundTransparency = 1

	button.MouseButton1Click:Connect(function()
		for _, tab in pairs(tabs) do
			tab.page.Visible = false
			tab.button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
		end
		page.Visible = true
		button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
	end)

	table.insert(tabs, {button = button, page = page})
	return page
end

-- // Criar páginas
local mainPage = createTab("Main")
local characterPage = createTab("Character")
createTab("Teleport")
createTab("Visual")
createTab("Combat")
createTab("Configuration")

-- // Ativar primeira aba por padrão
tabs[1].button:FireMouseButton1Click()

-- // Funções para sliders (Character)
local function createSlider(parent, labelText, minVal, maxVal, default, callback)
	local label = Instance.new("TextLabel", parent)
	label.Size = UDim2.new(1, -20, 0, 25)
	label.Position = UDim2.new(0, 10, 0, (#parent:GetChildren() - 1) * 35)
	label.Text = labelText
	label.TextColor3 = Color3.fromRGB(255, 255, 255)
	label.BackgroundTransparency = 1
	label.TextXAlignment = Enum.TextXAlignment.Left

	local box = Instance.new("TextBox", parent)
	box.Size = UDim2.new(0, 60, 0, 25)
	box.Position = UDim2.new(1, -70, 0, (#parent:GetChildren() - 1) * 35)
	box.Text = tostring(default)
	box.TextColor3 = Color3.fromRGB(255, 255, 255)
	box.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	box.BorderSizePixel = 0

	box.FocusLost:Connect(function()
		local num = tonumber(box.Text)
		if num then
			num = math.clamp(num, minVal, maxVal)
			callback(num)
		end
	end)
end

-- // Sliders funcionais
createSlider(characterPage, "Walk Speed", 1, 200, 16, function(val)
	game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
end)

createSlider(characterPage, "Jump Power", 1, 200, 50, function(val)
	game.Players.LocalPlayer.Character.Humanoid.JumpPower = val
end)

-- // Botão Fechar
closeBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
end)

-- // Botão Minimizar
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	contentFrame.Visible = not minimized
	sideBar.Visible = not minimized
end)
