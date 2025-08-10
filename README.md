local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function showNotification(text)
    local playerGui = LocalPlayer:WaitForChild("PlayerGui")

    local notifFrame = Instance.new("Frame")
    notifFrame.Size = UDim2.new(0, 300, 0, 50)
    notifFrame.Position = UDim2.new(0.5, -150, 0.1, 0)
    notifFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    notifFrame.BackgroundTransparency = 0.3
    notifFrame.BorderSizePixel = 0
    notifFrame.Parent = playerGui

    local notifLabel = Instance.new("TextLabel")
    notifLabel.Size = UDim2.new(1, 0, 1, 0)
    notifLabel.BackgroundTransparency = 1
    notifLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    notifLabel.Font = Enum.Font.GothamBold
    notifLabel.TextScaled = true
    notifLabel.Text = text
    notifLabel.Parent = notifFrame

    task.delay(3, function()
        for i = 1, 10 do
            notifFrame.BackgroundTransparency = notifFrame.BackgroundTransparency + 0.07
            notifLabel.TextTransparency = notifLabel.TextTransparency + 0.07
            task.wait(0.05)
        end
        notifFrame:Destroy()
    end)
end

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

local updating = false

Button.MouseButton1Click:Connect(function()
    if TextBox.Text == "GP" then
        ScreenGui:Destroy()

        -- MENU
        local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
        local Window = Rayfield:CreateWindow({
            Name = "Brazuca God",
            LoadingTitle = "Carregando...",
            LoadingSubtitle = "By GP7",
            ConfigurationSaving = { Enabled = false }
        })

        local Tab = Window:CreateTab("Funções", 4483362458)

        local speedOn = false
        local speedRageOn = false
        local infiniteJumpOn = false
        local noclipOn = false
        local savedPosition = nil

        game:GetService("RunService").Heartbeat:Connect(function()
            local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                if speedRageOn then
                    humanoid.WalkSpeed = 200
                elseif speedOn then
                    humanoid.WalkSpeed = 32
                else
                    humanoid.WalkSpeed = 16
                end
            end
        end)

        local function setSpeedToggle(normalValue, rageValue)
            speedOn = normalValue
            speedRageOn = rageValue

            updating = true
            Tab:SetToggle("SpeedHackToggle", normalValue)
            Tab:SetToggle("SpeedHackRageToggle", rageValue)
            updating = false
        end

        Tab:CreateToggle({
            Name = "Speed Hack",
            CurrentValue = false,
            Flag = "SpeedHackToggle",
            Callback = function(value)
                if updating then return end
                if value then
                    setSpeedToggle(true, false)
                else
                    setSpeedToggle(false, false)
                end
            end,
        })

        Tab:CreateToggle({
            Name = "Speed Hack Rage",
            CurrentValue = false,
            Flag = "SpeedHackRageToggle",
            Callback = function(value)
                if updating then return end
                if value then
                    setSpeedToggle(false, true)
                else
                    setSpeedToggle(false, false)
                end
            end,
        })

        game:GetService("UserInputService").JumpRequest:Connect(function()
            if infiniteJumpOn then
                local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end
        end)

        Tab:CreateToggle({
            Name = "Infinite Jump",
            CurrentValue = false,
            Flag = "InfiniteJumpToggle",
            Callback = function(value)
                infiniteJumpOn = value
            end,
        })

        game:GetService("RunService").Stepped:Connect(function()
            if LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = not noclipOn
                    end
                end
            end
        end)

        Tab:CreateToggle({
            Name = "No Clip",
            CurrentValue = false,
            Flag = "NoClipToggle",
            Callback = function(value)
                noclipOn = value
            end,
        })

        Tab:CreateButton({
            Name = "Salvar Posição",
            Callback = function()
                local char = LocalPlayer.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    savedPosition = char.HumanoidRootPart.Position
                    showNotification("Posição Salva com Sucesso!")
                end
            end,
        })

        Tab:CreateButton({
            Name = "Teleportar",
            Callback = function()
                local char = LocalPlayer.Character
                if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
                    char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
                    showNotification("Teleportando...")
                end
            end,
        })

        showNotification("Key aceita! Bem-vindo(a).")

    else
        Button.Text = "Key incorreta!"
        Button.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
        task.wait(1.5)
        Button.Text = "Confirmar"
        Button.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    end
end)