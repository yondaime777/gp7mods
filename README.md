-- GP7 MODS - Steal a Brainrot
-- Menu verde e preto, botão flutuante com minimizar
-- Funções: Speed Hack, Pulo Infinito, ESP verde (nome mesmo invisível)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Variables
local speedEnabled = false
local jumpEnabled = false
local espEnabled = false

local UIS = game:GetService("UserInputService")

-- Create GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GP7MODSGUI"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 280, 0, 180)
MainFrame.Position = UDim2.new(0.5, -140, 0.5, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 50, 0)
MainFrame.BorderColor3 = Color3.fromRGB(0, 255, 0)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Title
local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
Title.BorderSizePixel = 0
Title.Text = "GP7 MODS"
Title.TextColor3 = Color3.fromRGB(0, 255, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 24
Title.Parent = MainFrame

-- Minimize Button
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Name = "MinimizeBtn"
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -30, 0, 0)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextSize = 20
MinimizeBtn.Parent = MainFrame

-- Buttons Container
local ButtonsFrame = Instance.new("Frame")
ButtonsFrame.Name = "ButtonsFrame"
ButtonsFrame.Size = UDim2.new(1, -20, 1, -40)
ButtonsFrame.Position = UDim2.new(0, 10, 0, 35)
ButtonsFrame.BackgroundTransparency = 1
ButtonsFrame.Parent = MainFrame

-- Speed Hack Button
local SpeedBtn = Instance.new("TextButton")
SpeedBtn.Name = "SpeedBtn"
SpeedBtn.Size = UDim2.new(1, 0, 0, 40)
SpeedBtn.Position = UDim2.new(0, 0, 0, 0)
SpeedBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
SpeedBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
SpeedBtn.Font = Enum.Font.GothamBold
SpeedBtn.TextSize = 22
SpeedBtn.Text = "Speed Hack: OFF"
SpeedBtn.Parent = ButtonsFrame

-- Jump Hack Button
local JumpBtn = Instance.new("TextButton")
JumpBtn.Name = "JumpBtn"
JumpBtn.Size = UDim2.new(1, 0, 0, 40)
JumpBtn.Position = UDim2.new(0, 0, 0, 45)
JumpBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
JumpBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
JumpBtn.Font = Enum.Font.GothamBold
JumpBtn.TextSize = 22
JumpBtn.Text = "Pulo Infinito: OFF"
JumpBtn.Parent = ButtonsFrame

-- ESP Button
local ESPBtn = Instance.new("TextButton")
ESPBtn.Name = "ESPBtn"
ESPBtn.Size = UDim2.new(1, 0, 0, 40)
ESPBtn.Position = UDim2.new(0, 0, 0, 90)
ESPBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
ESPBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
ESPBtn.Font = Enum.Font.GothamBold
ESPBtn.TextSize = 22
ESPBtn.Text = "ESP Verde: OFF"
ESPBtn.Parent = ButtonsFrame

-- Floating Button
local FloatBtn = Instance.new("TextButton")
FloatBtn.Name = "FloatBtn"
FloatBtn.Size = UDim2.new(0, 60, 0, 30)
FloatBtn.Position = UDim2.new(0, 10, 0.7, 0)
FloatBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
FloatBtn.Text = "GP7"
FloatBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
FloatBtn.Font = Enum.Font.GothamBold
FloatBtn.TextSize = 20
FloatBtn.Parent = ScreenGui
FloatBtn.Active = true
FloatBtn.Draggable = true

-- Minimize / Maximize functionality
local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    if minimized == false then
        MainFrame.Size = UDim2.new(0, 280, 0, 30)
        ButtonsFrame.Visible = false
        minimized = true
    else
        MainFrame.Size = UDim2.new(0, 280, 0, 180)
        ButtonsFrame.Visible = true
        minimized = false
    end
end)

-- Show/hide main menu by floating button
FloatBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Função Speed Hack
SpeedBtn.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    SpeedBtn.Text = "Speed Hack: " .. (speedEnabled and "ON" or "OFF")
    if speedEnabled then
        -- Aplicar velocidade maior
        LocalPlayer.Character.Humanoid.WalkSpeed = 40
    else
        -- Resetar velocidade padrão
        LocalPlayer.Character.Humanoid.WalkSpeed = 16
    end
end)

-- Função Pulo infinito
JumpBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    JumpBtn.Text = "Pulo Infinito: " .. (jumpEnabled and "ON" or "OFF")
end)

-- ESP
local espLabels = {}

local function createESP(plr)
    if espLabels[plr] then return end -- Já existe

    local head = plr.Character and plr.Character:FindFirstChild("Head")
    if head then
        local Billboard = Instance.new("BillboardGui")
        Billboard.Name = "GP7ESP"
        Billboard.Adornee = head
        Billboard.Size = UDim2.new(0, 100, 0, 25)
        Billboard.StudsOffset = Vector3.new(0, 2.5, 0)
        Billboard.AlwaysOnTop = true
        Billboard.Parent = head

        local Label = Instance.new("TextLabel")
        Label.BackgroundTransparency = 1
        Label.Size = UDim2.new(1, 0, 1, 0)
        Label.TextColor3 = Color3.fromRGB(0, 255, 0)
        Label.TextStrokeTransparency = 0
        Label.Text = plr.Name
        Label.Font = Enum.Font.GothamBold
        Label.TextSize = 20
        Label.Parent = Billboard

        espLabels[plr] = Billboard
    end
end

local function removeESP(plr)
    if espLabels[plr] then
        espLabels[plr]:Destroy()
        espLabels[plr] = nil
    end
end

ESPBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    ESPBtn.Text = "ESP Verde: " .. (espEnabled and "ON" or "OFF")
    if not espEnabled then
        -- remover todos ESP
        for plr, gui in pairs(espLabels) do
            gui:Destroy()
        end
        espLabels = {}
    else
        -- criar ESP para todos os players
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer then
                createESP(plr)
            end
        end
    end
end)

-- Atualiza ESP quando jogador entra e sai
Players.PlayerAdded:Connect(function(plr)
    if espEnabled then
        plr.CharacterAdded:Connect(function()
            wait(1)
            createESP(plr)
        end)
    end
end)

Players.PlayerRemoving:Connect(function(plr)
    removeESP(plr)
end)

-- Pulo infinito implementação
RunService.Heartbeat:Connect(function()
    if jumpEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") then
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if UIS:IsKeyDown(Enum.KeyCode.Space) then
            if humanoid:GetState() == Enum.HumanoidStateType.Freefall or humanoid:GetState() == Enum.HumanoidStateType.Landed or humanoid:GetState() == Enum.HumanoidStateType.Running then
                humanoid.Jump = true
            end
        end
    end
end)

-- Speed hack manutenção (se o personagem respawnar, aplica velocidade novamente)
LocalPlayer.CharacterAdded:Connect(function(char)
    wait(1)
    if speedEnabled and char:FindFirstChildOfClass("Humanoid") then
        char:FindFirstChildOfClass("Humanoid").WalkSpeed = 40
    end
end)

-- ESP funciona mesmo se invisível
-- O truque é usar BillboardGui parented na cabeça, que é visível para todos, mesmo com invisibilidade (capa)
-- O script já adiciona ESP nos jogadores, sem bloquear pela invisibilidade

-- Final
print("GP7 MODS carregado! Use o botão flutuante para abrir/fechar o menu.")