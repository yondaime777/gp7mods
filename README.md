local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Tela principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "GP7MODS"
gui.ResetOnSpawn = false

-- RGB função
local function applyRGB(obj)
	local hue = 0
	RunService.RenderStepped:Connect(function()
		hue = (hue + 1) % 255
		local color = Color3.fromHSV(hue / 255, 1, 1)
		obj.BackgroundColor3 = color
		if obj:IsA("TextButton") or obj:IsA("TextLabel") then
			obj.TextColor3 = Color3.new(1,1,1)
		end
	end)
end

-- Botão flutuante (minimizado)
local floatButton = Instance.new("TextButton")
floatButton.Size = UDim2.new(0, 50, 0, 50)
floatButton.Position = UDim2.new(0, 20, 0.5, 0)
floatButton.BackgroundTransparency = 0
floatButton.Text = "+"
floatButton.TextSize = 30
floatButton.Font = Enum.Font.SourceSansBold
floatButton.ZIndex = 1000
floatButton.Parent = gui
floatButton.Visible = true
floatButton.ClipsDescendants = true
floatButton.TextColor3 = Color3.new(1, 1, 1)
floatButton.BackgroundColor3 = Color3.new(1, 0, 0)
floatButton.AutoButtonColor = true
floatButton.BorderSizePixel = 0
floatButton.AnchorPoint = Vector2.new(0.5, 0.5)
floatButton.TextWrapped = true
floatButton.Name = "FloatingButton"
floatButton.BackgroundTransparency = 0
floatButton.SizeConstraint = Enum.SizeConstraint.RelativeXY
floatButton.BorderMode = Enum.BorderMode.Outline
floatButton.ClipsDescendants = false
floatButton.TextStrokeTransparency = 0.5
floatButton.TextStrokeColor3 = Color3.fromRGB(0,0,0)
floatButton.TextScaled = true
floatButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
floatButton.TextColor3 = Color3.new(1, 1, 1)
floatButton.UICorner = Instance.new("UICorner", floatButton)
floatButton.UICorner.CornerRadius = UDim.new(1, 0)

applyRGB(floatButton)

-- Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 250, 0, 250)
frame.Position = UDim2.new(0, 80, 0.5, -125)
frame.AnchorPoint = Vector2.new(0, 0.5)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Parent = gui

local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 10)

applyRGB(frame)

-- Título
local title = Instance.new("TextLabel")
title.Text = "GP7 MODS"
title.Size = UDim2.new(1, 0, 0, 40)
title.Font = Enum.Font.Code
title.TextSize = 24
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1
title.Parent = frame

-- Função utilitária para criar botões com toggle
local function createToggle(name, parent, callback)
	local btn = Instance.new("TextButton")
	btn.Text = name .. ": Desativado"
	btn.Size = UDim2.new(1, -20, 0, 40)
	btn.Position = UDim2.new(0, 10, 0, #parent:GetChildren() * 45)
	btn.Font = Enum.Font.Code
	btn.TextSize = 18
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BackgroundColor3 = Color3.fromRGB(70,70,70)
	btn.Parent = parent

	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 6)
	applyRGB(btn)

	local enabled = false
	btn.MouseButton1Click:Connect(function()
		enabled = not enabled
		btn.Text = name .. ": " .. (enabled and "Ativado" or "Desativado")
		callback(enabled)
	end)
end

-- Funções
createToggle("Velocidade Rápida", frame, function(on)
	if on then
		humanoid.WalkSpeed = 50
	else
		humanoid.WalkSpeed = 16
	end
end)

createToggle("Velocidade Ultra", frame, function(on)
	if on then
		humanoid.WalkSpeed = 80
	else
		humanoid.WalkSpeed = 16
	end
end)

createToggle("Pulo Infinito", frame, function(on)
	if on then
		_G.infiniteJump = true
	else
		_G.infiniteJump = false
	end
end)

-- Pulo infinito funcional
UserInputService.JumpRequest:Connect(function()
	if _G.infiniteJump then
		local char = player.Character
		if char and char:FindFirstChildOfClass("Humanoid") then
			char:FindFirstChildOfClass("Humanoid"):ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end
end)

-- Minimizar e abrir
frame.Visible = false
floatButton.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)