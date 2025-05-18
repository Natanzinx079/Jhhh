--// Criar a UI principal
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Sidebar = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local CloseBtn = Instance.new("TextButton")
local MinimizeBtn = Instance.new("TextButton")
local Tabs = {}
local Contents = {}
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

ScreenGui.Name = "NatHub_Style"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

MainFrame.Size = UDim2.new(0, 500, 0, 300)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -150)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

Sidebar.Size = UDim2.new(0, 130, 1, 0)
Sidebar.Position = UDim2.new(0, 0, 0, 0)
Sidebar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Sidebar.Parent = MainFrame

Title.Text = "NatHub"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20
Title.Parent = Sidebar

CloseBtn.Text = "X"
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.Font = Enum.Font.SourceSans
CloseBtn.TextSize = 18
CloseBtn.Parent = MainFrame

MinimizeBtn.Text = "-"
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -60, 0, 0)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
MinimizeBtn.TextColor3 = Color3.new(1, 1, 1)
MinimizeBtn.Font = Enum.Font.SourceSans
MinimizeBtn.TextSize = 18
MinimizeBtn.Parent = MainFrame

-- Função para criar abas
local function createTab(name)
    local button = Instance.new("TextButton")
    button.Text = name
    button.Size = UDim2.new(1, 0, 0, 30)
    button.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 18
    button.Parent = Sidebar

    local content = Instance.new("Frame")
    content.Size = UDim2.new(1, -130, 1, -30)
    content.Position = UDim2.new(0, 130, 0, 30)
    content.BackgroundTransparency = 1
    content.Visible = false
    content.Parent = MainFrame

    table.insert(Tabs, button)
    table.insert(Contents, content)

    button.MouseButton1Click:Connect(function()
        for i = 1, #Contents do
            Contents[i].Visible = false
        end
        content.Visible = true
    end)

    return content
end

-- Cria a aba "Main"
local mainContent = createTab("Main")

local function createSlider(parent, labelText, minVal, maxVal, defaultVal, callback)
    local label = Instance.new("TextLabel")
    label.Text = labelText
    label.Size = UDim2.new(0, 100, 0, 30)
    label.Position = UDim2.new(0, 10, 0, (#parent:GetChildren() * 35))
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1, 1, 1)
    label.Font = Enum.Font.SourceSans
    label.TextSize = 18
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = parent

    local textbox = Instance.new("TextBox")
    textbox.Text = tostring(defaultVal)
    textbox.Size = UDim2.new(0, 100, 0, 30)
    textbox.Position = UDim2.new(0, 120, 0, (#parent:GetChildren() - 1) * 35)
    textbox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    textbox.TextColor3 = Color3.new(1, 1, 1)
    textbox.Font = Enum.Font.SourceSans
    textbox.TextSize = 18
    textbox.ClearTextOnFocus = false
    textbox.Parent = parent

    textbox.FocusLost:Connect(function()
        local value = tonumber(textbox.Text)
        if value then
            value = math.clamp(value, minVal, maxVal)
            textbox.Text = tostring(value)
            callback(value)
        else
            textbox.Text = tostring(defaultVal)
        end
    end)
end

createSlider(mainContent, "Walk Speed", 1, 200, 16, function(val)
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = val
    end
end)

createSlider(mainContent, "Jump Power", 1, 300, 50, function(val)
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.JumpPower = val
    end
end)

-- Inicializa primeira aba como visível
Contents[1].Visible = true

-- Botões fechar e minimizar
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

MinimizeBtn.MouseButton1Click:Connect(function()
    for i, content in pairs(Contents) do
        content.Visible = false
    end
    Sidebar.Visible = not Sidebar.Visible
end)
