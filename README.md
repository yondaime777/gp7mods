-- GP7 MENU v2 - ESP, Speed Hack e Infinite Jump

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Criar GUI
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 220)
frame.Position = UDim2.new(0, 20, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 0
frame.BackgroundTransparency = 0.3

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "GP7 MODS"
title.TextColor3 = Color3.fromRGB(0, 255, 0)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

-- Botão flutuante minimizar
local floatButton = Instance.new("TextButton", gui)
floatButton.Text = "GP7"
floatButton.Size = UDim2.new(0, 50, 0, 30)
floatButton.Position = UDim2.new(0, 10, 0, 10)
floatButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
floatButton.Visible = false

floatButton.MouseButton1Click:Connect(function()
	frame.Visible = true
	floatButton.Visible = false
end)

local function createButton(text, callback, posY)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, posY)
	btn.Text = text
	btn.TextColor3 = Color3.fromRGB(0, 255, 0)
	btn.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
	btn.Font = Enum.Font.GothamBold
	btn.TextScaled = true
	btn.MouseButton1Click:Connect(callback)
end

-- Minimizar botão
createButton("Minimizar", function()
	frame.Visible = false
	floatButton.Visible = true
end, 0.15)

-- ESP verde com caixa e nome
createButton("ESP Verde", function()
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local box = Instance.new("BoxHandleAdornment", plr.Character)
			box.Adornee = plr.Character:FindFirstChild("HumanoidRootPart")
			box.Size = Vector3.new(6, 9, 3)
			box.Color3 = Color3.fromRGB(0, 255, 0)
			box.Transparency = 0.5
			box.AlwaysOnTop = true

			local billboard = Instance.new("BillboardGui", plr.Character)
			billboard.Adornee = plr.Character:FindFirstChild("Head")
			billboard.Size = UDim2.new(0, 200, 0, 50)
			billboard.StudsOffset = Vector3.new(0, 2, 0)
			billboard.AlwaysOnTop = true

			local name = Instance.new("TextLabel", billboard)
			name.Size = UDim2.new(1, 0, 1, 0)
			name.Text = plr.Name
			name.TextColor3 = Color3.fromRGB(0, 255, 0)
			name.BackgroundTransparency = 1
			name.Font = Enum.Font.GothamBold
			name.TextScaled = true
		end
	end
end, 0.35)

-- Infinite Jump
createButton("Pulo Infinito", function()
	local jumping = false
	game:GetService("UserInputService").JumpRequest:Connect(function()
		if jumping then
			LocalPlayer.Character:FindFirstChildOfClass("Humanoid"):ChangeState("Jumping")
		end
	end)
	jumping = true
end, 0.55)

-- Speed Hack via Speed Coil
createButton("Speed Hack", function()
	local backpack = LocalPlayer:WaitForChild("Backpack")
	local function buyCoil()
		for _, obj in pairs(workspace:GetDescendants()) do
			if obj:IsA("ClickDetector") and obj.Parent:FindFirstChild("TouchInterest") == nil then
				fireclickdetector(obj)
			end
		end
	end

	buyCoil()
	wait(0.5)

	local function getCoil()
		for _, tool in pairs(backpack:GetChildren()) do
			if tool:IsA("Tool") and tool.Name:lower():find("speed") then
				return tool
			end
		end
	end

	local coil = getCoil()
	if coil then
		coil.Parent = LocalPlayer.Character
		wait(0.5)
		coil.Parent = backpack
	end

	LocalPlayer.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = 40
end, 0.75)