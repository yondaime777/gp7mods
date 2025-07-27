-- GP7 MENU - COPIA NÃO COMÉDIA

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Função para comprar item via RemoteFunction
local function buyItem(name)
    local success, err = pcall(function()
        game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("Net")
          :WaitForChild("RF/CoinsShopService/RequestBuy")
          :InvokeServer(name)
    end)
    if not success then warn("Erro ao comprar "..name..": "..tostring(err)) end
end

-- Variáveis de controle
local speedActive = false
local jumpActive = false

-- GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GP7MenuGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Fundo do menu
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 280, 0, 320)
mainFrame.Position = UDim2.new(0.5, -140, 0.5, -160)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 0, 0)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.Visible = true

-- Barra de título com texto "COPIA NÃO COMÉDIA"
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
titleBar.Parent = mainFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 1, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "GP7 MENU - COPIA NÃO COMÉDIA"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.TextScaled = true
titleLabel.Parent = titleBar

-- Função para criar botões
local function createButton(text, posY)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 240, 0, 40)
    btn.Position = UDim2.new(0, 20, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
    btn.BorderSizePixel = 0
    btn.Font = Enum.Font.GothamBold
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.TextScaled = true
    btn.Text = text
    btn.Parent = mainFrame
    return btn
end

-- Status label
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 20)
statusLabel.Position = UDim2.new(0, 0, 1, -20)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.new(1, 1, 1)
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextScaled = true
statusLabel.Text = ""
statusLabel.Parent = mainFrame

-- Botão minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -30, 0, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
minimizeBtn.Text = "-"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.TextScaled = true
minimizeBtn.Parent = titleBar

local isMinimized = false

minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    for _, child in pairs(mainFrame:GetChildren()) do
        if child ~= titleBar and child ~= minimizeBtn then
            child.Visible = not isMinimized
        end
    end
    if isMinimized then
        minimizeBtn.Text = "+"
    else
        minimizeBtn.Text = "-"
    end
end)

-- Botão flutuante para abrir menu quando minimizado
local floatBtn = Instance.new("TextButton")
floatBtn.Size = UDim2.new(0, 40, 0, 40)
floatBtn.Position = UDim2.new(0, 10, 0.5, -20)
floatBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
floatBtn.Text = "GP7"
floatBtn.Font = Enum.Font.GothamBold
floatBtn.TextColor3 = Color3.new(1,1,1)
floatBtn.TextScaled = true
floatBtn.Visible = false
floatBtn.Parent = screenGui

floatBtn.Active = true
floatBtn.Draggable = true

floatBtn.MouseButton1Click:Connect(function()
    isMinimized = false
    mainFrame.Visible = true
    for _, child in pairs(mainFrame:GetChildren()) do
        if child ~= titleBar and child ~= minimizeBtn then
            child.Visible = true
        end
    end
    floatBtn.Visible = false
end)

minimizeBtn.MouseButton1Click:Connect(function()
    if isMinimized then
        mainFrame.Visible = false
        floatBtn.Visible = true
    else
        floatBtn.Visible = false
        mainFrame.Visible = true
    end
end)

-- Slider para Speed Hack
local speedValue = 1

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(0, 240, 0, 20)
speedLabel.Position = UDim2.new(0, 20, 0, 80)
speedLabel.BackgroundTransparency = 1
speedLabel.TextColor3 = Color3.new(1, 1, 1)
speedLabel.Font = Enum.Font.GothamBold
speedLabel.TextScaled = true
speedLabel.Text = "Speed Hack: "..speedValue
speedLabel.Parent = mainFrame

local speedSlider = Instance.new("TextBox")
speedSlider.Size = UDim2.new(0, 240, 0, 30)
speedSlider.Position = UDim2.new(0, 20, 0, 100)
speedSlider.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
speedSlider.TextColor3 = Color3.new(1, 1, 1)
speedSlider.Font = Enum.Font.GothamBold
speedSlider.TextScaled = true
speedSlider.Text = tostring(speedValue)
speedSlider.ClearTextOnFocus = false
speedSlider.Parent = mainFrame

speedSlider.FocusLost:Connect(function(enter)
    local val = tonumber(speedSlider.Text)
    if val and val >= 1 and val <= 10 then
        speedValue = val
        speedLabel.Text = "Speed Hack: "..speedValue
        if speedActive then
            player.Character.Humanoid.WalkSpeed = 16 * speedValue
        end
    else
        speedSlider.Text = tostring(speedValue)
    end
end)

-- Botão Speed Hack
local speedBtn = createButton("Ativar Speed Hack", 140)
speedBtn.MouseButton1Click:Connect(function()
    if not speedActive then
        buyItem("Speed Coil")
        wait(1)
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.WalkSpeed = 16 * speedValue
            speedActive = true
            speedBtn.Text = "Desativar Speed Hack"
            statusLabel.Text = "Speed Hack ativado"
        end
    else
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.WalkSpeed = 16
            speedActive = false
            speedBtn.Text = "Ativar Speed Hack"
            statusLabel.Text = "Speed Hack desativado"
        end
    end
end)

-- Slider para Super Pulo
local jumpValue = 50

local jumpLabel = Instance.new("TextLabel")
jumpLabel.Size = UDim2.new(0, 240, 0, 20)
jumpLabel.Position = UDim2.new(0, 20, 0, 190)
jumpLabel.BackgroundTransparency = 1
jumpLabel.TextColor3 = Color3.new(1, 1, 1)
jumpLabel.Font = Enum.Font.GothamBold
jumpLabel.TextScaled = true
jumpLabel.Text = "Super Pulo: "..jumpValue
jumpLabel.Parent = mainFrame

local jumpSlider = Instance.new("TextBox")
jumpSlider.Size = UDim2.new(0, 240, 0, 30)
jumpSlider.Position = UDim2.new(0, 20, 0, 210)
jumpSlider.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
jumpSlider.TextColor3 = Color3.new(1, 1, 1)
jumpSlider.Font = Enum.Font.GothamBold
jumpSlider.TextScaled = true
jumpSlider.Text = tostring(jumpValue)
jumpSlider.ClearTextOnFocus = false
jumpSlider.Parent = mainFrame

jumpSlider.FocusLost:Connect(function(enter)
    local val = tonumber(jumpSlider.Text)
    if val and val >= 10 and val <= 100 then
        jumpValue = val
        jumpLabel.Text = "Super Pulo: "..jumpValue
        if jumpActive then
            player.Character.Humanoid.JumpPower = jumpValue
        end
    else
        jumpSlider.Text = tostring(jumpValue)
    end
end)

-- Botão Super Pulo
local jumpBtn = createButton("Ativar Super Pulo", 260)
jumpBtn.MouseButton1Click:Connect(function()
    if not jumpActive then
        buyItem("Gravity Coil")
        wait(1)
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.JumpPower = jumpValue
            jumpActive = true
            jumpBtn.Text = "Desativar Super Pulo"
            statusLabel.Text = "Super Pulo ativado"
        end
    else
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.JumpPower = 50
            jumpActive = false
            jumpBtn.Text = "Ativar Super Pulo"
            statusLabel.Text = "Super Pulo desativado"
        end
    end
end)