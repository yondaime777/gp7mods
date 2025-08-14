local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local BrazucaHubGui = Instance.new("ScreenGui")
BrazucaHubGui.Name = "BrazucaHubGui"
BrazucaHubGui.Parent = playerGui
BrazucaHubGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Menu principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = BrazucaHubGui
MainFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 220)
MainFrame.Visible = false -- inicia oculto

-- Botão fechar "X"
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Parent = MainFrame
CloseButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255,255,255)
CloseButton.TextSize = 18
CloseButton.Font = Enum.Font.SourceSansBold

-- Botão flutuante (único)
local FloatingButton = Instance.new("TextButton")
FloatingButton.Name = "FloatingButton"
FloatingButton.Parent = BrazucaHubGui
FloatingButton.BackgroundColor3 = Color3.fromRGB(30,30,30)
FloatingButton.Position = UDim2.new(0, 10, 0.5, -50)
FloatingButton.Size = UDim2.new(0, 50, 0, 50)
FloatingButton.Text = "BH"
FloatingButton.TextColor3 = Color3.fromRGB(255,255,255)
FloatingButton.TextSize = 18
FloatingButton.Font = Enum.Font.SourceSansBold
FloatingButton.Active = true
FloatingButton.Draggable = true
FloatingButton.Visible = true -- inicia visível

-- Eventos de clique
FloatingButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    FloatingButton.Visible = false
end)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    FloatingButton.Visible = true
end)

print("GUI carregada com sucesso!")

-- Botão ESP
local ESPButton = Instance.new("TextButton")
ESPButton.Name = "ESPButton"
ESPButton.Parent = MainFrame  -- certifique-se que MainFrame é o frame da interface
ESPButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ESPButton.Position = UDim2.new(0, 10, 0, 50)
ESPButton.Size = UDim2.new(0, 280, 0, 40)
ESPButton.Font = Enum.Font.SourceSans
ESPButton.Text = "ESP: Desativado"
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.TextSize = 16

-- Variáveis de controle ESP
local ESPEnabled = false
local espBoxes = {}
local espNameTags = {}

-- Função para criar a caixa ESP ao redor do personagem
local function createEspBoxForCharacter(character)
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    if not rootPart then return nil end

    local box = Instance.new("BoxHandleAdornment")
    box.Adornee = rootPart
    box.AlwaysOnTop = true
    box.ZIndex = 10
    box.Size = Vector3.new(3, 6, 1)  -- aumentei o tamanho da caixa
    box.Transparency = 0.5
    box.Color3 = Color3.new(0, 1, 0) -- verde
    box.Parent = rootPart

    return box
end

-- Função para criar o nome flutuante acima da cabeça
local function createNameTagForCharacter(character, playerName)
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    if not rootPart then return nil end

    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Adornee = rootPart
    billboardGui.AlwaysOnTop = true
    billboardGui.Size = UDim2.new(0, 100, 0, 40)
    billboardGui.StudsOffset = Vector3.new(0, 3.5, 0) -- posiciona acima da cabeça
    billboardGui.Parent = rootPart

    local textLabel = Instance.new("TextLabel")
    textLabel.BackgroundTransparency = 1
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.Text = playerName
    textLabel.TextColor3 = Color3.new(0, 1, 0) -- verde
    textLabel.TextStrokeTransparency = 0
    textLabel.TextScaled = true
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.Parent = billboardGui

    return billboardGui
end

-- Função para remover todas as caixas e nomes ESP
local function removeAllEsp()
    for _, box in pairs(espBoxes) do
        if box and box.Parent then
            box:Destroy()
        end
    end
    espBoxes = {}

    for _, tag in pairs(espNameTags) do
        if tag and tag.Parent then
            tag:Destroy()
        end
    end
    espNameTags = {}
end

ESPButton.MouseButton1Click:Connect(function()
    ESPEnabled = not ESPEnabled

    if ESPEnabled then
        ESPButton.Text = "ESP: Ativado"
        -- Criar caixas e nomes para jogadores (exceto local)
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr ~= game.Players.LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                espBoxes[plr.Name] = createEspBoxForCharacter(plr.Character)
                espNameTags[plr.Name] = createNameTagForCharacter(plr.Character, plr.Name)
            end
        end

        -- Atualiza ao adicionar novos jogadores
        game.Players.PlayerAdded:Connect(function(plr)
            plr.CharacterAdded:Connect(function(char)
                if ESPEnabled and char:FindFirstChild("HumanoidRootPart") then
                    espBoxes[plr.Name] = createEspBoxForCharacter(char)
                    espNameTags[plr.Name] = createNameTagForCharacter(char, plr.Name)
                end
            end)
        end)

        -- Remove ao sair do jogo
        game.Players.PlayerRemoving:Connect(function(plr)
            if espBoxes[plr.Name] then
                espBoxes[plr.Name]:Destroy()
                espBoxes[plr.Name] = nil
            end
            if espNameTags[plr.Name] then
                espNameTags[plr.Name]:Destroy()
                espNameTags[plr.Name] = nil
            end
        end)

    else
        ESPButton.Text = "ESP: Desativado"
        removeAllEsp()
    end
end)