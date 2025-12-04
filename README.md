--========================================================
-- RECURSOS COMPARTILHADOS
--========================================================
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

--========================================================
-- VARIÁVEIS DE ESTADO
--========================================================
local AutoTrickEnabled = false
local VoidRushEnabled = false

--========================================================
-- CRIAÇÃO DO MENU MÓVEL
--========================================================
local MainGui = Instance.new("ScreenGui")
MainGui.Name = "UnifiedMenu"
MainGui.ResetOnSpawn = false
MainGui.Parent = PlayerGui

-- BOTÃO FLUTUANTE
local OpenButton = Instance.new("TextButton")
OpenButton.Size = UDim2.new(0, 55, 0, 55)
OpenButton.Position = UDim2.new(0, 20, 0.3, 0)
OpenButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
OpenButton.Text = "≡"
OpenButton.TextScaled = true
OpenButton.Parent = MainGui

-- FRAME DO MENU
local MenuFrame = Instance.new("Frame")
MenuFrame.Size = UDim2.new(0, 440, 0, 210)
MenuFrame.Position = UDim2.new(0, 90, 0.25, 0)
MenuFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MenuFrame.Visible = false
MenuFrame.Parent = MainGui

--========================================================
-- ANIMAÇÃO DE ABRIR/FECHAR MENU
--========================================================
MenuFrame.BackgroundTransparency = 1
local closedPos = MenuFrame.Position - UDim2.new(0, 0, 0, 20)
local openPos = MenuFrame.Position

MenuFrame.Position = closedPos

local openTween = TweenService:Create(
    MenuFrame,
    TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out),
    {BackgroundTransparency = 0, Position = openPos}
)

local closeTween = TweenService:Create(
    MenuFrame,
    TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
    {BackgroundTransparency = 1, Position = closedPos}
)

OpenButton.MouseButton1Click:Connect(function()
    if not MenuFrame.Visible then
        MenuFrame.Visible = true
        openTween:Play()
    else
        closeTween:Play()
        closeTween.Completed:Once(function()
            MenuFrame.Visible = false
            MenuFrame.Position = closedPos
        end)
    end
end)

--========================================================
-- BOTÃO FACTORY
--========================================================
local function createButton(parent, posY, text, bg)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(1, -20, 0, 45)
    b.Position = UDim2.new(0, 10, 0, posY)
    b.BackgroundColor3 = bg
    b.Text = text
    b.TextScaled = true
    b.Parent = parent
    return b
end

--========================================================
-- COLUNAS
--========================================================
local Col1 = Instance.new("Frame")
Col1.Size = UDim2.new(0.5, -5, 1, 0)
Col1.Position = UDim2.new(0, 0, 0, 0)
Col1.BackgroundTransparency = 1
Col1.Parent = MenuFrame

local Col2 = Instance.new("Frame")
Col2.Size = UDim2.new(0.5, -5, 1, 0)
Col2.Position = UDim2.new(0.5, 5, 0, 0)
Col2.BackgroundTransparency = 1
Col2.Parent = MenuFrame

--========================================================
-- BOTÕES COLUNA 1
--========================================================
local VeeButton = createButton(Col1, 10, "Vee Auto Trick (OFF)", Color3.fromRGB(255,105,180))
local VoidButton = createButton(Col1, 60, "Void Rush Control (OFF)", Color3.fromRGB(255,255,255))
local BumperButton = createButton(Col1, 110, "Better Bumper", Color3.fromRGB(150,150,255))
local CoolGuiButton = createButton(Col1, 160, "c00l GUI", Color3.fromRGB(0,0,0))
CoolGuiButton.TextColor3 = Color3.fromRGB(255,0,0)

--========================================================
-- BOTÃO AUTO BACKSTAB (COLUNA 2)
--========================================================
local AutoBackstabButton = createButton(Col2, 10, "Auto Backstab", Color3.fromRGB(0, 100, 255))
AutoBackstabButton.TextColor3 = Color3.fromRGB(255,255,255)

AutoBackstabButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/skibidi399/Auto-backstab-two-time/refs/heads/main/bakctsab%20v2.3"))()
end)

