if not game:IsLoaded() then game.Loaded:Wait() end

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local Player = game.Players.LocalPlayer
local maxTries, tries = 4, 0
local correctKey = "244"

-- Key system com kick após 4 tentativas
local function validateKey(input)
    if input == correctKey then
        return true
    else
        tries += 1
        if tries >= maxTries then
            Player:Kick("Você errou a Key 4 vezes.")
        end
        return false
    end
end

local Window = Rayfield:CreateWindow({
    Name = "PIXOTE MENU - Protegido",
    LoadingTitle = "Carregando...",
    LoadingSubtitle = "By PIXOTE",
    ConfigurationSaving = {Enabled = false},
    Discord = {Enabled = false},
    KeySystem = true,
    KeySettings = {
        Title = "Sistema de Key PIXOTE",
        Subtitle = "Insira a key correta",
        Note = "Key: 244",
        FileName = "pixote_key",
        SaveKey = false,
        GrabKeyFromSite = false,
        Key = correctKey,
        ValidateKeyFunction = validateKey,
    }
})

-- Função para criar ESP Nome, Time, Vida e Corpo
local ESPenabled = {Nome = false, Time = false, Vida = false, Corpo = false}
local ESP_Labels = {}

local function createBillboard(player, label)
    local bill = Instance.new("BillboardGui")
    bill.Name = "ESP_"..label
    bill.Adornee = player.Character and player.Character:FindFirstChild("Head")
    bill.Size = UDim2.new(0, 150, 0, 50)
    bill.AlwaysOnTop = true
    bill.ExtentsOffset = Vector3.new(0, 2.5, 0)

    local text = Instance.new("TextLabel", bill)
    text.BackgroundTransparency = 1
    text.Size = UDim2.new(1, 0, 1, 0)
    text.TextColor3 = Color3.new(1,1,1)
    text.TextStrokeTransparency = 0
    text.TextSize = 14
    text.Font = Enum.Font.GothamBold
    text.Text = label
    text.TextWrapped = true

    return bill, text
end

local function updateESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= Player and player.Character and player.Character:FindFirstChild("Head") and player.Character:FindFirstChild("Humanoid") then
            -- Nome
            if ESPenabled.Nome then
                if not player.Character.Head:FindFirstChild("ESP_Nome") then
                    local bill, text = createBillboard(player, player.Name)
                    bill.Name = "ESP_Nome"
                    bill.Parent = player.Character.Head
                end
            else
                if player.Character.Head:FindFirstChild("ESP_Nome") then
                    player.Character.Head.ESP_Nome:Destroy()
                end
            end
            -- Time
            if ESPenabled.Time then
                local teamName = player.Team and player.Team.Name or "Nenhum"
                if not player.Character.Head:FindFirstChild("ESP_Time") then
                    local bill, text = createBillboard(player, "Time: "..teamName)
                    bill.Name = "ESP_Time"
                    bill.ExtentsOffset = Vector3.new(0, 3.5, 0)
                    bill.Parent = player.Character.Head
                else
                    player.Character.Head.ESP_Time.TextLabel.Text = "Time: "..teamName
                end
            else
                if player.Character.Head:FindFirstChild("ESP_Time") then
                    player.Character.Head.ESP_Time:Destroy()
                end
            end
            -- Vida
            if ESPenabled.Vida then
                local hum = player.Character:FindFirstChild("Humanoid")
                if hum then
                    local hp = math.floor(hum.Health)
                    if not player.Character.Head:FindFirstChild("ESP_Vida") then
                        local bill, text = createBillboard(player, "HP: "..hp)
                        bill.Name = "ESP_Vida"
                        bill.ExtentsOffset = Vector3.new(0, 4.5, 0)
                        bill.Parent = player.Character.Head
                    else
                        player.Character.Head.ESP_Vida.TextLabel.Text = "HP: "..hp
                    end
                end
            else
                if player.Character.Head:FindFirstChild("ESP_Vida") then
                    player.Character.Head.ESP_Vida:Destroy()
                end
            end
            -- Corpo (highlight)
            if ESPenabled.Corpo then
                if not player.Character:FindFirstChild("HighlightESP") then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "HighlightESP"
                    highlight.FillColor = player.Team and player.Team.TeamColor.Color or Color3.new(1,1,1)
                    highlight.OutlineColor = Color3.new(0,0,0)
                    highlight.Parent = player.Character
                    highlight.Adornee = player.Character
                end
            else
                if player.Character:FindFirstChild("HighlightESP") then
                    player.Character.HighlightESP:Destroy()
                end
            end
        end
    end
end

task.spawn(function()
    while true do
        updateESP()
        task.wait(0.5)
    end
end)

