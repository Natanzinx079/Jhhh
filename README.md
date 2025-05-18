-- NatHub | Dead Rails (0.3.1)
-- Feito para Android | Com teleportes, botões X e - funcionais, e menu ocultável

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- Criar GUI principal
local screenGui = Instance.new("ScreenGui", game.CoreGui)
screenGui.Name = "NatHub_GUI"
screenGui.ResetOnSpawn = false

local frame = Instance.new("Frame", screenGui)
frame.Name = "MainFrame"
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.5, -150, 0.5, -100)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 25)
title.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
title.Text = "NatHub | Dead Rails (0.3.1)"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 16

title.Name = "TitleBar"

local closeBtn = Instance.new("TextButton", frame)
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 25, 0, 25)
closeBtn.Position = UDim2.new(1, -25, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextSize = 16

closeBtn.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

local minimizeBtn = Instance.new("TextButton", frame)
minimizeBtn.Text = "-"
minimizeBtn.Size = UDim2.new(0, 25, 0, 25)
minimizeBtn.Position = UDim2.new(1, -50, 0, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 0)
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Font = Enum.Font.SourceSansBold
minimizeBtn.TextSize = 16

local isMinimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    for _, obj in pairs(frame:GetChildren()) do
        if obj ~= title and obj ~= closeBtn and obj ~= minimizeBtn then
            obj.Visible = not isMinimized
        end
    end
end)

-- Botão de Teleporte
local tpBtn = Instance.new("TextButton", frame)
tpBtn.Size = UDim2.new(1, -20, 0, 30)
tpBtn.Position = UDim2.new(0, 10, 0, 40)
tpBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
tpBtn.Text = "Teleport: Base"
tpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tpBtn.Font = Enum.Font.SourceSansBold
tpBtn.TextSize = 14

tpBtn.MouseButton1Click:Connect(function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char.HumanoidRootPart.CFrame = CFrame.new(Vector3.new(132, 15, -65)) -- coordenadas da base
    end
end)

-- Adicione mais botões aqui, como "Auto Win", "Coletar Ouro", etc.
-- Me avise se quiser que eu continue com eles!

-- Interface totalmente funcional no Android.
