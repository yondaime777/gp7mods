-- GP7 MENU - Interface Fluent UI Verde e Preto
-- Mantendo todas as funções originais

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")

-- Variáveis de controle
local noClipEnabled = false
local infJumpEnabled = false
local savedPosition = nil

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

local uiCorner = Instance.new("UICorner", menuFrame)
uiCorner.CornerRadius = UDim.new(0, 12)

-- Botão para abrir/fechar menu
floatButton.MouseButton1Click:Connect(function()
    menuFrame.Visible = not menuFrame.Visible
end)

-- Função para criar botões padrão
local function createButton(name, posY)
    local button = Instance.new("TextButton", menuFrame)
    button.Size = UDim2.new(0, 200, 0, 30)
    button.Position = UDim2.new(0, 10, 0, posY)
    button.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Text = name
    button.AutoButtonColor = true
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 8)
    return button
end

-- Speed com TextBox
local speedLabel = Instance.new("TextLabel", menuFrame)
speedLabel.Size = UDim2.new(0, 200, 0, 20)
speedLabel.Position = UDim2.new(0, 10, 0, 170)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Velocidade (16 - 200)"
speedLabel.TextColor3 = Color3.new(1, 1, 1)

local speedBox = Instance.new("TextBox", menuFrame)
speedBox.Size = UDim2.new(0, 200, 0, 30)
speedBox.Position = UDim2.new(0, 10, 0, 190)
speedBox.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
speedBox.TextColor3 = Color3.new(1, 1, 1)
speedBox.PlaceholderText = "Digite a velocidade"
speedBox.Text = ""

speedBox.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        local value = tonumber(speedBox.Text)
        if value and value >= 16 and value <= 200 then
            Humanoid.WalkSpeed = value
        else
            speedBox.Text = "Inválido"
        end
    end
end)

-- Pulo Infinito ON/OFF
local infJumpBtn = createButton("Pulo Infinito: OFF", 50)
infJumpBtn.MouseButton1Click:Connect(function()
    infJumpEnabled = not infJumpEnabled
    infJumpBtn.Text = "Pulo Infinito: " .. (infJumpEnabled and "ON" or "OFF")
end)

UserInputService.JumpRequest:Connect(function()
    if infJumpEnabled and Humanoid then
        Humanoid:ChangeState("Jumping")
    end
end)

-- No Clip ON/OFF
local noClipBtn = createButton("No Clip: OFF", 10)
noClipBtn.MouseButton1Click:Connect(function()
    noClipEnabled = not noClipEnabled
    noClipBtn.Text = "No Clip: " .. (noClipEnabled and "ON" or "OFF")
end)

RunService.Stepped:Connect(function()
    if noClipEnabled and Character then
        for _, part in pairs(Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

-- Salvar Posição
local savePosBtn = createButton("Salvar Posição", 90)
savePosBtn.MouseButton1Click:Connect(function()
    if RootPart then
        savedPosition = RootPart.CFrame
    end
end)

-- Teleportar
local teleportBtn = createButton("Teleportar", 130)
teleportBtn.MouseButton1Click:Connect(function()
    if RootPart and savedPosition then
        RootPart.CFrame = savedPosition
    end
end)