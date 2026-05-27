local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local Camera = workspace.CurrentCamera
local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- ======================================
-- SEÇÃO 1: CRIAÇÃO DA UI MODERNA
-- ======================================

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FuturisticUI"
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- Frame principal com sombra
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 350, 0, 450)
mainFrame.Position = UDim2.new(0.5, -175, 0.5, -225)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
mainFrame.BackgroundTransparency = 0.05
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Parent = screenGui

-- Sombra suave
local shadow = Instance.new("Frame")
shadow.Name = "Shadow"
shadow.Size = UDim2.new(1, 20, 1, 20)
shadow.Position = UDim2.new(0, -10, 0, -10)
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BackgroundTransparency = 0.7
shadow.BorderSizePixel = 0
shadow.Parent = mainFrame

-- Cantos arredondados
local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 12)
uiCorner.Parent = mainFrame

-- Container de cantos neon
local neonBorder = Instance.new("Frame")
neonBorder.Name = "NeonBorder"
neonBorder.Size = UDim2.new(1, 0, 1, 0)
neonBorder.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
neonBorder.BackgroundTransparency = 0.8
neonBorder.BorderSizePixel = 0
neonBorder.Parent = mainFrame
local borderCorner = Instance.new("UICorner")
borderCorner.CornerRadius = UDim.new(0, 12)
borderCorner.Parent = neonBorder

-- Título
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.BackgroundTransparency = 0.5
title.Text = "⚡ FUTURISTIC TOOLS ⚡"
title.TextColor3 = Color3.fromRGB(0, 255, 255)
title.TextSize = 18
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Center
title.Parent = mainFrame

-- Barra de arrastar
local dragBar = Instance.new("Frame")
dragBar.Size = UDim2.new(1, 0, 0, 40)
dragBar.BackgroundTransparency = 1
dragBar.Parent = mainFrame

-- Botão minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Name = "MinimizeBtn"
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -40, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
minimizeBtn.Text = "−"
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.TextSize = 20
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Parent = mainFrame

local minimizeCorner = Instance.new("UICorner")
minimizeCorner.CornerRadius = UDim.new(0, 6)
minimizeCorner.Parent = minimizeBtn

-- Container para conteúdo minimizável
local contentContainer = Instance.new("Frame")
contentContainer.Name = "ContentContainer"
contentContainer.Size = UDim2.new(1, 0, 1, -40)
contentContainer.Position = UDim2.new(0, 0, 0, 40)
contentContainer.BackgroundTransparency = 1
contentContainer.Parent = mainFrame

-- ======================================
-- SEÇÃO 2: SISTEMA DE ARRASTAR
-- ======================================

local dragging = false
local dragStartPos
local dragStartMousePos

dragBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStartPos = mainFrame.Position
        dragStartMousePos = input.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStartMousePos
        mainFrame.Position = UDim2.new(dragStartPos.X.Scale, dragStartPos.X.Offset + delta.X, 
                                       dragStartPos.Y.Scale, dragStartPos.Y.Offset + delta.Y)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- ======================================
-- SEÇÃO 3: ELEMENTOS DA UI
-- ======================================

local function createButton(parent, name, text, yPos, color)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Size = UDim2.new(0, 200, 0, 45)
    btn.Position = UDim2.new(0.5, -100, 0, yPos)
    btn.BackgroundColor3 = color or Color3.fromRGB(30, 30, 35)
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 16
    btn.Font = Enum.Font.GothamSemibold
    btn.Parent = parent
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = btn
    
    local btnStroke = Instance.new("UIStroke")
    btnStroke.Thickness = 1
    btnStroke.Color = Color3.fromRGB(0, 255, 255)
    btnStroke.Transparency = 0.7
    btnStroke.Parent = btn
    
    return btn
end

-- Botão Fly
local flyBtn = createButton(contentContainer, "FlyBtn", "🕊️ FLY MODE", 50, Color3.fromRGB(25, 25, 30))

-- Botão Aimbot
local aimbotBtn = createButton(contentContainer, "AimbotBtn", "🎯 AIMBOT", 115, Color3.fromRGB(25, 25, 30))

