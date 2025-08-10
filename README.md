--// Sistema de Key
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local Key = "GP"
local keyCorrect = false

-- Janela de Key
local keyGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
local keyFrame = Instance.new("Frame", keyGui)
keyFrame.Size = UDim2.new(0, 300, 0, 150)
keyFrame.Position = UDim2.new(0.5, -150, 0.5, -75)
keyFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
keyFrame.BorderSizePixel = 0

local keyBox = Instance.new("TextBox", keyFrame)
keyBox.Size = UDim2.new(1, -20, 0, 40)
keyBox.Position = UDim2.new(0, 10, 0, 20)
keyBox.PlaceholderText = "Digite a Key"
keyBox.Text = ""
keyBox.TextScaled = true

local keyButton = Instance.new("TextButton", keyFrame)
keyButton.Size = UDim2.new(1, -20, 0, 40)
keyButton.Position = UDim2.new(0, 10, 0, 80)
keyButton.Text = "Confirmar"
keyButton.TextScaled = true
keyButton.BackgroundColor3 = Color3.fromRGB(0, 170, 0)

keyButton.MouseButton1Click:Connect(function()
    if keyBox.Text == Key then
        keyCorrect = true
        keyGui:Destroy()
    else
        keyBox.Text = ""
        keyBox.PlaceholderText = "Key incorreta!"
    end
end)

repeat task.wait() until keyCorrect == true

--// Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Brazuca God",
    LoadingTitle = "Carregando Brazuca God",
    LoadingSubtitle = "By Você",
    DisableRayfieldPrompts = true
})

local Tab = Window:CreateTab("Funções", 4483362458)

-- Som função
local function PlaySound()
    local sound = Instance.new("Sound", workspace)
    sound.SoundId = "rbxassetid://12222124"
    sound.Volume = 2
    sound:Play()
    game.Debris:AddItem(sound, 2)
end

-- Função mensagem na tela
local function Notify(msg)
    Rayfield:Notify({
        Title = "Brazuca God",
        Content = msg,
        Duration = 2
    })
    PlaySound()
end

-- Estados salvos
local speedNormalOn = false
local speedRageOn = false
local infiniteJumpOn = false
local noclipOn = false
local savedPosition = nil

-- Função aplicar estados
local function ApplyStates()
    local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then
        if speedNormalOn then
            hum.WalkSpeed = 32
        elseif speedRageOn then
            hum.WalkSpeed = 200
        else
            hum.WalkSpeed = 16
        end
    end
end

-- Speed Hack normal
Tab:CreateToggle({
    Name = "Speed Hack",
    CurrentValue = false,
    Callback = function(state)
        speedNormalOn = state
        if state then speedRageOn = false end
        ApplyStates()
    end
})

-- Speed Hack Rage
Tab:CreateToggle({
    Name = "Speed Hack Rage",
    CurrentValue = false,
    Callback = function(state)
        speedRageOn = state
        if state then speedNormalOn = false end
        ApplyStates()
    end
})

-- Infinite Jump
UserInputService.JumpRequest:Connect(function()
    if infiniteJumpOn then
        local hum = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if hum then
            hum:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

Tab:CreateToggle({
    Name = "Infinite Jump",
    CurrentValue = false,
    Callback = function(state)
        infiniteJumpOn = state
    end
})

-- Noclip
RunService.Stepped:Connect(function()
    if noclipOn and LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

Tab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Callback = function(state)
        noclipOn = state
    end
})

-- Salvar posição
Tab:CreateButton({
    Name = "Salvar Posição",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            savedPosition = char.HumanoidRootPart.Position
            Notify("Posição Salva Com Sucesso!")
        end
    end
})

-- Teleportar
Tab:CreateButton({
    Name = "Teleportar",
    Callback = function()
        if savedPosition then
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                Notify("Teleportando...")
                char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
            end
        else
            Notify("Nenhuma posição salva!")
        end
    end
})

-- Reaplicar estados quando respawnar
LocalPlayer.CharacterAdded:Connect(function()
    task.wait(1)
    ApplyStates()
end)