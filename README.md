-- NatHub Remake 0.3.1 | Compatível com Delta Mobile
local CoreGui = game:GetService("CoreGui")
local Player = game.Players.LocalPlayer
local Char = Player.Character or Player.CharacterAdded:Wait()

-- Criar ScreenGui
local gui = Instance.new("ScreenGui")
gui.Name = "NatHubUI"
gui.ResetOnSpawn = false
gui.Parent = CoreGui

-- Janela principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 450, 0, 300)
mainFrame.Position = UDim2.new(0.5, -225, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui

-- Título
local titleBar = Instance.new("TextLabel")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
titleBar.Text = "  NatHub | Dead Rails (0.3.1)"
titleBar.TextColor3 = Color3.fromRGB(255, 255, 255)
titleBar.TextXAlignment = Enum.TextXAlignment.Left
titleBar.Font = Enum.Font.GothamBold
titleBar.TextSize = 14
titleBar.Parent = mainFrame

-- Botões de controle (fechar e ocultar)
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.Text = "X"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.BackgroundTransparency = 1
closeBtn.Parent = mainFrame

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -60, 0, 0)
minimizeBtn.Text = "-"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
minimizeBtn.BackgroundTransparency = 1
minimizeBtn.Parent = mainFrame

-- Menu lateral
local sideMenu = Instance.new("Frame")
sideMenu.Size = UDim2.new(0, 120, 1, -30)
sideMenu.Position = UDim2.new(0, 0, 0, 30)
sideMenu.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
sideMenu.Parent = mainFrame

-- Conteúdo da aba
local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1, -120, 1, -30)
contentFrame.Position = UDim2.new(0, 120, 0, 30)
contentFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
contentFrame.Parent = mainFrame

-- Função para criar botão de aba
local currentTab
local function createTab(name, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 30)
    button.Text = name
    button.Font = Enum.Font.Gotham
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundTransparency = 1
    button.Parent = sideMenu
    button.MouseButton1Click:Connect(function()
        if currentTab then currentTab:Destroy() end
        currentTab = Instance.new("Frame", contentFrame)
        currentTab.Size = UDim2.new(1, 0, 1, 0)
        currentTab.BackgroundTransparency = 1
        callback(currentTab)
    end)
end

-- Criação das abas e funções
createTab("Character", function(tab)
    local speedLabel = Instance.new("TextLabel", tab)
    speedLabel.Size = UDim2.new(1, -20, 0, 30)
    speedLabel.Position = UDim2.new(0, 10, 0, 10)
    speedLabel.Text = "Walk Speed: " .. Char.Humanoid.WalkSpeed
    speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    speedLabel.BackgroundTransparency = 1
    speedLabel.TextXAlignment = Enum.TextXAlignment.Left

    local jumpLabel = Instance.new("TextLabel", tab)
    jumpLabel.Size = UDim2.new(1, -20, 0, 30)
    jumpLabel.Position = UDim2.new(0, 10, 0, 50)
    jumpLabel.Text = "Jump Power: " .. Char.Humanoid.JumpPower
    jumpLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    jumpLabel.BackgroundTransparency = 1
    jumpLabel.TextXAlignment = Enum.TextXAlignment.Left
end)

createTab("Main", function(tab)
    local label = Instance.new("TextLabel", tab)
    label.Text = "Main Tab – em desenvolvimento..."
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.BackgroundTransparency = 1
    label.Position = UDim2.new(0, 10, 0, 10)
    label.Size = UDim2.new(1, -20, 0, 30)
end)

-- Fechar a UI
closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Ocultar/mostrar conteúdo
local isMinimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    sideMenu.Visible = not isMinimized
    contentFrame.Visible = not isMinimized
end)
