-- Suas declarações iniciais e GUI já existem acima

-- === INÍCIO DAS FUNÇÕES E VARIÁVEIS ===

local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

-- Variáveis de controle
local autoBondEnabled = false
local autoWinEnabled = false
local noclipEnabled = false
local espEnabled = false

-- Eventos remotos (ajuste os nomes conforme o jogo atual)
local bondEvent = ReplicatedStorage:WaitForChild("BondEvent") -- troque pelo nome correto
local winEvent = ReplicatedStorage:WaitForChild("WinEvent") -- troque pelo nome correto

-- Função Auto Bond
local autoBondConnection
local function toggleAutoBond()
    if autoBondEnabled then
        autoBondEnabled = false
        if autoBondConnection then autoBondConnection:Disconnect() end
        autoBondConnection = nil
        print("Auto Bond desativado")
    else
        autoBondEnabled = true
        autoBondConnection = RunService.Heartbeat:Connect(function()
            if bondEvent then
                pcall(function() bondEvent:FireServer() end)
            end
        end)
        print("Auto Bond ativado")
    end
end

-- Função Auto Win
local autoWinConnection
local function toggleAutoWin()
    if autoWinEnabled then
        autoWinEnabled = false
        if autoWinConnection then autoWinConnection:Disconnect() end
        autoWinConnection = nil
        print("Auto Win desativado")
    else
        autoWinEnabled = true
        autoWinConnection = RunService.Heartbeat:Connect(function()
            if winEvent then
                pcall(function() winEvent:FireServer() end)
            end
        end)
        print("Auto Win ativado")
    end
end

-- Teleporte para o trem
local function tpToTrain()
    local train = Workspace:FindFirstChild("Train")
    if train and train:IsA("Model") then
        local trainPos = train:FindFirstChild("HumanoidRootPart") or train:FindFirstChildWhichIsA("BasePart")
        if trainPos then
            HumanoidRootPart.CFrame = trainPos.CFrame + Vector3.new(0, 5, 0)
            print("Teleportado para o trem")
            return
        end
    end
    warn("Trem não encontrado")
end

-- Teleporte para o final
local function tpToEnd()
    local endBase = Workspace:FindFirstChild("EndBase") or Workspace:FindFirstChild("EndPoint")
    if endBase and endBase:IsA("BasePart") then
        HumanoidRootPart.CFrame = endBase.CFrame + Vector3.new(0, 5, 0)
        print("Teleportado para o final")
    else
        warn("Final do mapa não encontrado")
    end
end

-- Noclip
local function toggleNoclip()
    noclipEnabled = not noclipEnabled
    if noclipEnabled then
        print("Noclip ativado")
        RunService.Stepped:Connect(function()
            if noclipEnabled then
                for _, part in pairs(Character:GetChildren()) do
                    if part:IsA("BasePart") and part.CanCollide then
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        print("Noclip desativado")
        for _, part in pairs(Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end

-- Unlock Mouse (libera o mouse para mexer fora da janela)
local function unlockMouse()
    UserInputService.MouseBehavior = Enum.MouseBehavior.Default
    print("Mouse desbloqueado")
end

-- Unlock Camera (libera a câmera para girar livremente)
local function unlockCamera()
    UserInputService.MouseBehavior = Enum.MouseBehavior.LockCenter
    print("Camera desbloqueada")
end

-- ESP Simples para jogadores e trem
local ESPObjects = {}

local function createESP(object, color)
    local espBox = Instance.new("BoxHandleAdornment")
    espBox.Adornee = object
    espBox.AlwaysOnTop = true
    espBox.ZIndex = 10
    espBox.Size = object.Size
    espBox.Transparency = 0.5
    espBox.Color3 = color
    espBox.Parent = object
    return espBox
end

local function toggleESP()
    espEnabled = not espEnabled
    if espEnabled then
        -- ESP nos jogadores
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                if not ESPObjects[player] then
                    ESPObjects[player] = createESP(player.Character.HumanoidRootPart, Color3.new(0, 1, 0))
                end
            end
        end
        -- ESP no trem
        local train = Workspace:FindFirstChild("Train")
        if train and train:FindFirstChild("HumanoidRootPart") then
            if not ESPObjects["Train"] then
                ESPObjects["Train"] = createESP(train.HumanoidRootPart, Color3.new(1, 0, 0))
            end
        end
        print("ESP ativado")
    else
        for _, esp in pairs(ESPObjects) do
            if esp then esp:Destroy() end
        end
        ESPObjects = {}
        print("ESP desativado")
    end
end

-- Atualiza ESP caso entre um novo jogador
Players.PlayerAdded:Connect(function(player)
    if espEnabled then
        player.CharacterAdded:Connect(function(character)
            local hrp = character:WaitForChild("HumanoidRootPart", 5)
            if hrp and espEnabled then
                if not ESPObjects[player] then
                    ESPObjects[player] = createESP(hrp, Color3.new(0, 1, 0))
                end
            end
        end)
    end
end)

-- === FIM DAS FUNÇÕES ===

-- === CRIAÇÃO DOS BOTÕES NAS ABAS ===

-- Aba Teleport
local teleportTab = tabFrames["Teleport"]
local function addTeleportButton(text, y, func)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 160, 0, 35)
    btn.Position = UDim2.new(0, 20, 0, y)
    btn.BackgroundColor3 = accentColor
    btn.TextColor3 = textColor
    btn.Text = text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Parent = teleportTab
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 8)
    btn.MouseButton1Click:Connect(func)
    return btn
