-- GP7 MENU - Script para Steal a Brainrot
-- Feito para Delta Exploit - by ChatGPT

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- GUI
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "GP7_MENU"

local DragFrame = Instance.new("Frame", ScreenGui)
DragFrame.Size = UDim2.new(0, 220, 0, 230)
DragFrame.Position = UDim2.new(0, 100, 0, 100)
DragFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
DragFrame.BorderSizePixel = 0
DragFrame.Active = true
DragFrame.Draggable = true

local Title = Instance.new("TextLabel", DragFrame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Text = "🔥 GP7 MENU 🔥"
Title.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16

local function createButton(name, posY, callback)
	local btn = Instance.new("TextButton", DragFrame)
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, posY)
	btn.Text = name
	btn.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.MouseButton1Click:Connect(callback)
	return btn
end

-- ESP
local espOn = false
local espFolder = Instance.new("Folder", game.CoreGui)
espFolder.Name = "GP7_ESP"

local function createESP(player)
	if player == LocalPlayer then return end
	local head = player:FindFirstChild("Head")
	if head and not espFolder:FindFirstChild(player.Name) then
		local billboard = Instance.new("BillboardGui", espFolder)
		billboard.Name = player.Name
		billboard.Adornee = head
		billboard.Size = UDim2.new(0, 100, 0, 20)
		billboard.StudsOffset = Vector3.new(0, 2.5, 0)
		billboard.AlwaysOnTop = true

		local nameLabel = Instance.new("TextLabel", billboard)
		nameLabel.Size = UDim2.new(1, 0, 1, 0)
		nameLabel.BackgroundTransparency = 1
		nameLabel.Text = player.Name
		nameLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
		nameLabel.TextStrokeTransparency = 0
		nameLabel.TextSize = 14
	end
end

local function toggleESP()
	espOn = not espOn
	if espOn then
		for _, p in pairs(Players:GetPlayers()) do
			createESP(p)
		end
		Players.PlayerAdded:Connect(createESP)
	else
		espFolder:ClearAllChildren()
	end
end

-- Speed Hack
local speedEnabled = false
local function toggleSpeed()
	speedEnabled = not speedEnabled
	if speedEnabled then
		local args = { "Speed Coil" }
		game:GetService("ReplicatedStorage")
			:WaitForChild("Packages")
			:WaitForChild("Net")
			:WaitForChild("RF/CoinsShopService/RequestBuy")
			:InvokeServer(unpack(args))
	end
end

-- Pulo Infinito
local jumpEnabled = false
local function toggleJump()
	jumpEnabled = not jumpEnabled
	if jumpEnabled then
		local args = { "Gravity Coil" }
		game:GetService("ReplicatedStorage")
			:WaitForChild("Packages")
			:WaitForChild("Net")
			:WaitForChild("RF/CoinsShopService/RequestBuy")
			:InvokeServer(unpack(args))
	end
end

-- Botões
createButton("🔴 Ativar ESP (vermelho)", 40, toggleESP)
createButton("⚡ Speed Hack", 80, toggleSpeed)
createButton("🚀 Pulo Infinito", 120, toggleJump)

-- Minimizar
local minimized = false
local minimizeBtn = createButton("🔽 Minimizar", 160, function()
	minimized = not minimized
	for _, v in ipairs(DragFrame:GetChildren()) do
		if v:IsA("TextButton") and v ~= minimizeBtn then
			v.Visible = not minimized
		end
	end
	minimizeBtn.Text = minimized and "🔼 Maximizar" or "🔽 Minimizar"
end)