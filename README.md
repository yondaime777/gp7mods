-- GP7 MENU by @yondaime777
local Player = game.Players.LocalPlayer
local Backpack = Player:WaitForChild("Backpack")
local Character = Player.Character or Player.CharacterAdded:Wait()

local Gui = Instance.new("ScreenGui", game.CoreGui)
Gui.Name = "GP7Menu"
Gui.ResetOnSpawn = false

-- Botão flutuante
local FloatBtn = Instance.new("TextButton")
FloatBtn.Text = "GP7"
FloatBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
FloatBtn.Size = UDim2.new(0, 60, 0, 30)
FloatBtn.Position = UDim2.new(0, 10, 0, 10)
FloatBtn.Draggable = true
FloatBtn.Active = true
FloatBtn.TextColor3 = Color3.new(1, 1, 1)
FloatBtn.Parent = Gui

-- Menu principal
local Menu = Instance.new("Frame")
Menu.Size = UDim2.new(0, 250, 0, 160)
Menu.Position = UDim2.new(0, 80, 0, 10)
Menu.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
Menu.Visible = false
Menu.Parent = Gui

-- Título
local Title = Instance.new("TextLabel", Menu)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
Title.Text = "GP7 MENU"
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20

-- Função para comprar e equipar item
local function buyAndEquip(itemName)
    local args = { itemName }
    local success = pcall(function()
        game:GetService("ReplicatedStorage")
        :WaitForChild("Packages")
        :WaitForChild("Net")
        :WaitForChild("RF/CoinsShopService/RequestBuy")
        :InvokeServer(unpack(args))
    end)

    task.wait(1)

    -- Equipa da mochila
    local tool = Backpack:FindFirstChild(itemName) or Character:FindFirstChild(itemName)
    if tool then
        tool.Parent = Character
    end
end

-- Botão Speed Hack
local SpeedBtn = Instance.new("TextButton", Menu)
SpeedBtn.Size = UDim2.new(0.9, 0, 0, 30)
SpeedBtn.Position = UDim2.new(0.05, 0, 0, 40)
SpeedBtn.TextColor3 = Color3.new(1, 1, 1)
SpeedBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
SpeedBtn.Text = "Speed Hack [DESATIVADO]"

local speedAtivo = false
SpeedBtn.MouseButton1Click:Connect(function()
    speedAtivo = not speedAtivo
    if speedAtivo then
        SpeedBtn.Text = "Speed Hack [ATIVADO]"
        buyAndEquip("Speed Coil")
    else
        SpeedBtn.Text = "Speed Hack [DESATIVADO]"
    end
end)

-- Botão Super Pulo
local JumpBtn = Instance.new("TextButton", Menu)
JumpBtn.Size = UDim2.new(0.9, 0, 0, 30)
JumpBtn.Position = UDim2.new(0.05, 0, 0, 80)
JumpBtn.TextColor3 = Color3.new(1, 1, 1)
JumpBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
JumpBtn.Text = "Pulo Infinito [DESATIVADO]"

local puloAtivo = false
JumpBtn.MouseButton1Click:Connect(function()
    puloAtivo = not puloAtivo
    if puloAtivo then
        JumpBtn.Text = "Pulo Infinito [ATIVADO]"
        buyAndEquip("Gravity Coil")
    else
        JumpBtn.Text = "Pulo Infinito [DESATIVADO]"
    end
end)

-- Botão Fechar
local CloseBtn = Instance.new("TextButton", Menu)
CloseBtn.Size = UDim2.new(0.9, 0, 0, 30)
CloseBtn.Position = UDim2.new(0.05, 0, 0, 120)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
CloseBtn.Text = "Fechar"
CloseBtn.MouseButton1Click:Connect(function()
    Menu.Visible = false
    FloatBtn.Visible = true
end)

-- Mostrar menu
FloatBtn.MouseButton1Click:Connect(function()
    Menu.Visible = true
    FloatBtn.Visible = false
end)