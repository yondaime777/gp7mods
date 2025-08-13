local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")

local gui = Instance.new("ScreenGui")
gui.Name = "BrazucaHub"
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

local function showNotification(text)
local notifFrame = Instance.new("Frame")
notifFrame.Size = UDim2.new(0, 300, 0, 50)
notifFrame.Position = UDim2.new(1, -310, 1, -60)
notifFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
notifFrame.BackgroundTransparency = 0
notifFrame.BorderSizePixel = 0
notifFrame.Parent = gui

local notifLabel = Instance.new("TextLabel")
notifLabel.Size = UDim2.new(1, 0, 1, 0)
notifLabel.BackgroundTransparency = 1
notifLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
notifLabel.Font = Enum.Font.GothamBold
notifLabel.TextScaled = true
notifLabel.Text = text
notifLabel.TextTransparency = 0
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

-- Função para arrastar frames e botões (funciona com mouse e toque)
local function makeDraggable(frame, dragger)
local dragging = false
local dragInput = nil
local dragStart = nil
local startPos = nil

local function update(input)
local delta = input.Position - dragStart
frame.Position = UDim2.new(
startPos.X.Scale,
startPos.X.Offset + delta.X,
startPos.Y.Scale,
startPos.Y.Offset + delta.Y
)
end

dragger.InputBegan:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
dragging = true
dragStart = input.Position
startPos = frame.Position

input.Changed:Connect(function()
if input.UserInputState == Enum.UserInputState.End then
dragging = false
end
end)
end

end)

dragger.InputChanged:Connect(function(input)
if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
dragInput = input
end
end)

UserInputService.InputChanged:Connect(function(input)
if input == dragInput and dragging then
update(input)
end
end)

end

-- Janela principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.fromOffset(580, 460)
mainFrame.Position = UDim2.new(0.5, -290, 0.5, -230)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 100, 0) -- verde interior
mainFrame.BorderSizePixel = 3
mainFrame.BorderColor3 = Color3.fromRGB(255, 255, 0) -- borda amarela
mainFrame.Parent = gui
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 8)

-- Barra superior
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 40)
topBar.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- preto
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame
Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 8)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -70, 1, 0) -- espaço para botões
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Brazuca Hub"
title.TextColor3 = Color3.fromRGB(255, 255, 0)
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = topBar

-- Botão fechar
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0.5, -15)
closeBtn.Text = "X"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 14
closeBtn.TextColor3 = Color3.fromRGB(255, 70, 70)
closeBtn.BackgroundTransparency = 1
closeBtn.Parent = topBar

-- Botão minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -70, 0.5, -15)
minimizeBtn.Text = "–"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.TextColor3 = Color3.fromRGB(0, 255, 0)
minimizeBtn.BackgroundTransparency = 1
minimizeBtn.Parent = topBar

-- Área de abas
local tabBar = Instance.new("Frame")
tabBar.Size = UDim2.new(0, 120, 1, -40)
tabBar.Position = UDim2.new(0, 0, 0, 40)
tabBar.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- preto
tabBar.BorderSizePixel = 0
tabBar.Parent = mainFrame

-- Área de conteúdo
local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1, -120, 1, -40)
contentFrame.Position = UDim2.new(0, 120, 0, 40)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

-- Criar abas
local tabs = {}
local tabOrder = {}

local function createTab(name)
local index = #tabOrder + 1
table.insert(tabOrder, name)

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, 0, 0, 40)
btn.Position = UDim2.new(0, 0, 0, (index - 1) * 40)
btn.BackgroundTransparency = 1
btn.Text = name
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.Font = Enum.Font.GothamBold
btn.TextSize = 14
btn.Parent = tabBar

local page = Instance.new("Frame")
page.Size = UDim2.new(1, 0, 1, 0)
page.BackgroundTransparency = 1
page.Visible = false
page.Parent = contentFrame

tabs[name] = {button = btn, page = page}

btn.MouseButton1Click:Connect(function()
for _, tname in ipairs(tabOrder) do
tabs[tname].page.Visible = false
tabs[tname].button.TextColor3 = Color3.fromRGB(255, 255, 255)
end
page.Visible = true
btn.TextColor3 = Color3.fromRGB(255, 255, 0)
end)

return page

end

-- Função toggle com espaçamento automático
local function createToggle(parent, text)
local children = parent:GetChildren()
local count = 0
for _, c in ipairs(children) do
if c:IsA("TextButton") and c.Text:find(text) then
count = count + 1
end
end

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 200, 0, 35)
toggleBtn.Position = UDim2.new(0, 10, 0, 10 + count * 45)
toggleBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 0)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 16
toggleBtn.Text = text .. ": OFF"
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(0, 6)
toggleBtn.Parent = parent

