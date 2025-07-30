local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")

-- Variáveis
local speedState = 0 -- 0 = normal, 1 = rápida, 2 = ultra
local jumpEnabled = false

-- GUI Principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GP7_MODS"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Botão flutuante (minimizado)
local floatBtn = Instance.new("TextButton")
floatBtn.Name = "FloatButton"
floatBtn.Size = UDim2.new(0, 60, 0, 60)
floatBtn.Position = UDim2.new(0, 20, 0.5, -30)
floatBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
floatBtn.Text = "+"
floatBtn.TextColor3 = Color3.new(0, 0, 0)
floatBtn.TextScaled = true
floatBtn.Font = Enum.Font.SourceSansBold
floatBtn.AutoButtonColor = true
floatBtn.Parent = gui
floatBtn.BackgroundTransparency = 0
floatBtn.BorderSizePixel = 0
floatBtn.ClipsDescendants = true
floatBtn.AnchorPoint = Vector2.new(0, 0)
floatBtn.ZIndex = 5
floatBtn.BorderMode = Enum.BorderMode.Outline
floatBtn.BorderColor3 = Color3.fromRGB(255, 255, 0)
floatBtn.BackgroundTransparency = 0
floatBtn.TextWrapped = true
floatBtn.TextStrokeTransparency = 0.8
floatBtn.TextStrokeColor3 = Color3.fromRGB(255, 255, 0)
floatBtn.TextSize = 20
floatBtn.Visible = true
floatBtn.ClipsDescendants = true
floatBtn.BackgroundTransparency = 0
floatBtn.BorderSizePixel = 2
floatBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
floatBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
floatBtn.TextScaled = true
floatBtn.Font = Enum.Font.Code
floatBtn.TextWrapped = true
floatBtn.AutoButtonColor = true
floatBtn.ClipsDescendants = true
floatBtn.ZIndex = 100
floatBtn.AnchorPoint = Vector2.new(0.5, 0.5)
floatBtn.Position = UDim2.new(0, 50, 0, 50)
floatBtn.Size = UDim2.new(0, 50, 0, 50)
floatBtn.Text = "+"

-- Deixar redondo
local round = Instance.new("UICorner")
round.CornerRadius = UDim.new(1, 0)
round.Parent = floatBtn

-- Janela principal
local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.new(0, 250, 0, 250)
main.Position = UDim2.new(0, 80, 0, 40)
main.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
main.Visible = false
main.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 15)
corner.Parent = main

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
title.Text = "GP7 MODS"
title.Font = Enum.Font.Code
title.TextColor3 = Color3.new(0,0,0)
title.TextScaled = true
title.Parent = main

Instance.new("UICorner", title).CornerRadius = UDim.new(0, 10)

-- Botão Velocidade Rápida
local speed50Btn = Instance.new("TextButton")
speed50Btn.Size = UDim2.new(1, -20, 0, 40)
speed50Btn.Position = UDim2.new(0, 10, 0, 50)
speed50Btn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
speed50Btn.Text = "Velocidade Rápida (50): DESATIVADO"
speed50Btn.Font = Enum.Font.Code
speed50Btn.TextScaled = true
speed50Btn.TextColor3 = Color3.new(0,0,0)
speed50Btn.Parent = main
Instance.new("UICorner", speed50Btn).CornerRadius = UDim.new(0, 10)

-- Botão Velocidade Ultra Rápida
local speed80Btn = Instance.new("TextButton")
speed80Btn.Size = UDim2.new(1, -20, 0, 40)
speed80Btn.Position = UDim2.new(0, 10, 0, 100)
speed80Btn.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
speed80Btn.Text = "Velocidade Ultra (80): DESATIVADO"
speed80Btn.Font = Enum.Font.Code
speed80Btn.TextScaled = true
speed80Btn.TextColor3 = Color3.new(0,0,0)
speed80Btn.Parent = main
Instance.new("UICorner", speed80Btn).CornerRadius = UDim.new(0, 10)

-- Botão Pulo Infinito
local jumpBtn = Instance.new("TextButton")
jumpBtn.Size = UDim2.new(1, -20, 0, 40)
jumpBtn.Position = UDim2.new(0, 10, 0, 150)
jumpBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
jumpBtn.Text = "Pulo Infinito: DESATIVADO"
jumpBtn.Font = Enum.Font.Code
jumpBtn.TextScaled = true
jumpBtn.TextColor3 = Color3.new(0,0,0)
jumpBtn.Parent = main
Instance.new("UICorner", jumpBtn).CornerRadius = UDim.new(0, 10)

-- Alternar visibilidade
floatBtn.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
end)

-- Velocidade Rápida
speed50Btn.MouseButton1Click:Connect(function()
	if speedState ~= 1 then
		hum.WalkSpeed = 50
		speed50Btn.Text = "Velocidade Rápida (50): ATIVADO"
		speed80Btn.Text = "Velocidade Ultra (80): DESATIVADO"
		speedState = 1
	else
		hum.WalkSpeed = 16
		speed50Btn.Text = "Velocidade Rápida (50): DESATIVADO"
		speedState = 0
	end
end)

-- Velocidade Ultra Rápida
speed80Btn.MouseButton1Click:Connect(function()
	if speedState ~= 2 then
		hum.WalkSpeed = 80
		speed80Btn.Text = "Velocidade Ultra (80): ATIVADO"
		speed50Btn.Text = "Velocidade Rápida (50): DESATIVADO"
		speedState = 2
	else
		hum.WalkSpeed = 16
		speed80Btn.Text = "Velocidade Ultra (80): DESATIVADO"
		speedState = 0
	end
end)

-- Pulo infinito
jumpBtn.MouseButton1Click:Connect(function()
	jumpEnabled = not jumpEnabled
	jumpBtn.Text = "Pulo Infinito: " .. (jumpEnabled and "ATIVADO" or "DESATIVADO")
end)

-- Loop para pulo infinito
UIS.JumpRequest:Connect(function()
	if jumpEnabled then
		hum:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end)