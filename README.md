-- NatHub | Dead Rails Visual Redesign (Mantendo Funcionalidade)

-- // Serviços
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- // GUI Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatHub"
ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- // Janela Principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 600, 0, 400)
MainFrame.Position = UDim2.new(0.5, -300, 0.5, -200)
MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

-- // Arredondamento e Sombra
local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 12)

local Shadow = Instance.new("ImageLabel", MainFrame)
Shadow.BackgroundTransparency = 1
Shadow.Size = UDim2.new(1, 60, 1, 60)
Shadow.Position = UDim2.new(0, -30, 0, -30)
Shadow.Image = "rbxassetid://1316045217"
Shadow.ImageTransparency = 0.7
Shadow.ZIndex = 0

-- // Título
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 50)
Title.BackgroundTransparency = 1
Title.Text = "NatHub | Dead Rails"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 24
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Position = UDim2.new(0, 20, 0, 0)

-- // Abas com Ícones
local TabContainer = Instance.new("Frame")
TabContainer.Name = "TabContainer"
TabContainer.Parent = MainFrame
TabContainer.BackgroundTransparency = 1
TabContainer.Position = UDim2.new(0, 20, 0, 60)
TabContainer.Size = UDim2.new(0, 560, 0, 40)

local UIListLayoutTabs = Instance.new("UIListLayout")
UIListLayoutTabs.FillDirection = Enum.FillDirection.Horizontal
UIListLayoutTabs.HorizontalAlignment = Enum.HorizontalAlignment.Left
UIListLayoutTabs.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayoutTabs.Padding = UDim.new(0, 10)
UIListLayoutTabs.Parent = TabContainer

-- Função para criar abas com ícone e animação
local Tabs = {}
local function CreateTab(name, iconId)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0, 120, 1, 0)
    Button.Text = ""
    Button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.AutoButtonColor = false
    Button.Parent = TabContainer

    local Icon = Instance.new("ImageLabel")
    Icon.Parent = Button
    Icon.Image = iconId
    Icon.BackgroundTransparency = 1
    Icon.Size = UDim2.new(0, 20, 0, 20)
    Icon.Position = UDim2.new(0, 10, 0.5, -10)

    local Label = Instance.new("TextLabel")
    Label.Parent = Button
    Label.Text = name
    Label.BackgroundTransparency = 1
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.TextSize = 14
    Label.Font = Enum.Font.Gotham
    Label.Position = UDim2.new(0, 35, 0, 0)
    Label.Size = UDim2.new(1, -35, 1, 0)

    local UICorner = Instance.new("UICorner", Button)
    UICorner.CornerRadius = UDim.new(0, 8)

    Tabs[name] = Button
    return Button
end

-- // Área de Conteúdo
local ContentFrame = Instance.new("Frame")
ContentFrame.Name = "ContentFrame"
ContentFrame.Size = UDim2.new(1, -40, 1, -120)
ContentFrame.Position = UDim2.new(0, 20, 0, 110)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

-- // Conteúdos das abas
local Pages = {}
local function CreatePage(name)
    local Page = Instance.new("Frame")
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.Visible = false
    Page.BackgroundTransparency = 1
    Page.Parent = ContentFrame
    Pages[name] = Page
    return Page
end

-- // Animação ao trocar de aba
local function ShowPage(name)
    for pageName, page in pairs(Pages) do
        if pageName == name then
            page.Visible = true
            TweenService:Create(page, TweenInfo.new(0.3), {BackgroundTransparency = 0}):Play()
        else
            page.Visible = false
        end
    end
end

-- // Exemplo: Criar duas abas (só visuais)
local MainTab = CreateTab("Principal", "rbxassetid://6031094677")
local SettingsTab = CreateTab("Configurações", "rbxassetid://6031280882")

local MainPage = CreatePage("Principal")
local SettingsPage = CreatePage("Configurações")

MainTab.MouseButton1Click:Connect(function()
    ShowPage("Principal")
end)

SettingsTab.MouseButton1Click:Connect(function()
    ShowPage("Configurações")
end)

ShowPage("Principal")

-- // Você pode adicionar seus botões, sliders e outras funcionalidades em 'MainPage' e 'SettingsPage'
-- // Exemplo de botão:
local TestButton = Instance.new("TextButton")
TestButton.Text = "Executar Função"
TestButton.Size = UDim2.new(0, 200, 0, 40)
TestButton.Position = UDim2.new(0, 20, 0, 20)
TestButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
TestButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TestButton.Font = Enum.Font.Gotham
TestButton.TextSize = 14
TestButton.Parent = MainPage

local UICornerBtn = Instance.new("UICorner", TestButton)
UICornerBtn.CornerRadius = UDim.new(0, 6)

TestButton.MouseButton1Click:Connect(function()
    print("Função executada!")
end)