-- Configurações
local settingsLabel = Instance.new("TextLabel")
settingsLabel.Size = UDim2.new(0, 200, 0, 30)
settingsLabel.Position = UDim2.new(0.5, -100, 0, 175)
settingsLabel.BackgroundTransparency = 1
settingsLabel.Text = "⚙️ CONFIGURAÇÕES"
settingsLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
settingsLabel.TextSize = 14
settingsLabel.Font = Enum.Font.GothamBold
settingsLabel.Parent = contentContainer

-- Slider FOV
local fovLabel = Instance.new("TextLabel")
fovLabel.Size = UDim2.new(0, 200, 0, 20)
fovLabel.Position = UDim2.new(0.5, -100, 0, 210)
fovLabel.BackgroundTransparency = 1
fovLabel.Text = "FOV Size: 100"
fovLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
fovLabel.TextSize = 12
fovLabel.Font = Enum.Font.Gotham
fovLabel.Parent = contentContainer

local fovBox = Instance.new("TextBox")
fovBox.Size = UDim2.new(0, 200, 0, 35)
fovBox.Position = UDim2.new(0.5, -100, 0, 230)
fovBox.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
fovBox.PlaceholderText = "100"
fovBox.Text = "100"
fovBox.TextColor3 = Color3.fromRGB(255, 255, 255)
fovBox.TextSize = 14
fovBox.Font = Enum.Font.Gotham
fovBox.Parent = contentContainer

local fovBoxCorner = Instance.new("UICorner")
fovBoxCorner.CornerRadius = UDim.new(0, 6)
fovBoxCorner.Parent = fovBox

-- Slider velocidade fly
local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(0, 200, 0, 20)
speedLabel.Position = UDim2.new(0.5, -100, 0, 280)
speedLabel.BackgroundTransparency = 1
speedLabel.Text = "Fly Speed: 50"
speedLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
speedLabel.TextSize = 12
speedLabel.Font = Enum.Font.Gotham
speedLabel.Parent = contentContainer

local speedBox = Instance.new("TextBox")
speedBox.Size = UDim2.new(0, 200, 0, 35)
speedBox.Position = UDim2.new(0.5, -100, 0, 300)
speedBox.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
speedBox.PlaceholderText = "50"
speedBox.Text = "50"
speedBox.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBox.TextSize = 14
speedBox.Font = Enum.Font.Gotham
speedBox.Parent = contentContainer

local speedBoxCorner = Instance.new("UICorner")
speedBoxCorner.CornerRadius = UDim.new(0, 6)
speedBoxCorner.Parent = speedBox

-- ======================================
-- SEÇÃO 4: CÍRCULO FOV
-- ======================================

local fovCircle = Instance.new("Frame")
fovCircle.Name = "FOVCircle"
fovCircle.Size = UDim2.new(0, 100, 0, 100)
fovCircle.Position = UDim2.new(0.5, -50, 0.5, -50)
fovCircle.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
fovCircle.BackgroundTransparency = 0.85
fovCircle.BorderSizePixel = 2
fovCircle.BorderColor3 = Color3.fromRGB(0, 255, 255)
fovCircle.Visible = false
fovCircle.Parent = screenGui

local circleCorner = Instance.new("UICorner")
circleCorner.CornerRadius = UDim.new(1, 0)
circleCorner.Parent = fovCircle

-- ======================================
-- SEÇÃO 5: SISTEMA DE FLY
-- ======================================

local flying = false
local flySpeed = 50
local bodyVelocity = nil

