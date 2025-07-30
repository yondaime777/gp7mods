-- GP7 MENU Script Atualizado

local Players = game:GetService("Players") local RunService = game:GetService("RunService") local UserInputService = game:GetService("UserInputService") local LocalPlayer = Players.LocalPlayer

-- Criar GUI principal local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui")) gui.Name = "GP7Menu" gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui) frame.Size = UDim2.new(0, 220, 0, 260) frame.Position = UDim2.new(0, 20, 0.4, 0) frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) frame.BackgroundTransparency = 0.4 frame.BorderSizePixel = 0 frame.Active = true frame.Draggable = true

local title = Instance.new("TextLabel", frame) title.Size = UDim2.new(1, 0, 0, 40) title.Text = "GP7 MODS" title.BackgroundColor3 = Color3.fromRGB(0, 0, 0) title.Font = Enum.Font.Arcade -- Fonte quadrada

-- Ciclo RGB spawn(function() while true do for i = 0, 255, 5 do title.TextColor3 = Color3.fromHSV(i / 255, 1, 1) task.wait() end end end)

title.TextScaled = true

-- Botão flutuante minimizar local floatBtn = Instance.new("TextButton", gui) floatBtn.Text = "GP7" floatBtn.Size = UDim2.new(0, 60, 0, 35) floatBtn.Position = UDim2.new(0, 10, 0, 10) floatBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0) floatBtn.TextColor3 = Color3.new(0, 0, 0) floatBtn.Visible = false floatBtn.Font = Enum.Font.Arcade floatBtn.TextScaled = true

floatBtn.MouseButton1Click:Connect(function() frame.Visible = true floatBtn.Visible = false end)

local function createButton(text, posY, callback) local btn = Instance.new("TextButton", frame) btn.Size = UDim2.new(1, -20, 0, 40) btn.Position = UDim2.new(0, 10, 0, posY) btn.BackgroundColor3 = Color3.fromRGB(50, 0, 0) btn.TextColor3 = Color3.fromRGB(255, 0, 0) btn.Font = Enum.Font.Arcade btn.TextScaled = true btn.Text = text btn.BorderSizePixel = 0 btn.MouseButton1Click:Connect(callback) return btn end

createButton("Minimizar", 220, function() frame.Visible = false floatBtn.Visible = true end)

-- === Variáveis para Speed Hack === local speedState = { normal = 16, rapido = 40, ultra = 60 } local currentSpeed = "normal" local speedBtn

local function setSpeed(level) currentSpeed = level local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") if humanoid then humanoid.WalkSpeed = speedState[level] end end

createButton("Velocidade Rápida", 60, function(btn) if currentSpeed ~= "rapido" then setSpeed("rapido") btn.Text = "Velocidade Rápida ON" else setSpeed("normal") btn.Text = "Velocidade Rápida" end end)

createButton("Velocidade Ultra Rápida", 110, function(btn) if currentSpeed ~= "ultra" then setSpeed("ultra") btn.Text = "Velocidade Ultra Rápida ON" else setSpeed("normal") btn.Text = "Velocidade Ultra Rápida" end end)

-- === Infinite Jump === local infiniteJumpOn = false local humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")

UserInputService.JumpRequest:Connect(function() if infiniteJumpOn then humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid") if humanoid then humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end end end)

createButton("Pulo Infinito", 160, function(btn) infiniteJumpOn = not infiniteJumpOn btn.Text = infiniteJumpOn and "Pulo Infinito ON" or "Pulo Infinito" end)

