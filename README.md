local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local speedEnabled = false
local jumpEnabled = false

local normalSpeed = 16
local speedValue = 40

-- Função para atualizar referência do personagem e humanoid
local function onCharacterAdded(char)
    character = char
    humanoid = char:WaitForChild("Humanoid")
end
player.CharacterAdded:Connect(onCharacterAdded)

-- Reforça o speed hack constantemente
RunService.Heartbeat:Connect(function()
    if humanoid then
        if speedEnabled then
            if humanoid.WalkSpeed ~= speedValue then
                humanoid.WalkSpeed = speedValue
            end
        else
            if humanoid.WalkSpeed ~= normalSpeed then
                humanoid.WalkSpeed = normalSpeed
            end
        end
    end
end)

-- Infinite jump usando evento de tecla para simular múltiplos pulos
local canJump = true

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if jumpEnabled and input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode == Enum.KeyCode.Space then
        if canJump and humanoid and humanoid.Health > 0 then
            -- Simula um pulo 'real' com delay pra evitar travar
            canJump = false
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            wait(0.2)
            canJump = true
        end
    end
end)

-- GUI básico pra ligar/desligar

local playerGui = player:WaitForChild("PlayerGui")
local screenGui = Instance.new("ScreenGui", playerGui)
screenGui.Name = "GP7MODS"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 200, 0, 120)
frame.Position = UDim2.new(0.5, -100, 0.5, -60)
frame.BackgroundColor3 = Color3.fromRGB(0, 50, 0)
frame.BorderColor3 = Color3.fromRGB(0, 255, 0)
frame.Active = true
frame.Draggable = true

local speedBtn = Instance.new("TextButton", frame)
speedBtn.Size = UDim2.new(0, 180, 0, 40)
speedBtn.Position = UDim2.new(0, 10, 0, 10)
speedBtn.Text = "Speed Hack: OFF"
speedBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
speedBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
speedBtn.Font = Enum.Font.GothamBold
speedBtn.TextSize = 20

local jumpBtn = Instance.new("TextButton", frame)
jumpBtn.Size = UDim2.new(0, 180, 0, 40)
jumpBtn.Position = UDim2.new(0, 10, 0, 65)
jumpBtn.Text = "Infinite Jump: OFF"
jumpBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
jumpBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
jumpBtn.Font = Enum.Font.GothamBold
jumpBtn.TextSize = 20

speedBtn.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    speedBtn.Text = "Speed Hack: " .. (speedEnabled and "ON" or "OFF")
end)

jumpBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    jumpBtn.Text = "Infinite Jump: " .. (jumpEnabled and "ON" or "OFF")
end)