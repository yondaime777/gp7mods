local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui", PlayerGui)

-- Criar o menu principal
local menuFrame = Instance.new("Frame", screenGui)
menuFrame.Size = UDim2.new(0.3, 0, 0.45, 0)
menuFrame.Position = UDim2.new(0.35, 0, 0.3, 0)
menuFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
menuFrame.Visible = true

-- Botão de Minimizar
local minimizeButton = Instance.new("TextButton", menuFrame)
minimizeButton.Size = UDim2.new(0.3, 0, 0.1, 0)
minimizeButton.Position = UDim2.new(0.35, 0, 0, 0)
minimizeButton.Text = "Minimizar"
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

-- Bolinha flutuante (quando minimiza)
local floatBall = Instance.new("TextButton", screenGui)
floatBall.Size = UDim2.new(0, 50, 0, 50)
floatBall.Position = UDim2.new(0.9, 0, 0.1, 0)
floatBall.Text = "☰"
floatBall.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
floatBall.Visible = false

-- Minimizar e restaurar
minimizeButton.MouseButton1Click:Connect(function()
    menuFrame.Visible = false
    floatBall.Visible = true
end)

floatBall.MouseButton1Click:Connect(function()
    menuFrame.Visible = true
    floatBall.Visible = false
end)

-- Caixa de entrada para velocidade
local speedTextBox = Instance.new("TextBox", menuFrame)
speedTextBox.Size = UDim2.new(0.3, 0, 0.1, 0)
speedTextBox.Position = UDim2.new(0.35, 0, 0.15, 0)
speedTextBox.PlaceholderText = "Velocidade (16-200)"
speedTextBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

-- Botão para atualizar a velocidade
local setSpeedButton = Instance.new("TextButton", menuFrame)
setSpeedButton.Size = UDim2.new(0.3, 0, 0.1, 0)
setSpeedButton.Position = UDim2.new(0.35, 0, 0.28, 0)
setSpeedButton.Text = "Definir Velocidade"
setSpeedButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)

setSpeedButton.MouseButton1Click:Connect(function()
    local speed = tonumber(speedTextBox.Text)
    if speed and speed >= 16 and speed <= 200 then
        Player.Character.Humanoid.WalkSpeed = speed
    else
        warn("Por favor, insira um valor entre 16 e 200.")
    end
end)

-- Pulo Infinito
local jumpInfiniteButton = Instance.new("TextButton", menuFrame)
jumpInfiniteButton.Size = UDim2.new(0.3, 0, 0.1, 0)
jumpInfiniteButton.Position = UDim2.new(0.35, 0, 0.41, 0)
jumpInfiniteButton.Text = "Ativar Pulo Infinito"
jumpInfiniteButton.BackgroundColor3 = Color3.fromRGB(0, 0, 255)

local jumpInfinite = false
jumpInfiniteButton.MouseButton1Click:Connect(function()
    jumpInfinite = not jumpInfinite
    jumpInfiniteButton.Text = jumpInfinite and "Desativar Pulo Infinito" or "Ativar Pulo Infinito"
end)

game:GetService("UserInputService").JumpRequest:Connect(function()
    if jumpInfinite and Player.Character and Player.Character:FindFirstChild("Humanoid") then
        Player.Character.Humanoid:ChangeState("Jumping")
    end
end)

-- NoClip
local noClipButton = Instance.new("TextButton", menuFrame)
noClipButton.Size = UDim2.new(0.3, 0, 0.1, 0)
noClipButton.Position = UDim2.new(0.35, 0, 0.54, 0)
noClipButton.Text = "Ativar NoClip"
noClipButton.BackgroundColor3 = Color3.fromRGB(255, 128, 0)

local noclip = false
noClipButton.MouseButton1Click:Connect(function()
    noclip = not noclip
    noClipButton.Text = noclip and "Desativar NoClip" or "Ativar NoClip"
end)

game:GetService("RunService").Stepped:Connect(function()
    if noclip and Player.Character and Player.Character:FindFirstChild("Humanoid") then
        for _, part in pairs(Player.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)

-- Salvar Posição
local savedPosition
local savePosButton = Instance.new("TextButton", menuFrame)
savePosButton.Size = UDim2.new(0.3, 0, 0.1, 0)
savePosButton.Position = UDim2.new(0.35, 0, 0.67, 0)
savePosButton.Text = "Salvar Posição"
savePosButton.BackgroundColor3 = Color3.fromRGB(128, 255, 128)

savePosButton.MouseButton1Click:Connect(function()
    if Player.Character and Player.Character.PrimaryPart then
        savedPosition = Player.Character.PrimaryPart.CFrame
        warn("Posição salva!")
    end
end)

-- Voltar Posição
local loadPosButton = Instance.new("TextButton", menuFrame)
loadPosButton.Size = UDim2.new(0.3, 0, 0.1, 0)
loadPosButton.Position = UDim2.new(0.35, 0, 0.80, 0)
loadPosButton.Text = "Voltar Posição"
loadPosButton.BackgroundColor3 = Color3.fromRGB(255, 255, 128)

loadPosButton.MouseButton1Click:Connect(function()
    if savedPosition and Player.Character and Player.Character.PrimaryPart then
        Player.Character:SetPrimaryPartCFrame(savedPosition)
    else
        warn("Nenhuma posição salva!")
    end
end)