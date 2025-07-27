-- GP7 MENU - Steal a Brainrot
local plr = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")
local camera = workspace.CurrentCamera
local enabledESP, enabledJump, enabledSpeed = false, false, false
local speedValue = 16

-- Menu Flutuante
local gui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 240)
frame.Position = UDim2.new(0.3, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Text = "GP7 MENU"
title.Size = UDim2.new(1, 0, 0, 30)
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
title.BorderSizePixel = 0
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true

-- Botão: ESP
local btnESP = Instance.new("TextButton", frame)
btnESP.Position = UDim2.new(0, 10, 0, 40)
btnESP.Size = UDim2.new(0, 200, 0, 30)
btnESP.Text = "ESP: OFF"
btnESP.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
btnESP.TextColor3 = Color3.fromRGB(255, 255, 255)
btnESP.Font = Enum.Font.SourceSans
btnESP.TextScaled = true
btnESP.MouseButton1Click:Connect(function()
	enabledESP = not enabledESP
	btnESP.Text = enabledESP and "ESP: ON" or "ESP: OFF"
end)

-- Botão: Pulo Infinito
local btnJump = Instance.new("TextButton", frame)
btnJump.Position = UDim2.new(0, 10, 0, 80)
btnJump.Size = UDim2.new(0, 200, 0, 30)
btnJump.Text = "Pulo Infinito: OFF"
btnJump.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
btnJump.TextColor3 = Color3.fromRGB(255, 255, 255)
btnJump.Font = Enum.Font.SourceSans
btnJump.TextScaled = true
btnJump.MouseButton1Click:Connect(function()
	enabledJump = not enabledJump
	btnJump.Text = enabledJump and "Pulo Infinito: ON" or "Pulo Infinito: OFF"
end)

-- Botão: Speed Hack
local btnSpeed = Instance.new("TextButton", frame)
btnSpeed.Position = UDim2.new(0, 10, 0, 120)
btnSpeed.Size = UDim2.new(0, 200, 0, 30)
btnSpeed.Text = "Speed Hack: OFF"
btnSpeed.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
btnSpeed.TextColor3 = Color3.fromRGB(255, 255, 255)
btnSpeed.Font = Enum.Font.SourceSans
btnSpeed.TextScaled = true
btnSpeed.MouseButton1Click:Connect(function()
	enabledSpeed = not enabledSpeed
	btnSpeed.Text = enabledSpeed and "Speed Hack: ON" or "Speed Hack: OFF"
end)

-- Slider de velocidade
local slider = Instance.new("TextBox", frame)
slider.Position = UDim2.new(0, 10, 0, 160)
slider.Size = UDim2.new(0, 200, 0, 30)
slider.Text = "Velocidade: 16"
slider.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
slider.TextColor3 = Color3.fromRGB(255, 255, 255)
slider.Font = Enum.Font.SourceSans
slider.TextScaled = true
slider.FocusLost:Connect(function()
	local val = tonumber(slider.Text:match("%d+"))
	if val then
		speedValue = val
		slider.Text = "Velocidade: " .. speedValue
	end
end)

-- ESP Function
function updateESP()
	for _, player in ipairs(game.Players:GetPlayers()) do
		if player ~= plr and player.Character and player.Character:FindFirstChild("Head") then
			if not player.Character:FindFirstChild("ESPLabel") then
				local billboard = Instance.new("BillboardGui", player.Character.Head)
				billboard.Name = "ESPLabel"
				billboard.Size = UDim2.new(0, 100, 0, 20)
				billboard.StudsOffset = Vector3.new(0, 2, 0)
				billboard.AlwaysOnTop = true

				local text = Instance.new("TextLabel", billboard)
				text.Size = UDim2.new(1, 0, 1, 0)
				text.BackgroundTransparency = 1
				text.Text = player.Name
				text.TextColor3 = Color3.fromRGB(255, 0, 0)
				text.TextScaled = true
			end
		end
	end
end

-- Infinite Jump
UIS.JumpRequest:Connect(function()
	if enabledJump then
		plr.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
	end
end)

-- Speed Hack Loop
RS.RenderStepped:Connect(function()
	if enabledSpeed and plr.Character and plr.Character:FindFirstChild("Humanoid") then
		plr.Character.Humanoid.WalkSpeed = speedValue
	elseif not enabledSpeed and plr.Character and plr.Character:FindFirstChild("Humanoid") then
		plr.Character.Humanoid.WalkSpeed = 16
	end
end)

-- ESP Loop
while true do
	wait(1)
	if enabledESP then
		updateESP()
	end
end