local function enableFly()
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    local rootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    
    if not humanoid or not rootPart then return end
    
    flying = true
    humanoid.PlatformStand = true
    
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    bodyVelocity.Parent = rootPart
    
    local notif = Instance.new("TextLabel")
    notif.Size = UDim2.new(0, 200, 0, 40)
    notif.Position = UDim2.new(0.5, -100, 0.8, 0)
    notif.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
    notif.BackgroundTransparency = 0.2
    notif.Text = "✈️ FLY MODE ACTIVATED"
    notif.TextColor3 = Color3.fromRGB(255, 255, 255)
    notif.TextSize = 14
    notif.Font = Enum.Font.GothamBold
    notif.Parent = screenGui
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, 8)
    notifCorner.Parent = notif
    
    game:GetService("TweenService"):Create(notif, TweenInfo.new(1), {BackgroundTransparency = 1, TextTransparency = 1}):Play()
    game:GetService("Debris"):AddItem(notif, 2)
    
    RunService.RenderStepped:Connect(function()
        if not flying or not rootPart then return end
        local moveDirection = Vector3.new(
            (mouse.Hit.Position - rootPart.Position).Unit.X,
            (mouse.Hit.Position - rootPart.Position).Unit.Y,
            (mouse.Hit.Position - rootPart.Position).Unit.Z
        )
        if bodyVelocity then
            bodyVelocity.Velocity = moveDirection * flySpeed
        end
    end)
end

local function disableFly()
    flying = false
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.PlatformStand = false
    end
    if bodyVelocity then
        bodyVelocity:Destroy()
        bodyVelocity = nil
    end
    
    local notif = Instance.new("TextLabel")
    notif.Size = UDim2.new(0, 200, 0, 40)
    notif.Position = UDim2.new(0.5, -100, 0.8, 0)
    notif.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    notif.BackgroundTransparency = 0.2
    notif.Text = "🛑 FLY MODE DEACTIVATED"
    notif.TextColor3 = Color3.fromRGB(255, 255, 255)
    notif.TextSize = 14
    notif.Font = Enum.Font.GothamBold
    notif.Parent = screenGui
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, 8)
    notifCorner.Parent = notif
    
    game:GetService("TweenService"):Create(notif, TweenInfo.new(1), {BackgroundTransparency = 1, TextTransparency = 1}):Play()
    game:GetService("Debris"):AddItem(notif, 2)
end

-- ======================================
-- SEÇÃO 6: SISTEMA DE AIMBOT
-- ======================================

local aimbotActive = false
local fovSize = 100
local currentTarget = nil

local function updateFOV()
    fovCircle.Size = UDim2.new(0, fovSize, 0, fovSize)
    fovCircle.Position = UDim2.new(0.5, -fovSize/2, 0.5, -fovSize/2)
    fovLabel.Text = "FOV Size: " .. fovSize
end

local function getClosestPlayerInFOV()
    local closestPlayer = nil
    local closestDistance = fovSize / 2
    
    local centerScreen = Vector2.new(mouse.ViewSizeX / 2, mouse.ViewSizeY / 2)
    
    for _, otherPlayer in pairs(Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("Head") then
            local headPos = otherPlayer.Character.Head.Position
            local screenPos, onScreen = Camera:WorldToScreenPoint(headPos)
            
            if onScreen then
                local distanceFromCenter = (Vector2.new(screenPos.X, screenPos.Y) - centerScreen).Magnitude
                if distanceFromCenter < closestDistance then
                    closestDistance = distanceFromCenter
                    closestPlayer = otherPlayer
                end
            end
        end
    end
    
    return closestPlayer
end

local function aimbotUpdate()
    if not aimbotActive then return end
    
    local target = getClosestPlayerInFOV()
    currentTarget = target
    
    if target and target.Character and target.Character:FindFirstChild("Head") then
        local headPos = target.Character.Head.Position
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, headPos)
    end
end

-- ======================================
-- SEÇÃO 7: FUNÇÕES DOS BOTÕES
-- ======================================

local function animateButton(button)
    local originalSize = button.Size
    TweenService:Create(button, TweenInfo.new(0.1, Enum.EasingStyle.Quad), {Size = UDim2.new(originalSize.X.Scale, originalSize.X.Offset - 5, originalSize.Y.Scale, originalSize.Y.Offset - 5)}):Play()
    wait(0.1)
    TweenService:Create(button, TweenInfo.new(0.1, Enum.EasingStyle.Quad), {Size = originalSize}):Play()
end

flyBtn.MouseButton1Click:Connect(function()
    animateButton(flyBtn)
    if not flying then
        enableFly()
        flyBtn.Text = "🛸 FLY ACTIVE"
        flyBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 100)
    else
        disableFly()
        flyBtn.Text = "🕊️ FLY MODE"
        flyBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    end
