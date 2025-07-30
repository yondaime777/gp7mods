local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RS = game:GetService("RunService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")

-- Criar GUI
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GP7MODS"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Botão flutuante
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 50, 0, 50)
toggleButton.Position = UDim2.new(0, 20, 0.5, -25)
toggleButton.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
toggleButton.Text = "+"
toggleButton.TextScaled = true
toggleButton.TextColor3 = Color3.fromRGB(0, 0, 0)
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.Name = "ToggleButton"
toggleButton.Parent = gui
toggleButton.ClipsDescendants = true
toggleButton.BorderSizePixel = 0
toggleButton.AutoButtonColor = true
toggleButton.BackgroundTransparency = 0
toggleButton.AnchorPoint = Vector2.new(0, 0.5)
toggleButton.TextStrokeTransparency = 0.5
toggleButton.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
toggleButton.CornerRadius = UDim.new(1, 0)
Instance.new("UICorner", toggleButton)

-- Menu principal
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 200, 0, 200)
main.Position = UDim2.new(0, 80, 0.5, -100)
main.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
main.Visible = false
main.Parent = gui
Instance.new("UICorner", main)

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "GP7 MODS"
title.TextColor3 = Color3.fromRGB(0, 0, 0)
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true
title.Parent = main

-- Estado de cada função
local state = {
	speed50 = false,
	speed80 = false,
	jump = false
}

-- Função para criar botões
local function createButton(text, y, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(1, -20, 0, 40)
	btn.Position = UDim2.new(0, 10, 0, y)
	btn.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
	btn.Text = text .. " [Desativado]"
	btn.TextColor3 = Color3.fromRGB(0, 0, 0)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextScaled = true
	btn.Parent = main
	Instance.new("UICorner", btn)
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- Botão: Velocidade 50
local btn50 = createButton("Velocidade 50", 40, function()
	state.speed50 = not state.speed50
	if state.speed50 then
		hum.WalkSpeed = 50
		state.speed80 = false
		btn80.Text = "Velocidade 80 [Desativado]"
		btn50.Text = "Velocidade 50 [Ativado]"
	else
		hum.WalkSpeed = 16
		btn50.Text = "Velocidade 50 [Desativado]"
	end
end)

-- Botão: Velocidade 80
btn80 = createButton("Velocidade 80", 90, function()
	state.speed80 = not state.speed80
	if state.speed80 then
		hum.WalkSpeed = 80
		state.speed50 = false
		btn50.Text = "Velocidade 50 [Desativado]"
		btn80.Text = "Velocidade 80 [Ativado]"
	else
		hum.WalkSpeed = 16
		btn80.Text = "Velocidade 80 [Desativado]"
	end
end)

-- Botão: Pulo Infinito
createButton("Pulo Infinito", 140, function()
	state.jump = not state.jump
	if state.jump then
		btnJump.Text = "Pulo Infinito [Ativado]"
	else
		btnJump.Text = "Pulo Infinito [Desativado]"
	end
end)

-- Botão de pulo declarado separadamente
btnJump = main:FindFirstChildOfClass("TextButton", true)
btnJump = btnJump or createButton("Pulo Infinito", 140, function() end)

-- Ativar pulo infinito
UIS.JumpRequest:Connect(function()
	if state.jump and humanoid then
		hum.Jump = true
	end
end)

-- Toggle do menu
toggleButton.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
	toggleButton.Text = main.Visible and "X" or "+"
end)