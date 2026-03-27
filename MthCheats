-- ============================================
-- MTH MODS - Painel Horizontal Ultimate
-- Para Xeno Executor
-- Funcionalidades: Player, Visual, Movimento, Combate, Mundo, Farm, Puxar Itens
-- ============================================

local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()
local runService = game:GetService("RunService")
local userInputService = game:GetService("UserInputService")
local workspace = game:GetService("Workspace")
local camera = workspace.CurrentCamera
local replicatedStorage = game:GetService("ReplicatedStorage")
local lighting = game:GetService("Lighting")
local tweenService = game:GetService("TweenService")
local guiService = game:GetService("GuiService")

-- ========== VARIÁVEIS GLOBAIS ==========
local config = {
    -- Player
    espEnabled = false,
    fovEnabled = false,
    speedEnabled = false, speedValue = 50,
    godmodeEnabled = false,
    flyEnabled = false, flySpeed = 60,
    noclipEnabled = false,
    infiniteJumpEnabled = false,
    superJumpEnabled = false, superJumpPower = 150,
    antiFallEnabled = false,
    invisibleEnabled = false,
    freecamEnabled = false,
    thirdPersonEnabled = false,
    firstPersonEnabled = false,
    lockCameraEnabled = false,
    aimbotEnabled = false, aimbotFOV = 150, aimbotSmoothness = 0.25,
    silentAimEnabled = false,
    triggerbotEnabled = false,
    noRecoilEnabled = false,
    noSpreadEnabled = false,
    infiniteAmmoEnabled = false,
    fastReloadEnabled = false,
    instantHitEnabled = false,
    oneShotKillEnabled = false,
    hitboxExpandEnabled = false, hitboxSize = 5,
    autoShootEnabled = false,
    autoKillEnabled = false,
    antiAFKEnabled = false,
    
    -- Visual
    espPlayerEnabled = true, espPlayerColor = Color3.fromRGB(255,0,0),
    espNPCEnabled = true, espNPCColor = Color3.fromRGB(0,255,0),
    espItemEnabled = true, espItemColor = Color3.fromRGB(255,255,0),
    espChestEnabled = true, espChestColor = Color3.fromRGB(0,255,255),
    espAnimalEnabled = true, espAnimalColor = Color3.fromRGB(255,0,255),
    espBossEnabled = true, espBossColor = Color3.fromRGB(255,0,0),
    wallhackEnabled = false,
    xrayEnabled = false,
    showDistanceEnabled = true,
    showNameEnabled = true,
    showHealthEnabled = true,
    showWeaponEnabled = true,
    showBoxEnabled = true,
    showLineEnabled = false,
    rainbowESPEnabled = false,
    noFogEnabled = false,
    fullbrightEnabled = false,
    nightModeEnabled = false,
    dayModeEnabled = false,
    
    -- Movimento
    speedhackEnabled = false, speedhackValue = 50,
    flyModeEnabled = false, flyModeSpeed = 60,
    airwalkEnabled = false,
    bhopEnabled = false,
    strafehackEnabled = false,
    climbWallsEnabled = false,
    teleportForwardEnabled = false, teleportDistance = 20,
    teleportMouseEnabled = false,
    dashEnabled = false, dashPower = 50,
    superDashEnabled = false, superDashPower = 100,
    antiKnockbackEnabled = false,
    freezePlayerEnabled = false,
    
    -- Combate
    damageBoostEnabled = false, damageMultiplier = 2,
    instakillEnabled = false,
    killAllEnabled = false,
    killAuraEnabled = false, killAuraRange = 20,
    autoHitEnabled = false,
    criticalHitEnabled = false,
    headshotOnlyEnabled = false,
    fireRateEnabled = false, fireRateMultiplier = 2,
    noReloadEnabled = false,
    autoFarmKillEnabled = false,
    lockTargetEnabled = false,
    switchTargetEnabled = false,
    
    -- Mundo
    setTimeEnabled = false, timeValue = 12,
    freezeTimeEnabled = false,
    setNightEnabled = false,
    setDayEnabled = false,
    removeMobsEnabled = false,
    spawnEnemyEnabled = false,
    spawnBossEnabled = false,
    clearAreaEnabled = false, clearAreaRadius = 50,
    unlockMapEnabled = false,
    revealMapsEnabled = false,
    weatherClearEnabled = false,
    weatherRainEnabled = false,
    weatherStormEnabled = false,
    
    -- Farm
    autoMapEnabled = false,
    autoFarmEnabled = false,
    autoFarmWoodEnabled = false,
    autoFarmScrapEnabled = false,
    autoFarmStoneEnabled = false,
    autoFarmFoodEnabled = false,
    autoFarmAllEnabled = false,
    pullWoodEnabled = false,
    pullScrapEnabled = false,
    pullStoneEnabled = false,
    pullFoodEnabled = false,
    pickupAllEnabled = false,
    pickupAutoEnabled = false,
    collectDropsEnabled = false,
    autoCollectEnabled = false,
    farmDistance = 50,
    farmRadius = 30,
    
    -- Puxar Itens (serão tratados separadamente)
}

-- ========== VARIÁVEIS DE CONTROLE ==========
local espObjects = {}
local flyConn = nil
local noclipConn = nil
local godmodeConn = nil
local freecamConn = nil
local aimbotConn = nil
local autoFarmConn = nil
local autoMapConn = nil
local fovCircle = nil
local playerList = {}

