-- Criação do GUI principal
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "NatHubRemastered"

-- Janela principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 400, 0, 250)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true

-- Borda arredondada
local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 6)

-- Título
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, -50, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "NatHub | Dead Rails (0.3.1)"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.Gotham
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Botão de fechar (X)
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -30, 0, 0)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
CloseBtn.TextColor3 = Color3.new(1, 0.5, 0.5)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 14

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Botão de minimizar (-)
local MinBtn = Instance.new("TextButton", MainFrame)
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -60, 0, 0)
MinBtn.Text = "-"
MinBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MinBtn.TextColor3 = Color3.new(1, 1, 0.5)
MinBtn.Font = Enum.Font.GothamBold
MinBtn.TextSize = 14

local Minimized = false
MinBtn.MouseButton1Click:Connect(function()
    Minimized = not Minimized
    for _, v in pairs(MainFrame:GetChildren()) do
        if v:IsA("Frame") or v:IsA("TextButton") or v:IsA("TextLabel") then
            if v ~= Title and v ~= CloseBtn and v ~= MinBtn then
                v.Visible = not Minimized
            end
        end
    end
end)

-- Container das opções
local OptionsFrame = Instance.new("Frame", MainFrame)
OptionsFrame.Size = UDim2.new(1, -20, 1, -40)
OptionsFrame.Position = UDim2.new(0, 10, 0, 40)
OptionsFrame.BackgroundTransparency = 1

-- WalkSpeed Slider
local WalkSpeedSlider = Instance.new("TextButton", OptionsFrame)
WalkSpeedSlider.Size = UDim2.new(1, 0, 0, 40)
WalkSpeedSlider.Position = UDim2.new(0, 0, 0, 0)
WalkSpeedSlider.Text = "Walk Speed: 16"
WalkSpeedSlider.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
WalkSpeedSlider.TextColor3 = Color3.new(1, 1, 1)
WalkSpeedSlider.Font = Enum.Font.Gotham
WalkSpeedSlider.TextSize = 14

local WalkSpeed = 16
WalkSpeedSlider.MouseButton1Click:Connect(function()
    WalkSpeed = WalkSpeed + 10
    if WalkSpeed > 200 then WalkSpeed = 16 end
    WalkSpeedSlider.Text = "Walk Speed: " .. WalkSpeed
    local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = WalkSpeed end
end)

-- JumpPower Slider
local JumpPowerSlider = Instance.new("TextButton", OptionsFrame)
JumpPowerSlider.Size = UDim2.new(1, 0, 0, 40)
JumpPowerSlider.Position = UDim2.new(0, 0, 0, 50)
JumpPowerSlider.Text = "Jump Power: 50"
JumpPowerSlider.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
JumpPowerSlider.TextColor3 = Color3.new(1, 1, 1)
JumpPowerSlider.Font = Enum.Font.Gotham
JumpPowerSlider.TextSize = 14

local JumpPower = 50
JumpPowerSlider.MouseButton1Click:Connect(function()
    JumpPower = JumpPower + 25
    if JumpPower > 300 then JumpPower = 50 end
    JumpPowerSlider.Text = "Jump Power: " .. JumpPower
    local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.JumpPower = JumpPower end
end)
