-- Serviços
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

-- GUI principal
local gui = Instance.new("ScreenGui")
gui.Name = "BrazucaHub"
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")
gui.ResetOnSpawn = false

-- Função de notificação
local function showNotification(text)
    local notifFrame = Instance.new("Frame")
    notifFrame.Size = UDim2.new(0, 300, 0, 50)
    notifFrame.Position = UDim2.new(0.5, -150, 0.9, 0)
    notifFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    notifFrame.BorderSizePixel = 0
    notifFrame.Parent = gui

    local notifLabel = Instance.new("TextLabel")
    notifLabel.Size = UDim2.new(1,0,1,0)
    notifLabel.BackgroundTransparency = 1
    notifLabel.Font = Enum.Font.GothamBold
    notifLabel.TextScaled = true
    notifLabel.TextColor3 = Color3.fromRGB(255,255,255)
    notifLabel.Text = text
    notifLabel.Parent = notifFrame

    task.delay(3,function()
        notifFrame:Destroy()
    end)
end

-- Função para arrastar frames
local function makeDraggable(frame, dragger)
    local dragging, dragInput, dragStart, startPos = false, nil, nil, nil
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

-- MOD MENU
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.fromOffset(580,460)
mainFrame.Position = UDim2.new(0.5,-290,0.5,-230)
mainFrame.BackgroundColor3 = Color3.fromRGB(0,100,0)
mainFrame.BorderSizePixel = 3
mainFrame.BorderColor3 = Color3.fromRGB(255,255,0)
mainFrame.Parent = gui
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0,8)

-- Barra superior
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1,0,0,40)
topBar.BackgroundColor3 = Color3.fromRGB(0,0,0)
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame
Instance.new("UICorner", topBar).CornerRadius = UDim.new(0,8)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,-70,1,0)
title.Position = UDim2.new(0,10,0,0)
title.BackgroundTransparency = 1
title.Text = "Brazuca Hub"
title.TextColor3 = Color3.fromRGB(255,255,0)
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = topBar

-- Botões fechar/minimizar
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0,30,0,30)
closeBtn.Position = UDim2.new(1,-35,0.5,-15)
closeBtn.Text = "X"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 14
closeBtn.TextColor3 = Color3.fromRGB(255,70,70)
closeBtn.BackgroundTransparency = 1
closeBtn.Parent = topBar

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0,30,0,30)
minimizeBtn.Position = UDim2.new(1,-70,0.5,-15)
minimizeBtn.Text = "–"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.TextColor3 = Color3.fromRGB(0,255,0)
minimizeBtn.BackgroundTransparency = 1
minimizeBtn.Parent = topBar

-- Área de abas e conteúdo
local tabBar = Instance.new("Frame")
tabBar.Size = UDim2.new(0,120,1,-40)
tabBar.Position = UDim2.new(0,0,0,40)
tabBar.BackgroundColor3 = Color3.fromRGB(0,0,0)
tabBar.BorderSizePixel = 0
tabBar.Parent = mainFrame

local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1,-120,1,-40)
contentFrame.Position = UDim2.new(0,120,0,40)
contentFrame.BackgroundTransparency = 1
contentFrame.Parent = mainFrame

local tabs, tabOrder = {}, {}

local function createTab(name)
    local index = #tabOrder + 1
    table.insert(tabOrder,name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1,0,0,40)
    btn.Position = UDim2.new(0,0,0,(index-1)*40)
    btn.BackgroundTransparency = 1
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.Parent = tabBar

    local page = Instance.new("Frame")
    page.Size = UDim2.new(1,0,1,0)
    page.BackgroundTransparency = 1
    page.Visible = false
    page.Parent = contentFrame

    tabs[name] = {button=btn,page=page}

    btn.MouseButton1Click:Connect(function()
        for _, tname in ipairs(tabOrder) do
            tabs[tname].page.Visible = false
            tabs[tname].button.TextColor3 = Color3.fromRGB(255,255,255)
        end
        page.Visible = true
        btn.TextColor3 = Color3.fromRGB(255,255,0)
    end)

    return page
end