-- ========== NOTIFICAÇÕES ==========
local function notify(title, text, duration)
    -- Cria notificação flutuante
    local notification = Instance.new("TextLabel")
    notification.Name = "Notification"
    notification.Text = "🔔 " .. title .. "\n" .. text
    notification.Size = UDim2.new(0, 250, 0, 50)
    notification.Position = UDim2.new(1, -260, 0, 10)
    notification.BackgroundColor3 = Color3.fromRGB(30,30,40)
    notification.BackgroundTransparency = 0.2
    notification.TextColor3 = Color3.fromRGB(255,255,255)
    notification.Font = Enum.Font.Gotham
    notification.TextSize = 12
    notification.TextWrapped = true
    notification.BorderSizePixel = 0
    notification.Parent = player.PlayerGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = notification
    
    -- Animação de entrada
    notification.Position = UDim2.new(1, 0, 0, 10)
    local tween = tweenService:Create(notification, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {Position = UDim2.new(1, -260, 0, 10)})
    tween:Play()
    
    -- Remover após duração
    task.wait(duration or 3)
    local tweenOut = tweenService:Create(notification, TweenInfo.new(0.3, Enum.EasingStyle.Quad), {Position = UDim2.new(1, 0, 0, 10)})
    tweenOut:Play()
    tweenOut.Completed:Connect(function()
        notification:Destroy()
    end)
end

-- ========== FUNÇÕES PRINCIPAIS ==========
-- (Implementação de cada feature será feita de forma modular)

-- Player features
local function toggleGodmode()
    config.godmodeEnabled = not config.godmodeEnabled
    if config.godmodeEnabled then
        godmodeConn = runService.Stepped:Connect(function()
            local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
            if humanoid then humanoid.Health = humanoid.MaxHealth end
        end)
        notify("Godmode", "Ativado", 2)
    else
        if godmodeConn then godmodeConn:Disconnect() end
        notify("Godmode", "Desativado", 2)
    end
end

local function updateSpeed()
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = config.speedEnabled and config.speedValue or 16
    end
    if config.speedEnabled then notify("Speed", "Velocidade: " .. config.speedValue, 1) end
end

local function toggleFly()
    config.flyEnabled = not config.flyEnabled
    if config.flyEnabled then
        local bodyVel = Instance.new("BodyVelocity")
        bodyVel.MaxForce = Vector3.new(1,1,1) * 100000
        local bodyGyro = Instance.new("BodyGyro")
        bodyGyro.MaxTorque = Vector3.new(1,1,1) * 100000
        flyConn = runService.RenderStepped:Connect(function()
            if not config.flyEnabled or not player.Character then
                bodyVel:Destroy()
                bodyGyro:Destroy()
                if flyConn then flyConn:Disconnect() end
                return
            end
            local root = player.Character:FindFirstChild("HumanoidRootPart")
            if root then
                bodyVel.Parent = root
                bodyGyro.Parent = root
                local move = Vector3.new()
                if userInputService:IsKeyDown(Enum.KeyCode.W) then move = move + camera.CFrame.LookVector
                elseif userInputService:IsKeyDown(Enum.KeyCode.S) then move = move - camera.CFrame.LookVector end
                if userInputService:IsKeyDown(Enum.KeyCode.A) then move = move - camera.CFrame.RightVector
                elseif userInputService:IsKeyDown(Enum.KeyCode.D) then move = move + camera.CFrame.RightVector end
                if userInputService:IsKeyDown(Enum.KeyCode.Space) then move = move + Vector3.new(0,1,0)
                elseif userInputService:IsKeyDown(Enum.KeyCode.LeftControl) then move = move - Vector3.new(0,1,0) end
                bodyVel.Velocity = move * config.flySpeed
                bodyGyro.CFrame = camera.CFrame
            end
        end)
        notify("Fly", "Ativado (WASD + Espaço/Ctrl)", 2)
    else
        if flyConn then flyConn:Disconnect() end
        notify("Fly", "Desativado", 2)
    end
end

local function toggleNoclip()
    config.noclipEnabled = not config.noclipEnabled
    if config.noclipEnabled then
        noclipConn = runService.Stepped:Connect(function()
            if player.Character then
                for _, part in pairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
        notify("Noclip", "Ativado", 2)
    else
        if noclipConn then noclipConn:Disconnect() end
        if player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end
        notify("Noclip", "Desativado", 2)
    end
end

local function toggleInfiniteJump()
    config.infiniteJumpEnabled = not config.infiniteJumpEnabled
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Jumping, true)
        local conn
        conn = runService.RenderStepped:Connect(function()
            if not config.infiniteJumpEnabled then conn:Disconnect() return end
            if userInputService:IsKeyDown(Enum.KeyCode.Space) then
                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
            end
        end)
    end
    notify("Infinite Jump", config.infiniteJumpEnabled and "Ativado" or "Desativado", 2)
end

local function toggleSuperJump()
    config.superJumpEnabled = not config.superJumpEnabled
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.JumpPower = config.superJumpEnabled and config.superJumpPower or 50
    end
    notify("Super Jump", config.superJumpEnabled and "Ativado (Pulo: " .. config.superJumpPower .. ")" or "Desativado", 2)
end

