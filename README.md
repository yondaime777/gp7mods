local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local RunService = game:GetService("RunService")

-- Configurações iniciais
local speedEnabled = false
local jumpEnabled = false
local walkSpeedNormal = 16
local jumpPowerNormal = 50
local walkSpeedCurrent = walkSpeedNormal
local jumpPowerBoost = 150

local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Atualiza humanoid se personagem renascer
player.CharacterAdded:Connect(function(char)
    character = char
    humanoid = character:WaitForChild("Humanoid")
    humanoid.WalkSpeed = speedEnabled and walkSpeedCurrent or walkSpeedNormal
    humanoid.JumpPower = jumpEnabled and jumpPowerBoost or jumpPowerNormal
end)

-- Criar GUI

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GP7Menu"
screenGui.Parent = playerGui
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 170)
frame.Position = UDim2.new(0, 10, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
frame.BorderSizePixel = 0
frame.Parent = screenGui
frame.Active = true
frame.Draggable = true

-- Título
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "GP7 MENU"
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextColor3 = Color3.fromRGB(255, 50, 50)
title.TextStrokeColor3 = Color3.new(0,0,0)
title.TextStrokeTransparency = 0.5
title.TextWrapped = true

-- Speed Hack Label
local speedLabel = Instance.new("TextLabel", frame)
speedLabel.Size = UDim2.new(1, -20, 0, 25)
speedLabel.Position = UDim2.new(0, 10, 0, 40)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Speed Hack: OFF"
speedLabel.Font = Enum.Font.Gotham
speedLabel.TextSize = 18
speedLabel.TextColor3 = Color3.fromRGB(255, 100, 100)

-- Speed Hack Slider
local sliderFrame = Instance.new("Frame", frame)
sliderFrame.Size = UDim2.new(0, 200, 0, 20)
sliderFrame.Position = UDim2.new(0, 10, 0, 70)
sliderFrame.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
sliderFrame.BorderSizePixel = 0

local sliderBar = Instance.new("Frame", sliderFrame)
sliderBar.Size = UDim2.new(0.3, 0, 1, 0) -- inicial 30% (walkSpeedCurrent=walkSpeedNormal)
sliderBar.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
sliderBar.BorderSizePixel = 0

local dragging = false

sliderBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)

sliderBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

sliderFrame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local relativeX = math.clamp(input.Position.X - sliderFrame.AbsolutePosition.X, 0, sliderFrame.AbsoluteSize.X)
        local percent = relativeX / sliderFrame.AbsoluteSize.X
        sliderBar.Size = UDim2.new(percent, 0, 1, 0)
        walkSpeedCurrent = math.floor(16 + (84 * percent)) -- speed entre 16 e 100
        if speedEnabled then
            humanoid.WalkSpeed = walkSpeedCurrent
        end
    end
end)

-- Botão Pulo Infinito
local jumpBtn = Instance.new("TextButton", frame)
jumpBtn.Size = UDim2.new(0, 200, 0, 40)
jumpBtn.Position = UDim2.new(0, 10, 0, 100)
jumpBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
jumpBtn.TextColor3 = Color3.new(1,1,1)
jumpBtn.Font = Enum.Font.GothamBold
jumpBtn.TextSize = 18
jumpBtn.Text = "Ativar Pulo Infinito"

jumpBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    if jumpEnabled then
        jumpBtn.Text = "Desativar Pulo Infinito"
        humanoid.JumpPower = jumpPowerBoost
    else
        jumpBtn.Text = "Ativar Pulo Infinito"
        humanoid.JumpPower = jumpPowerNormal
    end
end)

-- Botão ligar/desligar Speed Hack
local speedToggleBtn = Instance.new("TextButton", frame)
speedToggleBtn.Size = UDim2.new(0, 200, 0, 40)
speedToggleBtn.Position = UDim2.new(0, 10, 0, 140)
speedToggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
speedToggleBtn.TextColor3 = Color3.new(1,1,1)
speedToggleBtn.Font = Enum.Font.GothamBold
speedToggleBtn.TextSize = 18
speedToggleBtn.Text = "Ativar Speed Hack"

speedToggleBtn.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    if speedEnabled then
        speedToggleBtn.Text = "Desativar Speed Hack"
        speedLabel.Text = ("Speed Hack: ON (%d)"):format(walkSpeedCurrent)
        humanoid.WalkSpeed = walkSpeedCurrent
    else
        speedToggleBtn.Text = "Ativar Speed Hack"
        speedLabel.Text = "Speed Hack: OFF"
        humanoid.WalkSpeed = walkSpeedNormal
    end
end)

-- Botão Minimizar
local minimizeBtn = Instance.new("TextButton", screenGui)
minimizeBtn.Size = UDim2.new(0, 40, 0, 40)
minimizeBtn.Position = UDim2.new(0, 10, 0.3, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
minimizeBtn.TextColor3 = Color3.fromRGB(255, 50, 50)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.Text = "≡"
minimizeBtn.Visible = false

minimizeBtn.MouseButton1Click:Connect(function()
    frame.Visible = true
    minimizeBtn.Visible = false
end)

local toggleMinimized = false
local function minimize()
    frame.Visible = false
    minimizeBtn.Visible = true
end

-- Botão para minimizar o menu
local minimizeButtonInFrame = Instance.new("TextButton", frame)
minimizeButtonInFrame.Size = UDim2.new(0, 30, 0, 30)
minimizeButtonInFrame.Position = UDim2.new(1, -35, 0, 2)
minimizeButtonInFrame.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
minimizeButtonInFrame.TextColor3 = Color3.new(1,1,1)
minimizeButtonInFrame.Font = Enum.Font.GothamBold
minimizeButtonInFrame.TextSize = 18
minimizeButtonInFrame.Text = "_"

minimizeButtonInFrame.MouseButton1Click:Connect(function()
    minimize()
end)