local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- Criar GUI principal
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 400)
frame.Position = UDim2.new(0, 20, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.4
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "GP7 MODS"
title.TextColor3 = Color3.fromRGB(0, 255, 0)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

-- Botão flutuante minimizar
local floatBtn = Instance.new("TextButton", gui)
floatBtn.Text = "GP7"
floatBtn.Size = UDim2.new(0, 60, 0, 35)
floatBtn.Position = UDim2.new(0, 10, 0, 10)
floatBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
floatBtn.TextColor3 = Color3.new(0, 0, 0)
floatBtn.Visible = false
floatBtn.Font = Enum.Font.GothamBold
floatBtn.TextScaled = true

floatBtn.MouseButton1Click:Connect(function()
    frame.Visible = true
    floatBtn.Visible = false
end)

-- Criar botões automáticos
local buttonY = 50 -- espaçamento vertical
local buttonIndex = 0
local function createButton(text, callback)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, 50 + (buttonIndex * buttonY))
    btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    btn.TextColor3 = Color3.fromRGB(0, 255, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    btn.Text = text
    btn.BorderSizePixel = 0
    btn.MouseButton1Click:Connect(function()
        callback(btn)
    end)
    buttonIndex += 1
    return btn
end

-- ===== SISTEMA DE VELOCIDADE =====
local speedMode = 0 -- 0 = normal, 1 = rápido, 2 = ultra
local SPEEDS = {16, 32, 200}

local function maintainSpeed()
    local character = LocalPlayer.Character
    if not character then return end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end
    humanoid.WalkSpeed = SPEEDS[speedMode + 1]
end
RunService.Heartbeat:Connect(maintainSpeed)

createButton("Velocidade: Normal", function(btn)
    speedMode = (speedMode + 1) % 3
    local nomes = {"Normal", "Rápida", "Ultra Rápida"}
    btn.Text = "Velocidade: " .. nomes[speedMode + 1]
end)

-- ===== INFINITE JUMP =====
local infiniteJumpOn = false
UserInputService.JumpRequest:Connect(function()
    if infiniteJumpOn then
        local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

createButton("Pulo Infinito OFF", function(btn)
    infiniteJumpOn = not infiniteJumpOn
    btn.Text = infiniteJumpOn and "Pulo Infinito ON" or "Pulo Infinito OFF"
end)

-- ===== TELEPORTE =====
local savedPosition = nil

createButton("Salvar Posição", function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        savedPosition = char.HumanoidRootPart.Position
        print("[Teleport] Posição salva:", savedPosition)
    end
end)

createButton("Teleportar", function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
        char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
        print("[Teleport] Teleportado!")
    else
        print("[Teleport] Nenhuma posição salva!")
    end
end)

-- ===== NOCLIP =====
local noclipOn = false
RunService.Stepped:Connect(function()
    if noclipOn and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

createButton("Noclip OFF", function(btn)
    noclipOn = not noclipOn
    btn.Text = noclipOn and "Noclip ON" or "Noclip OFF"
end)

-- ===== MINIMIZAR =====
createButton("Minimizar", function()
    frame.Visible = false
    floatBtn.Visible = true
end)