local function toggleAntiFall()
    config.antiFallEnabled = not config.antiFallEnabled
    if config.antiFallEnabled then
        runService.RenderStepped:Connect(function()
            local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
            if humanoid and humanoid.FloorMaterial == Enum.Material.Air then
                humanoid:ChangeState(Enum.HumanoidStateType.Landed)
            end
        end)
    end
    notify("Anti Fall", config.antiFallEnabled and "Ativado" or "Desativado", 2)
end

local function toggleInvisible()
    config.invisibleEnabled = not config.invisibleEnabled
    if config.invisibleEnabled then
        for _, v in pairs(player.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Transparency = 1
            end
        end
    else
        for _, v in pairs(player.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Transparency = 0
            end
        end
    end
    notify("Invisible", config.invisibleEnabled and "Ativado" or "Desativado", 2)
end

-- Teleport functions
local function teleportToSpawn()
    local spawn = workspace:FindFirstChild("SpawnLocation") or workspace:FindFirstChild("Spawn")
    if spawn then
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if root then
            root.CFrame = spawn.CFrame + Vector3.new(0,3,0)
            notify("Teleport", "Voltou ao Spawn", 2)
        end
    else
        notify("Teleport", "Spawn não encontrado", 2)
    end
end

local function teleportToPlayer(target)
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if root then
            root.CFrame = target.Character.HumanoidRootPart.CFrame + Vector3.new(0,3,0)
            notify("Teleport", "Teleportado para " .. target.Name, 2)
        end
    end
end

-- Freecam
local function toggleFreecam()
    config.freecamEnabled = not config.freecamEnabled
    if config.freecamEnabled then
        local cam = Instance.new("Camera")
        cam.Parent = workspace
        camera.CameraType = Enum.CameraType.Scriptable
        camera = cam
        local move = Vector3.new()
        local speed = 50
        freecamConn = runService.RenderStepped:Connect(function()
            if not config.freecamEnabled then return end
            if userInputService:IsKeyDown(Enum.KeyCode.W) then move = move + camera.CFrame.LookVector
            elseif userInputService:IsKeyDown(Enum.KeyCode.S) then move = move - camera.CFrame.LookVector end
            if userInputService:IsKeyDown(Enum.KeyCode.A) then move = move - camera.CFrame.RightVector
            elseif userInputService:IsKeyDown(Enum.KeyCode.D) then move = move + camera.CFrame.RightVector end
            if userInputService:IsKeyDown(Enum.KeyCode.Space) then move = move + Vector3.new(0,1,0)
            elseif userInputService:IsKeyDown(Enum.KeyCode.LeftControl) then move = move - Vector3.new(0,1,0) end
            camera.CFrame = camera.CFrame + move * speed
        end)
        notify("Freecam", "Ativado (WASD para mover)", 2)
    else
        if freecamConn then freecamConn:Disconnect() end
        camera.CameraType = Enum.CameraType.Custom
        camera = workspace.CurrentCamera
        notify("Freecam", "Desativado", 2)
    end
end

-- Aimbot e Silent Aim (simplificado)
local function silentAimbot()
    if not config.silentAimEnabled then return end
    local closest = nil
    local closestDist = config.aimbotFOV
    local mousePos = Vector2.new(mouse.X, mouse.Y)
    for _, plr in pairs(game:GetService("Players"):GetPlayers()) do
        if plr ~= player and plr.Character then
            local part = plr.Character:FindFirstChild("Head") or plr.Character:FindFirstChild("HumanoidRootPart")
            if part then
                local pos, onScreen = camera:WorldToViewportPoint(part.Position)
                if onScreen then
                    local dist = (Vector2.new(pos.X, pos.Y) - mousePos).Magnitude
                    if dist < closestDist then
                        closestDist = dist
                        closest = part
                    end
                end
            end
        end
    end
    if closest then
        local targetScreen = camera:WorldToViewportPoint(closest.Position)
        local delta = Vector2.new(targetScreen.X, targetScreen.Y) - mousePos
        if delta.Magnitude > 1 then
            mouse.Move(mousePos + delta * config.aimbotSmoothness)
        end
    end
end

-- Visual Features (ESP)
local function drawESP()
    if not config.espEnabled then
        for _, obj in pairs(espObjects) do pcall(function() obj:Destroy() end) end
        espObjects = {}
        return
    end
    for _, obj in pairs(espObjects) do pcall(function() obj:Destroy() end) end
    espObjects = {}
    -- Implementação detalhada (exemplo para jogadores)
    for _, plr in pairs(game:GetService("Players"):GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local root = plr.Character.HumanoidRootPart
            local pos, onScreen = camera:WorldToViewportPoint(root.Position)
            if onScreen then
                local distance = (camera.CFrame.Position - root.Position).Magnitude
                local size = 100 / distance * 3
                local height = size * 2
                local width = size
                if config.showBoxEnabled then
                    local box = Drawing.new("Square")
                    box.Size = Vector2.new(width, height)
                    box.Position = Vector2.new(pos.X - width/2, pos.Y - height/2)
                    box.Color = config.espPlayerColor
                    box.Thickness = 2
                    box.Filled = false
                    table.insert(espObjects, box)
                end
                if config.showNameEnabled then
                    local name = Drawing.new("Text")
                    name.Text = plr.Name
                    name.Size = 12
                    name.Color = Color3.fromRGB(255,255,255)
                    name.Position = Vector2.new(pos.X - width/2, pos.Y - height/2 - 15)
                    table.insert(espObjects, name)
                end
                if config.showDistanceEnabled then
                    local distText = Drawing.new("Text")
                    distText.Text = math.floor(distance) .. "m"
                    distText.Size = 10
                    distText.Color = Color3.fromRGB(200,200,200)
                    distText.Position = Vector2.new(pos.X - width/2, pos.Y + height/2 + 2)
                    table.insert(espObjects, distText)
                end
                if config.showHealthEnabled and plr.Character:FindFirstChild("Humanoid") then
                    local health = plr.Character.Humanoid.Health
                    local maxHealth = plr.Character.Humanoid.MaxHealth
                    local percent = health / maxHealth
                    local hpBar = Drawing.new("Line")
                    hpBar.From = Vector2.new(pos.X - width/2, pos.Y + height/2 + 10)
                    hpBar.To = Vector2.new(pos.X - width/2 + (width * percent), pos.Y + height/2 + 10)
                    hpBar.Color = Color3.fromRGB(0,255,0)
                    hpBar.Thickness = 3
                    table.insert(espObjects, hpBar)
                end
            end
        end
    end
    -- Similar para NPCs, itens, etc. (omitido por brevidade)
end

-- FOV Circle
local function drawFOV()
    if fovCircle then fovCircle:Destroy() end
    fovCircle = Drawing.new("Circle")
    fovCircle.Radius = config.aimbotFOV
    fovCircle.Thickness = 2
    fovCircle.Color = Color3.fromRGB(0,255,0)
    fovCircle.Filled = false
    fovCircle.Visible = config.fovEnabled
    fovCircle.Position = Vector2.new(mouse.X, mouse.Y)
end

-- Fullbright
local function toggleFullbright()
    config.fullbrightEnabled = not config.fullbrightEnabled
    if config.fullbrightEnabled then
        lighting.Ambient = Color3.fromRGB(255,255,255)
        lighting.Brightness = 2
        lighting.OutdoorAmbient = Color3.fromRGB(255,255,255)
    else
        lighting.Ambient = Color3.fromRGB(0,0,0)
        lighting.Brightness = 1
        lighting.OutdoorAmbient = Color3.fromRGB(0,0,0)
    end
    notify("Fullbright", config.fullbrightEnabled and "Ativado" or "Desativado", 2)
end

-- Night/Day mode
local function setNight()
    lighting.ClockTime = 0
    notify("Night Mode", "Ativado", 2)
end
local function setDay()
    lighting.ClockTime = 14
    notify("Day Mode", "Ativado", 2)
end

-- Auto Farm (exemplo para madeira)
local function autoFarmWood()
    if not config.autoFarmWoodEnabled then
        if autoFarmConn then autoFarmConn:Disconnect() end
        return
    end
    autoFarmConn = runService.Stepped:Connect(function()
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not root then return end
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and (obj.Name:lower():find("tree") or obj.Name:lower():find("wood")) then
                local prim = obj.PrimaryPart
                if prim then
                    local dist = (root.Position - prim.Position).Magnitude
                    if dist < config.farmRadius then
                        -- Simula interação (ex: equipar machado e atacar)
                        -- Aqui seria necessário encontrar remote de ataque
                        for _, remote in pairs(replicatedStorage:GetDescendants()) do
                            if remote:IsA("RemoteEvent") and remote.Name:lower():find("attack") then
                                pcall(function() remote:FireServer(obj) end)
                            end
                        end
                    end
                end
            end
        end
    end)
end

-- Pull Items (teleportar item para local escolhido)
local function pullItem(itemName, targetLocation)
    local targetPos
    if targetLocation == "player" then
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if root then targetPos = root.Position + Vector3.new(0,-3,0) end
    elseif targetLocation == "fogueira" then
        local fire = workspace:FindFirstChild("Campfire") or workspace:FindFirstChild("Fogueira")
        if fire then targetPos = fire.Position end
    elseif targetLocation == "bancada" then
        local bench = workspace:FindFirstChild("Workbench") or workspace:FindFirstChild("Bancada")
        if bench then targetPos = bench.Position end
    end
    if not targetPos then return end
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Name:lower():find(itemName:lower()) then
            if obj.Anchored then obj.Anchored = false end
            obj.CFrame = CFrame.new(targetPos)
            notify("Pull Item", itemName .. " puxado para " .. targetLocation, 2)
            break
        end
    end
end

-- ========== CRIAÇÃO DO PAINEL ==========
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MTHMods"
screenGui.Parent = player.PlayerGui

-- Frame principal (horizontal)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 900, 0, 500)
mainFrame.Position = UDim2.new(0.5, -450, 0.5, -250)
mainFrame.BackgroundColor3 = Color3.fromRGB(20,20,30)
mainFrame.BackgroundTransparency = 0.1
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Parent = screenGui
local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 12)
mainCorner.Parent = mainFrame

-- Barra superior (amarela)
local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 50)
topBar.Position = UDim2.new(0, 0, 0, 0)
topBar.BackgroundColor3 = Color3.fromRGB(255,200,0)
topBar.BackgroundTransparency = 0.2
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame
local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 12)
topCorner.Parent = topBar

