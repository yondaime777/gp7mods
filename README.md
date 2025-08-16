local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

-- Criar GUI
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "GP7Menu"
gui.ResetOnSpawn = false

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 160, 0, 40)
button.Position = UDim2.new(0.05, 0, 0.25, 0)
button.Text = "Speed Hack OFF"
button.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.SourceSansBold
button.TextSize = 18
button.Parent = gui

-- Estado
local speedOn = false
local loopSpam

-- Função de pegar/auto comprar o gancho
local function getGancho()
    local backpack = LocalPlayer:WaitForChild("Backpack")
    local gancho = backpack:FindFirstChild("Gancho De Aperto") or Character:FindFirstChild("Gancho De Aperto")

    if not gancho then
        -- tenta comprar (ajuste o RemoteEvent se o jogo usar outro nome!)
        local remote = ReplicatedStorage:FindFirstChild("BuyItem") or ReplicatedStorage:FindFirstChild("Remotes"):FindFirstChild("BuyItem")
        if remote then
            remote:FireServer("Gancho De Aperto") -- 🔴 pode ser "Gancho de Aperto", "GrappleHook" etc, depende do jogo
        end
        task.wait(1)
        gancho = backpack:FindFirstChild("Gancho De Aperto") or Character:FindFirstChild("Gancho De Aperto")
    end

    return gancho
end

-- Função de ativar/desativar
local function toggleSpeed()
    speedOn = not speedOn
    if speedOn then
        button.Text = "Speed Hack ON"
        button.BackgroundColor3 = Color3.fromRGB(0, 150, 0)

        -- Atualiza humanoid
        Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        Humanoid = Character:WaitForChild("Humanoid")

        -- Muda velocidade
        Humanoid.WalkSpeed = 150

        -- Pega/compra o gancho
        local tool = getGancho()
        if tool then
            tool.Parent = Character
        end

        -- Spamma o clique
        loopSpam = RunService.Heartbeat:Connect(function()
            pcall(function()
                local tool = Character:FindFirstChild("Gancho De Aperto")
                if tool then
                    tool:Activate()
                end
            end)
        end)

    else
        button.Text = "Speed Hack OFF"
        button.BackgroundColor3 = Color3.fromRGB(150, 0, 0)

        -- Reseta velocidade
        Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        Humanoid = Character:WaitForChild("Humanoid")
        Humanoid.WalkSpeed = 16

        -- Para spam
        if loopSpam then
            loopSpam:Disconnect()
            loopSpam = nil
        end
    end
end

button.MouseButton1Click:Connect(toggleSpeed)

-- Garante que ao morrer/respawnar o hack continua se estiver ligado
LocalPlayer.CharacterAdded:Connect(function(char)
    Character = char
    Humanoid = char:WaitForChild("Humanoid")
    if speedOn then
        Humanoid.WalkSpeed = 150
        local tool = getGancho()
        if tool then
            tool.Parent = Character
        end
    end
end)