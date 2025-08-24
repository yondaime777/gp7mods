local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

print("[GP7Menu] Iniciando script...")

-- Criar GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 250, 0, 420)
frame.Position = UDim2.new(0, 20, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.4
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Visible = true

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
local buttonY = 50
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

-- ===== SLIDER DE VELOCIDADE =====
local speedValue = 16
local sliderFrame = Instance.new("Frame", frame)
sliderFrame.Size = UDim2.new(1, -20, 0, 60)
sliderFrame.Position = UDim2.new(0, 10, 0, 50 + (buttonIndex * buttonY))
sliderFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
sliderFrame.BorderSizePixel = 0

local sliderTitle = Instance.new("TextLabel", sliderFrame)
sliderTitle.Size = UDim2.new(1, 0, 0, 20)
sliderTitle.Text = "Velocidade: " .. speedValue
sliderTitle.TextColor3 = Color3.fromRGB(0, 255, 0)
sliderTitle.BackgroundTransparency = 1
sliderTitle.Font = Enum.Font.GothamBold
sliderTitle.TextScaled = true

local sliderBar = Instance.new("Frame", sliderFrame)
sliderBar.Size = UDim2.new(1, -20, 0, 10)
sliderBar.Position = UDim2.new(0, 10, 0, 35)
sliderBar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
sliderBar.BorderSizePixel = 0

local sliderButton = Instance.new("Frame", sliderBar)
sliderButton.Size = UDim2.new(0, 10, 1, 0)
sliderButton.Position = UDim2.new(0, 0, 0, 0)
sliderButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
sliderButton.BorderSizePixel = 0

local dragging = false
sliderBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local pos = math.clamp((input.Position.X - sliderBar.AbsolutePosition.X) / sliderBar.AbsoluteSize.X, 0, 1)
        sliderButton.Position = UDim2.new(pos, -5, 0, 0)
        speedValue = math.floor(16 + (200 - 16) * pos)
        sliderTitle.Text = "Velocidade: " .. speedValue
    end
end)

buttonIndex += 1

RunService.Heartbeat:Connect(function()
    local character = LocalPlayer.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = speedValue
        end
    end
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
    end
end)

createButton("Teleportar", function()
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
        char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
    end
end)

-- ===== NOCLIP (SEM ATRAVESSAR PRA BAIXO) =====
local noclipOn = false
RunService.Stepped:Connect(function()
    if LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                if noclipOn then
                    if part.Name ~= "HumanoidRootPart" then
                        part.CanCollide = false
                    end
                else
                    part.CanCollide = true
                end
            end
        end
    end
end)

-- Impede de cair pra baixo (mantém colisão do chão)
RunService.Heartbeat:Connect(function()
    if noclipOn and LocalPlayer.Character then
        local hrp = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if hrp and hrp.Velocity.Y < 0 then
            hrp.Velocity = Vector3.new(hrp.Velocity.X, 0, hrp.Velocity.Z)
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

print("[GP7Menu] Script carregado com sucesso!")