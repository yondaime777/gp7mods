-- LocalScript (colocar em StarterPlayerScripts ou PlayerGui)
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local guiParent = player:WaitForChild("PlayerGui")

-- CONFIG
local AUTO_CLOSE_SECONDS = 12
local typingSpeed = 0.04 -- segundos por caractere

-- Cria ScreenGui
local screen = Instance.new("ScreenGui")
screen.Name = "HackerAlertSim"
screen.ResetOnSpawn = false
screen.Parent = guiParent

-- Fundo escuro semi-transparente
local overlay = Instance.new("Frame", screen)
overlay.Size = UDim2.new(1,0,1,0)
overlay.Position = UDim2.new(0,0,0,0)
overlay.BackgroundColor3 = Color3.fromRGB(0,0,0)
overlay.BackgroundTransparency = 0.45
overlay.ZIndex = 1

-- Painel principal
local panel = Instance.new("Frame", screen)
panel.Size = UDim2.new(0, 700, 0, 260)
panel.AnchorPoint = Vector2.new(0.5,0.5)
panel.Position = UDim2.new(0.5, 0, 0.4, 0)
panel.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
panel.BorderSizePixel = 0
panel.ZIndex = 2
panel.ClipsDescendants = true
panel.Rotation = 0
panel.Name = "AlertPanel"

-- Cabeçalho vermelho
local header = Instance.new("Frame", panel)
header.Size = UDim2.new(1,0,0,72)
header.Position = UDim2.new(0,0,0,0)
header.BackgroundColor3 = Color3.fromRGB(190, 10, 10)
header.BorderSizePixel = 0

local headerText = Instance.new("TextLabel", header)
headerText.Size = UDim2.new(1, -20, 1, 0)
headerText.Position = UDim2.new(0, 10, 0, 0)
headerText.BackgroundTransparency = 1
headerText.Text = "SEUS DISPOSITIVO ESTÁ SENDO HACKEADO, DELTA ATAQUES!!!"
headerText.TextColor3 = Color3.fromRGB(255,255,255)
headerText.Font = Enum.Font.GothamBlack
headerText.TextScaled = true
headerText.TextWrapped = true
headerText.ZIndex = 3

-- Mensagem secundária (typing)
local body = Instance.new("TextLabel", panel)
body.Size = UDim2.new(1, -40, 0, 110)
body.Position = UDim2.new(0, 20, 0, 90)
body.BackgroundTransparency = 1
body.Text = "" -- será preenchido por typing
body.TextColor3 = Color3.fromRGB(255,230,100)
body.Font = Enum.Font.SourceSansBold
body.TextScaled = true
body.TextWrapped = true
body.ZIndex = 3
body.TextXAlignment = Enum.TextXAlignment.Left
body.TextYAlignment = Enum.TextYAlignment.Top

-- Fechar (X)
local closeBtn = Instance.new("TextButton", panel)
closeBtn.Size = UDim2.new(0, 80, 0, 36)
closeBtn.Position = UDim2.new(1, -90, 0, 10)
closeBtn.Text = "Fechar"
closeBtn.Font = Enum.Font.GothamSemibold
closeBtn.TextScaled = true
closeBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.BorderSizePixel = 0
closeBtn.ZIndex = 4

-- Efeito de piscar do headerText
do
    local tweenInfo = TweenInfo.new(0.6, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, -1, true)
    local goal = {TextTransparency = 0.35}
    local t = TweenService:Create(headerText, tweenInfo, goal)
    t:Play()
end

-- Texto de typing que você pediu
local targetText = "ESTHER PARE DE USAR HACK"

-- Função typing (async)
spawn(function()
    body.Text = ""
    for i = 1, #targetText do
        body.Text = string.sub(targetText, 1, i)
        wait(typingSpeed)
    end
end)

-- Aparecer com pequena animação
do
    panel.Size = UDim2.new(0, 10, 0, 4)
    panel.Position = UDim2.new(0.5, 0, 0.35, 0)
    local tweenIn = TweenService:Create(panel, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Size = UDim2.new(0, 700, 0, 260), Position = UDim2.new(0.5, 0, 0.4, 0)})
    tweenIn:Play()
end

-- Fecha a janela (função)
local function closeAlert()
    if not screen then return end
    local tweenOut = TweenService:Create(panel, TweenInfo.new(0.22, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {Size = UDim2.new(0, 10, 0, 4), Position = UDim2.new(0.5, 0, 0.4, 0)})
    tweenOut:Play()
    tweenOut.Completed:Wait()
    screen:Destroy()
end

closeBtn.MouseButton1Click:Connect(closeAlert)

-- Auto close
delay(AUTO_CLOSE_SECONDS, function()
    if screen and screen.Parent then
        closeAlert()
    end
end)

-- observação visual: deixa claro que é brincadeira
local notice = Instance.new("TextLabel", panel)
notice.Size = UDim2.new(1, -40, 0, 36)
notice.Position = UDim2.new(0, 20, 1, -46)
notice.BackgroundTransparency = 1
notice.Text = "(SIMULAÇÃO) ISSO É APENAS UMA BRINCADEIRA"
notice.TextColor3 = Color3.fromRGB(200,200,200)
notice.Font = Enum.Font.Gotham
notice.TextScaled = true
notice.ZIndex = 4