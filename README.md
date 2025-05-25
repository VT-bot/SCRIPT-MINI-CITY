-- Carregar Rayfield UI
local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

-- Sistema de Key
local MenuLiberado = false
local KeyCorreta = "157"

local KeyGUI = Rayfield:CreateWindow({
    Name = "MENU HACK - Sistema de Key",
    LoadingTitle = "Verificando Key...",
    ConfigurationSaving = { Enabled = false }
})

local KeyTab = KeyGUI:CreateTab("Key", 4483362458)

KeyTab:CreateInput({
    Name = "Digite a senha para acessar o menu",
    PlaceholderText = "Senha",
    RemoveTextAfterFocusLost = false,
    Callback = function(Value)
        if Value == KeyCorreta then
            MenuLiberado = true
            Rayfield:Notify({
                Title = "Sucesso!",
                Content = "Key correta! Abrindo o menu...",
                Duration = 3
            })
            wait(1)
            KeyGUI:Destroy()
        else
            Rayfield:Notify({
                Title = "Erro!",
                Content = "Key incorreta.",
                Duration = 3
            })
        end
    end
})

repeat wait() until MenuLiberado

-- Menu Principal
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function sairSeForStaff()
    if LocalPlayer.Team and tostring(LocalPlayer.Team):lower():find("staff") then
        game:Shutdown()
    end
end

LocalPlayer:GetPropertyChangedSignal("Team"):Connect(sairSeForStaff)
for _, plr in pairs(Players:GetPlayers()) do
    plr:GetPropertyChangedSignal("Team"):Connect(sairSeForStaff)
end

-- Criar Janela Principal
local Janela = Rayfield:CreateWindow({
    Name = "MENU HACK - Feito por PIXOTE",
    LoadingTitle = "Carregando...",
    LoadingSubtitle = "Bem-vindo!",
    ConfigurationSaving = { Enabled = false }
})

-- Aba Início
local Inicio = Janela:CreateTab("Início", 4483362458)
Inicio:CreateParagraph({Title = "Bem-vindo, "..LocalPlayer.Name, Content = "SEJA BEM VINDO!"})

-- Aba Combate
local Combate = Janela:CreateTab("Combate", 4483362458)

Combate:CreateToggle({
    Name = "Hitbox (nos outros jogadores)",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    for _, part in ipairs(p.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.Size = Vector3.new(10, 10, 10)
                            part.Transparency = 0.7
                            part.BrickColor = BrickColor.new("Bright red")
                            part.Material = Enum.Material.Neon
                        end
                    end
                end
            end
        else
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    for _, part in ipairs(p.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.Size = Vector3.new(2, 2, 1)
                            part.Transparency = 0
                            part.BrickColor = BrickColor.new("Medium stone grey")
                            part.Material = Enum.Material.Plastic
                        end
                    end
                end
            end
        end
    end
})

Combate:CreateToggle({
    Name = "NoClip",
    CurrentValue = false,
    Callback = function(Value)
        game:GetService("RunService").Stepped:Connect(function()
            if Value and LocalPlayer.Character then
                for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                    if v:IsA("BasePart") then
                        v.CanCollide = false
                    end
                end
            end
        end)
    end
})

Combate:CreateButton({
    Name = "ESP NOME (cor do time)",
    Callback = function()
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
                local esp = Instance.new("BillboardGui", p.Character.Head)
                esp.Size = UDim2.new(0, 100, 0, 20)
                esp.AlwaysOnTop = true
                esp.StudsOffset = Vector3.new(0, 3, 0)

                local name = Instance.new("TextLabel", esp)
                name.Size = UDim2.new(1, 0, 1, 0)
                name.BackgroundTransparency = 1
                name.TextScaled = true
                name.TextColor3 = p.Team and p.Team.TeamColor.Color or Color3.fromRGB(255, 255, 255)
                name.Text = p.Name
                name.Font = Enum.Font.SourceSans
                name.TextSize = 10
            end
        end
    end
})

Combate:CreateButton({
    Name = "Aimbot",
    Callback = function()
        loadstring(game:HttpGet("https://gist.githubusercontent.com/Aimboter477387/582af6aec49782899d5d375ab239039e/raw/51b6ddf5dc74731a24f912134061f150b6f6b316/gistfile1.txt"))()
    end
})

Combate:CreateButton({
    Name = "Auto Roubar",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/VT-bot/auto-roubar-/refs/heads/main/README.md"))()
    end
})

Combate:CreateToggle({
    Name = "Auto CL (sair ao morrer)",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            LocalPlayer.Character:WaitForChild("Humanoid").Died:Connect(function()
                game:Shutdown()
            end)
        end
    end
})

-- Aba Teleportes
local Teleportes = Janela:CreateTab("Teleportes", 4483362458)
local function tp(pos)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character:MoveTo(pos)
    end
end

Teleportes:CreateButton({Name = "Praça", Callback = function() tp(Vector3.new(-289.2, 4.8, 331.1)) end})
Teleportes:CreateButton({Name = "Garagem", Callback = function() tp(Vector3.new(-464.3, 4.8, 307.3)) end})
Teleportes:CreateButton({Name = "Mecânica", Callback = function() tp(Vector3.new(-189.1, 4.7, -535.9)) end})
Teleportes:CreateButton({Name = "Banco", Callback = function() tp(Vector3.new(-46.0, 6.6, 413.5)) end})
Teleportes:CreateButton({Name = "Ilegal", Callback = function() tp(Vector3.new(11978.6, 35.5, 12785.5)) end})

-- Aba Outros
local Outros = Janela:CreateTab("Outros", 4483362458)
local playerList = {}
for _, p in ipairs(Players:GetPlayers()) do
    if p ~= LocalPlayer then
        table.insert(playerList, p.Name)
    end
end

local jogadorSelecionado = nil

local dropdown = Outros:CreateDropdown({
    Name = "Selecionar Jogador para Teleporte",
    Options = playerList,
    CurrentOption = "",
    Callback = function(Value)
        jogadorSelecionado = Value
    end
})

Outros:CreateButton({
    Name = "Teleportar até Jogador Selecionado",
    Callback = function()
        if jogadorSelecionado then
            local p = Players:FindFirstChild(jogadorSelecionado)
            if p and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                tp(p.Character.HumanoidRootPart.Position)
                Rayfield:Notify({
                    Title = "Teleporte realizado!",
                    Content = "Você foi teleportado até " .. jogadorSelecionado,
                    Duration = 3
                })
            else
                Rayfield:Notify({
                    Title = "Erro!",
                    Content = "Jogador inválido ou personagem não carregado.",
                    Duration = 3
                })
            end
        else
            Rayfield:Notify({
                Title = "Aviso",
                Content = "Selecione um jogador antes de teleportar.",
                Duration = 3
            })
        end
    end
})

Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        table.insert(playerList, player.Name)
        dropdown:SetOptions(playerList)
    end
end)

Players.PlayerRemoving:Connect(function(player)
    for i, name in ipairs(playerList) do
        if name == player.Name then
            table.remove(playerList, i)
            break
        end
    end
    dropdown:SetOptions(playerList)
end)
