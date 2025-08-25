-- Script do Menu com botão Oi
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MenuScript"
screenGui.Parent = playerGui

--// Botão Ícone (abrir/fechar)
local iconButton = Instance.new("TextButton")
iconButton.Name = "IconButton"
iconButton.Size = UDim2.new(0, 40, 0, 40) -- quadrado pequeno
iconButton.Position = UDim2.new(0, 10, 0, 10)
iconButton.Text = "≡" -- símbolo de menu
iconButton.Parent = screenGui

local iconCorner = Instance.new("UICorner")
iconCorner.CornerRadius = UDim.new(0, 6)
iconCorner.Parent = iconButton

--// Frame do Menu
local menuFrame = Instance.new("Frame")
menuFrame.Name = "MenuFrame"
menuFrame.Size = UDim2.new(0, 200, 0, 150)
menuFrame.Position = UDim2.new(0, 60, 0, 10)
menuFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
menuFrame.Visible = false
menuFrame.Parent = screenGui

local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0, 8)
frameCorner.Parent = menuFrame

--// Botão dentro do menu
local oiButton = Instance.new("TextButton")
oiButton.Name = "OiButton"
oiButton.Size = UDim2.new(0, 100, 0, 40)
oiButton.Position = UDim2.new(0.5, -50, 0.5, -20)
oiButton.Text = "Oi"
oiButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
oiButton.TextColor3 = Color3.fromRGB(255, 255, 255)
oiButton.Parent = menuFrame

local oiCorner = Instance.new("UICorner")
oiCorner.CornerRadius = UDim.new(0, 6)
oiCorner.Parent = oiButton

--// Função abrir/fechar menu
local aberto = false
iconButton.MouseButton1Click:Connect(function()
	aberto = not aberto
	menuFrame.Visible = aberto
end)

--// Função do botão Oi
oiButton.MouseButton1Click:Connect(function()
	print("Você clicou em Oi!")
end)
