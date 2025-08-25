local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Criar um ScreenGui
local screenGui = Instance.new("ScreenGui", PlayerGui)

-- Criar um Frame para o menu
local menuFrame = Instance.new("Frame", screenGui)
menuFrame.Size = UDim2.new(0.3, 0, 0.4, 0)
menuFrame.Position = UDim2.new(0.35, 0, 0.3, 0)
menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
menuFrame.Visible = true

-- Botão de Minimize
local minimizeButton = Instance.new("TextButton", menuFrame)
minimizeButton.Size = UDim2.new(0.3, 0, 0.1, 0)
minimizeButton.Position = UDim2.new(0.35, 0, 0, 0)
minimizeButton.Text = "Minimizar"
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

-- Função para minimizar o menu
minimizeButton.MouseButton1Click:Connect(function()
    menuFrame.Visible = not menuFrame.Visible
end)

-- Caixa de entrada para velocidade
local speedTextBox = Instance.new("TextBox", menuFrame)
speedTextBox.Size = UDim2.new(0.3, 0, 0.1, 0)
speedTextBox.Position = UDim2.new(0.35, 0, 0.2, 0)
speedTextBox.PlaceholderText = "Velocidade (16-200)"
speedTextBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

-- Botão para atualizar a velocidade
local setSpeedButton = Instance.new("TextButton", menuFrame)
setSpeedButton.Size = UDim2.new(0.3, 0, 0.1, 0)
setSpeedButton.Position = UDim2.new(0.35, 0, 0.35, 0)
setSpeedButton.Text = "Definir Velocidade"
setSpeedButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

-- Função para mudar a velocidade
setSpeedButton.MouseButton1Click:Connect(function()
    local speed = tonumber(speedTextBox.Text)
    if speed and speed >= 16 and speed <= 200 then
        Player.Character.Humanoid.WalkSpeed = speed
    else
        warn("Por favor, insira um valor de velocidade entre 16 e 200.")
    end
end)

-- Função para Pulo Infinito
local jumpInfiniteButton = Instance.new("TextButton", menuFrame)
jumpInfiniteButton.Size = UDim2.new(0.3, 0, 0.1, 0)
jumpInfiniteButton.Position = UDim2.new(0.35, 0, 0.5, 0)
jumpInfiniteButton.Text = "Pulo Infinito"
jumpInfiniteButton.BackgroundColor3 = Color3.fromRGB(0, 0, 255)

jumpInfiniteButton.MouseButton1Click:Connect(function()
    Player.Character.Humanoid.JumpPower = 100  -- Ajuste o pulo para ser maior
end)

-- Função de Teleportação
local teleportButton = Instance.new("TextButton", menuFrame)
teleportButton.Size = UDim2.new(0.3, 0, 0.1, 0)
teleportButton.Position = UDim2.new(0.35, 0, 0.65, 0)
teleportButton.Text = "Teletransportar"
teleportButton.BackgroundColor3 = Color3.fromRGB(0, 255, 255)

teleportButton.MouseButton1Click:Connect(function()
    -- Teleportar para uma posição específica (por exemplo, 0, 10, 0)
    Player.Character:SetPrimaryPartCFrame(CFrame.new(0, 10, 0))
end)