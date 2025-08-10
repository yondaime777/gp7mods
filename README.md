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
        local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
        local Window = Rayfield:CreateWindow({
            Name = "Brazuca God",
            LoadingTitle = "Carregando...",
            LoadingSubtitle = "By GP7",
            ConfigurationSaving = {
                Enabled = false
            }
        })

        local Tab = Window:CreateTab("Funções", 4483362458)

        -- Variáveis globais
        local speedOn = false
        local speedRageOn = false
        local infiniteJumpOn = false
        local noclipOn = false
        local savedPosition = nil

        -- SPEED HACK NORMAL
        Tab:CreateButton({
            Name = "Speed Hack",
            Callback = function()
                speedOn = not speedOn
                local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = speedOn and 32 or 16
                end
            end
        })

        -- SPEED HACK RAGE (200)
        Tab:CreateButton({
            Name = "Speed Hack Rage",
            Callback = function()
                speedRageOn = not speedRageOn
                local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = speedRageOn and 200 or 16
                end
            end
        })

        -- INFINITE JUMP
        game:GetService("UserInputService").JumpRequest:Connect(function()
            if infiniteJumpOn then
                local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end
        end)
        Tab:CreateButton({
            Name = "Infinite Jump",
            Callback = function()
                infiniteJumpOn = not infiniteJumpOn
            end
        })

        -- NOCLIP
        game:GetService("RunService").Stepped:Connect(function()
            if noclipOn and LocalPlayer.Character then
                for _, part in pairs(LocalPlayer.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
        Tab:CreateButton({
            Name = "No Clip",
            Callback = function()
                noclipOn = not noclipOn
            end
        })

        -- SALVAR POSIÇÃO
        Tab:CreateButton({
            Name = "Salvar Posição",
            Callback = function()
                local char = LocalPlayer.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    savedPosition = char.HumanoidRootPart.Position
                    
                    -- Mensagem de sucesso
                    local msg = Instance.new("Message", workspace)
                    msg.Text = "Posição Salva com Sucesso!"
                    game.Debris:AddItem(msg, 2)
                end
            end
        })

        -- TELEPORTAR
        Tab:CreateButton({
            Name = "Teleportar",
            Callback = function()
                local char = LocalPlayer.Character
                if char and char:FindFirstChild("HumanoidRootPart") and savedPosition then
                    char.HumanoidRootPart.CFrame = CFrame.new(savedPosition)
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