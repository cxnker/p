-- Create a GUI with a rounded frame, text box, button, close button, draggable functionality, and a black transparent background

local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Create the main frame with rounded corners and transparent background
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 200)
frame.Position = UDim2.new(0.5, -150, 0.5, -100)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- black background
frame.BackgroundTransparency = 0.4 -- 40% transparency
frame.BorderSizePixel = 0 -- No border
frame.ClipsDescendants = true -- Allow clipping for rounded corners
frame.Parent = screenGui

-- Rounded corners for the frame
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12) -- Set the corner radius for rounded effect
corner.Parent = frame

-- Create the text box with a black background and rounded corners
local textBox = Instance.new("TextBox")
textBox.Size = UDim2.new(0, 250, 0, 50)
textBox.Position = UDim2.new(0, 25, 0, 25)
textBox.PlaceholderText = "Enter Key"
textBox.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- black background for the text box
textBox.BackgroundTransparency = 0.3 -- Slight transparency for the text box
textBox.TextColor3 = Color3.fromRGB(255, 255, 255) -- White text
textBox.TextSize = 18
textBox.Font = Enum.Font.SourceSans
textBox.Parent = frame

-- Rounded corners for the text box
local textBoxCorner = Instance.new("UICorner")
textBoxCorner.CornerRadius = UDim.new(0, 12) -- Rounded edges for the text box
textBoxCorner.Parent = textBox

-- Create the submit button with rounded corners
local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 250, 0, 50)
button.Position = UDim2.new(0, 25, 0, 100)
button.Text = "Enter"
button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
button.BackgroundTransparency = 0.4
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.Font = Enum.Font.SourceSans
button.TextSize = 24
button.Parent = frame

-- Rounded corners for the submit button
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 12) -- Rounded edges for the submit button
buttonCorner.Parent = button

-- Create the close button (X) at the top-right (smaller size)
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 16, 0, 16) -- Smaller close button size
closeButton.Position = UDim2.new(1, -25, 0, 174) -- Adjusted position to keep it within bounds
closeButton.Text = "X"
closeButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
closeButton.BackgroundTransparency = 0.3
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.Font = Enum.Font.SourceSans
closeButton.TextSize = 14
closeButton.Parent = frame

-- Function to handle button click
button.MouseButton1Click:Connect(function()
    if textBox.Text == "ILoveMen:3" then
       loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/Dex%20Explorer.txt"))()
        -- Execute your code here, replace with your own Roblox-friendly code
    end
end)

-- Close the GUI when the close button is clicked
closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy() -- Destroys the GUI when "X" is clicked
end)

-- Draggable GUI functionality
local dragging = false
local dragInput, mousePos, framePos

frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        mousePos = input.Position
        framePos = frame.Position
    end
end)

frame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - mousePos
        frame.Position = UDim2.new(framePos.X.Scale, framePos.X.Offset + delta.X, framePos.Y.Scale, framePos.Y.Offset + delta.Y)
    end
end)

frame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
