-- GP7 MENU - Interface Fluent UI Verde e Preto (versão otimizada)
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

-- Atualizar referências após respawn e reaplicar WalkSpeed
LocalPlayer.CharacterAdded:Connect(function()
    Character, Humanoid, RootPart = getCharacterRefs()
    if Humanoid then
        Humanoid.WalkSpeed = currentSpeed
    end
end)

-- Criar GUI principal
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

-- Botão flutuante arrastável
local floatButton = Instance.new("TextButton", gui)
floatButton.Size = UDim2.new(0, 80, 0, 30)
floatButton.Position = UDim2.new(0, 100, 0, 100)
floatButton.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
floatButton.TextColor3 = Color3.new(1, 1, 1)
floatButton.Text = "GP7 MENU"
floatButton.AutoButtonColor = true
floatButton.Active = true
floatButton.Draggable = true

-- Menu principal
local menuFrame = Instance.new("Frame", gui)
menuFrame.Size = UDim2.new(0, 220, 0, 250)
menuFrame.Position = UDim2.new(0, 100, 0, 140)
menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
menuFrame.Visible = false
Instance.new("UICorner", menuFrame).CornerRadius = UDim.new(0, 12)

floatButton.MouseButton1Click:Connect(function()
    menuFrame.Visible = not menuFrame.Visible
end)

-- Função para criar botões
local function createButton(name, posY)
    local button = Instance.new("TextButton", menuFrame)
    button.Size = UDim2.new(0, 200, 0, 30)
    button.Position = UDim2.new(0, 10, 0, posY)
    button.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Text = name
    button.AutoButtonColor = true
    Instance.new("UICorner", button).CornerRadius = UDim.new(0, 8)
    return button
end

-- ===== SPEED HACK =====
local speedLabel = Instance.new("TextLabel", menuFrame)
speedLabel.Size = UDim2.new(0, 200, 0, 20)
speedLabel.Position = UDim2.new(0, 10, 0, 10)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Velocidade (16 - 200)"
speedLabel.TextColor3 = Color3.new(1, 1, 1)

local speedBox = Instance.new("TextBox", menuFrame)
speedBox.Size = UDim2.new(0, 200, 0, 30)
speedBox.Position = UDim2.new(0, 10, 0, 30)
speedBox.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
speedBox.TextColor3 = Color3.new(1, 1, 1)
speedBox.PlaceholderText = "Digite a velocidade"

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

-- ===== PULO INFINITO =====
local infJumpBtn = createButton("Pulo Infinito: OFF", 70)
infJumpBtn.MouseButton1Click:Connect(function()
    infJumpEnabled = not infJumpEnabled
    infJumpBtn.Text = "Pulo Infinito: " .. (infJumpEnabled and "ON" or "OFF")
end)

UserInputService.JumpRequest:Connect(function()
    if infJumpEnabled and Humanoid then
        Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- ===== NO CLIP COM RAYCAST =====
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local noClipEnabled = false
local noClipBtn = createButton("No Clip: OFF", 110)

noClipBtn.MouseButton1Click:Connect(function(btn)
    noClipEnabled = not noClipEnabled
    btn.Text = "No Clip: " .. (noClipEnabled and "ON" or "OFF")
end)

RunService.Stepped:Connect(function()
    if noClipEnabled and Character then
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then
                -- Raycast para verificar se há chão abaixo do personagem
                local rayOrigin = part.Position
                local rayDirection = Vector3.new(0, -5, 0) -- verifica 5 studs para baixo
                local raycastParams = RaycastParams.new()
                raycastParams.FilterDescendantsInstances = {Character}
                raycastParams.FilterType = Enum.RaycastFilterType.Blacklist

                local result = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
                
                if result then
                    -- Se houver chão, mantém colisão
                    part.CanCollide = true
                else
                    -- Se não houver chão, desativa colisão (atravessa paredes)
                    part.CanCollide = false
                end
            end
        end
    elseif Character then
        -- NoClip desativado → tudo colide normalmente
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)

-- ===== SALVAR POSIÇÃO =====
local savePosBtn = createButton("Salvar Posição", 150)
savePosBtn.MouseButton1Click:Connect(function()
    if RootPart then savedPosition = RootPart.CFrame end
end)

-- ===== TELEPORTAR =====
local teleportBtn = createButton("Teleportar", 190)
teleportBtn.MouseButton1Click:Connect(function()
    if RootPart and savedPosition then
        RootPart.CFrame = savedPosition
    end
end)