--========================================================
-- HITBOX EXPANDER + INFO
--========================================================

-- Botão principal
local HitboxButton = createButton(Col2, 60, "Hitbox Expander", Color3.fromRGB(255,255,255))
HitboxButton.TextColor3 = Color3.fromRGB(0,0,0)

-- Botão pequeno (i)
local InfoButton = Instance.new("TextButton")
InfoButton.Size = UDim2.new(0, 25, 0, 25)
InfoButton.Position = UDim2.new(1, -30, 0, 60)
InfoButton.BackgroundColor3 = Color3.fromRGB(255,255,255)
InfoButton.Text = "i"
InfoButton.TextScaled = true
InfoButton.Parent = Col2

-- Janela de informação
local InfoFrame = Instance.new("Frame")
InfoFrame.Size = UDim2.new(0, 260, 0, 120)
InfoFrame.Position = UDim2.new(0.5, -130, 0.5, -60)
InfoFrame.BackgroundColor3 = Color3.fromRGB(255,255,255)
InfoFrame.Visible = false
InfoFrame.Parent = MainGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0,10)
UICorner.Parent = InfoFrame

local InfoLabel = Instance.new("TextLabel")
InfoLabel.Size = UDim2.new(1, -20, 1, -20)
InfoLabel.Position = UDim2.new(0, 10, 0, 10)
InfoLabel.BackgroundTransparency = 1
InfoLabel.TextWrapped = true
InfoLabel.TextScaled = true
InfoLabel.TextColor3 = Color3.fromRGB(0,0,0)
InfoLabel.Text = "Não funciona com killers novos como Noli, Guest etc...\n(ative sempre que for o killer)"
InfoLabel.Parent = InfoFrame

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(255,0,0)
CloseButton.Text = "X"
CloseButton.TextScaled = true
CloseButton.TextColor3 = Color3.fromRGB(255,255,255)
CloseButton.Parent = InfoFrame

InfoButton.MouseButton1Click:Connect(function()
    InfoFrame.Visible = true
end)

CloseButton.MouseButton1Click:Connect(function()
    InfoFrame.Visible = false
end)

HitboxButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/XQZ-official/XQZscripts/refs/heads/main/HitboxModificationOB.txt"))()
end)

--========================================================
-- TODA A SUA LÓGICA ORIGINAL FICA AQUI EMBAIXO
--========================================================
-- (NÃO ALTEREI NADA)

--========================================================
-- AUTO TRICK SISTEMA
--========================================================

local function getBehaviorFolder()
    return ReplicatedStorage:WaitForChild("Assets")
        :WaitForChild("Survivors")
        :WaitForChild("Veeronica")
        :WaitForChild("Behavior")
end

local function getSprintingButton()
    return player.PlayerGui:WaitForChild("MainUI"):WaitForChild("SprintingButton")
end

local behaviorFolder = getBehaviorFolder()
local enabled = false
local activeMonitors = {}
local descendantAddedConn = nil

local function safeConnectPropertyChanged(instance, prop, fn)
    local ok, signal = pcall(function()
        return instance:GetPropertyChangedSignal(prop)
    end)
    if ok and signal then return signal:Connect(fn) end
end

local function monitorHighlight(h)
    if not h or activeMonitors[h] then return end

    local connections = {}
    local prevState = false

    local function cleanup()
        for _, c in ipairs(connections) do
            if c and c.Connected then c:Disconnect() end
        end
        activeMonitors[h] = nil
    end

    local function adorneeIsPlayerCharacter(h)
        local a = h.Adornee
        local char = player.Character
        if not a or not char then return false end
        return a == char or a:IsDescendantOf(char)
    end

    local function onChanged()
        if not enabled then return end
        if not h or not h.Parent then cleanup() return end

        local state = adorneeIsPlayerCharacter(h)
        if prevState ~= state then
            if state then
                local ok, btn = pcall(getSprintingButton)
                if ok and btn then
                    for _, v in pairs(getconnections(btn.MouseButton1Down)) do
                        pcall(function() v:Fire() end)
                        pcall(function() if v.Function then v:Function() end end)
                    end
                end
            end
        end

        prevState = state
    end

    table.insert(connections, safeConnectPropertyChanged(h, "Adornee", onChanged))
    table.insert(connections, h.AncestryChanged:Connect(function(_, parent)
        if not parent then cleanup() else onChanged() end
    end))

    table.insert(connections, player.CharacterAdded:Connect(onChanged))
    table.insert(connections, player.CharacterRemoving:Connect(onChanged))

    activeMonitors[h] = cleanup
    task.spawn(onChanged)
