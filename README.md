-- Natan Dead Reails 1.0 - Script Profissional Dead Rails
-- Estilo HUB: NatHub/Kiciahook/Lunor
-- Compatível Delta Executor & Android

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Evita execução múltipla
if _G.NatanDeadLoaded then
    warn("Natan Dead já está rodando")
    return
end
_G.NatanDeadLoaded = true

-- Cores
local Colors = {
    Primary = Color3.fromRGB(255, 0, 0),
    Secondary = Color3.fromRGB(30, 30, 30),
    Background = Color3.fromRGB(18, 18, 18),
    Accent = Color3.fromRGB(45, 45, 45),
    Text = Color3.fromRGB(240, 240, 240),
    Enabled = Color3.fromRGB(0, 180, 0),
    Disabled = Color3.fromRGB(180, 0, 0),
    Line = Color3.fromRGB(255, 50, 50),
}

-- Criar GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "NatanDeadReailsUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game:GetService("CoreGui")

-- Função para criar UI corner
local function CreateCorner(parent, radius)
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, radius or 6)
    corner.Parent = parent
    return corner
end

-- Função para criar botão estilizado
local function CreateButton(text)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 38)
    btn.BackgroundColor3 = Colors.Accent
    btn.TextColor3 = Colors.Text
    btn.Font = Enum.Font.GothamSemibold
    btn.TextSize = 17
    btn.Text = text
    btn.AutoButtonColor = false
    btn.Name = text:gsub("%s", "")
    CreateCorner(btn, 6)
    return btn
end

-- Função para criar Toggle Button
local function CreateToggleButton(text)
    local btn = CreateButton(text.." [OFF]")
    local enabled = false

    local function update()
        btn.Text = text.." ["..(enabled and "ON" or "OFF").."]"
        btn.BackgroundColor3 = enabled and Colors.Enabled or Colors.Accent
    end

    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        update()
    end)

    update()
    return btn, function() return enabled end, function(state)
        enabled = state
        update()
    end
end

-- Função para criar abas
local function CreateTabButton(text)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.BackgroundColor3 = Colors.Secondary
    btn.TextColor3 = Colors.Text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.Text = text
    btn.AutoButtonColor = false
    CreateCorner(btn, 6)
    return btn
end

-- Layout principal da janela
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 420, 0, 530)
MainFrame.Position = UDim2.new(0, 25, 0, 70)
MainFrame.BackgroundColor3 = Colors.Background
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui
MainFrame.Active = true
MainFrame.Draggable = true
CreateCorner(MainFrame, 12)

-- TopBar
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 50)
TopBar.BackgroundColor3 = Colors.Secondary
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame
CreateCorner(TopBar, 12)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -100, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 28
Title.TextColor3 = Colors.Primary
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Text = "NATAN DEAD REAILS 1.0"
Title.Parent = TopBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 40, 0, 40)
CloseBtn.Position = UDim2.new(1, -50, 0, 5)
CloseBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 24
CloseBtn.TextColor3 = Colors.Text
CloseBtn.Text = "X"
CloseBtn.Parent = TopBar
CreateCorner(CloseBtn, 6)

local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 40, 0, 40)
MinimizeBtn.Position = UDim2.new(1, -95, 0, 5)
MinimizeBtn.BackgroundColor3 = Colors.Secondary
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextSize = 26
MinimizeBtn.TextColor3 = Colors.Text
MinimizeBtn.Text = "-"
MinimizeBtn.Parent = TopBar
CreateCorner(MinimizeBtn, 6)

local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        MainFrame.Size = UDim2.new(0, 420, 0, 50)
        for _, child in pairs(MainFrame:GetChildren()) do
            if child ~= TopBar then
                child.Visible = false
            end
        end
    else
        MainFrame.Size = UDim2.new(0, 420, 0, 530)
        for _, child in pairs(MainFrame:GetChildren()) do
            child.Visible = true
        end
    end
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- SideBar para abas
local SideBar = Instance.new("Frame")
SideBar.Size = UDim2.new(0, 130, 1, -50)
SideBar.Position = UDim2.new(0, 0, 0, 50)
SideBar.BackgroundColor3 = Colors.Secondary
SideBar.BorderSizePixel = 0
SideBar.Parent = MainFrame
CreateCorner(SideBar, 12)

