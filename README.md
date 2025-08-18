-- Página de cheats (exemplo)
local stealerPage = createTab("Stealer")

-- Variáveis
local grappleActive = false
local grappleSpeed = 16 -- inicial

-- Botão ON/OFF do Grapple Hook Speed
local grappleBtn = Instance.new("TextButton")
grappleBtn.Size = UDim2.new(0, 200, 0, 35)
grappleBtn.Position = UDim2.new(0, 10, 0, 10)
grappleBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
grappleBtn.TextColor3 = Color3.fromRGB(255, 255, 0)
grappleBtn.Font = Enum.Font.GothamBold
grappleBtn.TextSize = 16
grappleBtn.Text = "Grapple Speed: OFF"
Instance.new("UICorner", grappleBtn).CornerRadius = UDim.new(0, 6)
grappleBtn.Parent = stealerPage

-- Slider de velocidade
local speedSlider = Instance.new("TextButton")
speedSlider.Size = UDim2.new(0, 200, 0, 35)
speedSlider.Position = UDim2.new(0, 10, 0, 60)
speedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
speedSlider.TextColor3 = Color3.fromRGB(255, 255, 255)
speedSlider.Font = Enum.Font.GothamBold
speedSlider.TextSize = 14
speedSlider.Text = "Velocidade: 16"
Instance.new("UICorner", speedSlider).CornerRadius = UDim.new(0, 6)
speedSlider.Parent = stealerPage

-- Função para ativar/desativar Grapple Hook
grappleBtn.MouseButton1Click:Connect(function()
    grappleActive = not grappleActive
    if grappleActive then
        grappleBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        grappleBtn.Text = "Grapple Speed: ON"
        -- Equipa Grapple Hook
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("GrappleHook") == nil then
            local tool = game:GetService("ReplicatedStorage"):FindFirstChild("GrappleHook")
            if tool then
                tool:Clone().Parent = char
            end
        end
    else
        grappleBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
        grappleBtn.Text = "Grapple Speed: OFF"
        -- Volta velocidade padrão
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid.WalkSpeed = 16
        end
    end
end)

-- Função do slider
speedSlider.MouseButton1Down:Connect(function()
    if not grappleActive then return end
    local mouse = game.Players.LocalPlayer:GetMouse()
    local dragging = true

    local function move()
        while dragging do
            local rel = math.clamp(mouse.X - speedSlider.AbsolutePosition.X, 0, speedSlider.AbsoluteSize.X)
            grappleSpeed = math.floor(16 + (rel / speedSlider.AbsoluteSize.X) * (150 - 16))
            speedSlider.Text = "Velocidade: " .. grappleSpeed
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("Humanoid") then
                char.Humanoid.WalkSpeed = grappleSpeed
            end
            task.wait(0.03)
        end
    end

    task.spawn(move)

    local conn
    conn = UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
            conn:Disconnect()
        end
    end)
end)