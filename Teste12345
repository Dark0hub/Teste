--[[
    DARK HUB - v18.1 (fix)
    Correção:
      • Forward declaration de mainFrame e UI
      • Fecha o problema de closures capturando globais (nil)
      • Botões X, -, Cancelar e Confirmar agora funcionam
]]

if _G.DarkHub and _G.DarkHub.Cleanup then
    pcall(_G.DarkHub.Cleanup)
end

local Players          = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService     = game:GetService("TweenService")
local RunService       = game:GetService("RunService")
local Workspace        = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local PlayerGui   = LocalPlayer:WaitForChild("PlayerGui")

--=====================================================================
-- CONFIG
--=====================================================================
local Config = {
    Title                = "Dark Hub | Mega Colmeia",
    TeleportInterval     = 1.0,
    VisitCooldown        = 1.5,
    ReturnToBaseDistance = 200,
    TeleportHeight       = 3,
    RescanInterval       = 5,
    MaxBusinessDistance  = 500,

    RejectedContainers = {
        "ExposicaoLuau", "ModelosExpostos", "SpawnToEvents", "PreviewModels",
    },

    EventItems = {
        { name = "Candy_LoveCandy",   priority = 1 },
        { name = "Candy_Lollipop",    priority = 2 },
        { name = "Candy_CandyStar",   priority = 3 },
        { name = "Candy_Marshmallow", priority = 4 },
        { name = "Candy_GreenGummy",  priority = 5 },
        { name = "Candy_PinkGummy",   priority = 6 },
        { name = "Candy_PurpleCandy", priority = 7 },
    },

    Businesses = {
        ["Small Apiary"]          = { "Small_Apiary", "SmallApiary", "Small Apiary" },
        ["Little beehive"]        = { "Little_Beehive", "LittleBeehive", "Little Beehive" },
        ["Honeycomb Apiary"]      = { "Honeycomb_Apiary", "HoneycombApiary", "Honeycomb Apiary" },
        ["Mister Giant Mustache"] = { "Mister_Giant_Mustache", "MisterGiantMustache", "Mister Giant Mustache" },
        ["Castle apiary"]         = { "Castle_Apiary", "CastleApiary", "Castle Apiary" },
        ["Beekeeping factory"]    = { "Beekeeping_Factory", "BeekeepingFactory", "Beekeeping Factory" },
        ["Apiary tree"]           = { "Apiary_Tree", "ApiaryTree", "Apiary Tree" },
        ["Basket Apiary"]         = { "Basket_Apiary", "BasketApiary", "Basket Apiary" },
        ["Swirl apiary"]          = { "Swirl_Apiary", "SwirlApiary", "Swirl Apiary" },
        ["Large apiary"]          = { "Large_Apiary", "LargeApiary", "Large Apiary" },
        ["Honey barrel"]          = { "Honey_Barrel", "HoneyBarrel", "Honey Barrel" },
        ["Rustic apiary"]         = { "Rustic_Apiary", "RusticApiary", "Rustic Apiary" },
        ["Flowering apiary"]      = {
            "Flowering_Apiary", "FloweringApiary", "Flowering Apiary",
            "Apiary_Flowering", "ApiaryFlowering", "Apiary Flowering",
        },
    },

    WalkSpeed = { min = 16, max = 200, default = 16 },
}

--=====================================================================
-- TEMAS
--=====================================================================
local Themes = {
    Dark = {
        windowBg=Color3.fromRGB(18,18,22), topbar=Color3.fromRGB(14,14,18),
        sidebar=Color3.fromRGB(22,22,27), content=Color3.fromRGB(24,24,29),
        cardBg=Color3.fromRGB(32,32,38), toggleOff=Color3.fromRGB(55,55,65),
        text=Color3.fromRGB(240,240,245), textMuted=Color3.fromRGB(150,150,165),
        textDim=Color3.fromRGB(95,95,110), accent=Color3.fromRGB(160,120,255),
        accent2=Color3.fromRGB(255,90,150), success=Color3.fromRGB(90,210,130),
        danger=Color3.fromRGB(230,70,90), border=Color3.fromRGB(45,45,55),
        icons={AutoEvento="✨",AutoFarm="🍯",WalkSpeed="⚡",Noclip="👻",Settings="⚙"},
    },
    Navy = {
        windowBg=Color3.fromRGB(15,22,38), topbar=Color3.fromRGB(11,16,30),
        sidebar=Color3.fromRGB(13,19,34), content=Color3.fromRGB(17,25,42),
        cardBg=Color3.fromRGB(25,36,58), toggleOff=Color3.fromRGB(50,68,100),
        text=Color3.fromRGB(230,240,255), textMuted=Color3.fromRGB(140,160,195),
        textDim=Color3.fromRGB(90,110,145), accent=Color3.fromRGB(80,140,255),
        accent2=Color3.fromRGB(255,120,160), success=Color3.fromRGB(90,210,150),
        danger=Color3.fromRGB(230,80,100), border=Color3.fromRGB(40,60,95),
        icons={AutoEvento="💎",AutoFarm="🐋",WalkSpeed="🌊",Noclip="🧊",Settings="🧭"},
    },
    BabyBlue = {
        windowBg=Color3.fromRGB(200,222,242), topbar=Color3.fromRGB(185,210,235),
        sidebar=Color3.fromRGB(178,205,230), content=Color3.fromRGB(210,228,245),
        cardBg=Color3.fromRGB(228,240,250), toggleOff=Color3.fromRGB(180,200,220),
        text=Color3.fromRGB(30,50,80), textMuted=Color3.fromRGB(80,110,145),
        textDim=Color3.fromRGB(120,145,175), accent=Color3.fromRGB(70,140,220),
        accent2=Color3.fromRGB(230,100,140), success=Color3.fromRGB(60,180,120),
        danger=Color3.fromRGB(220,80,90), border=Color3.fromRGB(160,190,215),
        icons={AutoEvento="☁",AutoFarm="🐳",WalkSpeed="🩵",Noclip="🦋",Settings="🎀"},
    },
    Purple = {
        windowBg=Color3.fromRGB(28,20,40), topbar=Color3.fromRGB(22,15,32),
        sidebar=Color3.fromRGB(25,17,36), content=Color3.fromRGB(32,23,46),
        cardBg=Color3.fromRGB(44,33,62), toggleOff=Color3.fromRGB(70,55,95),
        text=Color3.fromRGB(245,235,255), textMuted=Color3.fromRGB(175,155,205),
        textDim=Color3.fromRGB(120,100,150), accent=Color3.fromRGB(190,100,255),
        accent2=Color3.fromRGB(255,100,180), success=Color3.fromRGB(110,220,160),
        danger=Color3.fromRGB(240,90,110), border=Color3.fromRGB(62,46,88),
        icons={AutoEvento="🔮",AutoFarm="🦄",WalkSpeed="💫",Noclip="🌌",Settings="🪄"},
    },
    Green = {
        windowBg=Color3.fromRGB(18,28,22), topbar=Color3.fromRGB(13,22,17),
        sidebar=Color3.fromRGB(15,25,19), content=Color3.fromRGB(20,32,25),
        cardBg=Color3.fromRGB(30,46,36), toggleOff=Color3.fromRGB(55,75,62),
        text=Color3.fromRGB(230,245,235), textMuted=Color3.fromRGB(145,175,155),
        textDim=Color3.fromRGB(90,120,100), accent=Color3.fromRGB(80,200,120),
        accent2=Color3.fromRGB(255,130,100), success=Color3.fromRGB(120,220,150),
        danger=Color3.fromRGB(230,80,100), border=Color3.fromRGB(45,65,55),
        icons={AutoEvento="🌿",AutoFarm="🐢",WalkSpeed="🌱",Noclip="🍀",Settings="🧩"},
    },
    Neon = {
        windowBg=Color3.fromRGB(8,5,15), topbar=Color3.fromRGB(5,3,10),
        sidebar=Color3.fromRGB(10,6,18), content=Color3.fromRGB(12,8,22),
        cardBg=Color3.fromRGB(20,12,35), toggleOff=Color3.fromRGB(45,25,70),
        text=Color3.fromRGB(240,230,255), textMuted=Color3.fromRGB(180,130,255),
        textDim=Color3.fromRGB(120,80,200), accent=Color3.fromRGB(0,255,200),
        accent2=Color3.fromRGB(255,0,200), success=Color3.fromRGB(0,255,120),
        danger=Color3.fromRGB(255,40,100), border=Color3.fromRGB(60,20,120),
        icons={AutoEvento="⚡",AutoFarm="🤖",WalkSpeed="🚀",Noclip="🔥",Settings="💻"},
    },
    Sunset = {
        windowBg=Color3.fromRGB(40,20,35), topbar=Color3.fromRGB(30,15,28),
        sidebar=Color3.fromRGB(35,18,32), content=Color3.fromRGB(45,22,38),
        cardBg=Color3.fromRGB(60,30,50), toggleOff=Color3.fromRGB(90,50,75),
        text=Color3.fromRGB(255,235,220), textMuted=Color3.fromRGB(255,175,150),
        textDim=Color3.fromRGB(200,120,110), accent=Color3.fromRGB(255,140,80),
        accent2=Color3.fromRGB(255,80,120), success=Color3.fromRGB(120,220,130),
        danger=Color3.fromRGB(240,70,90), border=Color3.fromRGB(90,45,65),
        icons={AutoEvento="🌅",AutoFarm="🌴",WalkSpeed="🏄",Noclip="🌇",Settings="🎸"},
    },
    Matrix = {
        windowBg=Color3.fromRGB(5,12,5), topbar=Color3.fromRGB(3,8,3),
        sidebar=Color3.fromRGB(6,14,6), content=Color3.fromRGB(7,16,7),
        cardBg=Color3.fromRGB(12,25,12), toggleOff=Color3.fromRGB(25,50,25),
        text=Color3.fromRGB(180,255,180), textMuted=Color3.fromRGB(100,200,100),
        textDim=Color3.fromRGB(60,130,60), accent=Color3.fromRGB(0,255,70),
        accent2=Color3.fromRGB(120,255,120), success=Color3.fromRGB(0,255,100),
        danger=Color3.fromRGB(200,50,50), border=Color3.fromRGB(25,60,25),
        icons={AutoEvento="💊",AutoFarm="🟩",WalkSpeed="⏩",Noclip="🕶",Settings="📟"},
    },
    Sakura = {
        windowBg=Color3.fromRGB(45,30,42), topbar=Color3.fromRGB(35,22,32),
        sidebar=Color3.fromRGB(40,26,37), content=Color3.fromRGB(50,33,45),
        cardBg=Color3.fromRGB(70,48,62), toggleOff=Color3.fromRGB(100,75,92),
        text=Color3.fromRGB(255,240,245), textMuted=Color3.fromRGB(255,190,210),
        textDim=Color3.fromRGB(200,140,165), accent=Color3.fromRGB(255,140,180),
        accent2=Color3.fromRGB(200,130,255), success=Color3.fromRGB(150,220,190),
        danger=Color3.fromRGB(240,100,120), border=Color3.fromRGB(95,65,85),
        icons={AutoEvento="🌸",AutoFarm="🍡",WalkSpeed="🎋",Noclip="🌙",Settings="⛩"},
    },
    Ocean = {
        windowBg=Color3.fromRGB(10,30,40), topbar=Color3.fromRGB(6,22,30),
        sidebar=Color3.fromRGB(8,26,35), content=Color3.fromRGB(12,34,45),
        cardBg=Color3.fromRGB(18,50,65), toggleOff=Color3.fromRGB(35,80,100),
        text=Color3.fromRGB(210,245,255), textMuted=Color3.fromRGB(120,200,220),
        textDim=Color3.fromRGB(70,140,165), accent=Color3.fromRGB(0,200,220),
        accent2=Color3.fromRGB(120,240,180), success=Color3.fromRGB(80,230,180),
        danger=Color3.fromRGB(230,80,110), border=Color3.fromRGB(30,70,90),
        icons={AutoEvento="🐠",AutoFarm="🐙",WalkSpeed="🌊",Noclip="🦈",Settings="🧜"},
    },
}