local SideLayout = Instance.new("UIListLayout")
SideLayout.Parent = SideBar
SideLayout.SortOrder = Enum.SortOrder.LayoutOrder
SideLayout.Padding = UDim.new(0, 8)

-- Container principal para conteúdo das abas
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -130, 1, -50)
ContentFrame.Position = UDim2.new(0, 130, 0, 50)
ContentFrame.BackgroundColor3 = Colors.Background
ContentFrame.BorderSizePixel = 0
ContentFrame.Parent = MainFrame
CreateCorner(ContentFrame, 12)

-- Sistema de abas
local Tabs = {}
local SelectedTab = nil

local function CreateTab(name)
    local tabBtn = CreateTabButton(name)
    tabBtn.Parent = SideBar

    local tabContent = Instance.new("Frame")
    tabContent.Size = UDim2.new(1, -20, 1, -20)
    tabContent.Position = UDim2.new(0, 10, 0, 10)
    tabContent.BackgroundTransparency = 1
    tabContent.Visible = false
    tabContent.Parent = ContentFrame

    tabBtn.MouseButton1Click:Connect(function()
        for _, tab in pairs(Tabs) do
            tab.Button.BackgroundColor3 = Colors.Secondary
            tab.Content.Visible = false
        end
        tabBtn.BackgroundColor3 = Colors.Primary
        tabContent.Visible = true
        SelectedTab = tabContent
    end)

    table.insert(Tabs, {Button = tabBtn, Content = tabContent})

    return tabBtn, tabContent
end

-- Criar abas
local tabMainBtn, tabMain = CreateTab("Main")
local tabFarmBtn, tabFarm = CreateTab("Farm")
local tabPlayerBtn, tabPlayer = CreateTab("Player")
local tabVisualBtn, tabVisual = CreateTab("Visuals")

-- Inicializar aba Main como selecionada
tabMainBtn.BackgroundColor3 = Colors.Primary
tabMain.Visible = true
SelectedTab = tabMain

-- Função para Teleport (em cima da torreta)
local BasesData = {
    ["Base 1"] = "Base1Turret",
    ["Base 2"] = "Base2Turret",
    ["Base 3"] = "Base3Turret",
    ["Base 4"] = "Base4Turret",
    ["Base Final"] = "EndTurret"
}

local function TeleportToBase(baseName)
    local turretName = BasesData[baseName]
    if not turretName then return false end
    local turret = Workspace:FindFirstChild(turretName)
    if turret and turret:IsA("BasePart") then
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            char.HumanoidRootPart.CFrame = turret.CFrame * CFrame.new(0, 5, 0)
            return true
        end
    end
    return false
end

-- Teleporte banco do trem
local function TeleportToTrainSeat()
    local train = Workspace:FindFirstChild("Train")
    if train then
        local seat = train:FindFirstChildOfClass("VehicleSeat") or train:FindFirstChild("Seat")
        if seat then
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                char.HumanoidRootPart.CFrame = seat.CFrame + Vector3.new(0, 3, 0)
                return true
            end
        end
    end
    return false
end

-- Botões aba Main - Teleportes
do
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 10)
    title.BackgroundTransparency = 1
    title.TextColor3 = Colors.Primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Text = "Teleports"
    title.Parent = tabMain

    for i, baseName in pairs({"Base 1", "Base 2", "Base 3", "Base 4", "Base Final"}) do
        local btn = CreateButton(baseName)
        btn.Position = UDim2.new(0, 15, 0, 10 + i*45)
        btn.Parent = tabMain

        btn.MouseButton1Click:Connect(function()
            local success = TeleportToBase(baseName)
            if success then
                notify("Teleported to "..baseName)
            else
                notify("Failed to teleport to "..baseName)
            end
        end)
    end

    -- Botão trem
    local trainBtn = CreateButton("Teleport to Train")
    trainBtn.Position = UDim2.new(0, 15, 0, 10 + 6*45)
    trainBtn.Parent = tabMain
    trainBtn.MouseButton1Click:Connect(function()
        if TeleportToTrainSeat() then
            notify("Teleported to Train Seat")
        else
            notify("Failed to teleport to Train")
        end
    end)
