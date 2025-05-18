-- NatanHub com visual aprimorado + funções completas para Dead Rails

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")

local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

-- Cleanup existing GUI if any
if game.CoreGui:FindFirstChild("NatanHub") then
    game.CoreGui:FindFirstChild("NatanHub"):Destroy()
end

-- GUI Setup (igual ao seu, só com pequenas adaptações)
local gui = Instance.new("ScreenGui")
gui.Name = "NatanHub"
gui.Parent = game.CoreGui
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true

-- Cores
local darkColor = Color3.fromRGB(25, 25, 25)
local accentColor = Color3.fromRGB(0, 170, 255)
local textColor = Color3.fromRGB(255, 255, 255)
local highlightColor = Color3.fromRGB(35, 35, 35)
local redColor = Color3.fromRGB(255, 50, 50)

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 520, 0, 350)
mainFrame.Position = UDim2.new(0.5, -260, 0.5, -175)
mainFrame.BackgroundColor3 = darkColor
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui
mainFrame.Visible = true
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.ClipsDescendants = true
mainFrame.BackgroundTransparency = 0

local UICornerMain = Instance.new("UICorner", mainFrame)
UICornerMain.CornerRadius = UDim.new(0, 10)

local shadow = Instance.new("ImageLabel", mainFrame)
shadow.Image = "rbxassetid://1316045217"
shadow.ImageTransparency = 0.5
shadow.Size = UDim2.new(1, 30, 1, 30)
shadow.Position = UDim2.new(0, -15, 0, -15)
shadow.BackgroundTransparency = 1
shadow.ZIndex = 0

-- Title Bar
local titleBar = Instance.new("TextLabel")
titleBar.Parent = mainFrame
titleBar.Size = UDim2.new(1, 0, 0, 32)
titleBar.BackgroundColor3 = highlightColor
titleBar.Text = "NATAN DEAD REAILS 1.0"
titleBar.TextColor3 = redColor
titleBar.Font = Enum.Font.GothamBold
titleBar.TextSize = 18
titleBar.BorderSizePixel = 0

local UICornerTitle = Instance.new("UICorner", titleBar)
UICornerTitle.CornerRadius = UDim.new(0, 6)

-- Side Menu
local sideMenu = Instance.new("Frame")
sideMenu.Name = "SideMenu"
sideMenu.Size = UDim2.new(0, 130, 1, -32)
sideMenu.Position = UDim2.new(0, 0, 0, 32)
sideMenu.BackgroundColor3 = highlightColor
sideMenu.Parent = mainFrame

local UICornerSide = Instance.new("UICorner", sideMenu)
UICornerSide.CornerRadius = UDim.new(0, 6)

-- Tabs
local tabs = {"Main", "Character", "Teleport", "Visual", "Combat", "Configuration"}
local tabFrames = {}
local activeTab

for i, tabName in ipairs(tabs) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(1, 0, 0, 35)
    tabBtn.Position = UDim2.new(0, 0, 0, (i - 1) * 35)
    tabBtn.Text = tabName
    tabBtn.BackgroundColor3 = darkColor
    tabBtn.TextColor3 = textColor
    tabBtn.Font = Enum.Font.Gotham
    tabBtn.TextSize = 14
    tabBtn.Parent = sideMenu

    local UICornerBtn = Instance.new("UICorner", tabBtn)
    UICornerBtn.CornerRadius = UDim.new(0, 6)

    local tabFrame = Instance.new("ScrollingFrame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -32)
    tabFrame.Position = UDim2.new(0, 130, 0, 32)
    tabFrame.BackgroundColor3 = darkColor
    tabFrame.BorderSizePixel = 0
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame
    tabFrame.CanvasSize = UDim2.new(0,0,3,0)
    tabFrame.ScrollBarThickness = 6

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab then activeTab.Visible = false end
        tabFrame.Visible = true
        activeTab = tabFrame
    end)
end

-- FECHAR & MINIMIZAR
local closeBtn = Instance.new("TextButton")
closeBtn.Parent = mainFrame
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -30, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
closeBtn.TextColor3 = textColor
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 16

local minBtn = Instance.new("TextButton")
minBtn.Parent = mainFrame
minBtn.Text = "-"
minBtn.Size = UDim2.new(0, 30, 0, 30)
minBtn.Position = UDim2.new(1, -60, 0, 0)
minBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
minBtn.TextColor3 = textColor
minBtn.Font = Enum.Font.GothamBold
minBtn.TextSize = 16