local CurrentTheme = "Dark"
local Colors = Themes[CurrentTheme]

local function clamp(v, lo, hi)
    if v < lo then return lo end
    if v > hi then return hi end
    return v
end

--=====================================================================
-- ⭐ FIX v18.1: forward declarations
-- Sem isso, as closures de buildOpacitySnapshot e applyTheme
-- capturam variáveis GLOBAIS (nil) em vez das locais abaixo.
--=====================================================================
local mainFrame
local UI

--=====================================================================
-- REGISTRO DE TEMA / OPACIDADE
--=====================================================================
local ThemeTargets = {}
local function reg(inst, props)
    table.insert(ThemeTargets, { inst = inst, props = props })
    for prop, key in pairs(props) do
        inst[prop] = Colors[key]
    end
    return inst
end

local OpacityTargets = {}
local function buildOpacitySnapshot()
    OpacityTargets = {}
    for _, obj in ipairs(mainFrame:GetDescendants()) do
        local skip = obj:GetAttribute("SkipOpacity")
        if not skip then
            local entry = { inst = obj }
            if obj:IsA("GuiObject") then entry.baseBg = obj.BackgroundTransparency end
            if obj:IsA("TextLabel") or obj:IsA("TextButton") or obj:IsA("TextBox") then
                entry.baseTxt = obj.TextTransparency
            end
            if obj:IsA("ImageLabel") or obj:IsA("ImageButton") then
                entry.baseImg = obj.ImageTransparency
            end
            table.insert(OpacityTargets, entry)
        end
    end
end

local CurrentOpacity = 100
local function applyOpacity(percent)
    CurrentOpacity = percent
    local factor = 1 - (percent / 100)
    for _, e in ipairs(OpacityTargets) do
        local obj = e.inst
        if obj and obj.Parent then
            if e.baseBg ~= nil then
                obj.BackgroundTransparency = clamp(e.baseBg + (1 - e.baseBg) * factor, 0, 1)
            end
            if e.baseTxt ~= nil then
                obj.TextTransparency = clamp(e.baseTxt + (1 - e.baseTxt) * factor, 0, 1)
            end
            if e.baseImg ~= nil then
                obj.ImageTransparency = clamp(e.baseImg + (1 - e.baseImg) * factor, 0, 1)
            end
        end
    end
end

local function applyTheme(themeName)
    if not Themes[themeName] then return end
    CurrentTheme = themeName
    Colors = Themes[themeName]
    for _, entry in ipairs(ThemeTargets) do
        if entry.inst and entry.inst.Parent then
            for prop, key in pairs(entry.props) do
                pcall(function() entry.inst[prop] = Colors[key] end)
            end
        end
    end
    for tabId, tab in pairs(UI.tabs) do
        if tab.iconLabel and Colors.icons[tabId] then
            tab.iconLabel.Text = Colors.icons[tabId]
        end
    end
    buildOpacitySnapshot()
    applyOpacity(CurrentOpacity)
end

--=====================================================================
-- UTIL
--=====================================================================
local Util = {}

function Util.create(class, props, children)
    local inst = Instance.new(class)
    for k, v in pairs(props or {}) do
        if k ~= "Parent" then inst[k] = v end
    end
    for _, child in ipairs(children or {}) do child.Parent = inst end
    if props and props.Parent then inst.Parent = props.Parent end
    return inst
end

function Util.getObjectPosition(obj)
    if not obj or not obj.Parent then return nil end
    if obj:IsA("BasePart") then return obj.Position end
    if obj:IsA("Model") then
        local ok, pivot = pcall(function() return obj:GetPivot() end)
        if ok and pivot then return pivot.Position end
        local parts = {}
        for _, p in ipairs(obj:GetDescendants()) do
            if p:IsA("BasePart") then table.insert(parts, p) end
        end
        if #parts == 0 then return nil end
        local sum = Vector3.zero
        for _, p in ipairs(parts) do sum = sum + p.Position end
        return sum / #parts
    end
    return nil
end

function Util.teleportTo(position)
    if not position then return false, "Posição inválida" end
    local char = LocalPlayer.Character
    if not char then return false, "Sem personagem" end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return false, "Sem HumanoidRootPart" end
    local target = CFrame.new(position + Vector3.new(0, Config.TeleportHeight, 0))
    local ok = pcall(function() char:PivotTo(target) end)
    if ok then return true end
    local ok2 = pcall(function() hrp.CFrame = target end)
    if ok2 then return true end
    return false, "Teleporte bloqueado"
end

