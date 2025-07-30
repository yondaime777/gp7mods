local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")

-- Estados das funções
local state = {
	speed = 16,
	jump = false
}

-- Criar GUI
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GP7MODS"
gui.ResetOnSpawn = false

-- Botão flutuante
local toggle = Instance.new("TextButton")
toggle.Size = UDim2.new(0, 50, 0, 50)
toggle.Position = UDim2.new(0, 10, 0.5, -25)
toggle.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
toggle.Text = "+"
toggle.TextColor3 = Color3.fromRGB(0, 0, 0)
toggle.Font = Enum.Font.SourceSansBold
toggle.TextScaled = true
toggle.Parent = gui
Instance.new("UICorner", toggle)

-- Menu principal
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 220, 0, 200)
main.Position = UDim2.new(0, 70, 0.5, -100)
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

-- Função botão
local function criarBotao(nome, pos, callback)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 200, 0, 40)
	btn.Position = UDim2.new(0, 10, 0, pos)
	btn.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
	btn.TextColor3 = Color3.fromRGB(0, 0, 0)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextScaled = true
	btn.Text = nome
	btn.Parent = main
	Instance.new("UICorner", btn)
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- Botão Velocidade 50
local btn50 = criarBotao("Velocidade 50 [Desativado]", 40, function()
	humanoid.WalkSpeed = 50
	btn50.Text = "Velocidade 50 [Ativado]"
	btn80.Text = "Velocidade 80 [Desativado]"
end)

-- Botão Velocidade 80
local btn80 = criarBotao("Velocidade 80 [Desativado]", 90, function()
	humanoid.WalkSpeed = 80
	btn50.Text = "Velocidade 50 [Desativado]"
	btn80.Text = "Velocidade 80 [Ativado]"
end)

-- Botão Pulo Infinito
local btnJump = criarBotao("Pulo Infinito [Desativado]", 140, function()
	state.jump = not state.jump
	if state.jump then
		btnJump.Text = "Pulo Infinito [Ativado]"
	else
		btnJump.Text = "Pulo Infinito [Desativado]"
	end
end)

-- Ativador de pulo infinito
UIS.JumpRequest:Connect(function()
	if state.jump and humanoid then
		humanoid.Jump = true
	end
end)

-- Alternar menu ao clicar no botão +
toggle.MouseButton1Click:Connect(function()
	main.Visible = not main.Visible
	toggle.Text = main.Visible and "X" or "+"
end)