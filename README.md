-- Biblioteca Rayfield
loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local keyCorreta = "1234"
local player = game.Players.LocalPlayer

-- Anti-Ban & Anti-STAFF
spawn(function()
    while true do
        for _, plr in pairs(game.Players:GetPlayers()) do
            if plr.Team and plr.Team.Name:lower():match("staff") then
                game:Shutdown()
            end
        end
        if player.Team and player.Team.Name:lower():match("staff") then
            game:Shutdown()
        end
        task.wait(2)
    end
end)

-- Janela principal
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local Window = Rayfield:CreateWindow({
   Name = "MIX MENU | Feito por PIXOTE",
   LoadingTitle = "MIX MENU",
   LoadingSubtitle = "Feito por PIXOTE",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "MixMenuPixote"
   },
   KeySystem = true,
   KeySettings = {
      Title = "MIX MENU | SENHA",
      Subtitle = "Insira a Key para continuar",
      Note = "Key: 1234",
      FileName = "MixMenuKey",
      SaveKey = true,
      GrabKeyFromSite = false,
      Key = keyCorreta
   }
})

-- ABA INÍCIO
local abaInicio = Window:CreateTab("Início", nil)
abaInicio:CreateParagraph({Title = "Seja Bem Vindo!", Content = "Usuário: "..player.Name})
abaInicio:CreateLabel("Sistema Anti Ban e Anti Chest Ativado")

-- ABA COMBATE
local abaCombate = Window:CreateTab("Combate", nil)

abaCombate:CreateButton({
    Name = "Hitbox",
    Callback = function()
        for _,v in pairs(game:GetService("Players"):GetPlayers()) do
            if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                v.Character.HumanoidRootPart.Size = Vector3.new(10,10,10)
                v.Character.HumanoidRootPart.Transparency = 0.7
                v.Character.HumanoidRootPart.BrickColor = BrickColor.new("Bright red")
                v.Character.HumanoidRootPart.Material = Enum.Material.Neon
            end
        end
    end
})

abaCombate:CreateToggle({
    Name = "NoClip",
    CurrentValue = false,
    Callback = function(state)
        if state then
            noclip = true
            game:GetService("RunService").Stepped:Connect(function()
                if noclip and player.Character then
                    for _, v in pairs(player.Character:GetDescendants()) do
                        if v:IsA("BasePart") and v.CanCollide then
                            v.CanCollide = false
                        end
                    end
                end
            end)
        else
            noclip = false
        end
    end
})

abaCombate:CreateButton({
    Name = "ESP NOME (cor do time)",
    Callback = function()
        for _, v in pairs(game.Players:GetPlayers()) do
            if v ~= player and v.Character and v.Character:FindFirstChild("Head") then
                local esp = Instance.new("BillboardGui", v.Character.Head)
                esp.Name = "TeamESP"
                esp.Size = UDim2.new(0, 200, 0, 50)
                esp.StudsOffset = Vector3.new(0, 3, 0)
                esp.AlwaysOnTop = true

                local nameLabel = Instance.new("TextLabel", esp)
                nameLabel.Size = UDim2.new(1, 0, 1, 0)
                nameLabel.BackgroundTransparency = 1
                nameLabel.Text = v.Name
                nameLabel.TextColor3 = v.Team and v.Team.TeamColor.Color or Color3.new(1,1,1)
                nameLabel.TextStrokeTransparency = 0.5
                nameLabel.TextScaled = true
                nameLabel.Font = Enum.Font.SourceSansBold
            end
        end
    end
})

abaCombate:CreateButton({
    Name = "AUTO CL (sair ao morrer)",
    Callback = function()
        local function monitorarVida()
            player.CharacterAdded:Connect(function(char)
                local humanoid = char:WaitForChild("Humanoid", 10)
                if humanoid then
                    humanoid.Died:Connect(function()
                        game:Shutdown()
                    end)
                end
            end)
            if player.Character then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.Died:Connect(function()
                        game:Shutdown()
                    end)
                end
            end
        end
        monitorarVida()
    end
})

abaCombate:CreateButton({
    Name = "AIM BOT",
    Callback = function()
        loadstring(game:HttpGet("https://gist.githubusercontent.com/Aimboter477387/582af6aec49782899d5d375ab239039e/raw/51b6ddf5dc74731a24f912134061f150b6f6b316/gistfile1.txt"))()
    end
})

abaCombate:CreateButton({
    Name = "AUTO ROUBAR",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/VT-bot/auto-roubar-/refs/heads/main/README.md"))()
    end
})

-- ABA TELEPORTES
local abaTP = Window:CreateTab("Teleportes", nil)

local locais = {
    ["Praça"] = Vector3.new(-289.2, 4.8, 331.1),
    ["Garagem"] = Vector3.new(-464.3, 4.8, 307.3),
    ["Mecânica"] = Vector3.new(-189.1, 4.7, -535.9),
    ["Banco"] = Vector3.new(-46.0, 6.6, 413.5),
    ["Ilegal"] = Vector3.new(11978.6, 35.5, 12785.5),
}

for nome, pos in pairs(locais) do
    abaTP:CreateButton({
        Name = nome,
        Callback = function()
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = CFrame.new(pos)
            end
        end
    })
end