function Util.circleWalk(centerPos, radius, duration)
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    local startTime = tick()
    local angle = 0
    local stepDelay = 0.03
    local angularSpeed = (math.pi * 2) / math.max(duration, 0.1)
    while tick() - startTime < duration do
        if not hrp.Parent or not char.Parent then return end
        angle = angle + angularSpeed * stepDelay
        local offset = Vector3.new(math.cos(angle) * radius, 0, math.sin(angle) * radius)
        local newPos = centerPos + offset
        local tangent = Vector3.new(-math.sin(angle), 0, math.cos(angle))
        pcall(function() hrp.CFrame = CFrame.new(newPos, newPos + tangent) end)
        task.wait(stepDelay)
    end
end

function Util.normalize(str)
    return (str:gsub("[%s_%-]", "")):lower()
end

function Util.isDescendantOf(obj, ancestor)
    if not obj or not ancestor then return false end
    local current = obj
    while current do
        if current == ancestor then return true end
        current = current.Parent
    end
    return false
end

function Util.parseAssetId(input)
    if not input or input == "" then return nil, "Campo vazio" end
    input = input:gsub("^%s+", ""):gsub("%s+$", "")
    if input == "" then return nil, "Campo vazio" end
    if input:match("^rbxassetid://") or input:match("^rbxthumb://") then return input end
    if input:match("^%d+$") then return "rbxassetid://" .. input end
    local id = input:match("[?&]id=(%d+)")
    if id then return "rbxassetid://" .. id end
    return input
end

--=====================================================================
-- BASE / SPAWN / CACHES
--=====================================================================
local PlayerBase = nil

local function findPlayerBase()
    local name, id = LocalPlayer.Name, tostring(LocalPlayer.UserId)
    local plots = Workspace:FindFirstChild("Plots")
    if plots then
        local p = plots:FindFirstChild(name) or plots:FindFirstChild(id)
        if p then return p end
    end
    local byName = Workspace:FindFirstChild(name)
    if byName and (byName:IsA("Model") or byName:IsA("Folder")) then return byName end
    local byId = Workspace:FindFirstChild(id)
    if byId and (byId:IsA("Model") or byId:IsA("Folder")) then return byId end
    return nil
end

PlayerBase = findPlayerBase()
print("[Dark Hub] Base:", PlayerBase and PlayerBase:GetFullName() or "NÃO ENCONTRADA")

local SpawnCache = { position = nil, knownSpawnLocation = nil }
for _, obj in ipairs(Workspace:GetDescendants()) do
    if obj:IsA("SpawnLocation") then SpawnCache.knownSpawnLocation = obj; break end
end

local Connections = {}
local function captureSpawn(char)
    if not char then return end
    local hrp = char:WaitForChild("HumanoidRootPart", 5)
    if hrp then SpawnCache.position = hrp.Position end
end
if LocalPlayer.Character then task.spawn(captureSpawn, LocalPlayer.Character) end
table.insert(Connections, LocalPlayer.CharacterAdded:Connect(function(char)
    task.wait(1); captureSpawn(char)
end))

local ItemCache, BusinessCache = {}, {}

local function addToCache(cache, key, inst)
    cache[key] = cache[key] or {}
    for _, v in ipairs(cache[key]) do if v == inst then return end end
    table.insert(cache[key], inst)
end

local function removeFromCache(cache, key, inst)
    local list = cache[key]; if not list then return end
    for i, v in ipairs(list) do if v == inst then table.remove(list, i); return end end
end

local itemNameSet = {}
for _, item in ipairs(Config.EventItems) do itemNameSet[item.name] = true end

local businessVariantSet = {}
local businessVariantList = {}
for commercial, variants in pairs(Config.Businesses) do
    for _, v in ipairs(variants) do
        local n = Util.normalize(v)
        businessVariantSet[n] = commercial
        table.insert(businessVariantList, { normalized = n, commercial = commercial })
    end