end

-- Notificações
local function notify(text)
    local notif = Instance.new("Frame")
    notif.Size = UDim2.new(0, 300, 0, 40)
    notif.Position = UDim2.new(0.5, -150, 0.9, 0)
    notif.BackgroundColor3 = Colors.Primary
    notif.BorderSizePixel = 0
    notif.AnchorPoint = Vector2.new(0.5, 0)
    notif.Parent = ScreenGui
    CreateCorner(notif, 8)

    local label = Instance.new("TextLabel")
    label.BackgroundTransparency = 1
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Text = text
    label.TextColor3 = Colors.Text
    label.Font = Enum.Font.GothamBold
    label.TextSize = 20
    label.Parent = notif

    local tweenIn = TweenService:Create(notif, TweenInfo.new(0.3), {Position = UDim2.new(0.5, -150, 0.8, 0)})
    local tweenOut = TweenService:Create(notif, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out, 0, 1, 0.5), {Position = UDim2.new(0.5, -150, 0.9, 0)})

    tweenIn:Play()
    tweenIn.Completed:Wait()
    task.wait(2)
    tweenOut:Play()
    tweenOut.Completed:Connect(function()
        notif:Destroy()
    end)
end

-- Noclip toggle
local noclipEnabled = false
local function SetNoclip(state)
    noclipEnabled = state
end

local function noclipLoop()
    RunService.Stepped:Connect(function()
        if noclipEnabled and LocalPlayer.Character then
            for _, part in pairs(LocalPlayer.Character:GetChildren()) do
                if part:IsA("BasePart") and part.CanCollide then
                    part.CanCollide = false
                end
            end
        end
    end)
end
noclipLoop()

-- AutoFarm (funcional)
local autoFarmEnabled = false
local function AutoFarm()
    RunService.Heartbeat:Connect(function()
        if autoFarmEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LocalPlayer.Character.HumanoidRootPart
            if Workspace:FindFirstChild("Enemies") then
                for _, enemy in pairs(Workspace.Enemies:GetChildren()) do
                    if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                        local dist = (enemy.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if dist <= 50 then
                            LocalPlayer.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
                            -- Ataque simplificado
                            enemy.Humanoid:TakeDamage(50)
                        end
                    end
                end
            end
        end
    end)
end
AutoFarm()

-- Aba Farm
do
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 10)
    title.BackgroundTransparency = 1
    title.TextColor3 = Colors.Primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Text = "Farm"
    title.Parent = tabFarm

    local farmToggle, farmGet, farmSet = CreateToggleButton("Auto Farm")
    farmToggle.Position = UDim2.new(0, 15, 0, 60)
    farmToggle.Parent = tabFarm
    farmToggle.MouseButton1Click:Connect(function()
        autoFarmEnabled = farmGet()
    end)
end

-- Kill Aura toggle
local killAuraEnabled = false
local function KillAura()
    RunService.Heartbeat:Connect(function()
        if killAuraEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LocalPlayer.Character.HumanoidRootPart
            if Workspace:FindFirstChild("Enemies") then
                for _, npc in pairs(Workspace.Enemies:GetChildren()) do
                    if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                        local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if dist <= 15 then
                            npc.Humanoid:TakeDamage(50)
                        end
                    end
                end
            end
        end
    end)
end
KillAura()

-- Aba Player (Noclip e Kill Aura)
do
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 10)
    title.BackgroundTransparency = 1
    title.TextColor3 = Colors.Primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Text = "Player"
    title.Parent = tabPlayer

    local noclipToggle, noclipGet, noclipSet = CreateToggleButton("Noclip")
    noclipToggle.Position = UDim2.new(0, 15, 0, 60)
    noclipToggle.Parent = tabPlayer
    noclipToggle.MouseButton1Click:Connect(function()
        SetNoclip(noclipGet())
        if not noclipGet() then
            -- Reativa colisão
            if LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end)

    local killToggle, killGet, killSet = CreateToggleButton("Kill Aura")
    killToggle.Position = UDim2.new(0, 15, 0, 110)
    killToggle.Parent = tabPlayer
    killToggle.MouseButton1Click:Connect(function()
        killAuraEnabled = killGet()
    end)
