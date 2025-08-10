local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- =========================
-- 🎯 SISTEMA DE KEY
-- =========================
local keyCorrect = false
while not keyCorrect do
    local Key = Rayfield:Prompt({
        Title = "Brazuca God - Key System",
        Content = "Digite a Key para acessar o menu",
        Buttons = {"Confirmar"},
        Input = true
    })
    if Key == "GP" then
        keyCorrect = true
        Rayfield:Notify({
            Title = "Sucesso",
            Content = "Key correta! Bem-vindo ao Brazuca God 😎",
            Duration = 3
        })
    else
        Rayfield:Notify({
            Title = "Erro",
            Content = "Key incorreta! Tente novamente.",
            Duration = 3
        })
    end
end

-- =========================
-- 🎵 Função de Som
-- =========================
local function PlaySound()
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://6534947240" -- Som de clique
    sound.Volume = 3
    sound.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    sound:Play()
    game:GetService("Debris"):AddItem(sound, 3)
end

-- =========================
-- 🔥 Menu Principal
-- =========================
local Window = Rayfield:CreateWindow({
    Name = "Brazuca God",
    LoadingTitle = "Brazuca God",
    LoadingSubtitle = "By Você",
    DisableRayfieldPrompts = true, -- remove interface hidden
})

local Tab = Window:CreateTab("Funções", 4483362458)

local LocalPlayer = game.Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- ===== Speed Hack =====
local normalSpeed = 16
local rageSpeed = 200

Tab:CreateToggle({
    Name = "Speed Hack",
    CurrentValue = false,
    Callback = function(state)
        if state then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = 32
        else
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = normalSpeed
        end
    end
})

Tab:CreateToggle({
    Name = "Speed Hack Rage",
    CurrentValue = false,
    Callback = function(state)
        if state then
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = rageSpeed
        else
            LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = normalSpeed
        end
    end
})

-- ===== Infinite Jump =====
local infiniteJump = false
UserInputService.JumpRequest:Connect(function()
    if infiniteJump then
        LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

Tab:CreateToggle({
    Name = "Infinite Jump",
    CurrentValue = false,
    Callback = function(state)
        infiniteJump = state
    end
})

-- ===== Noclip =====
local noclip = false
RunService.Stepped:Connect(function()
    if noclip and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

Tab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Callback = function(state)
        noclip = state
    end
})

-- ===== Teleporte =====
local savedPosition = nil

Tab:CreateButton({
    Name = "Salvar Posição",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            savedPosition = char.HumanoidRootPart.Position
            Rayfield:Notify({
                Title = "Sucesso",
                Content = "Posição Salva com Sucesso!",
                Duration = 3
            })
            PlaySound()
        end
    end
})

Tab:CreateButton({
    Name = "Teleportar",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
            char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
            Rayfield:Notify({
                Title = "Info",
                Content = "Teleportando...",
                Duration = 3
            })
            PlaySound()
        end
    end
})