-- Aba Visual
local AbaVisual = Window:CreateTab("VISUAL", 4483362458)
AbaVisual:CreateToggle({Name = "ESP NOME", Flag = "esp_nome", Callback = function(value) ESPenabled.Nome = value end})
AbaVisual:CreateToggle({Name = "ESP TIME", Flag = "esp_time", Callback = function(value) ESPenabled.Time = value end})
AbaVisual:CreateToggle({Name = "ESP VIDA", Flag = "esp_vida", Callback = function(value) ESPenabled.Vida = value end})
AbaVisual:CreateToggle({Name = "ESP CORPO", Flag = "esp_corpo", Callback = function(value) ESPenabled.Corpo = value end})

-- Aba Combate
local AbaCombate = Window:CreateTab("COMBATE", 4483362458)

-- NoClip
local noclipEnabled = false
AbaCombate:CreateToggle({
    Name = "NOCLIP",
    Flag = "noclip",
    Callback = function(state)
        noclipEnabled = state
        if noclipEnabled then
            game:GetService("RunService").Stepped:Connect(function()
                if noclipEnabled and Player.Character then
                    for _, part in pairs(Player.Character:GetChildren()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        else
            if Player.Character then
                for _, part in pairs(Player.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end
})

-- Hitbox (aumenta a Hitbox do inimigo)
local hitboxEnabled = false
local originalSizes = {}
local function setHitbox(state)
    hitboxEnabled = state
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= Player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local hrp = plr.Character.HumanoidRootPart
            if state then
                if not originalSizes[plr] then
                    originalSizes[plr] = hrp.Size
                end
                hrp.Size = Vector3.new(10,10,10)
                hrp.Transparency = 0.5
            else
                if originalSizes[plr] then
                    hrp.Size = originalSizes[plr]
                    hrp.Transparency = 0
                    originalSizes[plr] = nil
                end
            end
        end
    end
end

AbaCombate:CreateToggle({Name = "HITBOX", Flag = "hitbox", Callback = function(value) setHitbox(value) end})

-- AimBot (carregado pelo loadstring original)
AbaCombate:CreateButton({
    Name = "AIM BOT",
    Callback = function()
        loadstring(game:HttpGet("https://gist.githubusercontent.com/Aimboter477387/582af6aec49782899d5d375ab239039e/raw/51b6ddf5dc74731a24f912134061f150b6f6b316/gistfile1.txt"))()
    end
})

-- Auto Roubar
AbaCombate:CreateButton({
    Name = "AUTO ROUBAR",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/VT-bot/auto-roubar-/refs/heads/main/README.md"))()
    end
})

-- Auto CL - sair ao morrer
local autoCL = false
local function setupAutoCL()
    if Player.Character then
        local hum = Player.Character:FindFirstChild("Humanoid")
        if hum then
            hum.Died:Connect(function()
                if autoCL then
                    game:Shutdown()
                end
            end)
        end
    end
end

AbaCombate:CreateToggle({
    Name = "AUTO CL",
    Flag = "auto_cl",
    Callback = function(value)
        autoCL = value
        setupAutoCL()
    end
})

-- Aba Teleportes
local AbaTP = Window:CreateTab("TELEPORTES", 4483362458)
local function tpTo(x,y,z)
    local root = Player.Character and Player.Character:FindFirstChild("HumanoidRootPart")
    if root then
        root.CFrame = CFrame.new(x,y,z)
    end
end

AbaTP:CreateButton({Name = "Praça", Callback = function() tpTo(-289.2,4.8,331.1) end})
AbaTP:CreateButton({Name = "Garagem", Callback = function() tpTo(-464.3,4.8,307.3) end})
AbaTP:CreateButton({Name = "Mecânica", Callback = function() tpTo(-189.1,4.7,-535.9) end})
AbaTP:CreateButton({Name = "Banco", Callback = function() tpTo(-46.0,6.6,413.5) end})
AbaTP:CreateButton({Name = "Ilegal", Callback = function() tpTo(11978.6,35.5,12785.5) end})

-- Anti Staff kick e notificação
local staffTeamName = "Staff"
local checkStaff = true
task.spawn(function()
    while checkStaff do
        for _, p in pairs(game.Players:GetPlayers()) do
            if p.Team and p.Team.Name == staffTeamName then
                if p == Player then
                    Player:Kick("Você entrou no time Staff.")
                else
                    local dist = (Player.Character and p.Character and (Player.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude) or "?"
                    Rayfield:Notify({Title = "STAFF DETECTADO!", Content = "Jogador "..p.Name.." está no time STAFF.\nDistância: "..math.floor(dist), Duration = 5})
                end
            end
        end
        task.wait(1)
    end
end)

-- Início - Nome e botão Seja Bem-vindo
local AbaInicio = Window:CreateTab("INÍCIO", 4483362458)
AbaInicio:CreateParagraph({Title = "Usuário", Content = "Você é: "..Player.Name})
AbaInicio:CreateButton({Name = "SEJA BEM-VINDO", Callback = function()
    Rayfield:Notify({Title = "Olá!", Content = "Seja bem-vindo, "..Player.Name.."!", Duration = 4})
end})