end

addTeleportButton("Teleportar para Trem", 20, tpToTrain)
addTeleportButton("Teleportar para Final", 70, tpToEnd)

-- Aba Combat
local combatTab = tabFrames["Combat"]
local function addToggleButton(text, y, getStateFunc, toggleFunc)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 160, 0, 35)
    btn.Position = UDim2.new(0, 20, 0, y)
    btn.BackgroundColor3 = accentColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Parent = combatTab
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 8)

    local function updateText()
        btn.Text = text .. (getStateFunc() and " [ON]" or " [OFF]")
    end

    btn.MouseButton1Click:Connect(function()
        toggleFunc()
        updateText()
    end)

    updateText()
    return btn
end

addToggleButton("Auto Bond", 20, function() return autoBondEnabled end, toggleAutoBond)
addToggleButton("Auto Win", 70, function() return autoWinEnabled end, toggleAutoWin)

-- Aba Visual
local visualTab = tabFrames["Visual"]
local espBtn = Instance.new("TextButton")
espBtn.Size = UDim2.new(0, 160, 0, 35)
espBtn.Position = UDim2.new(0, 20, 0, 20)
espBtn.BackgroundColor3 = accentColor
espBtn.TextColor3 = textColor
espBtn.Font = Enum.Font.GothamBold
espBtn.TextSize = 16
espBtn.Text = "Toggle ESP [OFF]"
espBtn.Parent = visualTab
local espCorner = Instance.new("UICorner", espBtn)
espCorner.CornerRadius = UDim.new(0, 8)

espBtn.MouseButton1Click:Connect(function()
    toggleESP()
    espBtn.Text = "Toggle ESP " .. (espEnabled and "[ON]" or "[OFF]")
end)

-- Aba Configuration
local configTab = tabFrames["Configuration"]
local unlockMouseBtn = Instance.new("TextButton")
unlockMouseBtn.Size = UDim2.new(0, 160, 0, 35)
unlockMouseBtn.Position = UDim2.new(0, 20, 0, 20)
unlockMouseBtn.BackgroundColor3 = accentColor
unlockMouseBtn.TextColor3 = textColor
unlockMouseBtn.Font = Enum.Font.GothamBold
unlockMouseBtn.TextSize = 16
unlockMouseBtn.Text = "Unlock Mouse"
unlockMouseBtn.Parent = configTab
local unlockMouseCorner = Instance.new("UICorner", unlockMouseBtn)
unlockMouseCorner.CornerRadius = UDim.new(0, 8)
unlockMouseBtn.MouseButton1Click:Connect(unlockMouse)

local unlockCameraBtn = Instance.new("TextButton")
unlockCameraBtn.Size = UDim2.new(0, 160, 0, 35)
unlockCameraBtn.Position = UDim2.new(0, 20, 0, 70)
unlockCameraBtn.BackgroundColor3 = accentColor
unlockCameraBtn.TextColor3 = textColor
unlockCameraBtn.Font = Enum.Font.GothamBold
unlockCameraBtn.TextSize = 16
unlockCameraBtn.Text = "Unlock Camera"
unlockCameraBtn.Parent = configTab
local unlockCameraCorner = Instance.new("UICorner", unlockCameraBtn)
unlockCameraCorner.CornerRadius = UDim.new(0, 8)
unlockCameraBtn.MouseButton1Click:Connect(unlockCamera)

-- Aba Character - Noclip
local characterTab = tabFrames["Character"]
local noclipBtn = Instance.new("TextButton")
noclipBtn.Size = UDim2.new(0, 160, 0, 35)
noclipBtn.Position = UDim2.new(0, 20, 0, 160)
noclipBtn.BackgroundColor3 = accentColor
noclipBtn.TextColor3 = textColor
noclipBtn.Font = Enum.Font.GothamBold
noclipBtn.TextSize = 16
noclipBtn.Text = "Toggle Noclip [OFF]"
noclipBtn.Parent = characterTab
local noclipCorner = Instance.new("UICorner", noclipBtn)
noclipCorner.CornerRadius = UDim.new(0, 8)

noclipBtn.MouseButton1Click:Connect(function()
    toggleNoclip()
    noclipBtn.Text = "Toggle Noclip " .. (noclipEnabled and "[ON]" or "[OFF]")
end)

-- === FINALIZAÇÃO ===

-- Deixa aba padrão aberta
if activeTab then activeTab.Visible = false end
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]