end
table.sort(businessVariantList, function(a, b) return #a.normalized > #b.normalized end)

local function matchBusiness(objectName)
    local n = Util.normalize(objectName)
    if businessVariantSet[n] then return businessVariantSet[n] end
    for _, entry in ipairs(businessVariantList) do
        if #entry.normalized >= 8 and n:find(entry.normalized, 1, true) then
            return entry.commercial
        end
    end
    return nil
end

local function scanInstance(obj)
    if not (obj:IsA("BasePart") or obj:IsA("Model")) then return end
    if itemNameSet[obj.Name] then addToCache(ItemCache, obj.Name, obj) end
    local commercial = matchBusiness(obj.Name)
    if commercial then addToCache(BusinessCache, commercial, obj) end
end

local function scanAll()
    for _, obj in ipairs(Workspace:GetDescendants()) do scanInstance(obj) end
end

table.insert(Connections, Workspace.DescendantAdded:Connect(scanInstance))
table.insert(Connections, Workspace.DescendantRemoving:Connect(function(obj)
    if not (obj:IsA("BasePart") or obj:IsA("Model")) then return end
    if itemNameSet[obj.Name] then removeFromCache(ItemCache, obj.Name, obj) end
    local commercial = matchBusiness(obj.Name)
    if commercial then removeFromCache(BusinessCache, commercial, obj) end
end))

scanAll()

task.spawn(function()
    while true do
        task.wait(Config.RescanInterval)
        scanAll()
    end
end)

local function isInRejectedContainer(obj)
    for _, name in ipairs(Config.RejectedContainers) do
        local cont = Workspace:FindFirstChild(name)
        if cont and Util.isDescendantOf(obj, cont) then return true end
    end
    return false
end

local function isValidBusiness(obj, pos)
    if isInRejectedContainer(obj) then return false, "decoração" end
    if SpawnCache.position and pos then
        local dist = (pos - SpawnCache.position).Magnitude
        if dist > Config.MaxBusinessDistance then
            return false, string.format("distante(%.0f)", dist)
        end
    end
    if PlayerBase and PlayerBase.Parent then
        if Util.isDescendantOf(obj, PlayerBase) then return true, "inBase" end
        return false, "fora da base"
    end
    return false, "sem base"
end

task.spawn(function()
    while true do
        if not PlayerBase or not PlayerBase.Parent then
            local found = findPlayerBase()
            if found then PlayerBase = found end
        end
        task.wait(10)
    end
end)

--=====================================================================
-- AUTO EVENTO
--=====================================================================
local AutoEvento = { active=false, thread=nil, processed=0, current=nil,
                     isPriority=false, status="Auto Evento desativado", visited={} }

local function findEventTarget()
    local best, bestPrio = nil, math.huge
    local now = tick()
    for _, entry in ipairs(Config.EventItems) do
        local list = ItemCache[entry.name]
        if list then
            for i = #list, 1, -1 do
                local obj = list[i]
                if not obj or not obj.Parent then table.remove(list, i)
                else
                    local last = AutoEvento.visited[obj]
                    if not last or (now-last) > Config.VisitCooldown then
                        local pos = Util.getObjectPosition(obj)
                        if pos and entry.priority < bestPrio then
                            best = { obj=obj, name=entry.name, priority=entry.priority, pos=pos }
                            bestPrio = entry.priority
                        end
                        break
                    end
                end
            end
        end
    end
    return best
end

local function countEventFound()
    local n = 0
    for _, entry in ipairs(Config.EventItems) do
        local list = ItemCache[entry.name]
        if list then for _, o in ipairs(list) do if o and o.Parent then n = n + 1 end end end
    end
    return n
end

function AutoEvento.start(onUpdate)
    if AutoEvento.active then return end
    AutoEvento.active = true; AutoEvento.processed = 0
    AutoEvento.status = "Procurando itens"
    AutoEvento.thread = task.spawn(function()
        while AutoEvento.active do
            local t = findEventTarget()
            if t then
                AutoEvento.current = t.name
                AutoEvento.isPriority = t.priority <= 3
                AutoEvento.status = "Indo para " .. t.name
                onUpdate()
                local ok, err = Util.teleportTo(t.pos)
                if not ok then AutoEvento.status = "Falha: " .. tostring(err)
                else
                    AutoEvento.processed = AutoEvento.processed + 1
                    AutoEvento.visited[t.obj] = tick()
                    task.wait(0.1)
                    if t.obj and t.obj.Parent then
                        local currentPos = Util.getObjectPosition(t.obj)
                        if currentPos then
                            AutoEvento.status = "Coletando " .. t.name
                            onUpdate()
                            Util.circleWalk(currentPos, 1.5, 0.7)
                        end
                    end
                end
            else
                AutoEvento.current = nil; AutoEvento.isPriority = false
                AutoEvento.status = "Procurando itens"
            end
            onUpdate(); task.wait(0.2)
        end
        AutoEvento.status = "Auto Evento desativado"; AutoEvento.current = nil; onUpdate()
    end)
end

function AutoEvento.stop(onUpdate)
    AutoEvento.active = false
    if AutoEvento.thread then pcall(task.cancel, AutoEvento.thread); AutoEvento.thread = nil end
    AutoEvento.status = "Auto Evento desativado"
    if onUpdate then onUpdate() end
end

--=====================================================================
-- AUTO FARM
--=====================================================================
local AutoFarm = { active=false, thread=nil, processed=0, current=nil,
                   status="Auto Farm desativado", visited={} }

local function findBusinessTarget()
    local now = tick()
    local best, bestLast = nil, nil
    for commercial, list in pairs(BusinessCache) do
        for i = #list, 1, -1 do
            local obj = list[i]
            if not obj or not obj.Parent then table.remove(list, i)
            else
                local last = AutoFarm.visited[obj] or 0
                if (now - last) > Config.VisitCooldown then
                    local pos = Util.getObjectPosition(obj)
                    if pos and isValidBusiness(obj, pos) then
                        if bestLast == nil or last < bestLast then
                            bestLast = last
                            best = { obj = obj, name = commercial, pos = pos }
                        end
                    end
                end
            end
        end
    end
    return best
end

local function countBusinessFound()
    local n = 0
    for _, list in pairs(BusinessCache) do
        for _, o in ipairs(list) do
            if o and o.Parent then
                local pos = Util.getObjectPosition(o)
                if pos and isValidBusiness(o, pos) then n = n + 1 end
            end
        end
    end
    return n
end

function AutoFarm.goToSpawn()
    if SpawnCache.position then return Util.teleportTo(SpawnCache.position)
    elseif SpawnCache.knownSpawnLocation then return Util.teleportTo(SpawnCache.knownSpawnLocation.Position) end
    return false, "Spawn não capturado"
end

function AutoFarm.start(onUpdate)
    if AutoFarm.active then return end
    AutoFarm.active = true; AutoFarm.processed = 0
    AutoFarm.status = "Voltando para a base..."
    if not PlayerBase or not PlayerBase.Parent then PlayerBase = findPlayerBase() end

    AutoFarm.thread = task.spawn(function()
        AutoFarm.goToSpawn(); onUpdate(); task.wait(Config.TeleportInterval)
        local emptyCycles = 0
        while AutoFarm.active do
            local char = LocalPlayer.Character
            if char and char:FindFirstChild("HumanoidRootPart") and SpawnCache.position then
                local d = (char.HumanoidRootPart.Position - SpawnCache.position).Magnitude
                if d > Config.ReturnToBaseDistance then
                    AutoFarm.status = "Retornando à base..."; onUpdate()
                    AutoFarm.goToSpawn(); task.wait(Config.TeleportInterval)
                end
            end
            local t = findBusinessTarget()
            if t then
                emptyCycles = 0
                AutoFarm.current = t.name
                AutoFarm.status = "Processando destino"
                onUpdate()
                local ok, err = Util.teleportTo(t.pos)
                if not ok then AutoFarm.status = "Falha: " .. tostring(err)
                else AutoFarm.processed = AutoFarm.processed + 1; AutoFarm.visited[t.obj] = tick() end
            else
                AutoFarm.current = nil
                AutoFarm.status = "Procurando negócios"
                emptyCycles = emptyCycles + 1
                if emptyCycles >= 5 then
                    AutoFarm.visited = {}; emptyCycles = 0
                    AutoFarm.status = "Reiniciando ciclo"
                end
            end
            onUpdate(); task.wait(Config.TeleportInterval)
        end
        AutoFarm.status = "Auto Farm desativado"; AutoFarm.current = nil; onUpdate()
    end)
end

function AutoFarm.stop(onUpdate)
    AutoFarm.active = false
    if AutoFarm.thread then pcall(task.cancel, AutoFarm.thread); AutoFarm.thread = nil end
    AutoFarm.status = "Auto Farm desativado"
    if onUpdate then onUpdate() end
end

--=====================================================================
-- WALKSPEED / NOCLIP
--=====================================================================
local WalkSpeed = { active=false, value=Config.WalkSpeed.default, originalSpeed=nil }

local function applyWalkSpeed()
    local char = LocalPlayer.Character; if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid"); if not hum then return end
    if WalkSpeed.originalSpeed == nil then WalkSpeed.originalSpeed = hum.WalkSpeed end
    hum.WalkSpeed = WalkSpeed.value
end

local function restoreWalkSpeed()
    local char = LocalPlayer.Character; if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid"); if not hum then return end
    if WalkSpeed.originalSpeed ~= nil then hum.WalkSpeed = WalkSpeed.originalSpeed end
end

function WalkSpeed.setActive(s) WalkSpeed.active = s; if s then applyWalkSpeed() else restoreWalkSpeed() end end
function WalkSpeed.setValue(v) WalkSpeed.value = v; if WalkSpeed.active then applyWalkSpeed() end end

table.insert(Connections, LocalPlayer.CharacterAdded:Connect(function()
    task.wait(0.5); if WalkSpeed.active then applyWalkSpeed() end
end))

local Noclip = { active=false, original={}, conn=nil }

local function applyNoclipFrame()
    if not Noclip.active then return end
    local char = LocalPlayer.Character; if not char then return end
    for _, p in ipairs(char:GetDescendants()) do
        if p:IsA("BasePart") then
            if Noclip.original[p] == nil then Noclip.original[p] = p.CanCollide end
            if p.CanCollide then p.CanCollide = false end
        end
    end
end

local function restoreNoclipAll()
    for p, v in pairs(Noclip.original) do
        if p and p.Parent then pcall(function() p.CanCollide = v end) end
    end
    Noclip.original = {}
end

function Noclip.setActive(s)
    Noclip.active = s
    if s then
        if not Noclip.conn then Noclip.conn = RunService.Stepped:Connect(applyNoclipFrame) end
        applyNoclipFrame()
    else
        if Noclip.conn then Noclip.conn:Disconnect(); Noclip.conn = nil end
        restoreNoclipAll()
    end
end

table.insert(Connections, LocalPlayer.CharacterAdded:Connect(function() Noclip.original = {} end))

--=====================================================================
-- UI
--=====================================================================
UI = { connections = {}, activeTab = "AutoEvento", tabs = {}, pages = {} }

local screenGui = Util.create("ScreenGui", {
    Name = "DarkHubUI", ResetOnSpawn = false,
    ZIndexBehavior = Enum.ZIndexBehavior.Sibling, Parent = PlayerGui,
})

local uiScale = Util.create("UIScale", { Scale = 1, Parent = screenGui })
local function updateScale()
    local cam = Workspace.CurrentCamera; if not cam then return end
    uiScale.Scale = clamp(cam.ViewportSize.X / 620, 0.75, 1)
end
updateScale()
if Workspace.CurrentCamera then
    table.insert(UI.connections, Workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(updateScale))
end

mainFrame = Util.create("Frame", {
    Name = "MainFrame",
    Size = UDim2.new(0, 520, 0, 340),
    Position = UDim2.new(0.5, 0, 0.5, 0),
    AnchorPoint = Vector2.new(0.5, 0.5),
    BorderSizePixel = 0, ClipsDescendants = true, Parent = screenGui,
})
reg(mainFrame, { BackgroundColor3 = "windowBg" })
Util.create("UICorner", { CornerRadius = UDim.new(0, 12), Parent = mainFrame })
reg(Util.create("UIStroke", { Thickness = 1, Transparency = 0.3, Parent = mainFrame }),
    { Color = "border" })

-- Imagem de fundo (fora do sistema de opacidade)
local bgImage = Util.create("ImageLabel", {
    Name = "CustomBackground",
    Size = UDim2.new(1, 0, 1, 0),
    BackgroundTransparency = 1,
    Image = "", ImageTransparency = 0,
    ScaleType = Enum.ScaleType.Crop, ZIndex = 0, Parent = mainFrame,
})
Util.create("UICorner", { CornerRadius = UDim.new(0, 12), Parent = bgImage })
bgImage:SetAttribute("SkipOpacity", true)

-- Topbar
local topBar = Util.create("Frame", {
    Size = UDim2.new(1, 0, 0, 40), BorderSizePixel = 0,
    ZIndex = 2, Parent = mainFrame,
})
reg(topBar, { BackgroundColor3 = "topbar" })
Util.create("UICorner", { CornerRadius = UDim.new(0, 12), Parent = topBar })
local topBarBottom = Util.create("Frame", {
    Size = UDim2.new(1, 0, 0, 12), Position = UDim2.new(0, 0, 1, -12),
    BorderSizePixel = 0, ZIndex = 2, Parent = topBar,
})
reg(topBarBottom, { BackgroundColor3 = "topbar" })

reg(Util.create("TextLabel", {
    Size = UDim2.new(0, 240, 1, 0), Position = UDim2.new(0, 14, 0, 0),
    BackgroundTransparency = 1, Font = Enum.Font.GothamBold,
    Text = Config.Title, TextSize = 14,
    TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 3, Parent = topBar,
}), { TextColor3 = "text" })

local searchBox = Util.create("Frame", {
    Size = UDim2.new(0, 120, 0, 22),
    Position = UDim2.new(1, -90, 0.5, 0),
    AnchorPoint = Vector2.new(1, 0.5),
    BackgroundTransparency = 0.85, BorderSizePixel = 0,
    ZIndex = 3, Parent = topBar,
})
reg(searchBox, { BackgroundColor3 = "cardBg" })
Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = searchBox })
reg(Util.create("TextLabel", {
    Size = UDim2.new(1, -20, 1, 0), Position = UDim2.new(0, 20, 0, 0),
    BackgroundTransparency = 1, Font = Enum.Font.Gotham, Text = "Search",
    TextSize = 11, TextXAlignment = Enum.TextXAlignment.Left,
    ZIndex = 4, Parent = searchBox,
}), { TextColor3 = "textDim" })

