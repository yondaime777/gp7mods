local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer

-- Função para comprar item
local function buyItem(itemName)
    local success, err = pcall(function()
        local net = ReplicatedStorage:WaitForChild("Packages"):WaitForChild("Net"):WaitForChild("RF/CoinsShopService/RequestBuy")
        net:InvokeServer(itemName)
    end)
    if not success then
        warn("Erro ao comprar "..itemName..": "..tostring(err))
    end
end

-- Compra os acessórios necessários ao iniciar
buyItem("Speed Coil")
buyItem("Gravity Coil")

-- Variáveis
local speedEnabled = false
local jumpEnabled = false
local currentSpeed = 50  -- velocidade padrão

-- Cria GUI básica
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "GP7MenuGui"

local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 250, 0, 300)
mainFrame.Position = UDim2.new(0.5, -125, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 0, 0)
mainFrame.BorderSizePixel = 0

local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "GP7 MENU"
title.TextColor3 = Color3.new(1, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

-- Botão Speed Hack
local btnSpeed = Instance.new("TextButton", mainFrame)
btnSpeed.Size = UDim2.new(0.9, 0, 0, 40)
btnSpeed.Position = UDim2.new(0.05, 0, 0, 50)
btnSpeed.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
btnSpeed.TextColor3 = Color3.new(1,1,1)
btnSpeed.Font = Enum.Font.GothamBold
btnSpeed.TextScaled = true
btnSpeed.Text = "Speed Hack: OFF"

-- Slider para ajustar velocidade
local speedSlider = Instance.new("TextBox", mainFrame)
speedSlider.Size = UDim2.new(0.9, 0, 0, 30)
speedSlider.Position = UDim2.new(0.05, 0, 0, 95)
speedSlider.BackgroundColor3 = Color3.fromRGB(20,20,20)
speedSlider.TextColor3 = Color3.new(1,1,1)
speedSlider.Font = Enum.Font.Gotham
speedSlider.TextScaled = true
speedSlider.Text = tostring(currentSpeed)

-- Botão Pulo Infinito
local btnJump = Instance.new("TextButton", mainFrame)
btnJump.Size = UDim2.new(0.9, 0, 0, 40)
btnJump.Position = UDim2.new(0.05, 0, 0, 140)
btnJump.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
btnJump.TextColor3 = Color3.new(1,1,1)
btnJump.Font = Enum.Font.GothamBold
btnJump.TextScaled = true
btnJump.Text = "Pulo Infinito: OFF"

-- Funções para ativar/desativar
local function setSpeed(value)
    if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
        player.Character.Humanoid.WalkSpeed = value
    end
end

local function setJump(enabled)
    if player.Character and player.Character:FindFirstChildOfClass("Humanoid") then
        player.Character.Humanoid.JumpPower = enabled and 100 or 50 -- 100 alto, 50 padrão
    end
end

btnSpeed.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    if speedEnabled then
        local val = tonumber(speedSlider.Text)
        if val and val >= 16 and val <= 200 then
            currentSpeed = val
        end
        setSpeed(currentSpeed)
        btnSpeed.Text = "Speed Hack: ON"
    else
        setSpeed(16) -- padrão Roblox
        btnSpeed.Text = "Speed Hack: OFF"
    end
end)

speedSlider.FocusLost:Connect(function()
    local val = tonumber(speedSlider.Text)
    if val and val >= 16 and val <= 200 then
        currentSpeed = val
        if speedEnabled then
            setSpeed(currentSpeed)
        end
    else
        speedSlider.Text = tostring(currentSpeed)
    end
end)

btnJump.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    setJump(jumpEnabled)
    btnJump.Text = "Pulo Infinito: "..(jumpEnabled and "ON" or "OFF")
end)

-- Reseta ao morrer
player.CharacterAdded:Connect(function(char)
    wait(1)
    if speedEnabled then
        setSpeed(currentSpeed)
    end
    if jumpEnabled then
        setJump(true)
    end
end)