local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "GP7_MODS"
ScreenGui.ResetOnSpawn = false

-- Função RGB
local function getRainbowColor(offset)
	local r = math.sin(tick() + offset) * 127 + 128
	local g = math.sin(tick() + offset + 2) * 127 + 128
	local b = math.sin(tick() + offset + 4) * 127 + 128
	return Color3.fromRGB(r, g, b)
end

-- Botão flutuante
local FloatButton = Instance.new("TextButton", ScreenGui)
FloatButton.Size = UDim2.new(0, 120, 0, 60)
FloatButton.Position = UDim2.new(0, 20, 0.5, -30)
FloatButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
FloatButton.Text = "GP7 MODS"
FloatButton.TextColor3 = Color3.new(1,1,1)
FloatButton.Font = Enum.Font.SourceSansBold
FloatButton.TextSize = 18
FloatButton.Draggable = true
FloatButton.Active = true
FloatButton.BorderSizePixel = 0
Instance.new("UICorner", FloatButton).CornerRadius = UDim.new(1, 0)

-- Menu principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 260, 0, 200)
MainFrame.Position = UDim2.new(0.5, -130, 0.5, -100)
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)

-- Título
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.BackgroundTransparency = 1
Title.Text = "GP7 MODS"
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20
Title.TextColor3 = Color3.new(1, 1, 1)
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Botão minimizar
local MinBtn = Instance.new("TextButton", MainFrame)
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -35, 0, 5)
MinBtn.Text = "-"
MinBtn.Font = Enum.Font.SourceSansBold
MinBtn.TextSize = 20
MinBtn.TextColor3 = Color3.new(1,1,1)
MinBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
MinBtn.BorderSizePixel = 0
Instance.new("UICorner", MinBtn).CornerRadius = UDim.new(0, 6)

-- Criar botão com estilo
local function createButton(text, y)
	local btn = Instance.new("TextButton", MainFrame)
	btn.Size = UDim2.new(1, -20, 0, 40)
	btn.Position = UDim2.new(0, 10, 0, y)
	btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
	btn.Text = text
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 16
	btn.TextColor3 = Color3.new(1,1,1)
	Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
	return btn
end

-- Botões
local SpeedBtn = createButton("Velocidade Rápida: Desativado", 40)
local UltraBtn = createButton("Velocidade Ultra: Desativado", 90)
local JumpBtn = createButton("Pulo Infinito: Desativado", 140)

-- Mostrar/ocultar
FloatButton.MouseButton1Click:Connect(function()
	MainFrame.Visible = true
	FloatButton.Visible = false
end)

MinBtn.MouseButton1Click:Connect(function()
	MainFrame.Visible = false
	FloatButton.Visible = true
end)

-- Funções
local speedOn = false
local ultraOn = false
local jumpOn = false

SpeedBtn.MouseButton1Click:Connect(function()
	speedOn = not speedOn
	ultraOn = false
	SpeedBtn.Text = "Velocidade Rápida: " .. (speedOn and "Ativado" or "Desativado")
	UltraBtn.Text = "Velocidade Ultra: Desativado"
	humanoid.WalkSpeed = speedOn and 50 or 16
end)

UltraBtn.MouseButton1Click:Connect(function()
	ultraOn = not ultraOn
	speedOn = false
	UltraBtn.Text = "Velocidade Ultra: " .. (ultraOn and "Ativado" or "Desativado")
	SpeedBtn.Text = "Velocidade Rápida: Desativado"
	humanoid.WalkSpeed = ultraOn and 80 or 16
end)

JumpBtn.MouseButton1Click:Connect(function()
	jumpOn = not jumpOn
	JumpBtn.Text = "Pulo Infinito: " .. (jumpOn and "Ativado" or "Desativado")
end)

UserInputService.JumpRequest:Connect(function()
	if jumpOn then
		local char = player.Character
		if char then
			local hum = char:FindFirstChildOfClass("Humanoid")
			if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
		end
	end
end)

-- Animação RGB
RunService.RenderStepped:Connect(function()
	local color = getRainbowColor(0)
	MainFrame.BackgroundColor3 = color
	Title.TextColor3 = color
	SpeedBtn.BackgroundColor3 = color
	UltraBtn.BackgroundColor3 = color
	JumpBtn.BackgroundColor3 = color
	MinBtn.BackgroundColor3 = color
	FloatButton.BackgroundColor3 = color
end)