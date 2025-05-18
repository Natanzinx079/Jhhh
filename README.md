-- Carregar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 300, 0, 200)
Frame.Position = UDim2.new(0.5, -150, 0.5, -100)
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

-- Título
local Title = Instance.new("TextLabel", Frame)
Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.Size = UDim2.new(1, 0, 0, 25)
Title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.BorderSizePixel = 0

-- Botão X (fechar)
local CloseBtn = Instance.new("TextButton", Frame)
CloseBtn.Text = "X"
CloseBtn.Size = UDim2.new(0, 25, 0, 25)
CloseBtn.Position = UDim2.new(1, -25, 0, 0)
CloseBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Font = Enum.Font.SourceSansBold
CloseBtn.TextSize = 16
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Botão de minimizar
local MinBtn = Instance.new("TextButton", Frame)
MinBtn.Text = "-"
MinBtn.Size = UDim2.new(0, 25, 0, 25)
MinBtn.Position = UDim2.new(1, -50, 0, 0)
MinBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 0)
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Font = Enum.Font.SourceSansBold
MinBtn.TextSize = 16

-- Conteúdo (será ocultado/mostrado)
local Content = Instance.new("Frame", Frame)
Content.Size = UDim2.new(1, 0, 1, -25)
Content.Position = UDim2.new(0, 0, 0, 25)
Content.BackgroundTransparency = 1

MinBtn.MouseButton1Click:Connect(function()
    Content.Visible = not Content.Visible
end)

-- Teleporte
local function teleportTo(position)
    local character = game.Players.LocalPlayer.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = CFrame.new(position)
    end
end

local TeleportButton = Instance.new("TextButton", Content)
TeleportButton.Text = "Teleport: Base"
TeleportButton.Size = UDim2.new(1, -20, 0, 40)
TeleportButton.Position = UDim2.new(0, 10, 0, 10)
TeleportButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TeleportButton.Font = Enum.Font.SourceSans
TeleportButton.TextSize = 16
TeleportButton.BorderSizePixel = 0

-- 🧭 Coordenadas da base (exemplo: você pode mudar depois)
local baseCoords = Vector3.new(-60, 5, -95)

TeleportButton.MouseButton1Click:Connect(function()
    teleportTo(baseCoords)
end)
