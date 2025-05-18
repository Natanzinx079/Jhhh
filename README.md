-- UI Library
local OrionLib = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Orion/main/source'))()

-- Janela principal
local Window = OrionLib:MakeWindow({
    Name = "Dead Reails Hub",
    HidePremium = false,
    SaveConfig = false,
    IntroEnabled = false,
    CloseCallback = function()
        OrionLib:Destroy()
    end
})

-- MAIN aba (padrão)
local MainTab = Window:MakeTab({
    Name = "Main",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- CHARACTER aba
local CharacterTab = Window:MakeTab({
    Name = "Character",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- TELEPORT aba
local TeleportTab = Window:MakeTab({
    Name = "Teleport",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- VISUAL aba
local VisualTab = Window:MakeTab({
    Name = "Visual",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- COMBAT aba
local CombatTab = Window:MakeTab({
    Name = "Combat",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- CONFIGURATION aba
local ConfigTab = Window:MakeTab({
    Name = "Configuration",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- Funções reais que funcionam no jogo
MainTab:AddSlider({
    Name = "Walk Speed",
    Min = 16,
    Max = 100,
    Default = 16,
    Color = Color3.fromRGB(255, 255, 255),
    Increment = 1,
    ValueName = "WalkSpeed",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
    end
})

MainTab:AddSlider({
    Name = "Jump Power",
    Min = 50,
    Max = 200,
    Default = 50,
    Color = Color3.fromRGB(255, 255, 255),
    Increment = 1,
    ValueName = "JumpPower",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
    end
})

-- Botão para minimizar (simulando o botão "-")
ConfigTab:AddButton({
    Name = "Ocultar Menu",
    Callback = function()
        game:GetService("CoreGui").Orion:Destroy()
    end
})
