local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

-- Estado das funções
local speedEnabled = false
local jumpEnabled = false
local espEnabled = false

local espLabels = {}
local espBoxes = {}

local speedVelocityValue = 100 -- velocidade para Velocity no speed hack
local jumpPowerDefault = 50

-- Função para criar ESP com caixa maior
local function createESP(plr)
    if espLabels[plr] or espBoxes[plr] then return end

    local character = plr.Character
    if not character then return end
    local head = character:FindFirstChild("Head")
    local hrp = character:FindFirstChild("HumanoidRootPart")

    if head then
        local Billboard = Instance.new("BillboardGui")
        Billboard.Name = "GP7ESPName"
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

    if hrp then
        local box = Instance.new("BoxHandleAdornment")
        box.Name = "GP7ESPBox"
        box.Adornee = hrp
        box.AlwaysOnTop = true
        box.ZIndex = 10
        box.Size = Vector3.new(5, 9, 3) -- caixa maior
        box.Color3 = Color3.fromRGB(0, 255, 0)
        box.Transparency = 0.5
        box.Parent = hrp

        espBoxes[plr] = box
    end
end

local function removeESP(plr)
    if espLabels[plr] then
        espLabels[plr]:Destroy()
        espLabels[plr] = nil
    end
    if espBoxes[plr] then
        espBoxes[plr]:Destroy()
        espBoxes[plr] = nil
    end
end

local function toggleESP(enabled)
    if not enabled then
        for plr, _ in pairs(espLabels) do
            removeESP(plr)
        end
        espLabels = {}
        espBoxes = {}
    else
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer then
                createESP(plr)
                plr.CharacterAdded:Connect(function()
                    wait(1)
                    if espEnabled then
                        createESP(plr)
                    end
                end)
            end
        end
    end
end

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GP7MODSGUI"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Frame Principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 280, 0, 180)
MainFrame.Position = UDim2.new(0.5, -140, 0.5, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 50, 0)
MainFrame.BorderColor3 = Color3.fromRGB(0, 255, 0)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

-- Título
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

-- Botão Minimizar
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

-- Container dos botões
local ButtonsFrame = Instance.new("Frame")
ButtonsFrame.Name = "ButtonsFrame"
ButtonsFrame.Size = UDim2.new(1, -20, 1, -40)
ButtonsFrame.Position = UDim2.new(0, 10, 0, 35)
ButtonsFrame.BackgroundTransparency = 1
ButtonsFrame.Parent = MainFrame

-- Botão Speed Hack
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

-- Botão Pulo Infinito
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

-- Botão ESP Verde
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

-- Botão flutuante
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

-- Função minimizar/maximizar menu
local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    if not minimized then
        MainFrame.Size = UDim2.new(0, 280, 0, 30)
        ButtonsFrame.Visible = false
        minimized = true
    else
        MainFrame.Size = UDim2.new(0, 280, 0, 180)
        ButtonsFrame.Visible = true
        minimized = false
    end
end)

-- Mostrar/ocultar menu com botão flutuante
FloatBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Speed Hack e Pulo Infinito via HumanoidRootPart Velocity e JumpPower
RunService.Heartbeat:Connect(function()
    local character = LocalPlayer.Character
    if character then
        local hrp = character:FindFirstChild("HumanoidRootPart")
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if hrp and humanoid then
            if speedEnabled then
                local moveDir = humanoid.MoveDirection
                if moveDir.Magnitude > 0 then
                    hrp.Velocity = Vector3.new(
                        moveDir.X * speedVelocityValue,
                        hrp.Velocity.Y,
                        moveDir.Z * speedVelocityValue
                    )
                else
                    hrp.Velocity = Vector3.new(0, hrp.Velocity.Y, 0)
                end
            else
                hrp.Velocity = Vector3.new(0, hrp.Velocity.Y, 0)
                humanoid.WalkSpeed = 16
            end

            if jumpEnabled then
                humanoid.JumpPower = 100
            else
                humanoid.JumpPower = 50
            end
        end
    end
end)

-- Forçar pulo infinito segurando espaço
RunService.Heartbeat:Connect(function()
    if jumpEnabled and LocalPlayer.Character then
        local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid and UIS:IsKeyDown(Enum.KeyCode.Space) then
            local state = humanoid:GetState()
            if state == Enum.HumanoidStateType.Landed or
               state == Enum.HumanoidStateType.Running or
               state == Enum.HumanoidStateType.RunningNoPhysics or
               state == Enum.HumanoidStateType.Freefall then
                humanoid.Jump = true
            end
        end
    end
end)

-- Conectar botões do menu
SpeedBtn.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    SpeedBtn.Text = "Speed Hack: " .. (speedEnabled and "ON" or "OFF")
end)

JumpBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    JumpBtn.Text = "Pulo Infinito: " .. (jumpEnabled and "ON" or "OFF")
end)

ESPBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    ESPBtn.Text = "ESP Verde: " .. (espEnabled and "ON" or "OFF")
    toggleESP(espEnabled)
end)

Players.PlayerRemoving:Connect(function(plr)
    removeESP(plr)
end)

print("GP7 MODS carregado com Speed Hack, Pulo Infinito e ESP gigante.")