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

-- === Variáveis de velocidade ===
local speedOn = false
local ultraSpeedOn = false
local NORMAL_SPEED = 16
local SPEED_50 = 50
local SPEED_80 = 80
local ITEM_NAME = "Speed Coil"

local function buySpeedCoil()
	for _, obj in pairs(workspace:GetDescendants()) do
		if obj:IsA("ProximityPrompt") then
			if obj.ActionText and obj.ActionText:lower():find("speed") or obj.ObjectText and obj.ObjectText:lower():find("coil") then
				pcall(function() obj:InputHoldBegin() task.wait(0.1) obj:InputHoldEnd() end)
			end
		elseif obj:IsA("ClickDetector") and obj.Parent and obj.Parent.Name:lower():find("speed") then
			pcall(function() fireclickdetector(obj) end)
		end
	end
end

local function equipSpeedCoil()
	local backpack = LocalPlayer:WaitForChild("Backpack")
	local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
	local humanoid = character:WaitForChild("Humanoid")
	for _, tool in pairs(backpack:GetChildren()) do
		if tool:IsA("Tool") and tool.Name:lower():find("speed") then
			humanoid:EquipTool(tool)
			task.wait(1)
			humanoid:UnequipTools()
			return true
		end
	end
	return false
end

local function maintainSpeed()
	local character = LocalPlayer.Character
	if not character then return end
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	if not humanoid then return end

	if speedOn then
		if humanoid.WalkSpeed ~= SPEED_50 then
			humanoid.WalkSpeed = SPEED_50
		end
	elseif ultraSpeedOn then
		if humanoid.WalkSpeed ~= SPEED_80 then
			humanoid.WalkSpeed = SPEED_80
		end
	else
		if humanoid.WalkSpeed ~= NORMAL_SPEED then
			humanoid.WalkSpeed = NORMAL_SPEED
		end
	end
end

-- Botão Velocidade Rápida
local btnSpeed50 = createButton("Velocidade Rápida OFF", 60, function(btn)
	speedOn = not speedOn
	ultraSpeedOn = false
	if speedOn then
		buySpeedCoil()
		task.wait(2)
		equipSpeedCoil()
		btn.Text = "Velocidade Rápida ON"
		btnUltra.Text = "Velocidade Ultra Rápida OFF"
	else
		btn.Text = "Velocidade Rápida OFF"
	end
end)

-- Botão Velocidade Ultra Rápida
local btnUltra = createButton("Velocidade Ultra Rápida OFF", 110, function(btn)
	ultraSpeedOn = not ultraSpeedOn
	speedOn = false
	if ultraSpeedOn then
		buySpeedCoil()
		task.wait(2)
		equipSpeedCoil()
		btn.Text = "Velocidade Ultra Rápida ON"
		btnSpeed50.Text = "Velocidade Rápida OFF"
	else
		btn.Text = "Velocidade Ultra Rápida OFF"
	end
end)

RunService.Heartbeat:Connect(maintainSpeed)

-- === Pulo Infinito ===
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