end

-- ESP
local espEnabled = false
local espObjects = {}

local function ClearESP()
    for _, v in pairs(espObjects) do
        if v and v.Parent then
            v:Destroy()
        end
    end
    espObjects = {}
end

local function CreateESP(target)
    if not target or not target:IsA("Model") then return end
    if espObjects[target] then return end

    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP_Billboard"
    billboard.Adornee = target:FindFirstChild("HumanoidRootPart")
    billboard.Size = UDim2.new(0, 120, 0, 30)
    billboard.AlwaysOnTop = true
    billboard.Parent = target

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.TextColor3 = Colors.Primary
    label.Font = Enum.Font.GothamBold
    label.TextStrokeColor3 = Color3.new(0,0,0)
    label.TextStrokeTransparency = 0.4
    label.TextSize = 18
    label.Text = target.Name
    label.Parent = billboard

    espObjects[target] = billboard
end

local function ESPLoop()
    RunService.Heartbeat:Connect(function()
        if espEnabled then
            if Workspace:FindFirstChild("Enemies") then
                for _, npc in pairs(Workspace.Enemies:GetChildren()) do
                    if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                        if not espObjects[npc] then
                            CreateESP(npc)
                        end
                    else
                        if espObjects[npc] then
                            espObjects[npc]:Destroy()
                            espObjects[npc] = nil
                        end
                    end
                end
            end
        else
            ClearESP()
        end
    end)
end
ESPLoop()

-- Aba Visuals (ESP toggle)
do
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 10)
    title.BackgroundTransparency = 1
    title.TextColor3 = Colors.Primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Text = "Visuals"
    title.Parent = tabVisual

    local espToggle, espGet, espSet = CreateToggleButton("ESP")
    espToggle.Position = UDim2.new(0, 15, 0, 60)
    espToggle.Parent = tabVisual
    espToggle.MouseButton1Click:Connect(function()
        espEnabled = espGet()
    end)
end

-- Linha apontando para os NPC (ESP Line)
local espLineEnabled = false
local espLine = nil

local function CreateESPLine()
    if espLine then espLine:Destroy() end
    local line = Drawing.new("Line")
    line.Visible = false
    line.Color = Colors.Line
    line.Thickness = 1.8
    line.Transparency = 1
    return line
end

