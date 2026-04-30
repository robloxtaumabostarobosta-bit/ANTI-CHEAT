local SINTONIA_ID = 115054138215106 -- ID do Sintonia RP (Verifique se é este o ID atual do mapa)
if game.PlaceId ~= SINTONIA_ID then 
    warn("Acesso Negado: Este script e exclusivo para o Sintonia RP.")
    return 
end

-- [[ SERVIÇOS ]]
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")

-- [[ PROTEÇÃO CONTRA VARREDURA ]]
local function RandomName()
    local chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local name = ""
    for i = 1, 15 do
        local r = math.random(1, #chars)
        name = name .. string.sub(chars, r, r)
    end
    return name
end

-- [[ INTERFACE PRINCIPAL ]]
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = RandomName()
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Name = RandomName()
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 22) -- Fundo Escuro
MainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
MainFrame.Size = UDim2.new(0, 400, 0, 320)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true

-- Borda Roxa Neon
local Stroke = Instance.new("UIStroke")
Stroke.Color = Color3.fromRGB(138, 43, 226) -- Roxo
Stroke.Thickness = 2
Stroke.Parent = MainFrame

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 10)
Corner.Parent = MainFrame

-- [[ BARRA DE TÍTULO & CONTROLES ]]
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "YORGUTE FROST HUB SCRIPT"
Title.TextColor3 = Color3.fromRGB(200, 200, 200)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.Parent = MainFrame

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.Parent = MainFrame
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(1, 0)

local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -70, 0, 5)
MinBtn.Text = "-"
MinBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.Parent = MainFrame
Instance.new("UICorner", MinBtn).CornerRadius = UDim.new(1, 0)

-- [[ SISTEMA DE ABAS ]]
local TabContainer = Instance.new("Frame")
TabContainer.Size = UDim2.new(1, -20, 0, 35)
TabContainer.Position = UDim2.new(0, 10, 0, 45)
TabContainer.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
TabContainer.Parent = MainFrame
Instance.new("UICorner", TabContainer)

local VisualTab = Instance.new("TextButton")
VisualTab.Size = UDim2.new(0.33, 0, 1, 0)
VisualTab.Text = "Visual"
VisualTab.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
VisualTab.TextColor3 = Color3.fromRGB(255, 255, 255)
VisualTab.Parent = TabContainer
Instance.new("UICorner", VisualTab)

-- [[ BOTÕES DE FUNÇÕES (ESTILO IMAGEM) ]]
local ButtonList = Instance.new("ScrollingFrame")
ButtonList.Size = UDim2.new(1, -20, 1, -100)
ButtonList.Position = UDim2.new(0, 10, 0, 90)
ButtonList.BackgroundTransparency = 1
ButtonList.ScrollBarThickness = 0
ButtonList.Parent = MainFrame

local Layout = Instance.new("UIListLayout")
Layout.Parent = ButtonList
Layout.Padding = UDim.new(0, 10)
Layout.HorizontalAlignment = Enum.HorizontalAlignment.Center

local function CreateFunction(text)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(0, 360, 0, 45)
    b.BackgroundColor3 = Color3.fromRGB(100, 30, 200) -- Roxo Escuro
    b.Text = text
    b.Font = Enum.Font.GothamSemibold
    b.TextSize = 14
    b.TextColor3 = Color3.fromRGB(255, 255, 255)
    b.Parent = ButtonList
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
    
    -- Efeito de Clique
    b.MouseButton1Click:Connect(function()
        print("Ativado: " .. text)
    end)
end

CreateFunction("ESP Player")
CreateFunction("ESP Name")
CreateFunction("ESP Caixa")
CreateFunction("ESP Linha")

-- [[ LÓGICA DE MINIMIZAR ]]
local isMinimized = false
MinBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    local targetSize = isMinimized and UDim2.new(0, 400, 0, 40) or UDim2.new(0, 400, 0, 320)
    TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quart), {Size = targetSize}):Play()
end)

-- [[ LÓGICA DE FECHAR ]]
CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- [[ LÓGICA DE ARRASTAR (DRAG) ]]
local dragToggle, dragStart, startPos
MainFrame.InputBegan:Connect(function(input)
    if (input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch) then
        dragToggle = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if dragToggle and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragToggle = false
    end
end)

print("Yorgute Frost Hub carregado com sucesso!")
