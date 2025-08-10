local kavoUi = loadstring(game:HttpGet("https://pastebin.com/raw/vff1bQ9F"))()
local Window = kavoUi.CreateLib("DeadlyHub","DarkTheme")



local MainTab = Window:NewTab("Main")
local MainSection = 
MainTab:NewSection("MainSection")

MainSection:NewButton("NAME ESP", "Q to toggle on and off", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Exunys/ESP-Script/main/ESP.lua"))()
end)

MainSection:NewButton("ESP", "", function()
  local FillColor = Color3.fromRGB(175,25,255)
local DepthMode = "AlwaysOnTop"
local FillTransparency = 0.5
local OutlineColor = Color3.fromRGB(255,255,255)
local OutlineTransparency = 0

local CoreGui = game:FindService("CoreGui")
local Players = game:FindService("Players")
local lp = Players.LocalPlayer
local connections = {}

local Storage = Instance.new("Folder")
Storage.Parent = CoreGui
Storage.Name = "Highlight_Storage"

local function Highlight(plr)
    local Highlight = Instance.new("Highlight")
    Highlight.Name = plr.Name
    Highlight.FillColor = FillColor
    Highlight.DepthMode = DepthMode
    Highlight.FillTransparency = FillTransparency
    Highlight.OutlineColor = OutlineColor
    Highlight.OutlineTransparency = 0
    Highlight.Parent = Storage
    
    local plrchar = plr.Character
    if plrchar then
        Highlight.Adornee = plrchar
    end

    connections[plr] = plr.CharacterAdded:Connect(function(char)
        Highlight.Adornee = char
    end)
end

Players.PlayerAdded:Connect(Highlight)
for i,v in next, Players:GetPlayers() do
    Highlight(v)
end

Players.PlayerRemoving:Connect(function(plr)
    local plrname = plr.Name
    if Storage[plrname] then
        Storage[plrname]:Destroy()
    end
    if connections[plr] then
        connections[plr]:Disconnect()
    end
end)
end)

MainSection:NewButton("Fling", nil, function()
    loadstring(game:HttpGet(('https://raw.githubusercontent.com/0Ben1/fe/main/obf_5wpM7bBcOPspmX7lQ3m75SrYNWqxZ858ai3tJdEAId6jSI05IOUB224FQ0VSAswH.lua.txt'),true))()
end)

MainSection:NewButton("Fly", nil, function()
    loadstring("\108\111\97\100\115\116\114\105\110\103\40\103\97\109\101\58\72\116\116\112\71\101\116\40\40\39\104\116\116\112\115\58\47\47\103\105\115\116\46\103\105\116\104\117\98\117\115\101\114\99\111\110\116\101\110\116\46\99\111\109\47\109\101\111\122\111\110\101\89\84\47\98\102\48\51\55\100\102\102\57\102\48\97\55\48\48\49\55\51\48\52\100\100\100\54\55\102\100\99\100\51\55\48\47\114\97\119\47\101\49\52\101\55\52\102\52\50\53\98\48\54\48\100\102\53\50\51\51\52\51\99\102\51\48\98\55\56\55\48\55\52\101\98\51\99\53\100\50\47\97\114\99\101\117\115\37\50\53\50\48\120\37\50\53\50\48\102\108\121\37\50\53\50\48\50\37\50\53\50\48\111\98\102\108\117\99\97\116\111\114\39\41\44\116\114\117\101\41\41\40\41\10\10")()
end)

MainSection:NewButton("Xray", "Press E to active/disable", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/HeyGyt/xray/main/main"))()
end)

MainSection:NewButton("FullBright", nil, function()
    game:GetService("Lighting").Brightness = 2
            game:GetService("Lighting").ClockTime = 14
            game:GetService("Lighting").FogEnd = 100000
            game:GetService("Lighting").GlobalShadows = false
            game:GetService("Lighting").OutdoorAmbient = Color3.fromRGB(128, 128, 128)
end)