local function makeTopBtn(text, xPos)
    local btn = Util.create("TextButton", {
        Size = UDim2.new(0, 24, 0, 24),
        Position = UDim2.new(1, xPos, 0.5, 0),
        AnchorPoint = Vector2.new(1, 0.5),
        BackgroundTransparency = 1, BorderSizePixel = 0,
        Text = text, Font = Enum.Font.GothamBold, TextSize = 16,
        AutoButtonColor = false, Parent = topBar,
    })
    reg(btn, { TextColor3 = "textMuted" })
    btn.MouseEnter:Connect(function() btn.TextColor3 = Colors.text end)
    btn.MouseLeave:Connect(function() btn.TextColor3 = Colors.textMuted end)
    return btn
end

local closeBtn = makeTopBtn("X", -10)
local minBtn   = makeTopBtn("-", -38)

-- Arrastar
do
    local dragging, dragStart, startPos
    table.insert(UI.connections, topBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; dragStart = input.Position; startPos = mainFrame.Position
        end
    end))
    table.insert(UI.connections, UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
                     or input.UserInputType == Enum.UserInputType.Touch) then
            local d = input.Position - dragStart
            mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X,
                                          startPos.Y.Scale, startPos.Y.Offset + d.Y)
        end
    end))
    table.insert(UI.connections, UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end))
end

-- Sidebar
local sidebar = Util.create("Frame", {
    Size = UDim2.new(0, 140, 1, -40), Position = UDim2.new(0, 0, 0, 40),
    BorderSizePixel = 0, ZIndex = 2, Parent = mainFrame,
})
reg(sidebar, { BackgroundColor3 = "sidebar" })

local sidebarList = Util.create("Frame", {
    Size = UDim2.new(1, -10, 1, -10), Position = UDim2.new(0, 5, 0, 5),
    BackgroundTransparency = 1, ZIndex = 3, Parent = sidebar,
}, {
    Util.create("UIListLayout", {
        Padding = UDim.new(0, 2), SortOrder = Enum.SortOrder.LayoutOrder,
    }),
})

local contentArea = Util.create("Frame", {
    Size = UDim2.new(1, -140, 1, -40), Position = UDim2.new(0, 140, 0, 40),
    BackgroundTransparency = 1, ZIndex = 3, Parent = mainFrame,
})

local function makeTab(id, icon, label, order)
    local btn = Util.create("TextButton", {
        Size = UDim2.new(1, 0, 0, 34),
        BackgroundTransparency = 1, BorderSizePixel = 0,
        Text = "", AutoButtonColor = false, LayoutOrder = order, Parent = sidebarList,
    })
    Util.create("UICorner", { CornerRadius = UDim.new(0, 6), Parent = btn })

    local iconLabel = reg(Util.create("TextLabel", {
        Size = UDim2.new(0, 22, 1, 0), Position = UDim2.new(0, 10, 0, 0),
        BackgroundTransparency = 1, Font = Enum.Font.GothamBold, Text = icon,
        TextSize = 14, Parent = btn,
    }), { TextColor3 = "textMuted" })

    local labelLabel = reg(Util.create("TextLabel", {
        Size = UDim2.new(1, -38, 1, 0), Position = UDim2.new(0, 34, 0, 0),
        BackgroundTransparency = 1, Font = Enum.Font.GothamMedium, Text = label,
        TextSize = 12, TextXAlignment = Enum.TextXAlignment.Left, Parent = btn,
    }), { TextColor3 = "textMuted" })

    local indicator = Util.create("Frame", {
        Size = UDim2.new(0, 3, 0.55, 0), Position = UDim2.new(0, 0, 0.5, 0),
        AnchorPoint = Vector2.new(0, 0.5), BorderSizePixel = 0,
        Visible = false, Parent = btn,
    })
    reg(indicator, { BackgroundColor3 = "accent" })
    Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = indicator })

    btn.MouseEnter:Connect(function()
        if UI.activeTab ~= id then btn.BackgroundTransparency = 0.9 end
    end)
    btn.MouseLeave:Connect(function()
        if UI.activeTab ~= id then btn.BackgroundTransparency = 1 end
    end)

    btn.MouseButton1Click:Connect(function()
        UI.activeTab = id
        for tabId, tab in pairs(UI.tabs) do
            local active = (tabId == id)
            tab.indicator.Visible = active
            tab.btn.BackgroundTransparency = active and 0.85 or 1
            tab.iconLabel.TextColor3 = active and Colors.text or Colors.textMuted
            tab.labelLabel.TextColor3 = active and Colors.text or Colors.textMuted
        end
        for tabId, page in pairs(UI.pages) do page.Visible = (tabId == id) end
    end)

    UI.tabs[id] = { btn=btn, indicator=indicator,
                    iconLabel=iconLabel, labelLabel=labelLabel }
    return btn
end

local function makePage(id)
    local page = Util.create("ScrollingFrame", {
        Size = UDim2.new(1, -16, 1, -16), Position = UDim2.new(0, 8, 0, 8),
        BackgroundTransparency = 1, BorderSizePixel = 0,
        CanvasSize = UDim2.new(0, 0, 0, 0),
        AutomaticCanvasSize = Enum.AutomaticSize.Y,
        ScrollBarThickness = 4, Visible = false, Parent = contentArea,
    }, {
        Util.create("UIListLayout", {
            Padding = UDim.new(0, 8), SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })
    UI.pages[id] = page
    return page
end

local function makeSection(parent, title, order, defaultOpen)
    local section = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 26), BackgroundTransparency = 1,
        AutomaticSize = Enum.AutomaticSize.Y, LayoutOrder = order, Parent = parent,
    }, {
        Util.create("UIListLayout", {
            Padding = UDim.new(0, 4), SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })

    local header = Util.create("TextButton", {
        Size = UDim2.new(1, 0, 0, 24), BackgroundTransparency = 1,
        Text = "", AutoButtonColor = false, LayoutOrder = 1, Parent = section,
    })

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, -24, 1, 0), BackgroundTransparency = 1,
        Font = Enum.Font.GothamBold, Text = title, TextSize = 13,
        TextXAlignment = Enum.TextXAlignment.Left, Parent = header,
    }), { TextColor3 = "text" })

    local chevron = reg(Util.create("TextLabel", {
        Size = UDim2.new(0, 16, 1, 0), Position = UDim2.new(1, -16, 0, 0),
        BackgroundTransparency = 1, Font = Enum.Font.GothamBold, Text = "v",
        TextSize = 12, Parent = header,
    }), { TextColor3 = "textMuted" })

    local body = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 0), BackgroundTransparency = 1,
        AutomaticSize = Enum.AutomaticSize.Y, Visible = defaultOpen ~= false,
        LayoutOrder = 2, Parent = section,
    }, {
        Util.create("UIListLayout", {
            Padding = UDim.new(0, 6), SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })

    header.MouseButton1Click:Connect(function()
        body.Visible = not body.Visible
        chevron.Text = body.Visible and "v" or ">"
    end)

    return body
end

local function makeToggle(parent, initialState, onChanged)
    local container = Util.create("Frame", {
        Size = UDim2.new(0, 36, 0, 20), BackgroundTransparency = 1, Parent = parent,
    })
    local track = Util.create("Frame", {
        Size = UDim2.new(1, 0, 1, 0), BorderSizePixel = 0, Parent = container,
    })
    reg(track, { BackgroundColor3 = initialState and "accent" or "toggleOff" })
    Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = track })

    local knob = Util.create("Frame", {
        Size = UDim2.new(0, 16, 0, 16),
        Position = UDim2.new(0, initialState and 18 or 2, 0.5, 0),
        AnchorPoint = Vector2.new(0, 0.5), BorderSizePixel = 0,
        BackgroundColor3 = Color3.fromRGB(255, 255, 255), Parent = track,
    })
    Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = knob })

    local state = initialState
    local clickArea = Util.create("TextButton", {
        Size = UDim2.new(1, 0, 1, 0), BackgroundTransparency = 1,
        Text = "", Parent = container,
    })

    clickArea.MouseButton1Click:Connect(function()
        state = not state
        TweenService:Create(track, TweenInfo.new(0.15), {
            BackgroundColor3 = state and Colors.accent or Colors.toggleOff,
        }):Play()
        TweenService:Create(knob, TweenInfo.new(0.15), {
            Position = UDim2.new(0, state and 18 or 2, 0.5, 0),
        }):Play()
        onChanged(state)
    end)
    return container
