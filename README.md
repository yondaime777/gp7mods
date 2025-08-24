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
frame.Size = UDim2.new(0, 220, 0, 400)
frame.Position = UDim2.new(0, 20, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.4
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Visible = true -- garante visibilidade inicial

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
print("[GP7Menu] Abrindo menu...")
frame.Visible = true
floatBtn.Visible = false
end)

-- Criar botões automáticos
local buttonY = 50 -- espaçamento vertical
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
print("[GP7Menu] Botão clicado: " .. text)
callback(btn)
end)
buttonIndex += 1
return btn
end

-- ===== SLIDER VELOCIDADE =====
local sliderFrame = Instance.new("Frame", frame)
sliderFrame.Size = UDim2.new(1,-20,0,50)
sliderFrame.Position = UDim2.new(0,10,0,40)
sliderFrame.BackgroundTransparency = 1

local sliderBar = Instance.new("Frame", sliderFrame)
sliderBar.Size = UDim2.new(1,0,0,10)
sliderBar.Position = UDim2.new(0,0,0.5,-5)
sliderBar.BackgroundColor3 = Color3.fromRGB(0,255,0)

local sliderHandle = Instance.new("TextButton", sliderBar)
sliderHandle.Size = UDim2.new(0,20,2,0)
sliderHandle.Position = UDim2.new(0,0,0,0)
sliderHandle.BackgroundColor3 = Color3.fromRGB(255,0,0)
sliderHandle.Text = ""
sliderHandle.Active = true
sliderHandle.Draggable = true

local speedLabel = Instance.new("TextLabel", sliderFrame)
speedLabel.Size = UDim2.new(1,0,0,20)
speedLabel.Position = UDim2.new(0,0,0,-20)
speedLabel.TextColor3 = Color3.fromRGB(0,255,0)
speedLabel.TextScaled = true
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Speed: 16"

-- Atualiza velocidade em tempo real
local function updateSpeed()
local barPos = sliderHandle.Position.X.Offset
local barSize = sliderBar.AbsoluteSize.X - sliderHandle.AbsoluteSize.X
local scale = math.clamp(barPos / barSize,0,1)
local speed = math.floor(16 + scale * (200-16))
speedLabel.Text = "Speed: "..speed

local char = LocalPlayer.Character
if char then
local humanoid = char:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid.WalkSpeed = speed
end
end

end

sliderHandle:GetPropertyChangedSignal("Position"):Connect(updateSpeed)
RunService.RenderStepped:Connect(updateSpeed)


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
print("[GP7Menu] Pulo Infinito: " .. (infiniteJumpOn and "ON" or "OFF"))
end)

-- ===== TELEPORTE =====
local savedPosition = nil

createButton("Salvar Posição", function()
local char = LocalPlayer.Character
if char and char:FindFirstChild("HumanoidRootPart") then
savedPosition = char.HumanoidRootPart.Position
print("[Teleport] Posição salva:", savedPosition)
else
print("[Teleport] Personagem não encontrado para salvar posição.")
end
end)

createButton("Teleportar", function()
local char = LocalPlayer.Character
if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
print("[Teleport] Teleportado para posição salva!")
else
print("[Teleport] Nenhuma posição salva para teleportar!")
end
end)

-- ===== NOCLIP =====
local noclipOn = false
RunService.Stepped:Connect(function()
if LocalPlayer.Character then
for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
if part:IsA("BasePart") then
if noclipOn then
part.CanCollide = false
else
part.CanCollide = true
end
end
end
end
end)

createButton("Noclip OFF", function(btn)
noclipOn = not noclipOn
btn.Text = noclipOn and "Noclip ON" or "Noclip OFF"
print("[GP7Menu] Noclip: " .. (noclipOn and "ON" or "OFF"))
end)

-- ===== MINIMIZAR =====
createButton("Minimizar", function()
frame.Visible = false
floatBtn.Visible = true
print("[GP7Menu] Menu minimizado")
end)

print("[GP7Menu] Script carregado com sucesso!")
