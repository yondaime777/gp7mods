local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local humanoid = nil
local character = player.Character or player.CharacterAdded:Wait()

local speedEnabled = false
local jumpEnabled = false

local normalSpeed = 16
local speedValue = 40

local function onCharacterAdded(char)
    character = char
    humanoid = char:WaitForChild("Humanoid")
end

onCharacterAdded(character)
player.CharacterAdded:Connect(onCharacterAdded)

-- Speed hack constante
RunService.Heartbeat:Connect(function()
    if humanoid then
        if speedEnabled then
            humanoid.WalkSpeed = speedValue
        else
            humanoid.WalkSpeed = normalSpeed
        end
    end
end)

-- Infinite jump: forçar estado de pulo sem teleportar
UserInputService.JumpRequest:Connect(function()
    if jumpEnabled and humanoid then
        humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- GUI bem simples para ligar/desligar

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