MainSection:NewButton("Infinite Jump", nil, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/HeyGyt/infjump/main/main"))()
end)

MainSection:NewButton("Airswim", nil, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/HeyGyt/airswim/main/main"))()
end)






local Games = Window:NewTab("Games🎮")
local GamesSection = Games:NewSection("Games")


GamesSection:NewButton("🏠Brookhaven🏠", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/IceMael7/NewIceHub/main/Brookhaven"))()
end)

GamesSection:NewButton("🔪MM2🔪", "", function()
    loadstring(game:HttpGet(('https://raw.githubusercontent.com/MarsQQ/ScriptHubScripts/master/MM2%20Admin%20Panel'),true))()
end)

GamesSection:NewButton("Bedwars🛌", "", function()
    	loadstring(game:HttpGet("https://raw.githubusercontent.com/7GrandDadPGN/VapeV4ForRoblox/main/NewMainScript.lua", true))()	
end)

GamesSection:NewButton("Prison Life", "", function()
    loadstring(game:HttpGet('https://pastebin.com/raw/iZ64yzjE'))()
end)

GamesSection:NewButton("💪Muscle Legends💪", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/harisiskandar178/Roblox-Script/main/Muscle%20Legend"))()
end)

GamesSection:NewButton("Break In", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Nikita365/Break-In-Story-/main/Break%20In%20Story%20Hub"))()
end)

GamesSection:NewButton("Nico's Nextbots", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/GamingScripter/Darkrai-X/main/Games/NicoNextBots", true))()
end)

GamesSection:NewButton("Doors👁", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/GamingScripter/Darkrai-X/main/Games/Doors"))()
end)

GamesSection:NewButton("Evade", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/GamingScripter/Darkrai-X/main/Games/Evade"))()
end)

GamesSection:NewButton("Rainbow Friends", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/JNHHGaming/Rainbow-Friends/main/Rainbow%20Friends"))()
end)

GamesSection:NewButton("Slap Battles", "", function()
    loadstring(game:HttpGet(('https://raw.githubusercontent.com/TheScriptMaster1/ScriptMaster-Hub/main/scriptmasterhub.lua')))()
end)

GamesSection:NewButton("Cook Burgers", "", function()
    loadstring(game:HttpGet(('https://pastebin.com/raw/YUqvvEAB')))()
end)





local Admins = Window:NewTab("Admins")
local AdminsSection = Admins:NewSection("Admins")

AdminsSection:NewButton("Infinite Yield", nil, function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
end)

AdminsSection:NewButton("CMD-X", nil, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source", true))()
end)

AdminsSection:NewButton("Fates Admin", nil, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/fatesc/fates-admin/main/main.lua"))()
end)




local Misc = Window:NewTab("Misc")
local MiscSection = Misc:NewSection("Misc")


MiscSection:NewButton("Keyboard", nil, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/advxzivhsjjdhxhsidifvsh/mobkeyboard/main/main.txt", true))()
end)

MiscSection:NewButton("FE Animations GUI", nil, function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/GamingScripter/Animation-Hub/main/Animation%20Gui", true))()
end)

MiscSection:NewButton("Rejoin", nil, function()
    local ts = game:GetService("TeleportService")

local p = game:GetService("Players").LocalPlayer

 

ts:Teleport(game.PlaceId, p)
end)




local Credits = Window:NewTab("Credits")
local CreditsSection = Credits:NewSection("Credits")


CreditsSection:NewLabel("Made by Deadly_Frenzy_")
CreditsSection:NewLabel("Sub to Deadly_Frenzy_ on YouTube")
CreditsSection:NewLabel("Follow deadly_frenzy_ on TikTok")
CreditsSection:NewLabel("Deadly_Frenzy_ Roblox Account: F0X42512")
CreditsSection:NewLabel("")
CreditsSection:NewLabel("imgood_atnameing_things helped me")
CreditsSection:NewLabel("Follow imgood_atnameing_things on TikTok")
CreditsSection:NewLabel("imgood_atnameing_things Roblox Account: duckarmy609")
