local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

-- Criar GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "FluentMenu"
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

-- Botão flutuante
local openButton = Instance.new("TextButton")
openButton.Size = UDim2.new(0, 50, 0, 50)
openButton.Position = UDim2.new(0, 20, 0.5, -25)
openButton.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
openButton.Text = "≡"
openButton.TextSize = 28
openButton.TextColor3 = Color3.new(1,1,1)
openButton.Font = Enum.Font.SourceSansBold
openButton.Parent = gui
openButton.BackgroundTransparency = 0.1
openButton.AutoButtonColor = true
openButton.BorderSizePixel = 0
openButton.ZIndex = 2
openButton.ClipsDescendants = true

-- Menu principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 250, 0, 300)
mainFrame.Position = UDim2.new(0, -260, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
mainFrame.BackgroundTransparency = 0.2
mainFrame.BorderSizePixel = 0
mainFrame.Parent = gui

-- Cantos arredondados
local corner1 = Instance.new("UICorner", openButton)
corner1.CornerRadius = UDim.new(0, 12)
local corner2 = Instance.new("UICorner", mainFrame)
corner2.CornerRadius = UDim.new(0, 16)

-- Efeito sombra
local shadow = Instance.new("ImageLabel", mainFrame)
shadow.Size = UDim2.new(1, 30, 1, 30)
shadow.Position = UDim2.new(0, -15, 0, -15)
shadow.BackgroundTransparency = 1
shadow.Image = "rbxassetid://1316045217"
shadow.ImageColor3 = Color3.new(0, 0, 0)
shadow.ImageTransparency = 0.5
shadow.ScaleType = Enum.ScaleType.Slice
shadow.SliceCenter = Rect.new(10, 10, 118, 118)
shadow.ZIndex = 0

-- Botão de exemplo no menu
local buttonExample = Instance.new("TextButton")
buttonExample.Size = UDim2.new(1, -20, 0, 40)
buttonExample.Position = UDim2.new(0, 10, 0, 10)
buttonExample.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
buttonExample.Text = "Ativar Função"
buttonExample.TextSize = 20
buttonExample.Font = Enum.Font.SourceSansBold
buttonExample.TextColor3 = Color3.new(1,1,1)
buttonExample.Parent = mainFrame
local btnCorner = Instance.new("UICorner", buttonExample)
btnCorner.CornerRadius = UDim.new(0, 8)

buttonExample.MouseButton1Click:Connect(function()
    print("Função ativada!")
end)

-- Abrir / Fechar com animação
local aberto = false
openButton.MouseButton1Click:Connect(function()
    if aberto then
        TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quint), {Position = UDim2.new(0, -260, 0.5, -150)}):Play()
    else
        TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quint), {Position = UDim2.new(0, 60, 0.5, -150)}):Play()
    end
    aberto = not aberto
end)