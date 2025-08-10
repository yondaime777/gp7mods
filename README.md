-- SISTEMA DE KEY
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Text = "Digite a Key para acessar"
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextScaled = true
Title.Parent = Frame

local TextBox = Instance.new("TextBox")
TextBox.Size = UDim2.new(1, -20, 0, 40)
TextBox.Position = UDim2.new(0, 10, 0, 50)
TextBox.PlaceholderText = "Digite a Key..."
TextBox.Text = ""
TextBox.TextColor3 = Color3.fromRGB(0, 0, 0)
TextBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextBox.Font = Enum.Font.Gotham
TextBox.TextScaled = true
TextBox.Parent = Frame

local Button = Instance.new("TextButton")
Button.Size = UDim2.new(1, -20, 0, 40)
Button.Position = UDim2.new(0, 10, 0, 100)
Button.Text = "Confirmar"
Button.TextColor3 = Color3.fromRGB(255, 255, 255)
Button.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
Button.Font = Enum.Font.GothamBold
Button.TextScaled = true
Button.Parent = Frame

Button.MouseButton1Click:Connect(function()
    if TextBox.Text == "GP" then
        ScreenGui:Destroy()
        
        -- AQUI COMEÇA O BRAZUCA GOD
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local StarterGui = game:GetService("StarterGui")

local LocalPlayer = Players.LocalPlayer

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Brazuca God",
    LoadingTitle = "Brazuca God",
    LoadingSubtitle = "by GP7",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "BrazucaGod",
        FileName = "Config"
    },
    Discord = {
        Enabled = false,
    },
    KeySystem = false,
})

local Tab = Window:CreateTab("Funções")

local speedEnabled = false
local rageEnabled = false
local speedValue = 16
local infiniteJumpOn = false
local noclipOn = false
local savedPosition = nil

-- Speed Hack e Speed Rage
RunService.Heartbeat:Connect(function()
    local character = LocalPlayer.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            if rageEnabled then
                humanoid.WalkSpeed = 200
            elseif speedEnabled then
                humanoid.WalkSpeed = 32
            else
                humanoid.WalkSpeed = 16
            end
        end
    end
end)

local speedToggle = Tab:CreateToggle({
    Name = "Speed Hack",
    CurrentValue = false,
    Flag = "SpeedHack",
    Callback = function(value)
        speedEnabled = value
        if value then
            rageToggle:Set(false)
        end
    end
})

local rageToggle = Tab:CreateToggle({
    Name = "Speed Hack Rage (200)",
    CurrentValue = false,
    Flag = "SpeedRage",
    Callback = function(value)
        rageEnabled = value
        if value then
            speedEnabled = false
            speedToggle:Set(false)
        end
    end
})

-- Infinite Jump
UserInputService.JumpRequest:Connect(function()
    if infiniteJumpOn then
        local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

local infiniteJumpToggle = Tab:CreateToggle({
    Name = "Infinite Jump",
    CurrentValue = false,
    Flag = "InfiniteJump",
    Callback = function(value)
        infiniteJumpOn = value
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
    elseif LocalPlayer.Character then
        for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)

local noclipToggle = Tab:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "Noclip",
    Callback = function(value)
        noclipOn = value
    end
})

-- Salvar posição com notificação
Tab:CreateButton({
    Name = "Salvar Posição",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            savedPosition = char.HumanoidRootPart.Position
            StarterGui:SetCore("SendNotification", {
                Title = "Brazuca God",
                Text = "Posição Salva Com Sucesso !",
                Duration = 3
            })
        else
            StarterGui:SetCore("SendNotification", {
                Title = "Brazuca God",
                Text = "Não foi possível salvar a posição.",
                Duration = 3
            })
        end
    end
})

-- Teleportar para posição salva
Tab:CreateButton({
    Name = "Teleportar para posição salva",
    Callback = function()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
            char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
        else
            StarterGui:SetCore("SendNotification", {
                Title = "Brazuca God",
                Text = "Nenhuma posição salva para teleportar!",
                Duration = 3
            })
        end
    end
})