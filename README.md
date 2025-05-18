local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

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

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 520, 0, 320)
mainFrame.Position = UDim2.new(0.5, -260, 0.5, -160)
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

-- Shadow
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
titleBar.Text = "Natan Dead Reails 1.0"
titleBar.TextColor3 = Color3.fromRGB(255, 0, 0) -- vermelho conforme pedido
titleBar.Font = Enum.Font.GothamBold
titleBar.TextSize = 16
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

    local tabFrame = Instance.new("Frame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -32)
    tabFrame.Position = UDim2.new(0, 130, 0, 32)
    tabFrame.BackgroundColor3 = darkColor
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab then activeTab.Visible = false end
        tabFrame.Visible = true
        activeTab = tabFrame
    end)
end

-- Função para criar toggle buttons
local function createToggleButton(parent, name, posY, callback)
    local btn = Instance.new("TextButton")
    btn.Text = name .. ": OFF"
    btn.Size = UDim2.new(0, 200, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = highlightColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.Parent = parent
    btn.AutoButtonColor = false

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    local active = false

    btn.MouseButton1Click:Connect(function()
        active = not active
        btn.Text = name .. ": " .. (active and "ON" or "OFF")
        callback(active)
    end)
end

-- === Aba Main ===
local mainTab = tabFrames["Main"]
local autoBondActive = false
local autoWinActive = false

-- Auto Bond
createToggleButton(mainTab, "Auto Bond", 20, function(state)
    autoBondActive = state
    task.spawn(function()
        while autoBondActive do
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("TouchTransmitter") and v.Parent and v.Parent.Name:lower():find("bond") then
                    firetouchinterest(LocalPlayer.Character.HumanoidRootPart, v.Parent, 0)
                    firetouchinterest(LocalPlayer.Character.HumanoidRootPart, v.Parent, 1)
                    task.wait(0.2)
                end
            end
            task.wait(1)
        end
    end)
end)

-- Auto Win
createToggleButton(mainTab, "Auto Win", 70, function(state)
    autoWinActive = state
    task.spawn(function()
        while autoWinActive do
            local checkpoints = workspace:FindFirstChild("Checkpoints")
            if checkpoints then
                for _, cp in ipairs(checkpoints:GetChildren()) do
                    if cp:IsA("BasePart") and cp.Name:lower():find("end") then
                        if LocalPlayer.Character then
                            LocalPlayer.Character:MoveTo(cp.Position + Vector3.new(0, 5, 0))
                        end
                        break
                    end
                end
            end
            task.wait(2)
        end
    end)
end)

-- === Aba Character ===
local charTab = tabFrames["Character"]

local function createLabel(parent, text, posY)
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

local function createInputBox(parent, value, posY, callback)
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

createLabel(charTab, "Walk Speed", 20)
createInputBox(charTab, LocalPlayer.Character.Humanoid.WalkSpeed, 50, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
end)

createLabel(charTab, "Jump Power", 90)
createInputBox(charTab, LocalPlayer.Character.Humanoid.JumpPower, 120, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = val
    end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

-- Controle AutoBond
local autoBondEnabled = false
local autoBondConnection

-- Evento remoto de exemplo para bond (troque pelo nome real do jogo)
local bondEvent = ReplicatedStorage:FindFirstChild("BondEvent") or ReplicatedStorage:WaitForChild("BondEvent")

local function toggleAutoBond()
    if autoBondEnabled then
        autoBondEnabled = false
        if autoBondConnection then
            autoBondConnection:Disconnect()
            autoBondConnection = nil
        end
        print("AutoBond desativado")
    else
        autoBondEnabled = true
        autoBondConnection = RunService.Heartbeat:Connect(function()
            if autoBondEnabled and bondEvent then
                pcall(function()
                    bondEvent:FireServer()
                end)
            end
        end)
        print("AutoBond ativado")
    end
end

-- Teleporte para o final do mapa
local function tpToEnd()
    local endBase = workspace:FindFirstChild("EndBase") or workspace:FindFirstChild("EndPoint")
    if endBase and endBase:IsA("BasePart") then
        HumanoidRootPart.CFrame = endBase.CFrame + Vector3.new(0, 5, 0)
        print("Teleportado para o final!")
    else
        warn("Local do final não encontrado.")
    end
end

-- Exemplo botões (adicione no seu GUI Main)
-- Supondo que tabFrames["Main"] exista e seu GUI esteja carregado

local function createButton(parent, text, posY, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 150, 0, 40)
    btn.Position = UDim2.new(0, 20, 0, posY)
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Parent = parent
    btn.ClipsDescendants = true
    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)
    btn.MouseButton1Click:Connect(callback)
    return btn
end

if tabFrames and tabFrames["Main"] then
    createButton(tabFrames["Main"], "Toggle AutoBond", 60, toggleAutoBond)
    createButton(tabFrames["Main"], "Teleport to End", 110, tpToEnd)
end

-- Fechar & Minimizar
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

-- Botão flutuante
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

-- Ativar aba padrão
task.wait()
tabFrames["Main"].Visible = true
activeTab = tabFrames["Main"]
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

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

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 520, 0, 320)
mainFrame.Position = UDim2.new(0.5, -260, 0.5, -160)
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

