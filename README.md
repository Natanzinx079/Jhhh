local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/natxhub/nathubui/main/ui.lua"))()
local hub = library:CreateWindow("NatHub | Dead Rails (0.3.1)", "by NataX | .gg/nathub")

-- Tabs
local main = hub:AddTab("Main", "Settings and automation")
local character = hub:AddTab("Character", "Movement cheats")
local teleport = hub:AddTab("Teleport", "Area teleporters")
local visual = hub:AddTab("Visual", "Graphics/ESP")
local combat = hub:AddTab("Combat", "Combat advantages")
local config = hub:AddTab("Configuration", "UI & hotkeys")

-- Main
main:AddButton("Auto Collect Gold", function()
    task.spawn(function()
        while wait(0.5) do
            for _, gold in ipairs(workspace.Gold:GetChildren()) do
                if gold:IsA("Part") then
                    gold.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
                end
            end
        end
    end)
end)

main:AddButton("Reset Position", function()
    game.Players.LocalPlayer.Character:MoveTo(Vector3.new(0, 10, 0))
end)

-- Character
character:AddSlider("Walk Speed", 16, 500, function(value)
    local humanoid = game.Players.LocalPlayer.Character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then humanoid.WalkSpeed = value end
end)

character:AddSlider("Jump Power", 50, 500, function(value)
    local humanoid = game.Players.LocalPlayer.Character:FindFirstChildWhichIsA("Humanoid")
    if humanoid then humanoid.JumpPower = value end
end)

-- Infinite Jump fix (com debounce)
local InfiniteJumpEnabled = false
character:AddToggle("Infinite Jump", false, function(state)
    InfiniteJumpEnabled = state
end)

game:GetService("UserInputService").JumpRequest:Connect(function()
    if InfiniteJumpEnabled then
        local humanoid = game.Players.LocalPlayer.Character:FindFirstChildWhichIsA("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- Combat
local AutoKillEnabled = false
combat:AddToggle("Auto Kill", false, function(state)
    AutoKillEnabled = state
    task.spawn(function()
        while AutoKillEnabled do
            for _, enemy in ipairs(workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("Humanoid") then
                    enemy.Humanoid.Health = 0
                end
            end
            task.wait(0.2)
        end
    end)
end)

combat:AddToggle("One Hit Kill", false, function(state)
    if state then
        for _, tool in ipairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
            if tool:IsA("Tool") and tool:FindFirstChild("Handle") then
                tool.Handle.Touched:Connect(function(hit)
                    local hum = hit.Parent:FindFirstChild("Humanoid")
                    if hum then hum.Health = 0 end
                end)
            end
        end
    end
end)

combat:AddToggle("God Mode", false, function(state)
    if state then
        local humanoid = game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
        if humanoid then humanoid.Name = "God" end
    end
end)

-- Teleport
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
    local pos = {
        ["Base 1"] = Vector3.new(10, 10, 10),
        ["Base 2"] = Vector3.new(50, 10, 50),
        ["Base 3"] = Vector3.new(100, 10, 100)
    }
    if _G.SelectedBase then
        game.Players.LocalPlayer.Character:MoveTo(pos[_G.SelectedBase])
    end
end)

-- Visual
visual:AddToggle("No Fog", false, function(state)
    game:GetService("Lighting").FogEnd = state and 1e10 or 1000
end)

visual:AddToggle("Theme Switch", false, function(state)
    library:SetTheme(state and "Light" or "Dark")
end)

-- Config
config:AddKeybind("Toggle UI", Enum.KeyCode.RightShift, function()
    library:ToggleUI()
end)

config:AddButton("Unload UI", function()
    library:Destroy()
end)
