local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Estado
local speedActive = false
local jumpActive = false
local speedValue = 50 -- valor inicial
local jumpValue = 100 -- valor inicial

-- Função pra comprar item (invoca o Remote)
local function buyItem(name)
    local success, err = pcall(function()
        game:GetService("ReplicatedStorage"):WaitForChild("Packages")
            :WaitForChild("Net"):WaitForChild("RF/CoinsShopService/RequestBuy")
            :InvokeServer(name)
    end)
    if not success then warn("Falha ao comprar:", err) end
end

-- Atualiza valores do Humanoid se ativo
local function updateSpeed()
    if speedActive and player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = speedValue
    end
end

local function updateJump()
    if jumpActive and player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.JumpPower = jumpValue
    end
end

-- GUI básica
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
screenGui.Name = "GP7MenuGui"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 260, 0, 150)
frame.Position = UDim2.new(0, 50, 0, 50)
frame.BackgroundColor3 = Color3.fromRGB(25, 0, 0)
frame.BorderColor3 = Color3.fromRGB(150, 0, 0)
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "GP7 MENU"
title.TextColor3 = Color3.new(1, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

local statusLabel = Instance.new("TextLabel", frame)
statusLabel.Size = UDim2.new(1, -20, 0, 25)
statusLabel.Position = UDim2.new(0, 10, 0, 30)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.new(1, 1, 1)
statusLabel.Text = "Status: Desativado"
statusLabel.TextScaled = true
statusLabel.Font = Enum.Font.GothamBold

-- Função para criar botão
local function createButton(parent, text, posY)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0.4, 0, 0, 30)
    btn.Position = UDim2.new(0.05, 0, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    btn.Text = text
    return btn
end

-- Função para criar slider (basicão)
local function createSlider(parent, posY, min, max, initial, labelText)
    local label = Instance.new("TextLabel", parent)
    label.Size = UDim2.new(0.9, 0, 0, 20)
    label.Position = UDim2.new(0.05, 0, 0, posY)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1,1,1)
    label.Font = Enum.Font.GothamBold
    label.TextScaled = true
    label.Text = labelText .. ": " .. tostring(initial)
    
    local box = Instance.new("TextBox", parent)
    box.Size = UDim2.new(0.9, 0, 0, 20)
    box.Position = UDim2.new(0.05, 0, 0, posY + 20)
    box.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
    box.TextColor3 = Color3.new(1,1,1)
    box.Font = Enum.Font.GothamBold
    box.TextScaled = true
    box.Text = tostring(initial)
    
    return label, box
end

-- Criar botões e sliders
local speedBtn = createButton(frame, "Ativar Speed Hack", 60)
local jumpBtn = createButton(frame, "Ativar Super Pulo", 100)

local speedLabel, speedSlider = createSlider(frame, 140, 16, 100, speedValue, "Velocidade")
local jumpLabel, jumpSlider = createSlider(frame, 180, 50, 200, jumpValue, "Pulo")

-- Eventos sliders
speedSlider.FocusLost:Connect(function()
    local val = tonumber(speedSlider.Text)
    if val and val >= 16 and val <= 100 then
        speedValue = val
        speedLabel.Text = "Velocidade: " .. speedValue
        updateSpeed()
    else
        speedSlider.Text = tostring(speedValue)
    end
end)

jumpSlider.FocusLost:Connect(function()
    local val = tonumber(jumpSlider.Text)
    if val and val >= 50 and val <= 200 then
        jumpValue = val
        jumpLabel.Text = "Pulo: " .. jumpValue
        updateJump()
    else
        jumpSlider.Text = tostring(jumpValue)
    end
end)

-- Eventos botões
speedBtn.MouseButton1Click:Connect(function()
    if not speedActive then
        buyItem("Speed Coil")
        wait(1)
        speedActive = true
        speedBtn.Text = "Desativar Speed Hack"
        statusLabel.Text = "Speed Hack ativado"
        updateSpeed()
    else
        speedActive = false
        speedBtn.Text = "Ativar Speed Hack"
        statusLabel.Text = "Speed Hack desativado"
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.WalkSpeed = 16
        end
    end
end)

jumpBtn.MouseButton1Click:Connect(function()
    if not jumpActive then
        buyItem("Gravity Coil")
        wait(1)
        jumpActive = true
        jumpBtn.Text = "Desativar Super Pulo"
        statusLabel.Text = "Super Pulo ativado"
        updateJump()
    else
        jumpActive = false
        jumpBtn.Text = "Ativar Super Pulo"
        statusLabel.Text = "Super Pulo desativado"
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.JumpPower = 50
        end
    end
end)