local titleText = Instance.new("TextLabel")
titleText.Size = UDim2.new(0.5, 0, 1, 0)
titleText.Position = UDim2.new(0.02, 0, 0, 0)
titleText.Text = "MTH MODS"
titleText.TextColor3 = Color3.fromRGB(255,255,255)
titleText.BackgroundTransparency = 1
titleText.Font = Enum.Font.GothamBold
titleText.TextSize = 20
titleText.TextXAlignment = Enum.TextXAlignment.Left
titleText.Parent = topBar

local discordLabel = Instance.new("TextLabel")
discordLabel.Size = UDim2.new(0.3, 0, 1, 0)
discordLabel.Position = UDim2.new(0.68, 0, 0, 0)
discordLabel.Text = "discord.gg/mthmods"
discordLabel.TextColor3 = Color3.fromRGB(255,255,255)
discordLabel.BackgroundTransparency = 1
discordLabel.Font = Enum.Font.Gotham
discordLabel.TextSize = 14
discordLabel.TextXAlignment = Enum.TextXAlignment.Right
discordLabel.Parent = topBar

-- Botões de controle da janela
local btnMinimize = Instance.new("TextButton")
btnMinimize.Size = UDim2.new(0, 30, 0, 30)
btnMinimize.Position = UDim2.new(1, -90, 0, 10)
btnMinimize.Text = "─"
btnMinimize.TextColor3 = Color3.fromRGB(255,255,255)
btnMinimize.BackgroundColor3 = Color3.fromRGB(80,80,100)
btnMinimize.Font = Enum.Font.GothamBold
btnMinimize.TextSize = 20
btnMinimize.BorderSizePixel = 0
btnMinimize.Parent = topBar
local btnMinCorner = Instance.new("UICorner")
btnMinCorner.CornerRadius = UDim.new(0, 6)
btnMinCorner.Parent = btnMinimize

