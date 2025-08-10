local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

local Window = Rayfield:CreateWindow({
    Name = "Brazuca God",
    LoadingTitle = "Carregando Brazuca God",
    LoadingSubtitle = "by GP7",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "ConfigBrazucaGod"
    }
})

local Tab = Window:CreateTab("Principal", 4483362458)

-- Variáveis internas
local speedMode = 0 -- 0=normal,1=rápido,2=ultra
local SPEEDS = {16, 32, 200}
local infiniteJumpOn = false
local noclipOn = false
local savedPosition = nil

-- Função para manter velocidade
local function maintainSpeed()
    local character = LocalPlayer.Character
    if not character then return end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end
    humanoid.WalkSpeed = SPEEDS[speedMode + 1]
end
RunService.Heartbeat:Connect(maintainSpeed)

-- Botão Toggle: Velocidade
local SpeedToggle = Tab:CreateToggle({
    Name = "Speed Mode",
    CurrentValue = false,
    Flag = "SpeedToggle",
    Callback = function(enabled)
        if enabled then
            speedMode = (speedMode + 1) % 3
            print("[BrazucaGod] Velocidade alterada para modo " .. speedMode)
        else
            speedMode = 0
            print("[BrazucaGod] Velocidade normal ativada")
        end
    end
})

-- Botão Toggle: Pulo Infinito
local InfiniteJumpToggle = Tab:CreateToggle({
    Name = "Pulo Infinito",
    CurrentValue = false,
    Flag = "InfiniteJumpToggle",
    Callback = function(enabled)
        infiniteJumpOn = enabled
        print("[BrazucaGod] Pulo Infinito: " .. (enabled and "ON" or "OFF"))
    end
})

UserInputService.JumpRequest:Connect(function()
    if infiniteJumpOn then
        local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- Botão Toggle: Noclip
local NoclipToggle = Tab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "NoclipToggle",
    Callback = function(enabled)
        noclipOn = enabled
        print("[BrazucaGod] Noclip: " .. (enabled and "ON" or "OFF"))
    end
})

RunService.Stepped:Connect(function()
    if noclipOn and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    elseif LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)

-- Botão para salvar posição
local SavePosButton = Tab:CreateButton({
    Name = "Salvar Posição",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            savedPosition = char.HumanoidRootPart.Position
            print("[BrazucaGod] Posição salva:", savedPosition)
        else
            print("[BrazucaGod] Personagem não encontrado para salvar posição.")
        end
    end
})

-- Botão para teleportar para posição salva
local TeleportButton = Tab:CreateButton({
    Name = "Teleportar para Posição Salva",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
            char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
            print("[BrazucaGod] Teleportado para posição salva!")
        else
            print("[BrazucaGod] Nenhuma posição salva para teleportar!")
        end
    end
})

-- Botão para minimizar menu
local MinimizeButton = Tab:CreateButton({
    Name = "Minimizar Menu",
    Callback = function()
        Window:Toggle()
        print("[BrazucaGod] Menu minimizado/aberto.")
    end
})

print("[BrazucaGod] Script carregado!")