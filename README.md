local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

print("[Brazuca God] Iniciando script...")

local Window = Rayfield:CreateWindow({
    Name = "Brazuca God",
    LoadingTitle = "Brazuca God",
    LoadingSubtitle = "by GP7",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "BrazucaGod", 
        FileName = "Config"
    },
    Discord = {
        Enabled = false,
        Invite = "", 
        RememberJoins = true 
    }
})

-- Estados iniciais
local speedMode = 0 -- 0 = normal, 1 = rápido, 2 = ultra
local SPEEDS = {16, 32, 200}
local infiniteJumpOn = false
local noclipOn = false
local savedPosition = nil

-- Função para manter a velocidade atualizada
local function maintainSpeed()
    local character = LocalPlayer.Character
    if not character then return end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end
    humanoid.WalkSpeed = SPEEDS[speedMode + 1]
end
RunService.Heartbeat:Connect(maintainSpeed)

local MainTab = Window:CreateTab("Main")

-- Toggle velocidade
MainTab:CreateToggle({
    Name = "Velocidade Ultra",
    CurrentValue = false,
    Flag = "UltraSpeed",
    Callback = function(value)
        if value then
            speedMode = 2
        else
            speedMode = 0
        end
        print("[Brazuca God] Velocidade alterada para: " .. (value and "Ultra Rápida" or "Normal"))
    end,
})

-- Toggle velocidade rápida (se quiser um toggle separado para a rápida)
MainTab:CreateToggle({
    Name = "Velocidade Rápida",
    CurrentValue = false,
    Flag = "FastSpeed",
    Callback = function(value)
        if value then
            speedMode = 1
        else
            speedMode = 0
        end
        print("[Brazuca God] Velocidade alterada para: " .. (value and "Rápida" or "Normal"))
    end,
})

-- Toggle pulo infinito
MainTab:CreateToggle({
    Name = "Pulo Infinito",
    CurrentValue = false,
    Flag = "InfiniteJump",
    Callback = function(value)
        infiniteJumpOn = value
        print("[Brazuca God] Pulo Infinito: " .. (value and "ON" or "OFF"))
    end,
})

UserInputService.JumpRequest:Connect(function()
    if infiniteJumpOn then
        local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

-- Toggle noclip
MainTab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "Noclip",
    Callback = function(value)
        noclipOn = value
        print("[Brazuca God] Noclip: " .. (value and "ON" or "OFF"))
    end,
})

RunService.Stepped:Connect(function()
    if LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = not noclipOn
            end
        end
    end
end)

-- Botão para salvar posição
MainTab:CreateButton({
    Name = "Salvar Posição",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            savedPosition = char.HumanoidRootPart.Position
            print("[Brazuca God] Posição salva:", savedPosition)
        else
            print("[Brazuca God] Personagem não encontrado para salvar posição.")
        end
    end,
})

-- Botão para teleportar para posição salva
MainTab:CreateButton({
    Name = "Teleportar para Posição Salva",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
            char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
            print("[Brazuca God] Teleportado para posição salva!")
        else
            print("[Brazuca God] Nenhuma posição salva para teleportar!")
        end
    end,
})

-- Toggle para minimizar o menu
MainTab:CreateToggle({
    Name = "Minimizar Menu",
    CurrentValue = false,
    Flag = "MinimizeToggle",
    Callback = function(value)
        Window:Toggle()
        print("[Brazuca God] Menu minimizado/aberto")
    end,
})

print("[Brazuca God] Script carregado com sucesso!")