local closeCorner = Instance.new("UICorner", closeBtn)
closeCorner.CornerRadius = UDim.new(0, 6)
local minCorner = Instance.new("UICorner", minBtn)
minCorner.CornerRadius = UDim.new(0, 6)

closeBtn.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

minBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    floatBtn.Visible = true
end)

-- Botão flutuante para abrir a GUI
local floatBtn = Instance.new("TextButton")
floatBtn.Parent = gui
floatBtn.Text = "NatanHub"
floatBtn.Size = UDim2.new(0, 120, 0, 40)
floatBtn.Position = UDim2.new(0, 10, 0, 10)
floatBtn.BackgroundColor3 = accentColor
floatBtn.TextColor3 = textColor
floatBtn.Font = Enum.Font.GothamBold
floatBtn.TextSize = 16
floatBtn.Visible = false
floatBtn.Active = true
floatBtn.Draggable = true

local floatCorner = Instance.new("UICorner", floatBtn)
floatCorner.CornerRadius = UDim.new(0, 8)

floatBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    floatBtn.Visible = false
end)

-- Função para criar labels
local function createLabel(text, posY, parent)
    local label = Instance.new("TextLabel")
    label.Text = text
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Size = UDim2.new(0, 200, 0, 25)
    label.BackgroundTransparency = 1
    label.TextColor3 = textColor
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.Parent = parent
    return label
end

-- Função para criar botões toggle
local function createToggle(text, posY, parent, default, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 100, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = highlightColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.Text = text .. ": OFF"
    btn.Parent = parent

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    local toggled = default

    local function update()
        if toggled then
            btn.Text = text .. ": ON"
            btn.BackgroundColor3 = accentColor
        else
            btn.Text = text .. ": OFF"
            btn.BackgroundColor3 = highlightColor
        end
    end

    btn.MouseButton1Click:Connect(function()
        toggled = not toggled
        update()
        callback(toggled)
    end)

    update()
    return btn
end

-- Função para criar InputBox para números
local function createInputBox(value, posY, parent, callback)
    local box = Instance.new("TextBox")
    box.Text = tostring(value)
    box.Size = UDim2.new(0, 100, 0, 30)
    box.Position = UDim2.new(0, 10, 0, posY)
    box.BackgroundColor3 = highlightColor
    box.TextColor3 = textColor
    box.Font = Enum.Font.Gotham
    box.TextSize = 14
    box.Parent = parent

    local corner = Instance.new("UICorner", box)
    corner.CornerRadius = UDim.new(0, 6)

    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then pcall(callback, val) end
    end)

    return box
end

-- ====================
-- Aba Character (WalkSpeed e JumpPower)
local charTab = tabFrames["Character"]

createLabel("Walk Speed", 20, charTab)
createInputBox(Humanoid.WalkSpeed, 50, charTab, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
end)

createLabel("Jump Power", 90, charTab)
createInputBox(Humanoid.JumpPower, 120, charTab, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = val
    end
end)

-- ====================
-- Aba Teleport
local tpTab = tabFrames["Teleport"]

-- Função para Teleportar o jogador para uma posição
local function teleportTo(pos)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(pos)
    end
end

createLabel("Teleport Options", 10, tpTab)

-- Botões para teleport diferentes
local btnTrainTP = Instance.new("TextButton")
btnTrainTP.Size = UDim2.new(0, 150, 0, 35)
btnTrainTP.Position = UDim2.new(0, 10, 0, 40)
btnTrainTP.Text = "Teleport to Train"
btnTrainTP.BackgroundColor3 = accentColor
btnTrainTP.TextColor3 = textColor
btnTrainTP.Font = Enum.Font.GothamBold
btnTrainTP.TextSize = 16
btnTrainTP.Parent = tpTab

local cornerTrainTP = Instance.new("UICorner", btnTrainTP)
cornerTrainTP.CornerRadius = UDim.new(0, 6)

