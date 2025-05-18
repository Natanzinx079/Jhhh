-- NatHub | Dead Rails (0.3.1) com abas e funções adicionais
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))

-- Criar Janela Principal
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 300, 0, 300)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = true
mainFrame.Name = "NatHub"

-- Título
local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 25)
title.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
title.Text = "NatHub | Dead Rails (0.3.1)"
title.TextColor3 = Color3.new(1, 1, 1)
title.TextScaled = true

-- Botão de fechar
local closeButton = Instance.new("TextButton", mainFrame)
closeButton.Size = UDim2.new(0, 25, 0, 25)
closeButton.Position = UDim2.new(1, -25, 0, 0)
closeButton.Text = "X"
closeButton.BackgroundColor3 = Color3.new(0.2, 0, 0)
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Botão de ocultar
local minimizeButton = Instance.new("TextButton", mainFrame)
minimizeButton.Size = UDim2.new(0, 25, 0, 25)
minimizeButton.Position = UDim2.new(1, -50, 0, 0)
minimizeButton.Text = "-"
minimizeButton.BackgroundColor3 = Color3.new(0.2, 0.2, 0)
minimizeButton.TextColor3 = Color3.new(1, 1, 1)

local contentVisible = true

minimizeButton.MouseButton1Click:Connect(function()
    contentVisible = not contentVisible
    for _, child in pairs(mainFrame:GetChildren()) do
        if child ~= title and child ~= closeButton and child ~= minimizeButton then
            child.Visible = contentVisible
        end
    end
end)

-- Abas
local tabs = {
    "Main",
    "Character",
    "Teleport",
    "Visual",
    "Combat",
    "Configuration"
}

local selectedTab = "Main"

-- Criar Abas
local tabButtons = {}
for i, tabName in ipairs(tabs) do
    local tab = Instance.new("TextButton", mainFrame)
    tab.Size = UDim2.new(0, 100, 0, 25)
    tab.Position = UDim2.new(0, (i - 1) * 100, 0, 30)
    tab.Text = tabName
    tab.TextColor3 = Color3.new(1, 1, 1)
    tab.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    tab.MouseButton1Click:Connect(function()
        selectedTab = tabName
        for _, v in pairs(mainFrame:GetChildren()) do
            if v:IsA("Frame") and v.Name ~= "NatHub" then
                v.Visible = false
            end
        end
        if mainFrame:FindFirstChild(tabName) then
            mainFrame[tabName].Visible = true
        end
    end)
    table.insert(tabButtons, tab)
end

-- Criar Conteúdo de cada aba
local function createTabContent(name)
    local frame = Instance.new("Frame", mainFrame)
    frame.Name = name
    frame.Size = UDim2.new(1, -10, 1, -65)
    frame.Position = UDim2.new(0, 5, 0, 60)
    frame.BackgroundColor3 = Color3.new(0.15, 0.15, 0.15)
    frame.Visible = name == "Main"
    return frame
end

local mainTab = createTabContent("Main")
local charTab = createTabContent("Character")
local teleportTab = createTabContent("Teleport")
local visualTab = createTabContent("Visual")
local combatTab = createTabContent("Combat")
local configTab = createTabContent("Configuration")

-- Funções para Teleporte
local teleportBtn = Instance.new("TextButton", teleportTab)
teleportBtn.Size = UDim2.new(0.8, 0, 0, 30)
teleportBtn.Position = UDim2.new(0.1, 0, 0, 10)
teleportBtn.Text = "Teleport: Base"
teleportBtn.TextColor3 = Color3.new(1, 1, 1)
teleportBtn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.4)
teleportBtn.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    if char and char:FindFirstChild("HumanoidRootPart") then
        char:MoveTo(Vector3.new(0, 10, 0)) -- Posição exemplo da base
    end
end)

-- Placeholder para outras funções
local function addPlaceholderButton(tab, text, ypos)
    local btn = Instance.new("TextButton", tab)
    btn.Size = UDim2.new(0.8, 0, 0, 30)
    btn.Position = UDim2.new(0.1, 0, 0, ypos)
    btn.Text = text
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
    return btn
end

addPlaceholderButton(mainTab, "Auto Win (Em breve)", 10)
addPlaceholderButton(mainTab, "Coletar Ouro (Em breve)", 50)
addPlaceholderButton(charTab, "Aumentar Velocidade (Em breve)", 10)
addPlaceholderButton(combatTab, "Ataque Rápido (Em breve)", 10)
addPlaceholderButton(visualTab, "Tema Escuro (Ativo)", 10)
addPlaceholderButton(configTab, "Reiniciar GUI", 10).MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Pronto para adicionar mais funções reais!
