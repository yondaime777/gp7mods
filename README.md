local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

-- Criar GUI principal
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 260)
frame.Position = UDim2.new(0, 20, 0.4, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.4
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "GP7 MODS"
title.TextColor3 = Color3.fromRGB(0, 255, 0)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

-- Botão flutuante minimizar
local floatBtn = Instance.new("TextButton", gui)
floatBtn.Text = "GP7"
floatBtn.Size = UDim2.new(0, 60, 0, 35)
floatBtn.Position = UDim2.new(0, 10, 0, 10)
floatBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
floatBtn.TextColor3 = Color3.new(0, 0, 0)
floatBtn.Visible = false
floatBtn.Font = Enum.Font.GothamBold
floatBtn.TextScaled = true

floatBtn.MouseButton1Click:Connect(function()
    frame.Visible = true
    floatBtn.Visible = false
end)

local function createButton(text, posY, callback)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1, -20, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    btn.TextColor3 = Color3.fromRGB(0, 255, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    btn.Text = text
    btn.BorderSizePixel = 0
    btn.MouseButton1Click:Connect(callback)
    return btn
end

createButton("Minimizar", 220, function()
    frame.Visible = false
    floatBtn.Visible = true
end)

-- === Variáveis para Speed Hack ===
local speedOn = false
local SPEED_VALUE = 40
local NORMAL_SPEED = 16
local ITEM_NAME = "Speed Coil"

local function buySpeedCoil()
    -- Tenta ativar todos ProximityPrompts e ClickDetectors no workspace que vendem Speed Coil
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("ProximityPrompt") then
            if obj.ActionText and obj.ActionText:lower():find("speed") or obj.ObjectText and obj.ObjectText:lower():find("coil") then
                pcall(function() obj:InputHoldBegin() task.wait(0.1) obj:InputHoldEnd() end)
            end
        elseif obj:IsA("ClickDetector") and obj.Parent and obj.Parent.Name:lower():find("speed") then
            pcall(function() fireclickdetector(obj) end)
        end
    end
end

local function equipSpeedCoil()
    local backpack = LocalPlayer:WaitForChild("Backpack")
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")
    -- Procura a ferramenta Speed Coil na mochila
    for _, tool in pairs(backpack:GetChildren()) do
        if tool:IsA("Tool") and tool.Name:lower():find("speed") then
            -- Equipa por 1 segundo
            humanoid:EquipTool(tool)
            task.wait(1)
            -- Desequipa para manter efeito
            humanoid:UnequipTools()
            return true
        end
    end
    return false
end

local function maintainSpeed()
    local character = LocalPlayer.Character
    if not character then return end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end
    if speedOn then
        if humanoid.WalkSpeed ~= SPEED_VALUE then
            humanoid.WalkSpeed = SPEED_VALUE
        end
    else
        if humanoid.WalkSpeed ~= NORMAL_SPEED then
            humanoid.WalkSpeed = NORMAL_SPEED
        end
    end
end

-- Botão Speed Hack
createButton("Speed Hack OFF", 60, function(btn)
    speedOn = not speedOn
    if speedOn then
        buySpeedCoil()
        task.wait(2)
        equipSpeedCoil()
        btn.Text = "Speed Hack ON"
    else
        btn.Text = "Speed Hack OFF"
    end
end)

-- Atualiza a velocidade todo frame
RunService.Heartbeat:Connect(maintainSpeed)

-- === ESP Verde com caixa e nome ===
local espOn = false
local espBoxes = {}

local function createEspForPlayer(plr)
    if espBoxes[plr] then return end
    local character = plr.Character
    if not character then return end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    local head = character:FindFirstChild("Head")
    if not hrp or not head then return end

    local box = Instance.new("BoxHandleAdornment", hrp)
    box.Adornee = hrp
    box.Size = Vector3.new(6, 9, 3)
    box.Color3 = Color3.fromRGB(0, 255, 0)
    box.Transparency = 0.5
    box.AlwaysOnTop = true
    espBoxes[plr] = box

    local billboard = Instance.new("BillboardGui", head)
    billboard.Adornee = head
    billboard.Size = UDim2.new(0, 200, 0, 50)
    billboard.StudsOffset = Vector3.new(0, 2, 0)
    billboard.AlwaysOnTop = true

    local label = Instance.new("TextLabel", billboard)
    label.Size = UDim2.new(1, 0, 1, 0)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.fromRGB(0, 255, 0)
    label.Font = Enum.Font.GothamBold
    label.TextScaled = true
    label.Text = plr.Name
end

local function removeEspForPlayer(plr)
    if espBoxes[plr] then
        espBoxes[plr]:Destroy()
        espBoxes[plr] = nil
    end
end

local function toggleEsp()
    espOn = not espOn
    if espOn then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer then
                createEspForPlayer(plr)
            end
        end
        -- Atualizar ESP ao entrar/sair de players
        Players.PlayerAdded:Connect(function(plr)
            if espOn and plr ~= LocalPlayer then
                createEspForPlayer(plr)
            end
        end)
        Players.PlayerRemoving:Connect(function(plr)
            removeEspForPlayer(plr)
        end)
    else
        for plr, box in pairs(espBoxes) do
            removeEspForPlayer(plr)
        end
    end
end

createButton("ESP Verde", 110, toggleEsp)

-- === Infinite Jump ===
local infiniteJumpOn = false
local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")

UserInputService.JumpRequest:Connect(function()
    if infiniteJumpOn then
        humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

createButton("Pulo Infinito", 160, function()
    infiniteJumpOn = not infiniteJumpOn
end)