-- Shadow
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
titleBar.Text = "Natan Dead Reails 1.0"
titleBar.TextColor3 = Color3.fromRGB(255, 0, 0) -- vermelho conforme pedido
titleBar.Font = Enum.Font.GothamBold
titleBar.TextSize = 16
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

    local tabFrame = Instance.new("Frame")
    tabFrame.Name = tabName .. "Frame"
    tabFrame.Size = UDim2.new(1, -130, 1, -32)
    tabFrame.Position = UDim2.new(0, 130, 0, 32)
    tabFrame.BackgroundColor3 = darkColor
    tabFrame.Visible = false
    tabFrame.Parent = mainFrame

    tabFrames[tabName] = tabFrame

    tabBtn.MouseButton1Click:Connect(function()
        if activeTab then activeTab.Visible = false end
        tabFrame.Visible = true
        activeTab = tabFrame
    end)
end

-- Função para criar toggle buttons
local function createToggleButton(parent, name, posY, callback)
    local btn = Instance.new("TextButton")
    btn.Text = name .. ": OFF"
    btn.Size = UDim2.new(0, 200, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = highlightColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.Parent = parent
    btn.AutoButtonColor = false

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    local active = false

    btn.MouseButton1Click:Connect(function()
        active = not active
        btn.Text = name .. ": " .. (active and "ON" or "OFF")
        callback(active)
    end)
end

-- === Aba Main ===
local mainTab = tabFrames["Main"]
local autoBondActive = false
local autoWinActive = false

-- Auto Bond
createToggleButton(mainTab, "Auto Bond", 20, function(state)
    autoBondActive = state
    task.spawn(function()
        while autoBondActive do
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("TouchTransmitter") and v.Parent and v.Parent.Name:lower():find("bond") then
                    firetouchinterest(LocalPlayer.Character.HumanoidRootPart, v.Parent, 0)
                    firetouchinterest(LocalPlayer.Character.HumanoidRootPart, v.Parent, 1)
                    task.wait(0.2)
                end
            end
            task.wait(1)
        end
    end)
end)

-- Auto Win
createToggleButton(mainTab, "Auto Win", 70, function(state)
    autoWinActive = state
    task.spawn(function()
        while autoWinActive do
            local checkpoints = workspace:FindFirstChild("Checkpoints")
            if checkpoints then
                for _, cp in ipairs(checkpoints:GetChildren()) do
                    if cp:IsA("BasePart") and cp.Name:lower():find("end") then
                        if LocalPlayer.Character then
                            LocalPlayer.Character:MoveTo(cp.Position + Vector3.new(0, 5, 0))
                        end
                        break
                    end
                end
            end
            task.wait(2)
        end
    end)
end)

-- === Aba Character ===
local charTab = tabFrames["Character"]

local function createLabel(parent, text, posY)
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

local function createInputBox(parent, value, posY, callback)
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

createLabel(charTab, "Walk Speed", 20)
createInputBox(charTab, LocalPlayer.Character.Humanoid.WalkSpeed, 50, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
end)

createLabel(charTab, "Jump Power", 90)
createInputBox(charTab, LocalPlayer.Character.Humanoid.JumpPower, 120, function(val)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.JumpPower = val
    end
end)

-- Fechar & Minimizar
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

-- Botão flutuante
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

-- Ativar aba padrão
task.wait()
tabFrames["Main"].Visible = true
activeTab = tabFrames["Main"]