end

local function makeOptionRow(parent, title, description, order, createRight)
    local row = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 54), BackgroundTransparency = 1,
        LayoutOrder = order, Parent = parent,
    })

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, -70, 0, 18), BackgroundTransparency = 1,
        Font = Enum.Font.GothamBold, Text = title, TextSize = 12,
        TextXAlignment = Enum.TextXAlignment.Left, Parent = row,
    }), { TextColor3 = "text" })

    if description and description ~= "" then
        reg(Util.create("TextLabel", {
            Size = UDim2.new(1, -70, 0, 32), Position = UDim2.new(0, 0, 0, 18),
            BackgroundTransparency = 1, Font = Enum.Font.Gotham, Text = description,
            TextSize = 10, TextXAlignment = Enum.TextXAlignment.Left,
            TextYAlignment = Enum.TextYAlignment.Top, TextWrapped = true, Parent = row,
        }), { TextColor3 = "textMuted" })
    end

    if createRight then
        local right = createRight(row)
        if right then
            right.Position = UDim2.new(1, -6, 0, 4)
            right.AnchorPoint = Vector2.new(1, 0)
        end
    end
    return row
end

local function makeSlider(parent, min, max, default, order, onChanged)
    local container = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 26), BackgroundTransparency = 1,
        LayoutOrder = order, Parent = parent,
    })

    local track = Util.create("Frame", {
        Size = UDim2.new(1, -50, 0, 4),
        Position = UDim2.new(0, 0, 0.5, 0),
        AnchorPoint = Vector2.new(0, 0.5), BorderSizePixel = 0, Parent = container,
    })
    reg(track, { BackgroundColor3 = "cardBg" })
    Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = track })

    local fill = Util.create("Frame", {
        Size = UDim2.new(0, 0, 1, 0), BorderSizePixel = 0, Parent = track,
    })
    reg(fill, { BackgroundColor3 = "accent" })
    Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = fill })

    local knob = Util.create("Frame", {
        Size = UDim2.new(0, 14, 0, 14),
        AnchorPoint = Vector2.new(0.5, 0.5), Position = UDim2.new(0, 0, 0.5, 0),
        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
        BorderSizePixel = 0, Parent = track,
    })
    Util.create("UICorner", { CornerRadius = UDim.new(1, 0), Parent = knob })

    local valueLabel = reg(Util.create("TextLabel", {
        Size = UDim2.new(0, 44, 1, 0), Position = UDim2.new(1, -44, 0, 0),
        BackgroundTransparency = 1, Font = Enum.Font.GothamBold,
        Text = tostring(default), TextSize = 11, Parent = container,
    }), { TextColor3 = "text" })

    local value = default
    local dragging = false

    local function syncVisual()
        local frac = (value - min) / math.max(max - min, 1)
        fill.Size = UDim2.new(frac, 0, 1, 0)
        knob.Position = UDim2.new(frac, 0, 0.5, 0)
        valueLabel.Text = tostring(value)
    end

    local function updateFromX(absX)
        local rel = absX - track.AbsolutePosition.X
        local frac = clamp(rel / math.max(track.AbsoluteSize.X, 1), 0, 1)
        value = math.floor(min + (max - min) * frac + 0.5)
        syncVisual()
        onChanged(value)
    end

    local clickArea = Util.create("TextButton", {
        Size = UDim2.new(1, 0, 0, 24),
        Position = UDim2.new(0, 0, 0.5, 0),
        AnchorPoint = Vector2.new(0, 0.5),
        BackgroundTransparency = 1, Text = "", Parent = container,
    })
    clickArea.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true; updateFromX(input.Position.X)
        end
    end)
    table.insert(UI.connections, UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
                     or input.UserInputType == Enum.UserInputType.Touch) then
            updateFromX(input.Position.X)
        end
    end))
    table.insert(UI.connections, UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end))

    syncVisual()
    return { setValue = function(v) value = v; syncVisual() end, getValue = function() return value end }
end

local function makeOutlineBtn(parent, text, order, widthScale)
    local btn = Util.create("TextButton", {
        Size = UDim2.new(widthScale or 0.5, -3, 0, 28),
        BackgroundTransparency = 0.25, BorderSizePixel = 0,
        Text = text, Font = Enum.Font.GothamBold,
        TextSize = 11, AutoButtonColor = false,
        LayoutOrder = order, Parent = parent,
    })
    reg(btn, { BackgroundColor3 = "cardBg", TextColor3 = "text" })
    Util.create("UICorner", { CornerRadius = UDim.new(0, 6), Parent = btn })
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), { BackgroundTransparency = 0 }):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), { BackgroundTransparency = 0.25 }):Play()
    end)
    return btn
end

local function makeInfoLabel(parent, text, order, colorKey)
    return reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 14), BackgroundTransparency = 1,
        Font = Enum.Font.GothamMedium, Text = text, TextSize = 11,
        TextXAlignment = Enum.TextXAlignment.Left,
        LayoutOrder = order, Parent = parent,
    }), { TextColor3 = colorKey or "textMuted" })
end

--=====================================================================
-- PÁGINAS
--=====================================================================

