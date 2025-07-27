-- GP7 MENU - versão manual (compra apenas)
local Player = game.Players.LocalPlayer
local Gui = Instance.new("ScreenGui", game.CoreGui)
Gui.Name = "GP7MenuManual"
Gui.ResetOnSpawn = false

-- Botão flutuante
local FloatBtn = Instance.new("TextButton", Gui)
FloatBtn.Text = "GP7"
FloatBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
FloatBtn.Size = UDim2.new(0, 60, 0, 30)
FloatBtn.Position = UDim2.new(0, 10, 0, 10)
FloatBtn.Draggable = true
FloatBtn.Active = true
FloatBtn.TextColor3 = Color3.new(1, 1, 1)

-- Menu
local Menu = Instance.new("Frame", Gui)
Menu.Size = UDim2.new(0, 250, 0, 150)
Menu.Position = UDim2.new(0, 80, 0, 10)
Menu.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
Menu.Visible = false

local Title = Instance.new("TextLabel", Menu)
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
Title.Text = "GP7 MENU"
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20

-- Função de compra
local function comprarItem(nome)
    local sucesso = pcall(function()
        game:GetService("ReplicatedStorage")
        :WaitForChild("Packages")
        :WaitForChild("Net")
        :WaitForChild("RF/CoinsShopService/RequestBuy")
        :InvokeServer(nome)
    end)
    return sucesso
end

-- Botão Speed Coil
local SpeedBtn = Instance.new("TextButton", Menu)
SpeedBtn.Size = UDim2.new(0.9, 0, 0, 30)
SpeedBtn.Position = UDim2.new(0.05, 0, 0, 40)
SpeedBtn.TextColor3 = Color3.new(1, 1, 1)
SpeedBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
SpeedBtn.Text = "Comprar Speed Coil"
SpeedBtn.MouseButton1Click:Connect(function()
    if comprarItem("Speed Coil") then
        SpeedBtn.Text = "Speed Coil comprado ✔"
    else
        SpeedBtn.Text = "Erro ao comprar"
    end
end)

-- Botão Gravity Coil
local JumpBtn = Instance.new("TextButton", Menu)
JumpBtn.Size = UDim2.new(0.9, 0, 0, 30)
JumpBtn.Position = UDim2.new(0.05, 0, 0, 80)
JumpBtn.TextColor3 = Color3.new(1, 1, 1)
JumpBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
JumpBtn.Text = "Comprar Gravity Coil"
JumpBtn.MouseButton1Click:Connect(function()
    if comprarItem("Gravity Coil") then
        JumpBtn.Text = "Gravity Coil comprado ✔"
    else
        JumpBtn.Text = "Erro ao comprar"
    end
end)

-- Botão fechar
local FecharBtn = Instance.new("TextButton", Menu)
FecharBtn.Size = UDim2.new(0.9, 0, 0, 30)
FecharBtn.Position = UDim2.new(0.05, 0, 0, 120)
FecharBtn.TextColor3 = Color3.new(1, 1, 1)
FecharBtn.BackgroundColor3 = Color3.fromRGB(80, 0, 0)
FecharBtn.Text = "Fechar"
FecharBtn.MouseButton1Click:Connect(function()
    Menu.Visible = false
    FloatBtn.Visible = true
end)

-- Mostrar menu
FloatBtn.MouseButton1Click:Connect(function()
    Menu.Visible = true
    FloatBtn.Visible = false
end)