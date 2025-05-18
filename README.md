-- NatHub 0.3.1 Remastered (com novas funções)
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/natxhub/nathubui/main/ui.lua"))()
local hub = library:CreateWindow("NatHub | Dead Rails (0.3.1)", "by NataX | .gg/nathub")

-- Abas
local main = hub:AddTab("Main", "Settings and automation")
local character = hub:AddTab("Character", "Movement cheats")
local teleport = hub:AddTab("Teleport", "Area teleporters")
local visual = hub:AddTab("Visual", "Graphics/ESP")
local combat = hub:AddTab("Combat", "Combat advantages")
local config = hub:AddTab("Configuration", "UI & hotkeys")

-- Main
main:AddButton("Auto Collect Gold", function()
    while task.wait(0.5) do
        for _, gold in pairs(game:GetService("Workspace").Gold:GetChildren()) do
            if gold:IsA("Part") then
                gold.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
            end
        end
    end
end)

main:AddButton("Reset Position", function()
    game.Players.LocalPlayer.Character:MoveTo(Vector3.new(0, 10, 0))
end)

-- Character
character:AddSlider("Speed Hack", 16, 500, function(value)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
end)

character:AddSlider("Jump Power", 50, 500, function(value)
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
end)

character:AddToggle("Infinite Jump", false, function(enabled)
    game:GetService("UserInputService").JumpRequest:Connect(function()
        if enabled then
            game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
        end
    end)
end)

-- Combat
combat:AddToggle("Auto Kill", false, function(enabled)
    while enabled do
        task.wait(0.1)
        for _, enemy in pairs(workspace.Enemies:GetChildren()) do
            if enemy:FindFirstChild("Humanoid") then
                enemy.Humanoid.Health = 0
            end
        end
    end
end)

combat:AddToggle("One Hit Kill", false, function(enabled)
    if enabled then
        for _, tool in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
            if tool:IsA("Tool") and tool:FindFirstChild("Handle") then
                tool.Handle.Touched:Connect(function(hit)
                    local hum = hit.Parent:FindFirstChild("Humanoid")
                    if hum then hum.Health = 0 end
                end)
            end
        end
    end
end)

combat:AddToggle("God Mode", false, function(enabled)
    if enabled then
        local char = game.Players.LocalPlayer.Character
        char:FindFirstChild("Humanoid").Name = "God"
    end
end)

-- Teleport (mantido igual ao original)
teleport:AddButton("Tp To Tesla", function()
    game.Players.LocalPlayer.Character:MoveTo(Vector3.new(350, 10, -280))
end)

teleport:AddButton("Tp To End", function()
    game.Players.LocalPlayer.Character:MoveTo(Vector3.new(999, 10, 999))
end)

teleport:AddDropdown("Select Base", {"Base 1", "Base 2", "Base 3"}, function(selected)
    _G.SelectedBase = selected
end)

teleport:AddButton("Teleport", function()
    local positions = {
        ["Base 1"] = Vector3.new(10, 10, 10),
        ["Base 2"] = Vector3.new(50, 10, 50),
        ["Base 3"] = Vector3.new(100, 10, 100)
    }
    game.Players.LocalPlayer.Character:MoveTo(positions[_G.SelectedBase])
end)

-- Visual
visual:AddToggle("ESP", false, function(state)
    -- Seu sistema ESP aqui
end)

visual:AddToggle("Highlight Items", false, function(state)
    -- Código para destacar itens
end)

visual:AddToggle("No Fog", false, function(state)
    if state then
        game:GetService("Lighting").FogEnd = 1e10
    else
        game:GetService("Lighting").FogEnd = 1000
    end
end)

visual:AddToggle("Theme Switch", false, function(state)
    library:SetTheme(state and "Light" or "Dark")
end)

-- Configuração
config:AddKeybind("Toggle UI", Enum.KeyCode.RightShift, function()
    library:ToggleUI()
end)

config:AddButton("Unload UI", function()
    library:Destroy()
end)
