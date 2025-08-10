local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- GUI para Key (igual seu original)
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

        -- Tocar som ao abrir o menu
        local sound = Instance.new("Sound", workspace)
        sound.SoundId = "rbxassetid://142376088" -- som simples de confirmação
        sound.Volume = 0.5
        sound:Play()
        game.Debris:AddItem(sound, 3)

        local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
        local Window = Rayfield:CreateWindow({
            Name = "Brazuca God",
            LoadingTitle = "Carregando...",
            LoadingSubtitle = "By GP7",
            ConfigurationSaving = { Enabled = false }
        })

        local Tab = Window:CreateTab("Funções", 4483362458)

        -- Variáveis controle
        local speedOn = false
        local speedRageOn = false
        local infiniteJumpOn = false
        local noclipOn = false
        local savedPosition = nil

        -- Função para manter speed constante
        RunService.Heartbeat:Connect(function()
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

            -- Noclip
            if noclipOn and LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)

        -- Speed Hack normal toggle
        Tab:CreateToggle({
            Name = "Speed Hack",
            CurrentValue = false,
            Flag = "SpeedHack",
            Callback = function(value)
                speedOn = value
                if value then
                    speedRageOn = false -- desativa rage se speed normal ativado
                    Window.Flags["SpeedHack Rage"] = false
                end
            end
        })

        -- Speed Hack Rage toggle
        Tab:CreateToggle({
            Name = "Speed Hack Rage",
            CurrentValue = false,
            Flag = "SpeedHack Rage",
            Callback = function(value)
                speedRageOn = value
                if value then
                    speedOn = false -- desativa speed normal se rage ativado
                    Window.Flags["Speed Hack"] = false
                end
            end
        })

        -- Infinite Jump toggle
        UserInputService.JumpRequest:Connect(function()
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
            Flag = "InfiniteJump",
            Callback = function(value)
                infiniteJumpOn = value
            end
        })

        -- Noclip toggle
        Tab:CreateToggle({
            Name = "No Clip",
            CurrentValue = false,
            Flag = "NoClip",
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
                    Window:Notify({
                        Title = "Brazuca God",
                        Content = "Posição Salva com Sucesso!",
                        Duration = 3
                    })
                end
            end
        })

        -- Teleportar com notificação
        Tab:CreateButton({
            Name = "Teleportar",
            Callback = function()
                local char = LocalPlayer.Character
                if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
                    char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
                    Window:Notify({
                        Title = "Brazuca God",
                        Content = "Teleportando...",
                        Duration = 2
                    })
                end
            end
        })

    else
        Button.Text = "Key incorreta!"
        Button.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
        task.wait(1.5)
        Button.Text = "Confirmar"
        Button.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
    end
end)