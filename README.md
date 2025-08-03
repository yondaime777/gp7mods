local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

-- Criar GUI principal
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 260)
frame.Position = UDim2.new(0, 20, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.4
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

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
frame.Visible = true
floatBtn.Visible = false
end)

local function createButton(text, posY, callback)
local btn = Instance.new("TextButton", frame)
btn.Size = UDim2.new(1, -20, 0, 40)
btn.Position = UDim2.new(0, 10, 0, posY)
btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
btn.TextColor3 = Color3.fromRGB(0, 255, 0)
btn.Font = Enum.Font.GothamBold
btn.TextScaled = true
btn.Text = text
btn.BorderSizePixel = 0
btn.MouseButton1Click:Connect(function()
callback(btn)
end)
return btn
end

createButton("Minimizar", 220, function()
frame.Visible = false
floatBtn.Visible = true
end)

-- === Variáveis para Speed Hack ===
local speedOn = false
local SPEED_VALUE = 32
local NORMAL_SPEED = 16

local function maintainSpeed()
local character = LocalPlayer.Character
if not character then return end
local humanoid = character:FindFirstChildOfClass("Humanoid")
if not humanoid then return end
if speedOn then
humanoid.WalkSpeed = SPEED_VALUE
else
humanoid.WalkSpeed = NORMAL_SPEED
end
end

-- Botão Velocidade Rápida
createButton("Velocidade Rápida OFF", 60, function(btn)
speedOn = not speedOn
btn.Text = speedOn and "Velocidade Rápida ON" or "Velocidade Rápida OFF"
end)

RunService.Heartbeat:Connect(maintainSpeed)

-- === Infinite Jump ===
local infiniteJumpOn = false
local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")

UserInputService.JumpRequest:Connect(function()
if infiniteJumpOn then
humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
end
end
end)

createButton("Pulo Infinito OFF", 160, function(btn)
infiniteJumpOn = not infiniteJumpOn
btn.Text = infiniteJumpOn and "Pulo Infinito ON" or "Pulo Infinito OFF"
end)

local hitboxExpandOn = false

createButton("Hitbox Expand OFF", 190, function(btn)
	hitboxExpandOn = not hitboxExpandOn
	toggleHitboxExpand(hitboxExpandOn)
	btn.Text = hitboxExpandOn and "Hitbox Expand ON" or "Hitbox Expand OFF"
	btn.BackgroundColor3 = hitboxExpandOn and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(0, 100, 0)
end)

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local hitboxPart = nil
local hitboxOn = false

local function createHitbox()
	local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

	if hitboxPart then
		hitboxPart:Destroy()
		hitboxPart = nil
	end

	hitboxPart = Instance.new("Part")
	hitboxPart.Name = "HitboxExpand"
	hitboxPart.Size = Vector3.new(6, 5, 6)  -- tamanho maior, ajuste se quiser
	hitboxPart.Transparency = 1  -- invisível
	hitboxPart.CanCollide = true  -- pode colidir para tentar aumentar alcance
	hitboxPart.Anchored = false
	hitboxPart.Massless = true
	hitboxPart.Parent = character

	-- Conecta com o HumanoidRootPart com Weld para mover junto
	local hrp = character:FindFirstChild("HumanoidRootPart")
	if hrp then
		local weld = Instance.new("WeldConstraint")
		weld.Part0 = hitboxPart
		weld.Part1 = hrp
		weld.Parent = hitboxPart

		hitboxPart.CFrame = hrp.CFrame
	end
end

local function removeHitbox()
	if hitboxPart then
		hitboxPart:Destroy()
		hitboxPart = nil
	end
end

local function toggleHitboxExpand(ativo)
	if ativo then
		createHitbox()
	else
		removeHitbox()
	end
end