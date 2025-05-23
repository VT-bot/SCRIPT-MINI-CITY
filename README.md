--[[
    Sonic Menu • Feito por PIXOTE
    Abas: Início, Combate, Anti-Ban
    Adicionado: NoClip, Auto CL
--]]

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "SonicMenu"

-- Sistema de Key
local keyFrame = Instance.new("Frame", gui)
keyFrame.Size = UDim2.new(0, 300, 0, 150)
keyFrame.Position = UDim2.new(0.5, -150, 0.5, -75)
keyFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
keyFrame.BorderSizePixel = 0
keyFrame.Active = true
keyFrame.Draggable = true

local title = Instance.new("TextLabel", keyFrame)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
title.Text = "SONIC MENU - DIGITE A KEY"
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 14

local keyBox = Instance.new("TextBox", keyFrame)
keyBox.Size = UDim2.new(0.8, 0, 0, 30)
keyBox.Position = UDim2.new(0.1, 0, 0.5, -15)
keyBox.PlaceholderText = "Digite a key (123)"
keyBox.Text = ""
keyBox.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
keyBox.TextColor3 = Color3.new(1,1,1)
keyBox.Font = Enum.Font.Gotham
keyBox.TextSize = 14

local keyBtn = Instance.new("TextButton", keyFrame)
keyBtn.Size = UDim2.new(0.6, 0, 0, 30)
keyBtn.Position = UDim2.new(0.2, 0, 0.75, 0)
keyBtn.Text = "ENTRAR"
keyBtn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
keyBtn.TextColor3 = Color3.new(1, 1, 1)
keyBtn.Font = Enum.Font.GothamBold
keyBtn.TextSize = 14

