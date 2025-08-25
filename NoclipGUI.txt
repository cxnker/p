-- Noclip GUI
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Rimuove GUI esistente
pcall(function()
    game.CoreGui.NoclipGui:Destroy()
end)

-- Crea GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "NoclipGui"
gui.ResetOnSpawn = false

-- Frame principale
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 200, 0, 100)
frame.Position = UDim2.new(0, 20, 0, 20)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BackgroundTransparency = 0.3
frame.BorderSizePixel = 0
frame.Active = true -- necessario per il drag
frame.Draggable = false -- lo gestiamo manualmente per mobile

-- Etichetta "by aminos29T"
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "by Roun95"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.BackgroundTransparency = 1
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20

-- Etichetta "noclip"
local label = Instance.new("TextLabel", frame)
label.Size = UDim2.new(1, 0, 0, 20)
label.Position = UDim2.new(0, 0, 0, 30)
label.Text = "noclip"
label.TextColor3 = Color3.fromRGB(200, 200, 200)
label.BackgroundTransparency = 1
label.Font = Enum.Font.SourceSans
label.TextSize = 18

-- Pulsante toggle noclip
local button = Instance.new("TextButton", frame)
button.Size = UDim2.new(0.8, 0, 0, 30)
button.Position = UDim2.new(0.1, 0, 0, 60)
button.Text = "Attiva Noclip"
button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.SourceSans
button.TextSize = 16

-- Noclip logic
local noclip = false
local RunService = game:GetService("RunService")

local function toggleNoclip()
	noclip = not noclip
	button.Text = noclip and "Disattiva Noclip" or "Attiva Noclip"

	if noclip then
		connection = RunService.Stepped:Connect(function()
			for _, part in pairs(player.Character:GetDescendants()) do
				if part:IsA("BasePart") and part.CanCollide then
					part.CanCollide = false
				end
			end
		end)
	else
		if connection then connection:Disconnect() end
		for _, part in pairs(player.Character:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = true
			end
		end
	end
end

button.MouseButton1Click:Connect(toggleNoclip)

-- 💡 Mobile/PC Drag System
local UIS = game:GetService("UserInputService")
local dragging, dragInput, dragStart, startPos

frame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = frame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

frame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UIS.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		local delta = input.Position - dragStart
		frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)