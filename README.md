-- NatanHub com visual aprimorado (sem alterar funções)
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
mainFrame:SetAttribute("Rounded", true)

-- Arredondamento
local UICornerMain = Instance.new("UICorner", mainFrame)
UICornerMain.CornerRadius = UDim.new(0, 10)

-- Sombra
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
titleBar.Text = "NatHub | Dead Rails (0.3.1)"
titleBar.TextColor3 = textColor
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

-- Conteúdo do Character
local charTab = tabFrames["Character"]

local function createLabel(text, posY)
    local label = Instance.new("TextLabel")
    label.Text = text
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Size = UDim2.new(0, 200, 0, 25)
    label.BackgroundTransparency = 1
    label.TextColor3 = textColor
    label.Font = Enum.Font.Gotham
    label.TextSize = 14
    label.Parent = charTab
    return label
end

local function createInputBox(value, posY, callback)
    local box = Instance.new("TextBox")
    box.Text = tostring(value)
    box.Size = UDim2.new(0, 100, 0, 30)
    box.Position = UDim2.new(0, 10, 0, posY)
    box.BackgroundColor3 = highlightColor
    box.TextColor3 = textColor
    box.Font = Enum.Font.Gotham
    box.TextSize = 14
    box.Parent = charTab

    local corner = Instance.new("UICorner", box)
    corner.CornerRadius = UDim.new(0, 6)

    box.FocusLost:Connect(function()
        local val = tonumber(box.Text)
        if val then pcall(callback, val) end
    end)

    return box
end

createLabel("Walk Speed", 20)
createInputBox(LocalPlayer.Character.Humanoid.WalkSpeed, 50, function(val)
    LocalPlayer.Character.Humanoid.WalkSpeed = val
end)

createLabel("Jump Power", 90)
createInputBox(LocalPlayer.Character.Humanoid.JumpPower, 120, function(val)
    LocalPlayer.Character.Humanoid.JumpPower = val
end)

-- CONTINUAÇÃO DO SCRIPT ABAIXO (para colocar no final do seu código atual)

local visualTab = tabFrames["Visual"]

-- [1] Função para criar botões de toggle
local function createToggle(name, posY, callback)
    local btn = Instance.new("TextButton")
    btn.Text = name .. ": OFF"
    btn.Size = UDim2.new(0, 200, 0, 30)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = highlightColor
    btn.TextColor3 = textColor
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.Parent = visualTab

    local corner = Instance.new("UICorner", btn)
    corner.CornerRadius = UDim.new(0, 6)

    local enabled = false
    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        btn.Text = name .. ": " .. (enabled and "ON" or "OFF")
        callback(enabled)
    end)
end

-- [2] ESP Utility
local RunService = game:GetService("RunService")
local function createESP(target, color, name, drawLine)
    local Billboard = Instance.new("BillboardGui")
    Billboard.Size = UDim2.new(0, 100, 0, 40)
    Billboard.AlwaysOnTop = true
    Billboard.Adornee = target
    Billboard.Name = "ESP_" .. name
    Billboard.Parent = target

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, 0, 1, 0)
    Label.BackgroundTransparency = 1
    Label.TextColor3 = color
    Label.TextStrokeTransparency = 0
    Label.Text = name
    Label.Font = Enum.Font.GothamBold
    Label.TextScaled = true
    Label.Parent = Billboard

    local line
    if drawLine then
        line = Drawing.new("Line")
        line.Color = color
        line.Thickness = 2
        line.Transparency = 1

        local conn
        conn = RunService.RenderStepped:Connect(function()
            if not target or not target:IsDescendantOf(game) then
                if line then line:Remove() end
                if Billboard then Billboard:Destroy() end
                conn:Disconnect()
                return
            end
            local pos, visible = workspace.CurrentCamera:WorldToViewportPoint(target.Position)
            if visible then
                line.Visible = true
                line.From = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y)
                line.To = Vector2.new(pos.X, pos.Y)
            else
                line.Visible = false
            end
        end)
    end
end

-- [3] ESPs

createToggle("ESP Trem", 20, function(state)
    for _,v in ipairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and v.Name:lower():find("train") and not v:FindFirstChild("ESP_Train") then
            if state then
                createESP(v, Color3.fromRGB(255, 0, 0), "Train", true)
            end
        elseif v:FindFirstChild("ESP_Train") and not state then
            v:FindFirstChild("ESP_Train"):Destroy()
        end
    end
end)

createToggle("ESP Ouro", 60, function(state)
    for _,v in ipairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and v.Name:lower():find("gold") and not v:FindFirstChild("ESP_Gold") then
            if state then
                createESP(v, Color3.fromRGB(255, 255, 0), "Ouro", true)
            end
        elseif v:FindFirstChild("ESP_Gold") and not state then
            v:FindFirstChild("ESP_Gold"):Destroy()
        end
    end
end)

-- [4] Mouse Lock para Mobile
createToggle("Mouse Lock", 100, function(state)
    pcall(function()
        local cam = workspace.CurrentCamera
        if state then
            cam.CameraType = Enum.CameraType.Scriptable
        else
            cam.CameraType = Enum.CameraType.Custom
        end
    end)
end)

-- Botão flutuante
local floatBtn = Instance.new("TextButton")
floatBtn.Parent = gui
floatBtn.Text = "NatHub"
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

floatBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    floatBtn.Visible = false
end)

-- Ativar aba padrão
task.wait()
tabFrames["Character"].Visible = true
activeTab = tabFrames["Character"]