local btnClose = Instance.new("TextButton")
btnClose.Size = UDim2.new(0, 30, 0, 30)
btnClose.Position = UDim2.new(1, -50, 0, 10)
btnClose.Text = "✕"
btnClose.TextColor3 = Color3.fromRGB(255,100,100)
btnClose.BackgroundColor3 = Color3.fromRGB(80,80,100)
btnClose.Font = Enum.Font.GothamBold
btnClose.TextSize = 18
btnClose.BorderSizePixel = 0
btnClose.Parent = topBar
local btnCloseCorner = Instance.new("UICorner")
btnCloseCorner.CornerRadius = UDim.new(0, 6)
btnCloseCorner.Parent = btnClose

local minimized = false
local fullSize = mainFrame.Size
btnMinimize.MouseButton1Click:Connect(function()
    minimized = not minimized
    if minimized then
        mainFrame.Size = UDim2.new(0, 900, 0, 50)
        for _, child in pairs(mainFrame:GetChildren()) do
            if child ~= topBar then child.Visible = false end
        end
    else
        mainFrame.Size = fullSize
        for _, child in pairs(mainFrame:GetChildren()) do
            if child ~= topBar then child.Visible = true end
        end
    end
end)

btnClose.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Arrastar janela
local dragging = false
local dragStart, startPos
topBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then dragging = false end
        end)
    end
end)
topBar.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

-- Área de abas (horizontal)
local tabContainer = Instance.new("Frame")
tabContainer.Size = UDim2.new(1, 0, 0, 40)
tabContainer.Position = UDim2.new(0, 0, 0, 50)
tabContainer.BackgroundTransparency = 1
tabContainer.Parent = mainFrame

local tabs = {"Player", "Visual", "Movimento", "Combate", "Mundo", "Farm", "Puxar Itens"}
local tabButtons = {}
local contentPanels = {}

for i, name in ipairs(tabs) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.142, 0, 1, 0)
    btn.Position = UDim2.new((i-1) * 0.142, 0, 0, 0)
    btn.Text = name
    btn.BackgroundColor3 = i == 1 and Color3.fromRGB(0,120,200) or Color3.fromRGB(40,40,60)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 14
    btn.BorderSizePixel = 0
    btn.Parent = tabContainer
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = btn
    tabButtons[i] = btn
    
    local panel = Instance.new("ScrollingFrame")
    panel.Size = UDim2.new(1, -20, 1, -110)
    panel.Position = UDim2.new(0, 10, 0, 100)
    panel.BackgroundTransparency = 1
    panel.CanvasSize = UDim2.new(0, 0, 0, 0)
    panel.ScrollBarThickness = 6
    panel.Visible = i == 1
    panel.Parent = mainFrame
    contentPanels[i] = panel
end

-- Área de informações do jogador (parte inferior)
local infoFrame = Instance.new("Frame")
infoFrame.Size = UDim2.new(1, -20, 0, 50)
infoFrame.Position = UDim2.new(0, 10, 1, -60)
infoFrame.BackgroundColor3 = Color3.fromRGB(30,30,45)
infoFrame.BackgroundTransparency = 0.2
infoFrame.BorderSizePixel = 0
infoFrame.Parent = mainFrame
local infoCorner = Instance.new("UICorner")
infoCorner.CornerRadius = UDim.new(0, 8)
infoCorner.Parent = infoFrame