local function createToggle(parent,text,callback)
    local children = parent:GetChildren()
    local count = 0
    for _,c in ipairs(children) do
        if c:IsA("TextButton") and c.Text:find(text) then count = count+1 end
    end

    local toggleBtn = Instance.new("TextButton")
    toggleBtn.Size = UDim2.new(0,200,0,35)
    toggleBtn.Position = UDim2.new(0,10,0,10+count*45)
    toggleBtn.BackgroundColor3 = Color3.fromRGB(170,0,0)
    toggleBtn.TextColor3 = Color3.fromRGB(255,255,0)
    toggleBtn.Font = Enum.Font.GothamBold
    toggleBtn.TextSize = 16
    toggleBtn.Text = text..": OFF"
    Instance.new("UICorner",toggleBtn).CornerRadius=UDim.new(0,6)
    toggleBtn.Parent = parent

    local state = false
    toggleBtn.MouseButton1Click:Connect(function()
        state = not state
        if state then
            toggleBtn.BackgroundColor3 = Color3.fromRGB(0,200,0)
            toggleBtn.Text = text..": ON"
        else
            toggleBtn.BackgroundColor3 = Color3.fromRGB(170,0,0)
            toggleBtn.Text = text..": OFF"
        end
        if callback then callback(state) end
    end)
end

-- Cria abas
local stealerPage = createTab("Stealer")
local helperPage = createTab("Helper")
local playerPage = createTab("Player")

-- AUTO-ROUBO toggle
local autoStealActive = false
createToggle(stealerPage,"Auto Steal",function(state)
    autoStealActive = state
end)

-- ESP Brainrot
local brainESP = {}
local function createBrainESP(base)
    local adorn = Instance.new("BillboardGui")
    adorn.Size = UDim2.new(0,120,0,50)
    adorn.Adornee = base
    adorn.AlwaysOnTop = true
    adorn.Parent = base

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1,0,1,0)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.GothamBold
    label.TextScaled = true
    label.TextColor3 = Color3.fromRGB(255,0,255)
    label.Parent = adorn

    return label
end

-- ESP Players
local playerESP = {}
local function createPlayerESP(plr)
    if plr == LocalPlayer then return end
    if playerESP[plr] then return playerESP[plr] end

    local ador = Instance.new("BillboardGui")
    ador.Size = UDim2.new(0,80,0,30)
    ador.Adornee = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
    ador.AlwaysOnTop = true
    ador.Parent = plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1,0,1,0)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.GothamBold
    label.TextScaled = true
    label.TextColor3 = Color3.fromRGB(0,255,255)
    label.Text = plr.Name
    label.Parent = ador

    playerESP[plr] = ador
end

RunService.Heartbeat:Connect(function()
    -- Brainrot ESP
    for _,base in pairs(Workspace:GetChildren()) do
        if base:FindFirstChild("Classification") then
            local class = base.Classification.Value
            if class == "God" or class == "Secret" then
                if not brainESP[base] then brainESP[base] = createBrainESP(base) end
                local label = brainESP[base]
                local timeRemaining = base:FindFirstChild("OpenTime") and math.floor(base.OpenTime.Value) or 0
                label.Text = base.Name.." | "..timeRemaining.."s"
            elseif brainESP[base] then
                brainESP[base]:Destroy()
                brainESP[base] = nil
            end
        end
    end

    -- Player ESP
    for _,plr in pairs(Players:GetPlayers()) do
        if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            createPlayerESP(plr)
        end
    end
end)

-- AUTO-ROUBO funcional
local function autoSteal()
    while autoStealActive do
        for _,base in pairs(Workspace:GetChildren()) do
            if base:FindFirstChild("Classification") then
                local class = base.Classification.Value
                if class == "God" or class == "Secret" then
                    local char = LocalPlayer.Character
                    if char and char:FindFirstChild("HumanoidRootPart") then
                        local hrp = char.HumanoidRootPart
                        hrp.CFrame = CFrame.new(base.Position + Vector3.new(0,5,0)) -- aproxima acima da base

                        -- procura ProximityPrompt
                        for _,prompt in pairs(base:GetDescendants()) do
                            if prompt:IsA("ProximityPrompt") then
                                while autoStealActive and prompt.Enabled do
                                    prompt:InputHoldBegin()
                                    task.wait(0.1)
                                end
                            end
                        end
                    end
                end
            end
        end
        task.wait(0.5)
    end
end

task.spawn(autoSteal)