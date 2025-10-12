-- 🔰 GP7 MENU - Interface Bola Flutuante + Menu Arrastável
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Variáveis de controle
local currentSpeed = 16
local noClipEnabled = false
local infJumpEnabled = false
local savedPosition = nil

-- Referências do personagem
local function getCharacterRefs()
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local hum = char:WaitForChild("Humanoid")
    local root = char:WaitForChild("HumanoidRootPart")
    return char, hum, root
end

local Character, Humanoid, RootPart = getCharacterRefs()
LocalPlayer.CharacterAdded:Connect(function()
    Character, Humanoid, RootPart = getCharacterRefs()
    if Humanoid then
        Humanoid.WalkSpeed = currentSpeed
    end
end)

-- GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- 🔘 Botão flutuante circular
local floatBtn = Instance.new("TextButton", gui)
floatBtn.Size = UDim2.new(0, 70, 0, 70)
floatBtn.Position = UDim2.new(0, 100, 0, 200)
floatBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
floatBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
floatBtn.Text = "GP7"
floatBtn.Font = Enum.Font.GothamBlack
floatBtn.TextScaled = true
floatBtn.Active = true
floatBtn.Draggable = true
Instance.new("UICorner", floatBtn).CornerRadius = UDim.new(1, 0)

-- 🧭 Menu principal
local menuFrame = Instance.new("Frame", gui)
menuFrame.Size = UDim2.new(0, 240, 0, 300)
menuFrame.Position = UDim2.new(0, 90, 0, 180)
menuFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
menuFrame.BorderSizePixel = 0
menuFrame.Visible = false
menuFrame.Active = true
menuFrame.Draggable = true
Instance.new("UICorner", menuFrame).CornerRadius = UDim.new(0, 15)

-- Título
local title = Instance.new("TextLabel", menuFrame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "🧠 GP7 MENU"
title.TextColor3 = Color3.fromRGB(0, 255, 0)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.Font = Enum.Font.GothamBlack
title.TextScaled = true

-- Botão flutuante sempre visível
local floatBtn = Instance.new("TextButton", gui)
floatBtn.Size = UDim2.new(0, 80, 0, 30)
floatBtn.Position = UDim2.new(0, 100, 0, 100)
floatBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
floatBtn.TextColor3 = Color3.new(1, 1, 1)
floatBtn.Text = "GP7"
floatBtn.Font = Enum.Font.GothamBold
floatBtn.TextScaled = true
floatBtn.AutoButtonColor = true
floatBtn.Active = true
floatBtn.Draggable = true

-- Abrir/Fechar menu ao clicar
floatBtn.MouseButton1Click:Connect(function()
    menuFrame.Visible = not menuFrame.Visible
end)

-- Remove o botão minimizar completamente
-- menuFrame já pode ser fechado/aberto só com o floatBtn

-- Função criar botões
local function createButton(name, posY)
    local btn = Instance.new("TextButton", menuFrame)
    btn.Size = UDim2.new(0, 210, 0, 35)
    btn.Position = UDim2.new(0, 15, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = name
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
    return btn
end

-- ===== SPEED =====
local speedLabel = Instance.new("TextLabel", menuFrame)
speedLabel.Size = UDim2.new(0, 210, 0, 20)
speedLabel.Position = UDim2.new(0, 15, 0, 50)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Velocidade (16 - 200)"
speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedLabel.Font = Enum.Font.Gotham

local speedBox = Instance.new("TextBox", menuFrame)
speedBox.Size = UDim2.new(0, 210, 0, 30)
speedBox.Position = UDim2.new(0, 15, 0, 70)
speedBox.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
speedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBox.PlaceholderText = "Digite velocidade"
speedBox.Font = Enum.Font.GothamBold
speedBox.TextScaled = true
Instance.new("UICorner", speedBox).CornerRadius = UDim.new(0, 8)

speedBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local value = tonumber(speedBox.Text)
        if value and value >= 16 and value <= 200 then
            currentSpeed = value
            if Humanoid then Humanoid.WalkSpeed = currentSpeed end
        else
            speedBox.Text = "Inválido"
        end
    end
end)

RunService.RenderStepped:Connect(function()
    if Humanoid then Humanoid.WalkSpeed = currentSpeed end
end)

-- ===== INFINITE JUMP =====
local infJumpBtn = createButton("Pulo Infinito: OFF", 110)
infJumpBtn.MouseButton1Click:Connect(function()
    infJumpEnabled = not infJumpEnabled
    infJumpBtn.Text = "Pulo Infinito: " .. (infJumpEnabled and "ON" or "OFF")
end)

UserInputService.JumpRequest:Connect(function()
    if infJumpEnabled and Humanoid then
        Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- ===== NOCLIP REFEITO =====
local noClipEnabled = false
local noClipBtn = createButton("No Clip: OFF", 155)

noClipBtn.MouseButton1Click:Connect(function()
    noClipEnabled = not noClipEnabled
    noClipBtn.Text = "No Clip: " .. (noClipEnabled and "ON" or "OFF")

    local char = game.Players.LocalPlayer.Character
    if not char then return end

    -- quando desativar, reativa colisão normalmente
    if not noClipEnabled then
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)

-- sistema mais leve e preciso (não causa flutuação)
task.spawn(function()
    local runService = game:GetService("RunService")
    while task.wait() do
        if noClipEnabled then
            local char = game.Players.LocalPlayer.Character
            if char then
                for _, part in ipairs(char:GetDescendants()) do
                    if part:IsA("BasePart") and part.CanCollide then
                        part.CanCollide = false
                    end
                end
            end
        end
    end
end)

-- ===== SALVAR POSIÇÃO =====
local savePosBtn = createButton("Salvar Posição", 200)
savePosBtn.MouseButton1Click:Connect(function()
    if RootPart then savedPosition = RootPart.CFrame end
end)

-- ===== TELEPORTAR =====
local teleportBtn = createButton("Teleportar", 245)
teleportBtn.MouseButton1Click:Connect(function()
    if RootPart and savedPosition then
        RootPart.CFrame = savedPosition
    end
end)