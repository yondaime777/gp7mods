local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GP7MENU"
gui.ResetOnSpawn = false

-- Estados
local speedOn = false
local jumpOn = false
local menuVisible = true

-- Funções de compra
local function buyItem(item)
	local args = { item }
	local shop = game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("Net"):WaitForChild("RF/CoinsShopService/RequestBuy")
	shop:InvokeServer(unpack(args))
end

-- Menu principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 210)
frame.Position = UDim2.new(0.3, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
frame.BorderColor3 = Color3.fromRGB(255, 0, 0)
frame.Active = true
frame.Draggable = true

-- Título
local title = Instance.new("TextLabel", frame)
title.Text = "GP7 MENU"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextSize = 16

-- Botão SPEED
local speedBtn = Instance.new("TextButton", frame)
speedBtn.Position = UDim2.new(0, 10, 0, 50)
speedBtn.Size = UDim2.new(1, -20, 0, 40)
speedBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
speedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBtn.Font = Enum.Font.Gotham
speedBtn.TextSize = 14
speedBtn.Text = "Ativar Speed Hack (DESATIVADO)"

-- Botão JUMP
local jumpBtn = Instance.new("TextButton", frame)
jumpBtn.Position = UDim2.new(0, 10, 0, 100)
jumpBtn.Size = UDim2.new(1, -20, 0, 40)
jumpBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
jumpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
jumpBtn.Font = Enum.Font.Gotham
jumpBtn.TextSize = 14
jumpBtn.Text = "Ativar Super Pulo (DESATIVADO)"

-- Botão Minimizar
local minimizeBtn = Instance.new("TextButton", frame)
minimizeBtn.Position = UDim2.new(0, 10, 0, 160)
minimizeBtn.Size = UDim2.new(1, -20, 0, 30)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 14
minimizeBtn.Text = "Minimizar"

-- Botão flutuante
local floatingBtn = Instance.new("TextButton", gui)
floatingBtn.Position = UDim2.new(0, 20, 0.5, -30)
floatingBtn.Size = UDim2.new(0, 100, 0, 30)
floatingBtn.Text = "GP7 MENU"
floatingBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
floatingBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
floatingBtn.Font = Enum.Font.GothamBold
floatingBtn.TextSize = 14
floatingBtn.Visible = false

-- Funções dos botões
speedBtn.MouseButton1Click:Connect(function()
	if not speedOn then
		buyItem("Speed Coil")
	end
	speedOn = not speedOn
	speedBtn.Text = speedOn and "Ativar Speed Hack (ATIVADO)" or "Ativar Speed Hack (DESATIVADO)"
	character.Humanoid.WalkSpeed = speedOn and 50 or 16
end)

jumpBtn.MouseButton1Click:Connect(function()
	if not jumpOn then
		buyItem("Gravity Coil")
	end
	jumpOn = not jumpOn
	jumpBtn.Text = jumpOn and "Ativar Super Pulo (ATIVADO)" or "Ativar Super Pulo (DESATIVADO)"
	character.Humanoid.JumpPower = jumpOn and 120 or 50
end)

minimizeBtn.MouseButton1Click:Connect(function()
	menuVisible = false
	frame.Visible = false
	floatingBtn.Visible = true
end)

floatingBtn.MouseButton1Click:Connect(function()
	menuVisible = true
	frame.Visible = true
	floatingBtn.Visible = false
end)

-- Garante que ao morrer reaplique
player.CharacterAdded:Connect(function(char)
	character = char
	char:WaitForChild("Humanoid").Died:Connect(function()
		speedOn = false
		jumpOn = false
		speedBtn.Text = "Ativar Speed Hack (DESATIVADO)"
		jumpBtn.Text = "Ativar Super Pulo (DESATIVADO)"
	end)
end)