-- AUTO EVENTO
do
    makeTab("AutoEvento", "✨", "Auto Evento", 1)
    local page = makePage("AutoEvento")
    local secStatus = makeSection(page, "Status", 1, true)
    local statusLbl  = makeInfoLabel(secStatus, "Auto Evento desativado", 1, "text")
    local currentLbl = makeInfoLabel(secStatus, "Destino atual: -", 2)
    local prioLbl    = makeInfoLabel(secStatus, "Prioritario: nao", 3)
    local foundLbl   = makeInfoLabel(secStatus, "Itens encontrados: 0", 4)
    local procLbl    = makeInfoLabel(secStatus, "Processados: 0", 5)

    local secCtrl = makeSection(page, "Controles", 2, true)
    local btnRow = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 28), BackgroundTransparency = 1,
        LayoutOrder = 1, Parent = secCtrl,
    }, {
        Util.create("UIListLayout", {
            FillDirection = Enum.FillDirection.Horizontal,
            Padding = UDim.new(0, 6), SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })

    local function update()
        statusLbl.Text  = "Status: " .. AutoEvento.status
        currentLbl.Text = "Destino atual: " .. (AutoEvento.current or "-")
        prioLbl.Text    = "Prioritario: " .. (AutoEvento.isPriority and "SIM" or "nao")
        prioLbl.TextColor3 = AutoEvento.isPriority and Colors.accent2 or Colors.textMuted
        foundLbl.Text   = "Itens encontrados: " .. countEventFound()
        procLbl.Text    = "Processados: " .. AutoEvento.processed
    end

    local startBtn = makeOutlineBtn(btnRow, "Iniciar", 1)
    local stopBtn  = makeOutlineBtn(btnRow, "Parar", 2)
    startBtn.MouseButton1Click:Connect(function() AutoEvento.start(update) end)
    stopBtn.MouseButton1Click:Connect(function()  AutoEvento.stop(update)  end)

    task.spawn(function()
        while screenGui.Parent do update(); task.wait(0.5) end
    end)
end

-- AUTO FARM
do
    makeTab("AutoFarm", "🍯", "Auto Farm", 2)
    local page = makePage("AutoFarm")
    local secStatus = makeSection(page, "Status", 1, true)
    local statusLbl  = makeInfoLabel(secStatus, "Auto Farm desativado", 1, "text")
    local currentLbl = makeInfoLabel(secStatus, "Negocio atual: -", 2)
    local foundLbl   = makeInfoLabel(secStatus, "Negocios na base: 0", 3)
    local procLbl    = makeInfoLabel(secStatus, "Processados: 0", 4)
    local baseLbl    = makeInfoLabel(secStatus, "Base: buscando...", 5, "accent2")

    local secCtrl = makeSection(page, "Controles", 2, true)
    local btnRow = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 28), BackgroundTransparency = 1,
        LayoutOrder = 1, Parent = secCtrl,
    }, {
        Util.create("UIListLayout", {
            FillDirection = Enum.FillDirection.Horizontal,
            Padding = UDim.new(0, 5), SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })

    local function update()
        statusLbl.Text  = "Status: " .. AutoFarm.status
        currentLbl.Text = "Negocio atual: " .. (AutoFarm.current or "-")
        foundLbl.Text   = "Negocios na base: " .. countBusinessFound()
        procLbl.Text    = "Processados: " .. AutoFarm.processed
        if PlayerBase and PlayerBase.Parent then
            baseLbl.Text = "Base: " .. PlayerBase.Name .. " OK"
            baseLbl.TextColor3 = Colors.success
        else
            baseLbl.Text = "Base: NAO ENCONTRADA"
            baseLbl.TextColor3 = Colors.accent2
        end
    end

    local startBtn = makeOutlineBtn(btnRow, "Iniciar", 1, 0.40)
    local stopBtn  = makeOutlineBtn(btnRow, "Parar", 2, 0.30)
    local spawnBtn = makeOutlineBtn(btnRow, "Spawn", 3, 0.30)

    startBtn.MouseButton1Click:Connect(function() AutoFarm.start(update) end)
    stopBtn.MouseButton1Click:Connect(function()  AutoFarm.stop(update)  end)
    spawnBtn.MouseButton1Click:Connect(function()
        local ok = AutoFarm.goToSpawn()
        if not ok then statusLbl.Text = "Status: Spawn ainda nao capturado" end
    end)

    task.spawn(function()
        while screenGui.Parent do update(); task.wait(0.5) end
    end)
end

-- WALKSPEED
do
    makeTab("WalkSpeed", "⚡", "WalkSpeed", 3)
    local page = makePage("WalkSpeed")
    local sec = makeSection(page, "Configuracoes", 1, true)

    makeOptionRow(sec, "WalkSpeed", "Ativar velocidade personalizada do personagem.", 1,
        function(row)
            return makeToggle(row, false, function(s) WalkSpeed.setActive(s) end)
        end)

    local sliderContainer = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 44), BackgroundTransparency = 1,
        LayoutOrder = 2, Parent = sec,
    })
    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 16), BackgroundTransparency = 1,
        Font = Enum.Font.GothamBold, Text = "Velocidade (studs/s)",
        TextSize = 12, TextXAlignment = Enum.TextXAlignment.Left, Parent = sliderContainer,
    }), { TextColor3 = "text" })
    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 14), Position = UDim2.new(0, 0, 0, 15),
        BackgroundTransparency = 1, Font = Enum.Font.Gotham,
        Text = "Padrao do Roblox: 16.",
        TextSize = 10, TextXAlignment = Enum.TextXAlignment.Left, Parent = sliderContainer,
    }), { TextColor3 = "textMuted" })
    makeSlider(sliderContainer, Config.WalkSpeed.min, Config.WalkSpeed.max, Config.WalkSpeed.default, 3,
        function(v) WalkSpeed.setValue(v) end)
    sliderContainer:FindFirstChildWhichIsA("Frame").Position = UDim2.new(0, 0, 0, 22)
end

-- NOCLIP
do
    makeTab("Noclip", "👻", "Noclip", 4)
    local page = makePage("Noclip")
    local sec = makeSection(page, "Configuracoes", 1, true)
    makeOptionRow(sec, "Noclip", "Atravessa paredes e obstaculos.", 1,
        function(row)
            return makeToggle(row, false, function(s) Noclip.setActive(s) end)
        end)
end

-- SETTINGS
do
    makeTab("Settings", "⚙", "Settings", 5)
    local page = makePage("Settings")

    local secAppearance = makeSection(page, "Aparencia", 1, true)

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 16), BackgroundTransparency = 1,
        Font = Enum.Font.GothamBold, Text = "Tema",
        TextSize = 12, TextXAlignment = Enum.TextXAlignment.Left,
        LayoutOrder = 1, Parent = secAppearance,
    }), { TextColor3 = "text" })

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 14), BackgroundTransparency = 1,
        Font = Enum.Font.Gotham, Text = "Cor e icones do menu.",
        TextSize = 10, TextXAlignment = Enum.TextXAlignment.Left,
        LayoutOrder = 2, Parent = secAppearance,
    }), { TextColor3 = "textMuted" })

    local themeGrid = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 92), BackgroundTransparency = 1,
        LayoutOrder = 3, Parent = secAppearance,
    }, {
        Util.create("UIGridLayout", {
            CellSize = UDim2.new(0, 90, 0, 28),
            CellPadding = UDim2.new(0, 4, 0, 4),
            SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })

    local themeButtons = {}
    local themeOrder = { "Dark", "Navy", "BabyBlue", "Purple", "Green", "Neon", "Sunset", "Matrix", "Sakura", "Ocean" }
    local themeLabels = {
        Dark="Dark", Navy="Navy", BabyBlue="Baby Blue", Purple="Roxo", Green="Verde",
        Neon="Neon", Sunset="Sunset", Matrix="Matrix", Sakura="Sakura", Ocean="Ocean",
    }

    local function refreshThemeButtons()
        for name, btn in pairs(themeButtons) do
            local active = (name == CurrentTheme)
            btn.BackgroundColor3 = active and Colors.accent or Colors.cardBg
            btn.TextColor3 = active and Color3.fromRGB(255,255,255) or Colors.textMuted
            btn.BackgroundTransparency = active and 0 or 0.2
        end
    end

    for i, name in ipairs(themeOrder) do
        local btn = Util.create("TextButton", {
            Size = UDim2.new(0, 90, 0, 28),
            BackgroundTransparency = 0.2, BorderSizePixel = 0,
            Text = themeLabels[name] or name, Font = Enum.Font.GothamBold,
            TextSize = 11, AutoButtonColor = false,
            LayoutOrder = i, Parent = themeGrid,
        })
        Util.create("UICorner", { CornerRadius = UDim.new(0, 5), Parent = btn })
        btn.MouseButton1Click:Connect(function()
            applyTheme(name)
            refreshThemeButtons()
        end)
        themeButtons[name] = btn
    end
    refreshThemeButtons()

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 16), BackgroundTransparency = 1,
        Font = Enum.Font.GothamBold, Text = "Opacidade do menu",
        TextSize = 12, TextXAlignment = Enum.TextXAlignment.Left,
        LayoutOrder = 4, Parent = secAppearance,
    }), { TextColor3 = "text" })

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 14), BackgroundTransparency = 1,
        Font = Enum.Font.Gotham, Text = "100 = solido / 20 = ve o jogo atras.",
        TextSize = 10, TextXAlignment = Enum.TextXAlignment.Left,
        LayoutOrder = 5, Parent = secAppearance,
    }), { TextColor3 = "textMuted" })

    local opacityContainer = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 26), BackgroundTransparency = 1,
        LayoutOrder = 6, Parent = secAppearance,
    })
    makeSlider(opacityContainer, 20, 100, 100, 1, function(v)
        applyOpacity(v)
    end)

    -- Fundo personalizado
    local secBg = makeSection(page, "Fundo personalizado", 2, true)

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 16), BackgroundTransparency = 1,
        Font = Enum.Font.GothamBold, Text = "ID ou URL da imagem",
        TextSize = 12, TextXAlignment = Enum.TextXAlignment.Left,
        LayoutOrder = 1, Parent = secBg,
    }), { TextColor3 = "text" })

    reg(Util.create("TextLabel", {
        Size = UDim2.new(1, 0, 0, 26), BackgroundTransparency = 1,
        Font = Enum.Font.Gotham,
        Text = "Ex: 1234567 | rbxassetid://1234567 | URL com id=",
        TextSize = 10, TextXAlignment = Enum.TextXAlignment.Left,
        TextWrapped = true, LayoutOrder = 2, Parent = secBg,
    }), { TextColor3 = "textMuted" })

    local inputBox = Util.create("TextBox", {
        Size = UDim2.new(1, 0, 0, 30),
        BackgroundTransparency = 0.15, BorderSizePixel = 0,
        Font = Enum.Font.Gotham, Text = "",
        PlaceholderText = "Cole o ID ou URL aqui...",
        PlaceholderColor3 = Color3.fromRGB(130, 130, 150),
        TextSize = 12, TextXAlignment = Enum.TextXAlignment.Left,
        ClearTextOnFocus = false, LayoutOrder = 3, Parent = secBg,
    })
    reg(inputBox, { BackgroundColor3 = "cardBg", TextColor3 = "text" })
    Util.create("UICorner", { CornerRadius = UDim.new(0, 6), Parent = inputBox })
    Util.create("UIPadding", {
        PaddingLeft = UDim.new(0, 10), PaddingRight = UDim.new(0, 10),
        Parent = inputBox,
    })

    local bgBtnRow = Util.create("Frame", {
        Size = UDim2.new(1, 0, 0, 28), BackgroundTransparency = 1,
        LayoutOrder = 4, Parent = secBg,
    }, {
        Util.create("UIListLayout", {
            FillDirection = Enum.FillDirection.Horizontal,
            Padding = UDim.new(0, 6), SortOrder = Enum.SortOrder.LayoutOrder,
        }),
    })

    local applyBtn = makeOutlineBtn(bgBtnRow, "Aplicar", 1)
    local clearBtn = makeOutlineBtn(bgBtnRow, "Limpar", 2)

    local bgStatus = makeInfoLabel(secBg, "Sem fundo personalizado.", 5, "textMuted")

    local function applyBackground()
        local raw = inputBox.Text
        local assetId, err = Util.parseAssetId(raw)
        if not assetId then
            bgStatus.Text = "Status: " .. (err or "erro")
            bgStatus.TextColor3 = Colors.danger
            return
        end
        local ok = pcall(function() bgImage.Image = assetId end)
        if not ok then
            bgStatus.Text = "Status: formato de imagem invalido"
            bgStatus.TextColor3 = Colors.danger
            return
        end
        bgStatus.Text = "Aplicado: " .. assetId:sub(1, 40)
        bgStatus.TextColor3 = Colors.success
    end

    applyBtn.MouseButton1Click:Connect(applyBackground)
    inputBox.FocusLost:Connect(function(enterPressed)
        if enterPressed then applyBackground() end
    end)
    clearBtn.MouseButton1Click:Connect(function()
        bgImage.Image = ""
        inputBox.Text = ""
        bgStatus.Text = "Sem fundo personalizado."
        bgStatus.TextColor3 = Colors.textMuted
    end)

    local secInfo = makeSection(page, "Informacoes", 3, true)
    makeInfoLabel(secInfo, "Base: " .. (PlayerBase and PlayerBase.Name or "-"), 1, "text")
    makeInfoLabel(secInfo, "Spawn: " .. (SpawnCache.position and "capturado" or "pendente"), 2)
    makeInfoLabel(secInfo, "Versao: v18.1", 3, "textDim")