local function ESPLineLoop()
    RunService.RenderStepped:Connect(function()
        if espLineEnabled and espEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = LocalPlayer.Character.HumanoidRootPart
            local closestNPC = nil
            local closestDist = math.huge
            for _, npc in pairs(Workspace.Enemies:GetChildren()) do
                if npc:FindFirstChild("HumanoidRootPart") and npc:FindFirstChild("Humanoid") and npc.Humanoid.Health > 0 then
                    local dist = (npc.HumanoidRootPart.Position - hrp.Position).Magnitude
                    if dist < closestDist then
                        closestDist = dist
                        closestNPC = npc
                    end
                end
            end
            if closestNPC and closestNPC:FindFirstChild("HumanoidRootPart") then
                local npcPos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(closestNPC.HumanoidRootPart.Position)
                local playerPos, _ = workspace.CurrentCamera:WorldToViewportPoint(hrp.Position)
                if onScreen then
                    if not espLine then
                        espLine = CreateESPLine()
                    end
                    espLine.From = Vector2
                    -- ESP
local espEnabled = false
local espObjects = {}

local function CreateESPBox(part)
    local box = Instance.new("BoxHandleAdornment")
    box.Adornee = part
    box.AlwaysOnTop = true
    box.ZIndex = 5
    box.Size = part.Size + Vector3.new(0.1, 0.1, 0.1)
    box.Color3 = Colors.Primary
    box.Transparency = 0.5
    box.Parent = part
    return box
end

local function RemoveESP()
    for part, box in pairs(espObjects) do
        if box and box.Parent then
            box:Destroy()
        end
    end
    espObjects = {}
end

local function ToggleESP(state)
    espEnabled = state
    if not espEnabled then
        RemoveESP()
        return
    end

    local function UpdateESP()
        if not espEnabled then return end
        RemoveESP()
        if Workspace:FindFirstChild("Enemies") then
            for _, enemy in pairs(Workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("HumanoidRootPart") and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
                    local hrp = enemy.HumanoidRootPart
                    espObjects[hrp] = CreateESPBox(hrp)
                end
            end
        end
    end

    UpdateESP()
    -- Atualiza a cada 2 segundos para captar inimigos novos
    spawn(function()
        while espEnabled do
            task.wait(2)
            UpdateESP()
        end
    end)
end

-- Aba Visuals (ESP toggle)
do
    local title = Instance.new("TextLabel")
    title.Size = UDim2.new(1, 0, 0, 35)
    title.Position = UDim2.new(0, 0, 0, 10)
    title.BackgroundTransparency = 1
    title.TextColor3 = Colors.Primary
    title.Font = Enum.Font.GothamBold
    title.TextSize = 24
    title.Text = "Visuals"
    title.Parent = tabVisual

    local espToggle, espGet, espSet = CreateToggleButton("ESP Enemies")
    espToggle.Position = UDim2.new(0, 15, 0, 60)
    espToggle.Parent = tabVisual
    espToggle.MouseButton1Click:Connect(function()
        ToggleESP(espGet())
    end)
end

-- Função para desbloquear mouse e câmera (exemplo)
local unlockMouseEnabled = false
local function ToggleUnlockMouse(state)
    unlockMouseEnabled = state
    UserInputService.MouseBehavior = unlockMouseEnabled and Enum.MouseBehavior.Default or Enum.MouseBehavior.LockCenter
    UserInputService.MouseIconEnabled = unlockMouseEnabled
end

-- Adicionando botão Unlock Mouse na aba Visuals
do
    local unlockMouseToggle, unlockMouseGet, unlockMouseSet = CreateToggleButton("Unlock Mouse")
    unlockMouseToggle.Position = UDim2.new(0, 15, 0, 110)
    unlockMouseToggle.Parent = tabVisual
    unlockMouseToggle.MouseButton1Click:Connect(function()
        ToggleUnlockMouse(unlockMouseGet())
    end)
end

-- Remover névoa e adicionar FullBright (exemplo básico)
local fogRemoved = false
local function RemoveFog(state)
    fogRemoved = state
    if fogRemoved then
        game.Lighting.FogEnd = 100000
        game.Lighting.FogStart = 0
    else
        -- Reseta para valores padrões (exemplo)
        game.Lighting.FogEnd = 1000
        game.Lighting.FogStart = 0
    end
end

local fullBrightEnabled = false
local originalBrightness = game.Lighting.Brightness

local function ToggleFullBright(state)
    fullBrightEnabled = state
    if fullBrightEnabled then
        originalBrightness = game.Lighting.Brightness
        game.Lighting.Brightness = 5
        game.Lighting.Ambient = Color3.new(1, 1, 1)
    else
        game.Lighting.Brightness = originalBrightness
        game.Lighting.Ambient = Color3.new(0.5, 0.5, 0.5)
    end
end

-- Botões para Fog e FullBright aba Visuals
do
    local removeFogToggle, removeFogGet, removeFogSet = CreateToggleButton("Remove Fog")
    removeFogToggle.Position = UDim2.new(0, 15, 0, 160)
    removeFogToggle.Parent = tabVisual
    removeFogToggle.MouseButton1Click:Connect(function()
        RemoveFog(removeFogGet())
    end)

    local fullBrightToggle, fullBrightGet, fullBrightSet = CreateToggleButton("Full Bright")
    fullBrightToggle.Position = UDim2.new(0, 15, 0, 210)
    fullBrightToggle.Parent = tabVisual
    fullBrightToggle.MouseButton1Click:Connect(function()
        ToggleFullBright(fullBrightGet())
    end)
end

-- Finalização
notify("Natan Dead Reails 1.0 Loaded!")

return ScreenGui
