local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ContextActionService = game:GetService("ContextActionService")

local LocalPlayer = Players.LocalPlayer
local humanoid
local character

local speedEnabled = false
local jumpEnabled = false
local espEnabled = false

local walkSpeedValue = 70
local normalWalkSpeed = 16

-- Atualiza referência do personagem e humanoid
local function updateCharacter()
    character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    humanoid = character:WaitForChild("Humanoid")
end

updateCharacter()
LocalPlayer.CharacterAdded:Connect(updateCharacter)

-- Speed Hack: força a velocidade no Heartbeat
RunService.Heartbeat:Connect(function()
    if speedEnabled and humanoid then
        if humanoid.WalkSpeed ~= walkSpeedValue then
            humanoid.WalkSpeed = walkSpeedValue
        end
    elseif humanoid then
        if humanoid.WalkSpeed ~= normalWalkSpeed then
            humanoid.WalkSpeed = normalWalkSpeed
        end
    end
end)

-- Infinite jump usando JumpRequest (mais confiável)
local function onJumpRequest()
    if jumpEnabled and humanoid then
        humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end

UserInputService.JumpRequest:Connect(onJumpRequest)

-- ESP básico para nomes verdes (caixa fica opcional pra depois)
local espLabels = {}
local espBoxes = {}

local function createESP(plr)
    if espLabels[plr] then return end
    local char = plr.Character
    if not char then return end
    local head = char:FindFirstChild("Head")
    local hrp = char:FindFirstChild("HumanoidRootPart")

    if head then
        local billboard = Instance.new("BillboardGui")
        billboard.Name = "GP7ESPName"
        billboard.Adornee = head
        billboard.Size = UDim2.new(0, 100, 0, 25)
        billboard.StudsOffset = Vector3.new(0, 2.5, 0)
        billboard.AlwaysOnTop = true
        billboard.Parent = head

        local label = Instance.new("TextLabel")
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(1, 0, 1, 0)
        label.TextColor3 = Color3.fromRGB(0, 255, 0)
        label.TextStrokeTransparency = 0
        label.Text = plr.Name
        label.Font = Enum.Font.GothamBold
        label.TextSize = 20
        label.Parent = billboard

        espLabels[plr] = billboard
    end

    -- Caixa verde simples
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

-- GUI simples pra ligar e desligar funções

local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "GP7MODS_GUI"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 250, 0, 160)
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -80)
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 50, 0)
MainFrame.BorderColor3 = Color3.fromRGB(0, 255, 0)
MainFrame.Active = true
MainFrame.Draggable = true

local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
Title.BorderSizePixel = 0
Title.Text = "GP7 MODS"
Title.TextColor3 = Color3.fromRGB(0, 255, 0)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 24

local function createButton(text, posY)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    btn.TextColor3 = Color3.fromRGB(0, 255, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 22
    btn.Text = text
    return btn
end

local speedBtn = createButton("Speed Hack: OFF", 40)
local jumpBtn = createButton("Pulo Infinito: OFF", 85)
local espBtn = createButton("ESP Verde: OFF", 130)

speedBtn.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    speedBtn.Text = "Speed Hack: " .. (speedEnabled and "ON" or "OFF")
end)

jumpBtn.MouseButton1Click:Connect(function()
    jumpEnabled = not jumpEnabled
    jumpBtn.Text = "Pulo Infinito: " .. (jumpEnabled and "ON" or "OFF")
end)

espBtn.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    espBtn.Text = "ESP Verde: " .. (espEnabled and "ON" or "OFF")
    toggleESP(espEnabled)
end)

Players.PlayerRemoving:Connect(function(plr)
    removeESP(plr)
end)