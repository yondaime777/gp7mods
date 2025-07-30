local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")

-- Criar GUI principal
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "GP7_MODS"
ScreenGui.ResetOnSpawn = false

-- Cores RGB animadas
local function getRainbowColor(timeOffset)
	local r = math.sin(tick() + timeOffset) * 127 + 128
	local g = math.sin(tick() + timeOffset + 2) * 127 + 128
	local b = math.sin(tick() + timeOffset + 4) * 127 + 128
	return Color3.fromRGB(r, g, b)
end

-- Botão flutuante para abrir o menu
local FloatButton = Instance.new("TextButton", ScreenGui)
FloatButton.Size = UDim2.new(0, 120, 0, 40)
FloatButton.Position = UDim2.new(0, 20, 0.5, -100)
FloatButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
FloatButton.Text = "GP7 MODS"
FloatButton.TextColor3 = Color3.new(1,1,1)
FloatButton.Font = Enum.Font.SourceSansBold
FloatButton.TextSize = 16
FloatButton.Draggable = true
FloatButton.Active = true

-- Menu principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 240, 0, 180)
MainFrame.Position = UDim2.new(0.5, -120, 0.5, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true

-- Título com efeito RGB
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundTransparency = 1
Title.Text = "GP7 MODS"
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20
Title.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Botão minimizar
local MinBtn = Instance.new("TextButton", MainFrame)
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -35, 0, 0)
MinBtn.Text = "-"
MinBtn.Font = Enum.Font.SourceSansBold
MinBtn.TextSize = 20
MinBtn.TextColor3 = Color3.new(1,1,1)
MinBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)

-- Botão Velocidade Rápida
local FastSpeed = Instance.new("TextButton", MainFrame)
FastSpeed.Size = UDim2.new(1, -20, 0, 40)
FastSpeed.Position = UDim2.new(0, 10, 0, 40)
FastSpeed.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
FastSpeed.Text = "Velocidade Rápida (50)"
FastSpeed.Font = Enum.Font.SourceSansBold
FastSpeed.TextSize = 16
FastSpeed.TextColor3 = Color3.new(1,1,1)

-- Botão Velocidade Ultra
local UltraSpeed = Instance.new("TextButton", MainFrame)
UltraSpeed.Size = UDim2.new(1, -20, 0, 40)
UltraSpeed.Position = UDim2.new(0, 10, 0, 90)
UltraSpeed.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
UltraSpeed.Text = "Velocidade Ultra (80)"
UltraSpeed.Font = Enum.Font.SourceSansBold
UltraSpeed.TextSize = 16
UltraSpeed.TextColor3 = Color3.new(1,1,1)

-- Botão Pulo Infinito
local InfJump = Instance.new("TextButton", MainFrame)
InfJump.Size = UDim2.new(1, -20, 0, 40)
InfJump.Position = UDim2.new(0, 10, 0, 140)
InfJump.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
InfJump.Text = "Ativar Pulo Infinito"
InfJump.Font = Enum.Font.SourceSansBold
InfJump.TextSize = 16
InfJump.TextColor3 = Color3.new(1,1,1)

-- Abrir/Fechar menu
FloatButton.MouseButton1Click:Connect(function()
	MainFrame.Visible = not MainFrame.Visible
end)

MinBtn.MouseButton1Click:Connect(function()
	MainFrame.Visible = false
end)

-- Botões de velocidade
FastSpeed.MouseButton1Click:Connect(function()
	local char = player.Character or player.CharacterAdded:Wait()
	local hum = char:WaitForChild("Humanoid")
	hum.WalkSpeed = 50
end)

UltraSpeed.MouseButton1Click:Connect(function()
	local char = player.Character or player.CharacterAdded:Wait()
	local hum = char:WaitForChild("Humanoid")
	hum.WalkSpeed = 80
end)

-- Pulo Infinito
local infiniteJump = false
InfJump.MouseButton1Click:Connect(function()
	infiniteJump = not infiniteJump
	InfJump.Text = infiniteJump and "Pulo Infinito ON" or "Pulo Infinito OFF"
end)

UserInputService.JumpRequest:Connect(function()
	if infiniteJump then
		local char = player.Character or player.CharacterAdded:Wait()
		local hum = char:FindFirstChildOfClass("Humanoid")
		if hum then hum:ChangeState("Jumping") end
	end
end)

-- Atualizar título RGB
RunService.RenderStepped:Connect(function()
	Title.TextColor3 = getRainbowColor(0)
end)