end

local function startManager()
    if descendantAddedConn then return end
    for _, d in ipairs(behaviorFolder:GetDescendants()) do
        if d:IsA("Highlight") then monitorHighlight(d) end
    end

    descendantAddedConn = behaviorFolder.DescendantAdded:Connect(function(child)
        if child:IsA("Highlight") then monitorHighlight(child) end
    end)
end

local function stopManager()
    if descendantAddedConn then descendantAddedConn:Disconnect() end
    descendantAddedConn = nil

    for _, fn in pairs(activeMonitors) do
        pcall(fn)
    end

    activeMonitors = {}
end

local function setEnabled(v)
    if enabled == v then return end
    enabled = v
    if enabled then startManager() else stopManager() end
end

--========================================================
-- VOID RUSH SISTEMA
--========================================================

local voidrushcontrol = false

local function setupCharacter(character)
    local Humanoid = character:WaitForChild("Humanoid")
    local HRP = character:WaitForChild("HumanoidRootPart")
    _G.Humanoid = Humanoid
    _G.HumanoidRootPart = HRP
end

if player.Character then setupCharacter(player.Character) end
player.CharacterAdded:Connect(setupCharacter)

local ORIGINAL_DASH_SPEED = 60
local isOverrideActive = false
local connection

local function startOverride()
    if isOverrideActive then return end
    isOverrideActive = true

    connection = RunService.RenderStepped:Connect(function()
        if not _G.Humanoid or not _G.HumanoidRootPart then return end

        local hum = _G.Humanoid
        local root = _G.HumanoidRootPart

        hum.WalkSpeed = ORIGINAL_DASH_SPEED
        hum.AutoRotate = false

        local dir = root.CFrame.LookVector
        local horiz = Vector3.new(dir.X, 0, dir.Z)

        if horiz.Magnitude > 0 then
            hum:Move(horiz.Unit)
        end
    end)
end

local function stopOverride()
    if not isOverrideActive then return end
    isOverrideActive = false

    if _G.Humanoid then
        _G.Humanoid.WalkSpeed = 16
        _G.Humanoid.AutoRotate = true
        _G.Humanoid:Move(Vector3.new())
    end

    if connection then
        connection:Disconnect()
        connection = nil
    end
end

RunService.RenderStepped:Connect(function()
    if not VoidRushEnabled then return end
    local char = player.Character
    if not char then return end
    local state = char:GetAttribute("VoidRushState")

    if state == "Dashing" then
        startOverride()
    else
        stopOverride()
    end
end)

--========================================================
-- BOTÕES FUNCIONAIS
--========================================================

VeeButton.MouseButton1Click:Connect(function()
    AutoTrickEnabled = not AutoTrickEnabled
    setEnabled(AutoTrickEnabled)

    if AutoTrickEnabled then
        VeeButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        VeeButton.Text = "Vee Auto Trick (ON)"
    else
        VeeButton.BackgroundColor3 = Color3.fromRGB(255, 105, 180)
        VeeButton.Text = "Vee Auto Trick (OFF)"
    end
end)

VoidButton.MouseButton1Click:Connect(function()
    VoidRushEnabled = not VoidRushEnabled

    if VoidRushEnabled then
        VoidButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        VoidButton.Text = "Void Rush Control (ON)"
    else
        VoidButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        VoidButton.Text = "Void Rush Control (OFF)"
    end
end)

BumperButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/dumgum/dumscripts/refs/heads/main/Forsaken/Survivors/Veeronica/Better%20bumper"))()
end)

CoolGuiButton.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Altairkaaa/c00l/refs/heads/main/c00lguiv1.lua"))()
end)
