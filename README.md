local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local HRP = Character:WaitForChild("HumanoidRootPart")

local speedEnabled = false
local jumpEnabled = false
local espEnabled = false

local walkSpeedNormal = 16
local walkSpeedFast = 40

-- ESP tables
local espLabels = {}
local espBoxes = {}

-- Create ESP
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

    if hrp then
        local box = Instance.new("BoxHandleAdornment")
        box.Name = "GP7ESPBox"
        box.Adornee = hrp
        box.AlwaysOnTop = true
        box.ZIndex = 10
        box.Size = Vector3.new(6, 10, 4)
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
local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
ScreenGui.Name = "GP7MODS_GUI"

local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = UDim2.new(0, 280, 0, 180)
MainFrame.Position = UDim2.new(0.5, -140, 0.5, -90)
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

local MinimizeBtn = Instance.new("TextButton", MainFrame)
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -30, 0, 0)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.TextSize = 20

local ButtonsFrame = Instance.new("Frame", MainFrame)
ButtonsFrame.Size = UDim2.new(1, -20, 1, -40)
ButtonsFrame.Position = UDim2.new(0, 10, 0, 35)
ButtonsFrame.BackgroundTransparency = 1

local function createButton(text, posY)
    local btn = Instance.new("TextButton", ButtonsFrame)
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Position = UDim2.new(0, 0, 0, posY)
    btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
    btn.TextColor3 = Color3.fromRGB(0, 255, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 22
    btn.Text = text
    return btn
end

local speedBtn = createButton("Speed Hack: OFF", 0)
local jumpBtn = createButton("Pulo Infinito: OFF", 45)
local espBtn = createButton("ESP Verde: OFF", 90)

MinimizeBtn.MouseButton1Click:Connect(function()
    if ButtonsFrame.Visible then
        ButtonsFrame.Visible = false
        MainFrame.Size = UDim2.new(0, 280, 0, 30)
    else
        ButtonsFrame.Visible = true
        MainFrame.Size = UDim2.new(0, 280, 0, 180)
    end
end)

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

-- Atualiza humanoid/character se trocar
local function updateCharacter()
    Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    Humanoid = Character:WaitForChild("Humanoid")
    HRP = Character:WaitForChild("HumanoidRootPart")
end
updateCharacter()
LocalPlayer.CharacterAdded:Connect(updateCharacter)

-- Speed Hack: mantém velocidade constante
RunService.Heartbeat:Connect(function()
    if speedEnabled and Humanoid then
        if Humanoid.WalkSpeed ~= walkSpeedFast then
            Humanoid.WalkSpeed = walkSpeedFast
        end
    elseif Humanoid then
        if Humanoid.WalkSpeed ~= walkSpeedNormal then
            Humanoid.WalkSpeed = walkSpeedNormal
        end
    end
end)

-- Infinite jump com teleporte suave (lerp) pra cima para evitar respawn
local jumpCooldown = 0.3
local lastJump = 0
UserInputService.JumpRequest:Connect(function()
    if jumpEnabled and HRP and Humanoid and Humanoid.Health > 0 then
        local now = tick()
        if now - lastJump >= jumpCooldown then
            lastJump = now
            -- Teleporte suave pra cima, interpolando
            local targetPos = HRP.CFrame + Vector3.new(0, 5, 0)
            for i = 0, 1, 0.2 do
                HRP.CFrame = HRP.CFrame:Lerp(targetPos, i)
                task.wait(0.01)
            end
        end
    end
end)

print("GP7 MODS carregado — Speed Hack, Pulo Infinito suave e ESP verde.")