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

-- Função RGB animada
local function getRainbowColor(offset)
	local r = math.sin(tick() + offset) * 127 + 128
	local g = math.sin(tick() + offset + 2) * 127 + 128
	local b = math.sin(tick() + offset + 4) * 127 + 128
	return Color3.fromRGB(r, g, b)
end

-- Botão flutuante redondo com "GP7 MODS"
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
local fcorner = Instance.new("UICorner", FloatButton)
fcorner.CornerRadius = UDim.new(1, 0)

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

-- Botão velocidade rápida
local SpeedBtn = Instance.new("TextButton", MainFrame)
SpeedBtn.Size = UDim2.new(1, -20, 0, 40)
SpeedBtn.Position = UDim2.new(0, 10, 0, 40)
SpeedBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
SpeedBtn.Text = "Velocidade Rápida (50)"
SpeedBtn.Font = Enum.Font.SourceSansBold
SpeedBtn.TextSize = 16
SpeedBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", SpeedBtn).CornerRadius = UDim.new(0, 8)

-- Botão velocidade ultra
local UltraBtn = Instance.new("TextButton", MainFrame)
UltraBtn.Size = UDim2.new(1, -20, 0, 40)
UltraBtn.Position = UDim2.new(0, 10, 0, 90)
UltraBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
UltraBtn.Text = "Velocidade Ultra Rápida (80)"
UltraBtn.Font = Enum.Font.SourceSansBold
UltraBtn.TextSize = 16
UltraBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", UltraBtn).CornerRadius = UDim.new(0, 8)

-- Botão pulo infinito
local JumpBtn = Instance.new("TextButton", MainFrame)
JumpBtn.Size = UDim2.new(1, -20, 0, 40)
JumpBtn.Position = UDim2.new(0, 10, 0, 140)
JumpBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
JumpBtn.Text = "Pulo Infinito"
JumpBtn.Font = Enum.Font.SourceSansBold
JumpBtn.TextSize = 16
JumpBtn.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", JumpBtn).CornerRadius = UDim.new(0, 8)

-- Mostrar/ocultar menu
FloatButton.MouseButton1Click:Connect(function()
	MainFrame.Visible = true
	FloatButton.Visible = false
end)

MinBtn.MouseButton1Click:Connect(function()
	MainFrame.Visible = false
	FloatButton.Visible = true
end)

-- RGB animado em tudo
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