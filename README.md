local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Meu Script Rayfield",
    LoadingTitle = "Carregando...",
    LoadingSubtitle = "Por favor, aguarde",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "MeuScriptRayfield",
        FileName = "Config"
    },
    Discord = {
        Enabled = false,
        Invite = "", 
        RememberJoins = false
    },
    KeySystem = false, -- Defina true se quiser sistema de key
})

-- Criando uma aba
local Tab = Window:CreateTab("Principal", 4483362458) -- ícone opcional

-- Adicionando uma seção na aba
local Section = Tab:CreateSection("Seção 1")

-- Botão
Tab:CreateButton({
    Name = "Clique aqui",
    Callback = function()
        print("Botão clicado!")
    end
})

-- Toggle
local toggleState = false
Tab:CreateToggle({
    Name = "Ativar Toggle",
    CurrentValue = false,
    Flag = "toggle1", -- opcional para salvar configs
    Callback = function(value)
        toggleState = value
        print("Toggle:", value)
    end,
})

-- Slider
Tab:CreateSlider({
    Name = "Slider de exemplo",
    Range = {0, 100},
    Increment = 1,
    Suffix = "%",
    CurrentValue = 50,
    Flag = "slider1",
    Callback = function(value)
        print("Slider valor:", value)
    end,
})

-- Input Box
Tab:CreateInput({
    Name = "Input de texto",
    Placeholder = "Digite algo aqui...",
    RemoveTextAfterFocusLost = false,
    Callback = function(text)
        print("Texto inserido:", text)
    end
})