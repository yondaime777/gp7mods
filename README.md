local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local UIS = game:GetService("UserInputService")

local speedEnabled = false
local jumpEnabled = false
local espEnabled = false

local espLabels = {}
local espBoxes = {}

local desiredSpeed = 50       -- Velocidade aumentada
local desiredJumpPower = 100  -- Pulo alto

-- Função para criar ESP nome + caixa verde maior
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
        box.Size = Vector3.new(3, 7, 2) -- Caixa maior, destaque
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

-- Loop para manter Speed Hack e JumpPower
coroutine.wrap(function()
    while true do
        RunService.Heartbeat:Wait()
        if speedEnabled and LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = desiredSpeed
            end
        elseif LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = 16 -- padrão do Roblox
            end
        end

        if jumpEnabled and LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.JumpPower = desiredJumpPower
            end
        elseif LocalPlayer.Character then
            local humanoid = LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.JumpPower = 50 -- padrão Roblox
            end
        end
    end
end)()

-- Forçar pulo infinito ao apertar espaço
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

-- ESP Toggle
local Players = game:GetService("Players")
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

-- Buttons e GUI ficaram iguais aos anteriores, somente coloco as conexões agora para ativar/desativar:

-- Exemplo de como ativar/desativar (você deve colocar essas conexões no menu que já tem):

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

-- Remove ESP player sai
Players.PlayerRemoving:Connect(function(plr)
    removeESP(plr)
end)

print("GP7 MODS atualizado: Speed Hack e Pulo Infinito corrigidos, ESP maior ativado.")