btnTrainTP.MouseButton1Click:Connect(function()
    -- Tentativa de achar o trem (exemplo)
    local train = Workspace:FindFirstChild("Train") or Workspace:FindFirstChild("TrainModel")
    if train and train:FindFirstChild("PrimaryPart") then
        teleportTo(train.PrimaryPart.Position + Vector3.new(0,5,0))
    else
        -- Se não achar o trem, tenta achar outra base (exemplo)
        local base = Workspace:FindFirstChild("Base") or Workspace:FindFirstChild("SpawnPoint")
        if base and base:IsA("BasePart") then
            teleportTo(base.Position + Vector3.new(0,5,0))
        else
            warn("Não foi possível encontrar o trem ou base para teleportar.")
        end
    end
end)

-- Teleport para bases
createLabel("Bases Teleport", 90, tpTab)

local function createBaseButton(name, posY, position)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 150, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.Text = name
    btn.BackgroundColor3 = highlightColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.Parent = tpTab

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    btn.MouseButton1Click:Connect(function()
        teleportTo(position + Vector3.new(0,5,0))
    end)
end

-- Exemplo de bases, ajustar para as posições reais do jogo
createBaseButton("Base 1", 120, Vector3.new(100, 10, 100))
createBaseButton("Base 2", 160, Vector3.new(200, 10, 200))
createBaseButton("Base 3", 200, Vector3.new(300, 10, 300))

-- Teleport final (exemplo)
local btnFinalTP = Instance.new("TextButton")
btnFinalTP.Size = UDim2.new(0, 150, 0, 35)
btnFinalTP.Position = UDim2.new(0, 10, 0, 250)
btnFinalTP.Text = "Teleport to End"
btnFinalTP.BackgroundColor3 = accentColor
btnFinalTP.TextColor3 = textColor
btnFinalTP.Font = Enum.Font.GothamBold
btnFinalTP.TextSize = 16
btnFinalTP.Parent = tpTab

local cornerFinalTP = Instance.new("UICorner", btnFinalTP)
cornerFinalTP.CornerRadius = UDim.new(0, 6)

btnFinalTP.MouseButton1Click:Connect(function()
    -- Ajuste conforme o mapa real do jogo
    local endPos = Vector3.new(1000, 10, 1000)
    teleportTo(endPos)
end)

-- ====================
-- Aba Visual (ESP, FullBright, Remove Fog, Unlock Mouse)

local visualTab = tabFrames["Visual"]

createLabel("Visual Options", 10, visualTab)

-- ESP Variables
local espEnabled = false
local espLinesEnabled = false
local espObjects = {}

-- Função para criar ESP para um personagem
local function createESP(player)
    if espObjects[player] then return end
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Adornee = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not espBox.Adornee then return end
    espBox.AlwaysOnTop = true
    espBox.ZIndex = 10
    espBox.Size = Vector3.new(4, 6, 4)
    espBox.Color3 = Color3.fromRGB(0, 170, 255)
    espBox.Transparency = 0.5
    espBox.Parent = game.CoreGui
    espObjects[player] = espBox

    -- Linha apontando
    local line = Instance.new("LineHandleAdornment")
    line.Adornee = workspace.CurrentCamera
    line.From = workspace.CurrentCamera.CFrame.Position
    line.To = espBox.Adornee.Position
    line.Color3 = espBox.Color3
    line.AlwaysOnTop = true
    line.ZIndex = 10
    line.Parent = game.CoreGui
    espObjects[player .. "_line"] = line
end

local function removeESP()
    for _, obj in pairs(espObjects) do
        if obj and obj.Parent then
            obj:Destroy()
        end
    end
    espObjects = {}
end

local function updateESP()
    for player, espBox in pairs(espObjects) do
        if typeof(player) == "string" then continue end
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            espBox.Adornee = player.Character.HumanoidRootPart
            -- Update line positions
            local line = espObjects[player .. "_line"]
            if line and line.Parent then
                line.From = workspace.CurrentCamera.CFrame.Position
                line.To = espBox.Adornee.Position
            end
        else
            -- Remove ESP se o personagem sumiu
            espBox:Destroy()
            espObjects[player] = nil
            local line = espObjects[player .. "_line"]
            if line then
                line:Destroy()
                espObjects[player .. "_line"] = nil
            end
        end
    end
end

local espToggle = createToggle("ESP Players", 40, visualTab, false, function(state)
    espEnabled = state
    if not espEnabled then
        removeESP()
    else
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                createESP(player)
            end
        end
    end
end)

-- Atualiza ESP a cada frame se ativado
RunService.RenderStepped:Connect(function()
    if espEnabled then
        updateESP()
    end
end)

-- ESP para Trem
