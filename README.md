-- SONIC MENU PAGO
-- Créditos: SACOLA E TIGRINHO

local SonicMenu = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local MinimizeButton = Instance.new("TextButton")
local OpenButton = Instance.new("TextButton")
local ButtonsFrame = Instance.new("Frame")
local FunctionsFrame = Instance.new("Frame")
local FunctionsTitle = Instance.new("TextLabel")

-- Configuração do painel movível
SonicMenu.Parent = game.CoreGui
MainFrame.Parent = SonicMenu
MainFrame.Size = UDim2.new(0, 300, 0, 400)
MainFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.Draggable = true
MainFrame.Active = true
MainFrame.Selectable = true

MinimizeButton.Parent = MainFrame
MinimizeButton.Size = UDim2.new(0, 50, 0, 25)
MinimizeButton.Position = UDim2.new(1, -55, 0, 5)
MinimizeButton.Text = "-"
MinimizeButton.MouseButton1Click:Connect(function()
    ButtonsFrame.Visible = not ButtonsFrame.Visible
    FunctionsFrame.Visible = not FunctionsFrame.Visible
end)

ButtonsFrame.Parent = MainFrame
ButtonsFrame.Size = UDim2.new(1, 0, 0.5, -30)
ButtonsFrame.Position = UDim2.new(0, 0, 0, 30)
ButtonsFrame.BackgroundTransparency = 1

FunctionsFrame.Parent = MainFrame
FunctionsFrame.Size = UDim2.new(1, 0, 0.5, -30)
FunctionsFrame.Position = UDim2.new(0, 0, 0.5, 0)
FunctionsFrame.BackgroundTransparency = 1

FunctionsTitle.Parent = FunctionsFrame
FunctionsTitle.Size = UDim2.new(1, 0, 0, 30)
FunctionsTitle.Position = UDim2.new(0, 0, 0, 0)
FunctionsTitle.Text = "Funções"
FunctionsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
FunctionsTitle.BackgroundTransparency = 1

-- Função para teleporte
local function teleport(x, y, z)
    local player = game.Players.LocalPlayer
    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(x, y, z)
    end
end

-- Criando botões para teleportes
local locations = {
    {"Prédio", -1888.3, 4.8, 377.2},
    {"Casa Pescaria", -1205.7, 79.1, -391.0},
    {"Gas/Lixo", -441.9, 5.3, -31.0},
    {"Fazenda", 778.8, 4.8, -101.1},
    {"PM", -839.3, 12.2, 459.0},
    {"Praça", -287.4, 4.8, 338.5},
    {"Plantas", 12022.6, 27.3, 12796.3},
    {"Lavagem", 19830.8, 66.5, 13142.8}
}

for _, loc in pairs(locations) do
    local button = Instance.new("TextButton")
    button.Text = loc[1]
    button.Size = UDim2.new(1, 0, 0, 30)
    button.Parent = ButtonsFrame
    button.MouseButton1Click:Connect(function()
        teleport(loc[2], loc[3], loc[4])
    end)
end

-- Criando botões para funções
local functions = {"Velocidade", "ESP Nome", "ESP Pessoas", "No Clip", "Hit Box", "CL", "Teleporte Player"}

for _, func in pairs(functions) do
    local button = Instance.new("TextButton")
    button.Text = func
    button.Size = UDim2.new(1, 0, 0, 30)
    button.Parent = FunctionsFrame
end

-- Funções adicionais
loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