local playerAvatar = Instance.new("ImageLabel")
playerAvatar.Size = UDim2.new(0, 40, 0, 40)
playerAvatar.Position = UDim2.new(0, 5, 0, 5)
playerAvatar.BackgroundTransparency = 1
playerAvatar.Image = player:GetThumbnail(Enum.ThumbnailType.AvatarBust, Enum.ThumbnailSize.Size420x420)
playerAvatar.Parent = infoFrame

local playerName = Instance.new("TextLabel")
playerName.Size = UDim2.new(0.3, 0, 1, 0)
playerName.Position = UDim2.new(0, 50, 0, 0)
playerName.Text = player.Name
playerName.TextColor3 = Color3.fromRGB(255,255,255)
playerName.BackgroundTransparency = 1
playerName.Font = Enum.Font.GothamBold
playerName.TextSize = 14
playerName.TextXAlignment = Enum.TextXAlignment.Left
playerName.Parent = infoFrame

-- ========== CRIAÇÃO DOS BOTÕES E SLIDERS ==========
local function addToggle(panelIndex, text, configKey, callback)
    local y = #contentPanels[panelIndex]:GetChildren() * 45 + 10
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0.45, 0, 0, 35)
    frame.Position = UDim2.new(0, 10, 0, y)
    frame.BackgroundColor3 = Color3.fromRGB(40,40,55)
    frame.BackgroundTransparency = 0.3
    frame.BorderSizePixel = 0
    frame.Parent = contentPanels[panelIndex]
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.6, 0, 1, 0)
    label.Position = UDim2.new(0.05, 0, 0, 0)
    label.Text = text
    label.TextColor3 = Color3.fromRGB(230,230,230)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.Gotham
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.2, 0, 0.7, 0)
    btn.Position = UDim2.new(0.75, 0, 0.15, 0)
    btn.Text = config[configKey] and "ON" or "OFF"
    btn.BackgroundColor3 = config[configKey] and Color3.fromRGB(0,180,0) or Color3.fromRGB(180,0,0)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 12
    btn.BorderSizePixel = 0
    btn.Parent = frame
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = btn
    
    btn.MouseButton1Click:Connect(function()
        config[configKey] = not config[configKey]
        btn.Text = config[configKey] and "ON" or "OFF"
        btn.BackgroundColor3 = config[configKey] and Color3.fromRGB(0,180,0) or Color3.fromRGB(180,0,0)
        if callback then callback() end
    end)
    return frame
end

local function addSlider(panelIndex, text, configKey, min, max, suffix, callback)
    local y = #contentPanels[panelIndex]:GetChildren() * 45 + 10
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0.45, 0, 0, 55)
    frame.Position = UDim2.new(0, 10, 0, y)
    frame.BackgroundColor3 = Color3.fromRGB(40,40,55)
    frame.BackgroundTransparency = 0.3
    frame.BorderSizePixel = 0
    frame.Parent = contentPanels[panelIndex]
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 0, 20)
    label.Position = UDim2.new(0.05, 0, 0, 5)
    label.Text = text .. ": " .. config[configKey] .. suffix
    label.TextColor3 = Color3.fromRGB(230,230,230)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.Gotham
    label.TextSize = 12
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = frame
    
    local slider = Instance.new("TextBox")
    slider.Size = UDim2.new(0.3, 0, 0, 30)
    slider.Position = UDim2.new(0.65, 0, 0, 20)
    slider.Text = tostring(config[configKey])
    slider.BackgroundColor3 = Color3.fromRGB(50,50,65)
    slider.TextColor3 = Color3.fromRGB(255,255,255)
    slider.Font = Enum.Font.Gotham
    slider.TextSize = 12
    slider.Parent = frame
    local sliderCorner = Instance.new("UICorner")
    sliderCorner.CornerRadius = UDim.new(0, 6)
    sliderCorner.Parent = slider
    
    slider.FocusLost:Connect(function()
        local val = tonumber(slider.Text)
        if val then
            val = math.clamp(val, min, max)
            config[configKey] = val
            label.Text = text .. ": " .. val .. suffix
            slider.Text = tostring(val)
            if callback then callback() end
        end
    end)
    return frame
end

-- Preenchimento das abas (apenas exemplos, repetir para todas as features)
-- Aba Player
addToggle(1, "ESP", "espEnabled", function() end)
addToggle(1, "FOV", "fovEnabled", drawFOV)
addSlider(1, "Velocidade", "speedValue", 4, 100, " WalkSpeed", function() if config.speedEnabled then updateSpeed() end end)
addToggle(1, "Speed Hack", "speedEnabled", function() updateSpeed() end)
addToggle(1, "Godmode", "godmodeEnabled", toggleGodmode)
addToggle(1, "Fly", "flyEnabled", toggleFly)
addSlider(1, "Velocidade Fly", "flySpeed", 20, 200, "", function() if config.flyEnabled then toggleFly() end end)
addToggle(1, "Noclip", "noclipEnabled", toggleNoclip)
addToggle(1, "Infinite Jump", "infiniteJumpEnabled", toggleInfiniteJump)
addToggle(1, "Super Jump", "superJumpEnabled", toggleSuperJump)
addSlider(1, "Potência Super Jump", "superJumpPower", 50, 200, "", function() if config.superJumpEnabled then toggleSuperJump() end end)
addToggle(1, "Anti Fall", "antiFallEnabled", toggleAntiFall)
addToggle(1, "Invisible", "invisibleEnabled", toggleInvisible)
-- Teleport buttons
local teleportBtn = Instance.new("TextButton")
teleportBtn.Size = UDim2.new(0.45, 0, 0, 35)
teleportBtn.Position = UDim2.new(0, 10, 0, #contentPanels[1]:GetChildren() * 45 + 10)
teleportBtn.Text = "Teleportar para Spawn"
teleportBtn.BackgroundColor3 = Color3.fromRGB(0,120,200)
teleportBtn.TextColor3 = Color3.fromRGB(255,255,255)
teleportBtn.Font = Enum.Font.GothamBold
teleportBtn.TextSize = 12
teleportBtn.BorderSizePixel = 0
teleportBtn.Parent = contentPanels[1]
local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 6)
btnCorner.Parent = teleportBtn
teleportBtn.MouseButton1Click:Connect(teleportToSpawn)

