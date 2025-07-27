local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

local speedEnabled = false
local jumpEnabled = false
local espEnabled = false

local espLabels = {}
local espBoxes = {}

local walkSpeedValue = 40
local normalWalkSpeed = 16

local jumpPowerValue = 70
local normalJumpPower = 50

-- ESP Functions
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
        box.Size = Vector3.new(5, 9, 3)
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

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "GP7MODSGUI"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 280, 0, 180)
MainFrame.Position = UDim2.new(0.5, -140, 0.5, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 50, 0)
MainFrame.BorderColor3 = Color3.fromRGB(0, 255, 0)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

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

local ButtonsFrame = Instance.new("Frame")
ButtonsFrame.Name = "ButtonsFrame"
ButtonsFrame.Size = UDim2.new(1, -20, 1, -40)
ButtonsFrame.Position = UDim2.new(0, 10, 0, 35)
ButtonsFrame.BackgroundTransparency = 1
ButtonsFrame.Parent = MainFrame

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

FloatBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Speed Hack: muda WalkSpeed direto
RunService.Heartbeat:Connect(function()
    local character = LocalPlayer.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            if speedEnabled then
                humanoid.WalkSpeed = walkSpeedValue
            else
                humanoid.WalkSpeed = normalWalkSpeed
            end
            if not jumpEnabled then
                humanoid.JumpPower = normalJumpPower
            end
        end
    end
end)

-- Função para configurar o pulo infinito para cada humanoid (chama no respawn também)
local function setupInfiniteJump(character)
    local humanoid = character:WaitForChild("Humanoid")
    humanoid.StateChanged:Connect(function(oldState, newState)
        if jumpEnabled then
            if newState == Enum.HumanoidStateType.Landed then
                humanoid.Jump = true
            end
        end
    end)
    -- Ajusta JumpPower também
    humanoid.JumpPower = jumpEnabled and jumpPowerValue or normalJumpPower
end

-- Setup inicial e conexão pra respawn
if LocalPlayer.Character then
    setupInfiniteJump(LocalPlayer.Character)
end

LocalPlayer.CharacterAdded:Connect(function(character)
    wait(1)
    setupInfiniteJump(character)
end)

SpeedBtn.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    SpeedBtn.Text = "Speed Hack: " .. (speedEnabled and "ON" or "OFF")
end)

JumpBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    JumpBtn.Text = "Pulo Infinito: " .. (jumpEnabled and "ON" or "OFF")
    -- Ajusta JumpPower do humanoid atual
    local character = LocalPlayer.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.JumpPower = jumpEnabled and jumpPowerValue or normalJumpPower
        end
    end
end)

ESPBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    ESPBtn.Text = "ESP Verde: " .. (espEnabled and "ON" or "OFF")
    toggleESP(espEnabled)
end)

Players.PlayerRemoving:Connect(function(plr)
    removeESP(plr)
end)

print("GP7 MODS carregado com Speed Hack (WalkSpeed), Pulo Infinito e ESP verde!")