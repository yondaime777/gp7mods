local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer

local speedCoilName = "Speed Coil"
local gravityCoilName = "Gravity Coil"

local function buyItem(itemName)
    local success, err = pcall(function()
        local net = ReplicatedStorage:WaitForChild("Packages"):WaitForChild("Net"):WaitForChild("RF/CoinsShopService/RequestBuy")
        net:InvokeServer(itemName)
    end)
    if not success then
        warn("Erro ao comprar "..itemName..": "..tostring(err))
    end
end

local function equipItem(itemName)
    local backpack = player:WaitForChild("Backpack")
    local character = player.Character
    if not backpack or not character then return end

    local item = backpack:FindFirstChild(itemName)
    if item then
        player.Character.Humanoid:EquipTool(item)
    end
end

local speedEnabled = false
local jumpEnabled = false

local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "GP7MenuGui"

local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 220, 0, 150)
mainFrame.Position = UDim2.new(0.5, -110, 0.5, -75)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 0, 0)
mainFrame.BorderSizePixel = 0

local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "GP7 MENU"
title.TextColor3 = Color3.new(1, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

local btnSpeed = Instance.new("TextButton", mainFrame)
btnSpeed.Size = UDim2.new(0.9, 0, 0, 40)
btnSpeed.Position = UDim2.new(0.05, 0, 0, 50)
btnSpeed.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
btnSpeed.TextColor3 = Color3.new(1,1,1)
btnSpeed.Font = Enum.Font.GothamBold
btnSpeed.TextScaled = true
btnSpeed.Text = "Speed Hack: OFF"

local btnJump = Instance.new("TextButton", mainFrame)
btnJump.Size = UDim2.new(0.9, 0, 0, 40)
btnJump.Position = UDim2.new(0.05, 0, 0, 100)
btnJump.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
btnJump.TextColor3 = Color3.new(1,1,1)
btnJump.Font = Enum.Font.GothamBold
btnJump.TextScaled = true
btnJump.Text = "Pulo Infinito: OFF"

local function applySpeed()
    if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
        player.Character.Humanoid.WalkSpeed = 50
    end
end

local function applyJump()
    if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
        player.Character.Humanoid.JumpPower = 100
    end
end

btnSpeed.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    if speedEnabled then
        buyItem(speedCoilName)
        wait(0.5) -- espera o item entrar no backpack
        equipItem(speedCoilName)
        applySpeed()
        btnSpeed.Text = "Speed Hack: ON"
    else
        if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
            player.Character.Humanoid.WalkSpeed = 16
        end
        btnSpeed.Text = "Speed Hack: OFF"
    end
end)

btnJump.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    if jumpEnabled then
        buyItem(gravityCoilName)
        wait(0.5)
        equipItem(gravityCoilName)
        applyJump()
        btnJump.Text = "Pulo Infinito: ON"
    else
        if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
            player.Character.Humanoid.JumpPower = 50
        end
        btnJump.Text = "Pulo Infinito: OFF"
    end
end)

player.CharacterAdded:Connect(function()
    wait(1)
    if speedEnabled then
        equipItem(speedCoilName)
        applySpeed()
    end
    if jumpEnabled then
        equipItem(gravityCoilName)
        applyJump()
    end
end)