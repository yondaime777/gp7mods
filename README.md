local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local backpack = player:WaitForChild("Backpack")

local SPEED_VALUE = 40
local NORMAL_SPEED = 16
local ITEM_NAME = "Speed Coil" -- nome do item de velocidade
local speedOn = false

-- GUI simples
local playerGui = player:WaitForChild("PlayerGui")
local screenGui = Instance.new("ScreenGui", playerGui)
screenGui.Name = "GP7_Speed"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 160, 0, 60)
frame.Position = UDim2.new(0, 20, 0.5, -30)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 2
frame.Active = true
frame.Draggable = true

local toggleBtn = Instance.new("TextButton", frame)
toggleBtn.Size = UDim2.new(1, 0, 1, 0)
toggleBtn.Text = "Ativar Speed Hack"
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 20

-- Compra o item
local function comprarItem()
    local remote = ReplicatedStorage:FindFirstChild("BuySpeedCoil") -- nome pode variar
    if remote and remote:IsA("RemoteEvent") then
        remote:FireServer()
    end
end

-- Equipa o item
local function equiparItem()
    local ferramenta = backpack:FindFirstChild(ITEM_NAME)
    if ferramenta then
        ferramenta.Parent = character
    end
end

-- Ativa o speed
local function ativarSpeed()
    comprarItem()
    task.wait(0.5)
    equiparItem()
    speedOn = true
    humanoid.WalkSpeed = SPEED_VALUE
    toggleBtn.Text = "Desativar Speed Hack"
    toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
end

-- Desativa o speed
local function desativarSpeed()
    speedOn = false
    humanoid.WalkSpeed = NORMAL_SPEED
    toggleBtn.Text = "Ativar Speed Hack"
    toggleBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
end

-- Mantém velocidade mesmo sem o item
RunService.Heartbeat:Connect(function()
    if speedOn and humanoid.WalkSpeed ~= SPEED_VALUE then
        humanoid.WalkSpeed = SPEED_VALUE
    end
end)

-- Clique do botão
toggleBtn.MouseButton1Click:Connect(function()
    if speedOn then
        desativarSpeed()
    else
        ativarSpeed()
    end
end)