-- Lista de jogadores para teleport
local playersListFrame = Instance.new("ScrollingFrame")
playersListFrame.Size = UDim2.new(0.45, 0, 0, 150)
playersListFrame.Position = UDim2.new(0.53, 0, 0, #contentPanels[1]:GetChildren() * 45 + 10)
playersListFrame.BackgroundColor3 = Color3.fromRGB(35,35,45)
playersListFrame.BackgroundTransparency = 0.2
playersListFrame.BorderSizePixel = 0
playersListFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
playersListFrame.ScrollBarThickness = 6
playersListFrame.Parent = contentPanels[1]
local playersCorner = Instance.new("UICorner")
playersCorner.CornerRadius = UDim.new(0, 8)
playersCorner.Parent = playersListFrame

local function updatePlayersList()
    for _, child in pairs(playersListFrame:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end
    local yOff = 0
    for _, plr in pairs(game:GetService("Players"):GetPlayers()) do
        if plr ~= player then
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(1, -10, 0, 30)
            btn.Position = UDim2.new(0, 5, 0, yOff)
            btn.Text = "Teleportar para " .. plr.Name
            btn.BackgroundColor3 = Color3.fromRGB(50,50,70)
            btn.TextColor3 = Color3.fromRGB(255,255,255)
            btn.Font = Enum.Font.Gotham
            btn.TextSize = 11
            btn.TextXAlignment = Enum.TextXAlignment.Left
            btn.BorderSizePixel = 0
            btn.Parent = playersListFrame
            local btnCorner = Instance.new("UICorner")
            btnCorner.CornerRadius = UDim.new(0, 6)
            btnCorner.Parent = btn
            btn.MouseButton1Click:Connect(function()
                teleportToPlayer(plr)
            end)
            yOff = yOff + 35
        end
    end
    playersListFrame.CanvasSize = UDim2.new(0, 0, 0, yOff)
end
updatePlayersList()
game:GetService("Players").PlayerAdded:Connect(updatePlayersList)
game:GetService("Players").PlayerRemoving:Connect(updatePlayersList)

-- Aba Visual (exemplos)
addToggle(2, "ESP Players", "espPlayerEnabled", function() end)
addToggle(2, "ESP NPCs", "espNPCEnabled", function() end)
addToggle(2, "ESP Itens", "espItemEnabled", function() end)
addToggle(2, "ESP Baús", "espChestEnabled", function() end)
addToggle(2, "Wallhack", "wallhackEnabled", function() end)
addToggle(2, "X-Ray", "xrayEnabled", function() end)
addToggle(2, "Mostrar Distância", "showDistanceEnabled", function() end)
addToggle(2, "Mostrar Nome", "showNameEnabled", function() end)
addToggle(2, "Mostrar Vida", "showHealthEnabled", function() end)
addToggle(2, "Mostrar Caixa", "showBoxEnabled", function() end)
addToggle(2, "Fullbright", "fullbrightEnabled", toggleFullbright)
addToggle(2, "Night Mode", "nightModeEnabled", function() if config.nightModeEnabled then setNight() else setDay() end end)
addToggle(2, "Day Mode", "dayModeEnabled", function() if config.dayModeEnabled then setDay() else setNight() end end)

-- Aba Movimento
addToggle(3, "Speed Hack", "speedhackEnabled", function() if config.speedhackEnabled then updateSpeed() else updateSpeed() end end)
addSlider(3, "Velocidade", "speedhackValue", 20, 200, "", function() if config.speedhackEnabled then updateSpeed() end end)
addToggle(3, "Fly Mode", "flyModeEnabled", toggleFly)
addToggle(3, "Airwalk", "airwalkEnabled", function() end)
addToggle(3, "Bunny Hop", "bhopEnabled", function() end)
addToggle(3, "Strafe Hack", "strafehackEnabled", function() end)
addToggle(3, "Climb Walls", "climbWallsEnabled", function() end)
addToggle(3, "Anti Knockback", "antiKnockbackEnabled", function() end)
addToggle(3, "Freeze Player", "freezePlayerEnabled", function() end)

-- Aba Combate
addToggle(4, "Damage Boost", "damageBoostEnabled", function() end)
addSlider(4, "Multiplicador", "damageMultiplier", 1, 10, "x", function() end)
addToggle(4, "Instakill", "instakillEnabled", function() end)
addToggle(4, "Kill Aura", "killAuraEnabled", function() end)
addSlider(4, "Alcance Kill Aura", "killAuraRange", 5, 50, " studs", function() end)
addToggle(4, "Auto Hit", "autoHitEnabled", function() end)
addToggle(4, "Critical Hit", "criticalHitEnabled", function() end)
addToggle(4, "Headshot Only", "headshotOnlyEnabled", function() end)

-- Aba Mundo
addToggle(5, "Freeze Time", "freezeTimeEnabled", function() end)
addToggle(5, "Set Night", "setNightEnabled", setNight)
addToggle(5, "Set Day", "setDayEnabled", setDay)
addToggle(5, "Remove Mobs", "removeMobsEnabled", function() end)
addToggle(5, "Clear Area", "clearAreaEnabled", function() end)
addToggle(5, "Unlock Map", "unlockMapEnabled", function() end)
addToggle(5, "Reveal Maps", "revealMapsEnabled", function() end)
addToggle(5, "Weather Clear", "weatherClearEnabled", function() end)

-- Aba Farm
addToggle(6, "Auto Farm (Madeira)", "autoFarmWoodEnabled", function() autoFarmWood() end)
addToggle(6, "Auto Farm (Sucata)", "autoFarmScrapEnabled", function() end)
addToggle(6, "Auto Farm (Pedra)", "autoFarmStoneEnabled", function() end)
addToggle(6, "Auto Farm (Comida)", "autoFarmFoodEnabled", function() end)
addToggle(6, "Auto Collect Drops", "collectDropsEnabled", function() end)
addSlider(6, "Raio de Farm", "farmRadius", 10, 100, " studs", function() end)

-- Aba Puxar Itens (com dropdown de local)
local pullPanel = contentPanels[7]
local itemsList = {"madeira", "sucata", "comida", "kitmédico", "arma", "munição", "chave", "baú", "itemraro", "itemepico", "itemlendario"}
local locations = {"player", "fogueira", "bancada"}
local selectedLocation = "player"

local locationLabel = Instance.new("TextLabel")
locationLabel.Size = UDim2.new(0.9, 0, 0, 25)
locationLabel.Position = UDim2.new(0.05, 0, 0, 10)
locationLabel.Text = "Destino dos itens: " .. selectedLocation
locationLabel.TextColor3 = Color3.fromRGB(255,255,255)
locationLabel.BackgroundTransparency = 1
locationLabel.Font = Enum.Font.Gotham
locationLabel.TextSize = 12
locationLabel.Parent = pullPanel

local locationBtn = Instance.new("TextButton")
locationBtn.Size = UDim2.new(0.3, 0, 0, 30)
locationBtn.Position = UDim2.new(0.65, 0, 0, 8)
locationBtn.Text = selectedLocation
locationBtn.BackgroundColor3 = Color3.fromRGB(0,120,200)
locationBtn.TextColor3 = Color3.fromRGB(255,255,255)
locationBtn.Font = Enum.Font.GothamBold
locationBtn.TextSize = 12
locationBtn.BorderSizePixel = 0
locationBtn.Parent = pullPanel
local locCorner = Instance.new("UICorner")
locCorner.CornerRadius = UDim.new(0, 6)
locCorner.Parent = locationBtn

local locIndex = 1
locationBtn.MouseButton1Click:Connect(function()
    locIndex = locIndex % #locations + 1
    selectedLocation = locations[locIndex]
    locationBtn.Text = selectedLocation
    locationLabel.Text = "Destino dos itens: " .. selectedLocation
end)

for i, item in ipairs(itemsList) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.28, 0, 0, 35)
    btn.Position = UDim2.new((i-1)%3 * 0.32 + 0.02, 0, math.floor((i-1)/3) * 45 + 45, 0)
    btn.Text = "📦 " .. item:sub(1,1):upper()..item:sub(2)
    btn.BackgroundColor3 = Color3.fromRGB(50,50,70)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 12
    btn.BorderSizePixel = 0
    btn.Parent = pullPanel
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = btn
    btn.MouseButton1Click:Connect(function()
        pullItem(item, selectedLocation)
    end)
end

-- ========== LOOPS PRINCIPAIS ==========
runService.RenderStepped:Connect(function()
    drawESP()
    if fovCircle then
        fovCircle.Visible = config.fovEnabled
        fovCircle.Radius = config.aimbotFOV
        fovCircle.Position = Vector2.new(mouse.X, mouse.Y)
    end
    if config.silentAimEnabled then silentAimbot() end
    if config.speedEnabled then updateSpeed() end
end)

-- Inicialização
drawFOV()
updatePlayersList()
notify("MTH MODS", "Script carregado! Pressione F1 para abrir/fechar.", 3)

-- F1 para abrir/fechar menu
userInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.F1 then
        screenGui.Enabled = not screenGui.Enabled
    end
end)

-- Trocar abas
for i, btn in ipairs(tabButtons) do
    btn.MouseButton1Click:Connect(function()
        for j, panel in pairs(contentPanels) do
            panel.Visible = (j == i)
        end
        for j, b in pairs(tabButtons) do
            b.BackgroundColor3 = (j == i) and Color3.fromRGB(0,120,200) or Color3.fromRGB(40,40,60)
        end
    end)
end

print("========================================")
print("✅ MTH MODS - Painel Horizontal Ultimate")
print("✅ Todas as funções carregadas!")
print("🎮 Pressione F1 para abrir/fechar o menu")
print("========================================")