end)

aimbotBtn.MouseButton1Click:Connect(function()
    animateButton(aimbotBtn)
    aimbotActive = not aimbotActive
    fovCircle.Visible = aimbotActive
    
    if aimbotActive then
        aimbotBtn.Text = "🎯 AIMBOT ACTIVE"
        aimbotBtn.BackgroundColor3 = Color3.fromRGB(0, 100, 100)
        
        local notif = Instance.new("TextLabel")
        notif.Size = UDim2.new(0, 200, 0, 40)
        notif.Position = UDim2.new(0.5, -100, 0.7, 0)
        notif.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
        notif.BackgroundTransparency = 0.2
        notif.Text = "🎯 AIMBOT ACTIVATED"
        notif.TextColor3 = Color3.fromRGB(255, 255, 255)
        notif.TextSize = 14
        notif.Font = Enum.Font.GothamBold
        notif.Parent = screenGui
        
        local notifCorner = Instance.new("UICorner")
        notifCorner.CornerRadius = UDim.new(0, 8)
        notifCorner.Parent = notif
        
        TweenService:Create(notif, TweenInfo.new(1), {BackgroundTransparency = 1, TextTransparency = 1}):Play()
        game:GetService("Debris"):AddItem(notif, 2)
    else
        aimbotBtn.Text = "🎯 AIMBOT"
        aimbotBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        currentTarget = nil
        
        local notif = Instance.new("TextLabel")
        notif.Size = UDim2.new(0, 200, 0, 40)
        notif.Position = UDim2.new(0.5, -100, 0.7, 0)
        notif.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
        notif.BackgroundTransparency = 0.2
        notif.Text = "🎯 AIMBOT DEACTIVATED"
        notif.TextColor3 = Color3.fromRGB(255, 255, 255)
        notif.TextSize = 14
        notif.Font = Enum.Font.GothamBold
        notif.Parent = screenGui
        
        local notifCorner = Instance.new("UICorner")
        notifCorner.CornerRadius = UDim.new(0, 8)
        notifCorner.Parent = notif
        
        TweenService:Create(notif, TweenInfo.new(1), {BackgroundTransparency = 1, TextTransparency = 1}):Play()
        game:GetService("Debris"):AddItem(notif, 2)
    end
end)

fovBox.FocusLost:Connect(function()
    local newSize = tonumber(fovBox.Text)
    if newSize and newSize >= 50 and newSize <= 300 then
        fovSize = newSize
        updateFOV()
    else
        fovBox.Text = tostring(fovSize)
    end
end)

speedBox.FocusLost:Connect(function()
    local newSpeed = tonumber(speedBox.Text)
    if newSpeed and newSpeed >= 10 and newSpeed <= 200 then
        flySpeed = newSpeed
        speedLabel.Text = "Fly Speed: " .. flySpeed
    else
        speedBox.Text = tostring(flySpeed)
    end
end)

-- Minimizar/Restaurar
local minimized = false
local originalHeight = 450

minimizeBtn.MouseButton1Click:Connect(function()
    animateButton(minimizeBtn)
    minimized = not minimized
    
    if minimized then
        TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {Size = UDim2.new(0, 350, 0, 45)}):Play()
        contentContainer.Visible = false
        minimizeBtn.Text = "+"
    else
        TweenService:Create(mainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {Size = UDim2.new(0, 350, 0, originalHeight)}):Play()
        contentContainer.Visible = true
        minimizeBtn.Text = "−"
    end
end)

-- Loop do aimbot
RunService.RenderStepped:Connect(aimbotUpdate)

-- Animações de entrada
mainFrame.BackgroundTransparency = 1
TweenService:Create(mainFrame, TweenInfo.new(0.5, Enum.EasingStyle.Back), {BackgroundTransparency = 0.05}):Play()
TweenService:Create(neonBorder, TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.Out, -1, true), {BackgroundTransparency = 0.6}):Play()
· Uso eficiente dos serviços Roblox
· Responsivo para diferentes resoluções

A interface é completamente funcional e pronta para uso em qualquer executor Lua compatível com Roblox!# Bem-vind
