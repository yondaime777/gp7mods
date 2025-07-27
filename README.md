local player = game.Players.LocalPlayer
local uis = game:GetService("UserInputService")
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GP7MENU"

local dragging, dragInput, dragStart, startPos

-- Cria o menu principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 180)
frame.Position = UDim2.new(0.5, -100, 0.5, -90)
frame.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
frame.BorderColor3 = Color3.fromRGB(255, 0, 0)
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Text = "GP7 MENU"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextSize = 16

-- Botão Speed Hack
local speedBtn = Instance.new("TextButton", frame)
speedBtn.Position = UDim2.new(0, 10, 0, 40)
speedBtn.Size = UDim2.new(1, -20, 0, 30)
speedBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
speedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBtn.Text = "Comprar Speed + Ativar"
speedBtn.Font = Enum.Font.Gotham
speedBtn.TextSize = 14

-- Botão Super Pulo
local jumpBtn = Instance.new("TextButton", frame)
jumpBtn.Position = UDim2.new(0, 10, 0, 80)
jumpBtn.Size = UDim2.new(1, -20, 0, 30)
jumpBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
jumpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
jumpBtn.Text = "Comprar Pulo + Ativar"
jumpBtn.Font = Enum.Font.Gotham
jumpBtn.TextSize = 14

-- Variáveis de controle
local speedOn = false
local jumpOn = false

-- Compra e ativa speed
speedBtn.MouseButton1Click:Connect(function()
	if not speedOn then
		-- Compra o Speed Coil
		local args = { "Speed Coil" }
		game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("Net"):WaitForChild("RF/CoinsShopService/RequestBuy"):InvokeServer(unpack(args))
	end
	speedOn = not speedOn
	speedBtn.Text = speedOn and "Speed ATIVO" or "Speed DESATIVADO"

	-- Aplica o efeito
	if speedOn then
		player.Character.Humanoid.WalkSpeed = 50
	else
		player.Character.Humanoid.WalkSpeed = 16
	end
end)

-- Compra e ativa super pulo
jumpBtn.MouseButton1Click:Connect(function()
	if not jumpOn then
		-- Compra o Gravity Coil
		local args = { "Gravity Coil" }
		game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("Net"):WaitForChild("RF/CoinsShopService/RequestBuy"):InvokeServer(unpack(args))
	end
	jumpOn = not jumpOn
	jumpBtn.Text = jumpOn and "Pulo ATIVO" or "Pulo DESATIVADO"

	-- Aplica o efeito
	if jumpOn then
		player.Character.Humanoid.JumpPower = 120
	else
		player.Character.Humanoid.JumpPower = 50
	end
end)