end

-- Ativa primeira aba
do
    UI.activeTab = "AutoEvento"
    UI.pages.AutoEvento.Visible = true
    UI.tabs.AutoEvento.indicator.Visible = true
    UI.tabs.AutoEvento.btn.BackgroundTransparency = 0.85
    UI.tabs.AutoEvento.iconLabel.TextColor3 = Colors.text
    UI.tabs.AutoEvento.labelLabel.TextColor3 = Colors.text
end

-- ⭐ FIX v18.1: mainFrame já está atribuído, snapshot roda corretamente
buildOpacitySnapshot()

--=====================================================================
-- BOTÃO DE RESTAURAR
--=====================================================================
local restoreBtn = Util.create("TextButton", {
    Size = UDim2.new(0, 110, 0, 30),
    Position = UDim2.new(0, 20, 0, 20),
    BackgroundTransparency = 0.1, BorderSizePixel = 0,
    Text = "Dark Hub", Font = Enum.Font.GothamBold,
    TextSize = 12, AutoButtonColor = false,
    Visible = false, Parent = screenGui,
})
reg(restoreBtn, { BackgroundColor3 = "cardBg", TextColor3 = "text" })
Util.create("UICorner", { CornerRadius = UDim.new(0, 8), Parent = restoreBtn })

restoreBtn.MouseButton1Click:Connect(function()
    restoreBtn.Visible = false; mainFrame.Visible = true
end)
minBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false; restoreBtn.Visible = true
end)

--=====================================================================
-- DIÁLOGO DE CONFIRMAÇÃO
--=====================================================================
local confirmFrame = Util.create("Frame", {
    Size = UDim2.new(0, 260, 0, 120),
    Position = UDim2.new(0.5, 0, 0.5, 0),
    AnchorPoint = Vector2.new(0.5, 0.5),
    BorderSizePixel = 0, Visible = false, ZIndex = 80, Parent = screenGui,
})
reg(confirmFrame, { BackgroundColor3 = "windowBg" })
Util.create("UICorner", { CornerRadius = UDim.new(0, 10), Parent = confirmFrame })
reg(Util.create("UIStroke", { Thickness = 1, Parent = confirmFrame }), { Color = "border" })

reg(Util.create("TextLabel", {
    Size = UDim2.new(1, -20, 0, 22), Position = UDim2.new(0, 10, 0, 14),
    BackgroundTransparency = 1, Font = Enum.Font.GothamBold,
    Text = "Encerrar Dark Hub?", TextSize = 14,
    TextXAlignment = Enum.TextXAlignment.Center, ZIndex = 81, Parent = confirmFrame,
}), { TextColor3 = "text" })

reg(Util.create("TextLabel", {
    Size = UDim2.new(1, -20, 0, 30), Position = UDim2.new(0, 10, 0, 40),
    BackgroundTransparency = 1, Font = Enum.Font.Gotham,
    Text = "Todas as tarefas serao encerradas.",
    TextSize = 10, TextWrapped = true,
    TextXAlignment = Enum.TextXAlignment.Center, ZIndex = 81, Parent = confirmFrame,
}), { TextColor3 = "textMuted" })

local confirmBtnRow = Util.create("Frame", {
    Size = UDim2.new(1, -20, 0, 28), Position = UDim2.new(0, 10, 1, -38),
    BackgroundTransparency = 1, ZIndex = 81, Parent = confirmFrame,
}, {
    Util.create("UIListLayout", {
        FillDirection = Enum.FillDirection.Horizontal,
        Padding = UDim.new(0, 6), SortOrder = Enum.SortOrder.LayoutOrder,
    }),
})

local cancelBtn  = makeOutlineBtn(confirmBtnRow, "Cancelar", 1)
local confirmBtn = makeOutlineBtn(confirmBtnRow, "Confirmar", 2)
confirmBtn.TextColor3 = Colors.danger

--=====================================================================
-- CLEANUP
--=====================================================================
local function Cleanup()
    pcall(AutoEvento.stop)
    pcall(AutoFarm.stop)
    pcall(function() WalkSpeed.setActive(false) end)
    pcall(function() Noclip.setActive(false) end)
    for _, c in ipairs(Connections) do pcall(function() c:Disconnect() end) end
    for _, c in ipairs(UI.connections) do pcall(function() c:Disconnect() end) end
    pcall(function() screenGui:Destroy() end)
    _G.DarkHub = nil
end

_G.DarkHub = { Cleanup = Cleanup }

closeBtn.MouseButton1Click:Connect(function() confirmFrame.Visible = true end)
cancelBtn.MouseButton1Click:Connect(function() confirmFrame.Visible = false end)
confirmBtn.MouseButton1Click:Connect(function() Cleanup() end)

print("[Dark Hub] v18.1 carregado.")