keyBtn.MouseButton1Click:Connect(function()
    if keyBox.Text == "123" then
        keyFrame:Destroy()

        local menu = Instance.new("Frame", gui)
        menu.Size = UDim2.new(0, 500, 0, 320)
        menu.Position = UDim2.new(0.5, -250, 0.5, -160)
        menu.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
        menu.BorderSizePixel = 0
        menu.Active = true
        menu.Draggable = true

        local top = Instance.new("TextLabel", menu)
        top.Size = UDim2.new(1, 0, 0, 30)
        top.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
        top.Text = "SONIC MENU • Feito por PIXOTE"
        top.TextColor3 = Color3.new(1, 1, 1)
        top.Font = Enum.Font.GothamBold
        top.TextSize = 14

        local tabs = {"Início", "Combate", "Anti-Ban"}
        local tabButtons = {}
        local tabFrames = {}

        for i, tabName in ipairs(tabs) do
            local tabBtn = Instance.new("TextButton", menu)
            tabBtn.Size = UDim2.new(0, 160, 0, 25)
            tabBtn.Position = UDim2.new(0, (i-1)*165 + 5, 0, 35)
            tabBtn.Text = tabName
            tabBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            tabBtn.TextColor3 = Color3.new(1, 1, 1)
            tabBtn.Font = Enum.Font.GothamBold
            tabBtn.TextSize = 12
            tabButtons[tabName] = tabBtn

            local content = Instance.new("Frame", menu)
            content.Position = UDim2.new(0, 10, 0, 70)
            content.Size = UDim2.new(1, -20, 1, -80)
            content.BackgroundTransparency = 1
            content.Visible = (tabName == "Início")
            tabFrames[tabName] = content

            tabBtn.MouseButton1Click:Connect(function()
                for _, frame in pairs(tabFrames) do
                    frame.Visible = false
                end
                tabFrames[tabName].Visible = true
            end)
        end

        local function createButton(parent, text, callback)
            local btn = Instance.new("TextButton", parent)
            btn.Size = UDim2.new(0, 200, 0, 30)
            btn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
            btn.TextColor3 = Color3.new(1, 1, 1)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 12
            btn.Text = text
            btn.MouseButton1Click:Connect(callback)
            return btn
        end

        -- INÍCIO
        local info = Instance.new("TextLabel", tabFrames["Início"])
        info.Size = UDim2.new(1, 0, 0, 30)
        info.Text = "Usuário: " .. LocalPlayer.Name
        info.TextColor3 = Color3.new(1, 1, 1)
        info.BackgroundTransparency = 1
        info.Font = Enum.Font.Gotham
        info.TextSize = 14

        -- COMBATE
        local layoutCombat = Instance.new("UIListLayout", tabFrames["Combate"])
        layoutCombat.Padding = UDim.new(0, 5)

        createButton(tabFrames["Combate"], "Ativar Hitbox", function()
            for _, v in pairs(Players:GetPlayers()) do
                if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                    v.Character.HumanoidRootPart.Size = Vector3.new(20, 20, 20)
                    v.Character.HumanoidRootPart.Transparency = 0.5
                end
            end
        end)

createButton(tabFrames["Combate"], "ESP Inventário", function()
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("Head") then
            local billboard = Instance.new("BillboardGui", plr.Character.Head)
            billboard.Size = UDim2.new(0, 120, 0, 40)
            billboard.AlwaysOnTop = true

            local label = Instance.new("TextLabel", billboard)
            label.Size = UDim2.new(1, 0, 1, 0)
            label.BackgroundTransparency = 1
            label.TextColor3 = Color3.new(1, 1, 1)
            label.Font = Enum.Font.GothamBold
            label.TextSize = 12
            label.Text = "Carregando..."

            -- Atualizar nome do item segurado
            task.spawn(function()
                while billboard.Parent and plr.Character do
                    local tool = plr:FindFirstChildOfClass("Backpack") and plr.Backpack:FindFirstChildWhichIsA("Tool")
                    if plr.Character:FindFirstChildOfClass("Tool") then
                        label.Text = "Mão: " .. plr.Character:FindFirstChildOfClass("Tool").Name
                    elseif tool then
                        label.Text = "Mochila: " .. tool.Name
                    else
                        label.Text = "Nada equipado"
                    end
                    task.wait(1)
                end
            end)
        end
    end
end)
        createButton(tabFrames["Combate"], "ESP Nomes", function()
            for _, plr in pairs(Players:GetPlayers()) do
                if plr ~= LocalPlayer then
                    local billboard = Instance.new("BillboardGui", plr.Character:WaitForChild("Head"))
                    billboard.Size = UDim2.new(0, 100, 0, 40)
                    billboard.AlwaysOnTop = true

                    local label = Instance.new("TextLabel", billboard)
                    label.Size = UDim2.new(1, 0, 1, 0)
                    label.BackgroundTransparency = 1
                    label.TextColor3 = Color3.new(1, 1, 1)
                    label.Text = plr.Name
                    label.Font = Enum.Font.GothamBold
                    label.TextSize = 12
                end
            end
        end)

        createButton(tabFrames["Combate"], "Teleporte até jogador", function()
            local listGui = Instance.new("Frame", gui)
            listGui.Size = UDim2.new(0, 200, 0, 250)
            listGui.Position = UDim2.new(1, -210, 0.5, -125)
            listGui.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            listGui.Active = true
            listGui.Draggable = true

            local layout = Instance.new("UIListLayout", listGui)
            layout.Padding = UDim.new(0, 4)

            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer then
                    local btn = Instance.new("TextButton", listGui)
                    btn.Size = UDim2.new(1, 0, 0, 30)
                    btn.Text = p.Name
                    btn.TextColor3 = Color3.new(1,1,1)
                    btn.BackgroundColor3 = Color3.fromRGB(60,60,60)
                    btn.Font = Enum.Font.Gotham
                    btn.TextSize = 12
                    btn.MouseButton1Click:Connect(function()
                        if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                            LocalPlayer.Character:MoveTo(p.Character.HumanoidRootPart.Position + Vector3.new(2, 0, 2))
                        end
                    end)
                end
            end
        end)

        createButton(tabFrames["Combate"], "Ativar NoClip", function()
            local noclip = true
            RunService.Stepped:Connect(function()
                if noclip and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
                        if v:IsA("BasePart") and v.CanCollide then
                            v.CanCollide = false
                        end
                    end
                end
            end)
        end)

        createButton(tabFrames["Combate"], "Ativar Auto CL", function()
            if LocalPlayer.Character:FindFirstChild("Humanoid") then
                LocalPlayer.Character.Humanoid.Died:Connect(function()
                    LocalPlayer:Kick("Você morreu (Auto CL Ativado).")
                end)
            end
        end)

        -- ANTI-BAN
        local layoutProtect = Instance.new("UIListLayout", tabFrames["Anti-Ban"])
        layoutProtect.Padding = UDim.new(0, 5)

        createButton(tabFrames["Anti-Ban"], "Notificar se Staff entrar", function()
            Players.PlayerAdded:Connect(function(player)
                if string.find(player.Name:lower(), "staff") or string.find(player:GetRoleInGroup(1):lower(), "admin") then
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "Alerta Staff!",
                        Text = player.Name.." entrou!",
                        Duration = 5
                    })
                end
            end)
        end)

        createButton(tabFrames["Anti-Ban"], "Detectar Staff Próximo", function()
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and (string.find(p.Name:lower(), "staff") or string.find(p:GetRoleInGroup(1):lower(), "admin")) then
                    if p.Character and LocalPlayer.Character then
                        local dist = (p.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                        game.StarterGui:SetCore("SendNotification", {
                            Title = "Staff por perto!",
                            Text = "Distância: " .. math.floor(dist),
                            Duration = 5
                        })
                    end
                end
            end
        end)
    end
end)