local state = false
toggleBtn.MouseButton1Click:Connect(function()
state = not state
if state then
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
toggleBtn.Text = text .. ": ON"
else
toggleBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
toggleBtn.Text = text .. ": OFF"
end
print(text .. " ->", state)
end)

end

-- Criar abas e toggles
local stealerPage = createTab("Stealer")

-- Botão Auto Steal
local autoStealBtn = Instance.new("TextButton")
autoStealBtn.Size = UDim2.new(0, 200, 0, 35)
autoStealBtn.Position = UDim2.new(0, 10, 0, 10)
autoStealBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
autoStealBtn.TextColor3 = Color3.fromRGB(255, 255, 0)
autoStealBtn.Font = Enum.Font.GothamBold
autoStealBtn.TextSize = 16
autoStealBtn.Text = "Auto Steal: OFF"
Instance.new("UICorner", autoStealBtn).CornerRadius = UDim.new(0, 6)
autoStealBtn.Parent = stealerPage

local state = false
local savedPosition = nil

-- Botões ocultos inicialmente
local savePosButtons = {}
for i = 1, 4 do
local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0, 200, 0, 35)
btn.Position = UDim2.new(0, 10, 0, 10 + i * 45)
btn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
btn.TextColor3 = Color3.fromRGB(255, 255, 0)
btn.Font = Enum.Font.GothamBold
btn.TextSize = 16
btn.Text = "Salvar Posição " .. i
Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
btn.Parent = stealerPage
btn.Visible = false
savePosButtons[i] = btn
end

local stealBtn = Instance.new("TextButton")
stealBtn.Size = UDim2.new(0, 200, 0, 35)
stealBtn.Position = UDim2.new(0, 10, 0, 10 + 5 * 45)
stealBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
stealBtn.TextColor3 = Color3.fromRGB(255, 255, 0)
stealBtn.Font = Enum.Font.GothamBold
stealBtn.TextSize = 16
stealBtn.Text = "Roubar"
Instance.new("UICorner", stealBtn).CornerRadius = UDim.new(0, 6)
stealBtn.Parent = stealerPage
stealBtn.Visible = false

local savedPositions = {}

autoStealBtn.MouseButton1Click:Connect(function()
state = not state
if state then
autoStealBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
autoStealBtn.Text = "Auto Steal: ON"
for i = 1, 4 do
savePosButtons[i].Visible = true
end
stealBtn.Visible = true
else
autoStealBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
autoStealBtn.Text = "Auto Steal: OFF"
for i = 1, 4 do
savePosButtons[i].Visible = false
end
stealBtn.Visible = false
end
end)

for i, btn in ipairs(savePosButtons) do
btn.MouseButton1Click:Connect(function()
local char = LocalPlayer.Character
if char and char:FindFirstChild("HumanoidRootPart") then
savedPositions[i] = char.HumanoidRootPart.Position
showNotification("Posição " .. i .. " salva com sucesso!")
end
end)
end

local RunService = game:GetService("RunService")
local isStealing = false

local RunService = game:GetService("RunService")

local RunService = game:GetService("RunService")



stealBtn.MouseButton1Click:Connect(function()
if isStealing then
isStealing = false
showNotification("Roubo cancelado.")
else
local pos = savedPositions[1]
if pos then
isStealing = true
task.spawn(function()
zigzagMove(pos)
showNotification("Chegou na posição!")
end)
else
showNotification("Nenhuma posição salva na posição 1!")
end
end
end)

local helperPage = createTab("Helper")
createToggle(helperPage, "Helper Toggle")

local playerPage = createTab("Player")
createToggle(playerPage, "Player Toggle")

-- Ativar primeira aba
tabs["Stealer"].button.TextColor3 = Color3.fromRGB(255, 255, 0)
tabs["Stealer"].page.Visible = true

-- Botão flutuante (bola)
local floatButton = Instance.new("TextButton")
floatButton.Size = UDim2.new(0, 40, 0, 40)
floatButton.Position = UDim2.new(0, 10, 0.5, -20)
floatButton.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
floatButton.Text = ">"
floatButton.Font = Enum.Font.GothamBold
floatButton.TextSize = 24
floatButton.TextColor3 = Color3.fromRGB(0, 100, 0)
floatButton.Parent = gui
Instance.new("UICorner", floatButton).CornerRadius = UDim.new(1, 0)
floatButton.Visible = false

-- Funções abrir e minimizar menu
local function openMenu()
mainFrame.Visible = true
floatButton.Visible = false
end

local function minimizeMenu()
mainFrame.Visible = false
floatButton.Visible = true
end

closeBtn.MouseButton1Click:Connect(function()
gui:Destroy()
end)

minimizeBtn.MouseButton1Click:Connect(function()
minimizeMenu()
end)

floatButton.MouseButton1Click:Connect(function()
openMenu()
end)

-- Tornar arrastáveis
makeDraggable(mainFrame, topBar)
makeDraggable(floatButton, floatButton)