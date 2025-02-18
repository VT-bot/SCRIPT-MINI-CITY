local player = game.Players.LocalPlayer
local mouse = player:GetMouse()

-- Variáveis de toggle para cada função
local teleportEnabled = false
local espNomeEnabled = false
local espLineEnabled = false
local velocidadeEnabled = false

-- Configuração do painel e menu
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 400)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Text = "Sonic Menu"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 255)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 20
title.Parent = frame

local minimizeButton = Instance.new("TextButton")
minimizeButton.Text = "Minimizar"
minimizeButton.Size = UDim2.new(0, 100, 0, 30)
minimizeButton.Position = UDim2.new(0.5, -50, 0, 350)
minimizeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
minimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeButton.Parent = frame

local menuVisible = true
minimizeButton.MouseButton1Click:Connect(function()
    menuVisible = not menuVisible
    if menuVisible then
        frame.Size = UDim2.new(0, 200, 0, 400)
    else
        frame.Size = UDim2.new(0, 200, 0, 30)
    end
end)

--------------------------------------
-- Função ESP Nome (toggle)
local function ativarESPNome()
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("Head") then
            local head = plr.Character.Head
            if not head:FindFirstChild("ESP_Name") then
                local billboard = Instance.new("BillboardGui")
                billboard.Name = "ESP_Name"
                billboard.Parent = head
                billboard.Size = UDim2.new(0, 100, 0, 50)
                billboard.Adornee = head
                billboard.StudsOffset = Vector3.new(0, 2, 0)
                
                local label = Instance.new("TextLabel")
                label.Parent = billboard
                label.Size = UDim2.new(1, 0, 1, 0)
                label.Text = plr.Name
                label.TextColor3 = Color3.fromRGB(0, 0, 255)
                label.TextSize = 14
                label.BackgroundTransparency = 1
            end
        end
    end
end

local function desativarESPNome()
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character then
            local head = plr.Character:FindFirstChild("Head")
            if head then
                for _, child in pairs(head:GetChildren()) do
                    if child.Name == "ESP_Name" then
                        child:Destroy()
                    end
                end
            end
        end
    end
end

--------------------------------------
-- Função ESP Line (toggle)
local function ativarESPLine()
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = plr.Character.HumanoidRootPart
            local line = Instance.new("Part")
            line.Name = "ESP_Line"
            line.Size = Vector3.new(0.1, 0.1, 0.1)
            line.Anchored = true
            line.CanCollide = false
            line.BrickColor = BrickColor.new("Bright green")
            line.Parent = workspace
            line.CFrame = CFrame.new(player.Character.HumanoidRootPart.Position, hrp.Position)
        end
    end
end

local function desativarESPLine()
    for _, obj in pairs(workspace:GetChildren()) do
        if obj.Name == "ESP_Line" then
            obj:Destroy()
        end
    end
end

--------------------------------------
-- Função Teleporte (modo toggle)
local function ativarTeleporte()
    teleportEnabled = true
    teleporteButton.Text = "Teleporte ON"
end

mouse.Button1Down:Connect(function()
    if teleportEnabled then
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local targetPos = mouse.Hit.p
            player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPos + Vector3.new(0, 5, 0))
        end
        teleportEnabled = false
        teleporteButton.Text = "Teleporte OFF"
    end
end)

--------------------------------------
-- Função Velocidade (toggle)
local defaultSpeed = 16
local function ativarVelocidade()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = 100
    end
end

local function desativarVelocidade()
    if player.Character and player.Character:FindFirstChild("Humanoid") then
        player.Character.Humanoid.WalkSpeed = defaultSpeed
    end
end

--------------------------------------
-- Botão do Discord
local linkButton = Instance.new("TextButton")
linkButton.Text = "Discord"
linkButton.Size = UDim2.new(0, 100, 0, 30)
linkButton.Position = UDim2.new(0.5, -50, 0, 60)
linkButton.BackgroundColor3 = Color3.fromRGB(0, 0, 255)
linkButton.TextColor3 = Color3.fromRGB(255, 255, 255)
linkButton.Parent = frame

linkButton.MouseButton1Click:Connect(function()
    setclipboard("https://discord.gg/WFW3Ydb9")
end)

--------------------------------------
-- Botão para ESP Nome
local espNomeButton = Instance.new("TextButton")
espNomeButton.Text = "ESP Nome OFF"
espNomeButton.Size = UDim2.new(0, 100, 0, 30)
espNomeButton.Position = UDim2.new(0.5, -50, 0, 100)
espNomeButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
espNomeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espNomeButton.Parent = frame

espNomeButton.MouseButton1Click:Connect(function()
    espNomeEnabled = not espNomeEnabled
    if espNomeEnabled then
        espNomeButton.Text = "ESP Nome ON"
        ativarESPNome()
    else
        espNomeButton.Text = "ESP Nome OFF"
        desativarESPNome()
    end
end)

--------------------------------------
-- Botão para ESP Line
local espLineButton = Instance.new("TextButton")
espLineButton.Text = "ESP Line OFF"
espLineButton.Size = UDim2.new(0, 100, 0, 30)
espLineButton.Position = UDim2.new(0.5, -50, 0, 140)
espLineButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
espLineButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espLineButton.Parent = frame

espLineButton.MouseButton1Click:Connect(function()
    espLineEnabled = not espLineEnabled
    if espLineEnabled then
        espLineButton.Text = "ESP Line ON"
        ativarESPLine()
    else
        espLineButton.Text = "ESP Line OFF"
        desativarESPLine()
    end
end)

--------------------------------------
-- Botão para Teleporte
local teleporteButton = Instance.new("TextButton")
teleporteButton.Text = "Teleporte OFF"
teleporteButton.Size = UDim2.new(0, 100, 0, 30)
teleporteButton.Position = UDim2.new(0.5, -50, 0, 180)
teleporteButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
teleporteButton.TextColor3 = Color3.fromRGB(255, 255, 255)
teleporteButton.Parent = frame

teleporteButton.MouseButton1Click:Connect(function()
    if teleportEnabled then
        teleportEnabled = false
        teleporteButton.Text = "Teleporte OFF"
    else
        ativarTeleporte()
    end
end)

--------------------------------------
-- Botão para Velocidade
local velocidadeButton = Instance.new("TextButton")
velocidadeButton.Text = "Velocidade OFF"
velocidadeButton.Size = UDim2.new(0, 100, 0, 30)
velocidadeButton.Position = UDim2.new(0.5, -50, 0, 220)
velocidadeButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
velocidadeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
velocidadeButton.Parent = frame

velocidadeButton.MouseButton1Click:Connect(function()
    velocidadeEnabled = not velocidadeEnabled
    if velocidadeEnabled then
        velocidadeButton.Text = "Velocidade ON"
        ativarVelocidade()
    else
        velocidadeButton.Text = "Velocidade OFF"
        desativarVelocidade()
    end
end)

--------------------------------------
-- Carregar script adicional (se necessário)
loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
