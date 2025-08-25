local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Criação da interface
local screenGui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local minimizeButton = Instance.new("TextButton")
local speedSliderFrame = Instance.new("Frame")
local speedSliderButton = Instance.new("TextButton")
local speedValueLabel = Instance.new("TextLabel")
local jumpButton = Instance.new("TextButton")
local noclipButton = Instance.new("TextButton")
local saveButton = Instance.new("TextButton")
local teleportButton = Instance.new("TextButton")

screenGui.Parent = player.PlayerGui
frame.Parent = screenGui
frame.Size = UDim2.new(0.3, 0, 0.5, 0)
frame.Position = UDim2.new(0.1, 0, 0.1, 0)
frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

minimizeButton.Parent = frame
minimizeButton.Size = UDim2.new(0.2, 0, 0.1, 0)
minimizeButton.Position = UDim2.new(0.1, 0, 0, 0)
minimizeButton.Text = "Minimizar"

-- Slider para velocidade
speedSliderFrame.Parent = frame
speedSliderFrame.Size = UDim2.new(0.8, 0, 0.1, 0)
speedSliderFrame.Position = UDim2.new(0.1, 0, 0.15, 0)

speedValueLabel.Parent = speedSliderFrame
speedValueLabel.Size = UDim2.new(0.5, 0, 1, 0)
speedValueLabel.Position = UDim2.new(0, 0, 0, 0)
speedValueLabel.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
speedValueLabel.Text = "Velocidade: 16"

speedSliderButton.Parent = speedSliderFrame
speedSliderButton.Size = UDim2.new(0.1, 0, 1, 0)
speedSliderButton.Position = UDim2.new(0.5, 0, 0, 0)
speedSliderButton.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
speedSliderButton.Text = "→"

jumpButton.Parent = frame
jumpButton.Size = UDim2.new(0.8, 0, 0.1, 0)
jumpButton.Position = UDim2.new(0.1, 0, 0.25, 0)
jumpButton.Text = "Pulo Infinito"

noclipButton.Parent = frame
noclipButton.Size = UDim2.new(0.8, 0, 0.1, 0)
noclipButton.Position = UDim2.new(0.1, 0, 0.35, 0)
noclipButton.Text = "No Clip"

saveButton.Parent = frame
saveButton.Size = UDim2.new(0.8, 0, 0.1, 0)
saveButton.Position = UDim2.new(0.1, 0, 0.45, 0)
saveButton.Text = "Salvar Posição"

teleportButton.Parent = frame
teleportButton.Size = UDim2.new(0.8, 0, 0.1, 0)
teleportButton.Position = UDim2.new(0.1, 0, 0.55, 0)
teleportButton.Text = "Teleporte"

-- Variáveis para controle
local savedPosition
local noclipEnabled = false
local originalJumpPower = character.Humanoid.JumpPower

-- Funções
local function minimize()
	frame.Visible = not frame.Visible
end

local function setSpeed(speed)
	if character:FindFirstChild("Humanoid") then
		character.Humanoid.WalkSpeed = speed
	end
end

local function toggleInfiniteJump()
	if character:FindFirstChild("Humanoid") then
		if character.Humanoid.JumpPower ~= 0 then
			character.Humanoid.JumpPower = 0
		else
			character.Humanoid.JumpPower = originalJumpPower
		end
	end
end

local function toggleNoClip()
	noclipEnabled = not noclipEnabled
	while noclipEnabled do
		wait
		()
		for _, obj in pairs(character:GetDescendants()) do
			if obj:IsA("BasePart") then
				obj.CanCollide = false
			end
		end
	end
	for _, obj in pairs(character:GetDescendants()) do
		if obj:IsA("BasePart") then
			obj.CanCollide = true
		end
	end
end

local function savePosition()
	savedPosition = character.HumanoidRootPart.Position
end

local function teleportToSavedPosition()
	if savedPosition then
		character.HumanoidRootPart.Position = savedPosition
	end
end

-- Atualiza a velocidade com base na posição do slider
speedSliderButton.MouseButton1Drag:Connect(function()
	local mouseX = game:GetService("UserInputService"):GetMouseLocation().X
	local sliderX = speedSliderFrame.AbsolutePosition.X
	local sliderWidth = speedSliderFrame.AbsoluteSize.X
	local relativePosition = (mouseX - sliderX) / sliderWidth

	-- Clamped entre 0 e 1
	relativePosition = math.clamp(relativePosition, 0, 1)

	-- Faz o slider se mover
	speedSliderButton.Position = UDim2.new(relativePosition, 0, 0, 0)

	-- Calcula a velocidade correspondente
	local speed = math.floor((relativePosition * (200 - 16)) + 16)
	speedValueLabel.Text = "Velocidade: " .. speed
	setSpeed(speed)
end)

-- Conexões de eventos
minimizeButton.MouseButton1Click:Connect(minimize)
jumpButton.MouseButton1Click:Connect(toggleInfiniteJump)
noclipButton.MouseButton1Click:Connect(toggleNoClip)
saveButton.MouseButton1Click:Connect(savePosition)
teleportButton.MouseButton1Click:Connect(teleportToSavedPosition)

-- Funcionalidade do botão flutuante (apenas um exemplo básico)
local floatButton = Instance.new("TextButton")
floatButton.Parent = screenGui
floatButton.Size = UDim2.new(0.1, 0, 0.1, 0)
floatButton.Position = UDim2.new(0.9, 0, 0.5, 0)
floatButton.Text = "Menu"
floatButton.AnchorPoint = Vector2.new(0.5, 0.5)

floatButton.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)