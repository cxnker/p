local musicIDs = {
    ["1"] = 89907278904871,
    ["2"] = 92209428926055,
    ["3"] = 133900561957103,
    ["4"] = 93768636184697,
    ["5"] = 92062588329352,
    ["6"] = 122966944593870,
    ["7"] = 87783857221289,
    ["8"] = 80164463388144,
}

local musicNames = {
    ["1"] = "Funk da Praia",
    ["2"] = "Switch The Colors (Jersey Club)",
    ["3"] = "Trash Funk",
    ["4"] = "2609 (Jersey Club)",
    ["5"] = "Spooky Scary Sunday (Jersey Club)",
    ["6"] = "Dark Piano",
    ["7"] = "Temptation",
    ["8"] = "One Two Step (Jersey Club)",
}

local player = game:GetService("Players").LocalPlayer
local MarketplaceService = game:GetService("MarketplaceService")
local tweenService = game:GetService("TweenService")
local gui = Instance.new("ScreenGui", game:GetService("CoreGui"))
gui.Name = "FreemanMusicUI"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local function destroyAllNotificationBlocks()
    for _, guiObj in ipairs(game:GetService("CoreGui"):GetChildren()) do
        if guiObj:IsA("ScreenGui") or guiObj:IsA("Frame") then
            for _, frame in ipairs(guiObj:GetDescendants()) do
                if frame.Name == "block" or (frame:IsA("Frame") and frame.ZIndex == 10000) then
                    pcall(function() frame:Destroy() end)
                end
            end
        end
    end
end

local function showSelectorPopup(titleText, options, callback)
    if gui:FindFirstChild("SelectorPopup") then gui.SelectorPopup:Destroy() end
    if gui:FindFirstChild("SelectorPopupBlock") then gui.SelectorPopupBlock:Destroy() end

    local block = Instance.new("Frame", gui)
    block.Name = "SelectorPopupBlock"
    block.Size = UDim2.new(1,0,1,0)
    block.BackgroundTransparency = 0.35
    block.BackgroundColor3 = Color3.fromRGB(0,0,0)
    block.ZIndex = 19999
    block.Active = true

    local popup = Instance.new("Frame", gui)
    popup.Name = "SelectorPopup"
    popup.Size = UDim2.new(0, 330, 0, 130)
    popup.Position = UDim2.new(0.5, -165, 0.5, -65)
    popup.BackgroundColor3 = Color3.fromRGB(25,25,25)
    popup.BorderSizePixel = 0
    popup.ZIndex = 20000
    popup.Active = true
    Instance.new("UICorner", popup).CornerRadius = UDim.new(0, 14)

    local title = Instance.new("TextLabel", popup)
    title.Size = UDim2.new(1, -16, 0, 32)
    title.Position = UDim2.new(0,8,0,7)
    title.BackgroundTransparency = 1
    title.Text = titleText
    title.TextColor3 = Color3.fromRGB(255,255,255)
    title.TextSize = 16
    title.Font = Enum.Font.GothamBold
    title.ZIndex = 20001

    local btnCount = #options
    local btnW = math.floor((298-(btnCount-1)*7)/btnCount)
    for i, opt in ipairs(options) do
        local btn = Instance.new("TextButton", popup)
        btn.Size = UDim2.new(0, btnW, 0, 38)
        btn.Position = UDim2.new(0, 16+((btnW+7)*(i-1)), 0, 50)
        btn.Text = tostring(opt)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 16
        btn.TextColor3 = Color3.fromRGB(255,255,255)
        btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
        btn.ZIndex = 20001
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
        btn.MouseButton1Click:Connect(function()
            popup:Destroy()
            block:Destroy()
            if callback then callback(opt) end
        end)
    end
end

-- Funções para tocar músicas em diferentes veículos/modos
local function playMotorcycleMusic(id)
    local args = { [1] = "VehicleMusicPlay", [2] = id }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sCa1r"):FireServer(unpack(args))
end

local function playScooterMusic(id)
    local args = { [1] = "PickingScooterMusicText", [2] = id }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
end

local function playVehicleMusic(id)
    local args = { [1] = "PickingCarMusicText", [2] = id }
    game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sCa1r"):FireServer(unpack(args))
end

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 340, 0, 360)
frame.Position = UDim2.new(1, -355, 0.5, -180)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)
frame.BackgroundTransparency = 0

local header = Instance.new("Frame", frame)
header.Size = UDim2.new(1, 0, 0, 35)
header.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
header.BorderSizePixel = 0
Instance.new("UICorner", header).CornerRadius = UDim.new(0, 12)

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -110, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Freeman HUB 🎵 - Brookhaven"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.FredokaOne
title.TextSize = 18
title.TextXAlignment = Enum.TextXAlignment.Left

local minimize = Instance.new("TextButton", header)
minimize.Size = UDim2.new(0, 35, 1, 0)
minimize.Position = UDim2.new(1, -70, 0, 0)
minimize.Text = "-"
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 18
minimize.TextColor3 = Color3.fromRGB(255, 255, 255)
minimize.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
minimize.BorderSizePixel = 0
Instance.new("UICorner", minimize).CornerRadius = UDim.new(0, 12)

local close = Instance.new("TextButton", header)
close.Size = UDim2.new(0, 35, 1, 0)
close.Position = UDim2.new(1, -35, 0, 0)
close.Text = "X"
close.Font = Enum.Font.GothamBold
close.TextSize = 18
close.TextColor3 = Color3.fromRGB(255, 255, 255)
close.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
close.BorderSizePixel = 0
Instance.new("UICorner", close).CornerRadius = UDim.new(0, 12)

local sideBar = Instance.new("Frame", frame)
sideBar.Size = UDim2.new(0, 44, 1, -35)
sideBar.Position = UDim2.new(1, -44, 0, 35)
sideBar.BackgroundTransparency = 1

local iconBtnY = 8

local creditsButton = (function()
    local btn = Instance.new("TextButton", sideBar)
    btn.Size = UDim2.new(1, -8, 0, 36)
    btn.Position = UDim2.new(0, 4, 0, iconBtnY)
    btn.Text = "👤"
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 24
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)
    btn.BorderSizePixel = 0
    return btn
end)()

local openIcon = Instance.new("TextButton", gui)
openIcon.Size = UDim2.new(0, 40, 0, 40)
openIcon.Position = UDim2.new(1, -50, 1, -50)
openIcon.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
openIcon.Text = "+"
openIcon.Visible = false
openIcon.TextSize = 24
openIcon.Font = Enum.Font.GothamBold
openIcon.TextColor3 = Color3.fromRGB(255, 255, 255)
Instance.new("UICorner", openIcon).CornerRadius = UDim.new(1, 0)
openIcon.Active = true
openIcon.Draggable = true

local mainFrame = Instance.new("ScrollingFrame", frame)
mainFrame.Position = UDim2.new(0, 0, 0, 35)
mainFrame.Size = UDim2.new(1, -44, 1, -110)
mainFrame.BackgroundTransparency = 1
mainFrame.CanvasSize = UDim2.new(0,0,0,0)
mainFrame.ScrollBarThickness = 6
mainFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y

local grid = Instance.new("UIGridLayout", mainFrame)
grid.CellSize = UDim2.new(0, 100, 0, 40)
grid.CellPadding = UDim2.new(0, 10, 0, 10)
grid.HorizontalAlignment = Enum.HorizontalAlignment.Center
grid.VerticalAlignment = Enum.VerticalAlignment.Top
grid.FillDirectionMaxCells = 2

local creditsFrame = Instance.new("Frame", frame)
creditsFrame.Position = UDim2.new(0, 0, 0, 35)
creditsFrame.Size = UDim2.new(1, -44, 1, -110)
creditsFrame.BackgroundTransparency = 1
creditsFrame.Visible = false

local creditsLabel = Instance.new("TextLabel", creditsFrame)
creditsLabel.Size = UDim2.new(1, -20, 1, -20)
creditsLabel.Position = UDim2.new(0, 10, 0, 10)
creditsLabel.Text = "Made by Freeman4i37!"
creditsLabel.Font = Enum.Font.Gotham
creditsLabel.TextColor3 = Color3.fromRGB(255,255,255)
creditsLabel.TextSize = 14
creditsLabel.TextWrapped = true
creditsLabel.TextYAlignment = Enum.TextYAlignment.Top
creditsLabel.BackgroundTransparency = 1

local settingsFrame = Instance.new("Frame", frame)
settingsFrame.Position = UDim2.new(0, 0, 0, 35)
settingsFrame.Size = UDim2.new(1, -44, 1, -110)
settingsFrame.BackgroundTransparency = 1
settingsFrame.Visible = false

local buttons = {}
for _, name in ipairs({"1", "2", "3", "4", "5", "6", "7", "8"}) do
    local id = musicIDs[name]
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 100, 0, 40)
    btn.Text = name
    btn.Font = Enum.Font.GothamBold
    btn.TextScaled = true
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)
    btn.Parent = mainFrame
    btn.MouseButton1Click:Connect(function()
        showSelectorPopup("Choose the type:", {"Vehicle", "Scooter", "Motorcycle"}, function(selected)
            if selected == "Vehicle" then
                playMotorcycleMusic(id)
            elseif selected == "Scooter" then
                playScooterMusic(id)
            elseif selected == "Motorcycle" then
                playVehicleMusic(id)
            end
        end)
    end)
    table.insert(buttons, btn)
end

local inputBox = Instance.new("TextBox", frame)
inputBox.PlaceholderText = "Audio ID here..."
inputBox.Size = UDim2.new(0.6, -10, 0, 35)
inputBox.Position = UDim2.new(0, 10, 1, -70)
inputBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
inputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
inputBox.PlaceholderColor3 = Color3.fromRGB(200, 200, 200)
inputBox.Font = Enum.Font.Gotham
inputBox.TextSize = 16
inputBox.Text = ""
inputBox.ClearTextOnFocus = false
Instance.new("UICorner", inputBox).CornerRadius = UDim.new(0, 10)

local playButton = Instance.new("TextButton", frame)
playButton.Text = "PLAY"
playButton.Size = UDim2.new(0.4, -10, 0, 35)
playButton.Position = UDim2.new(0.6, 0, 1, -70)
playButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
playButton.TextColor3 = Color3.fromRGB(255, 255, 255)
playButton.Font = Enum.Font.GothamBold
playButton.TextSize = 18
Instance.new("UICorner", playButton).CornerRadius = UDim.new(0, 10)

playButton.MouseButton1Click:Connect(function()
    local input = inputBox.Text:gsub("rbxassetid://", "")
    local id = tonumber(input)
    local foundName = nil
    for num, audioId in pairs(musicIDs) do
        if audioId == id then
            foundName = musicNames[num]
            break
        end
    end
    if id then
        local nameGot = foundName or ("Audio " .. id)
        local success, info = pcall(function()
            return MarketplaceService:GetProductInfo(id)
        end)
        if success and info and info.Name and not foundName then
            nameGot = info.Name
        end
        showSelectorPopup("Choose the type:", {"Vehicle", "Scooter", "Motorcycle"}, function(selected)
            if selected == "Vehicle" then
                playMotorcycleMusic(id)
            elseif selected == "Scooter" then
                playScooterMusic(id)
            elseif selected == "Motorcycle" then
                playVehicleMusic(id)
            end
        end)
        showAchievementBar("Now Playing: " .. nameGot, 6)
    else
        warn("INVALID ID")
    end
end)

local stopButton = Instance.new("TextButton", frame)
stopButton.Text = "Stop"
stopButton.Size = UDim2.new(0, 70, 0, 25)
stopButton.Position = UDim2.new(0, 10, 1, -35)
stopButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
stopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton.Font = Enum.Font.GothamBold
stopButton.TextSize = 12
Instance.new("UICorner", stopButton).CornerRadius = UDim.new(0, 10)
stopButton.Visible = false

local inCredits = false
creditsButton.MouseButton1Click:Connect(function()
    inCredits = not inCredits
    mainFrame.Visible = not inCredits
    creditsFrame.Visible = inCredits
    settingsFrame.Visible = false
end)

minimize.MouseButton1Click:Connect(function()
    local tween = tweenService:Create(frame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundTransparency = 1, Position = UDim2.new(1, -355, 0.5, -280)})
    tween:Play()
    tween.Completed:Wait()
    frame.Visible = false
    openIcon.Visible = true
end)

openIcon.MouseButton1Click:Connect(function()
    frame.Visible = true
    openIcon.Visible = false
    frame.BackgroundTransparency = 1
    frame.Position = UDim2.new(1, -355, 0.5, -280)
    local tween = tweenService:Create(frame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {BackgroundTransparency = 0, Position = UDim2.new(1, -355, 0.5, -180)})
    tween:Play()
end)

close.MouseButton1Click:Connect(function()
    destroyAllNotificationBlocks()
    gui:Destroy()
end)

function showAchievementBar(text, duration)
    local bar = Instance.new("Frame", gui)
    bar.Size = UDim2.new(0, 250, 0, 45)
    bar.Position = UDim2.new(1, -260, 0, -50)
    bar.BackgroundColor3 = Color3.fromRGB(30,30,30)
    bar.BackgroundTransparency = 0.2
    bar.BorderSizePixel = 0
    bar.AnchorPoint = Vector2.new(0,0)
    local uicorner = Instance.new("UICorner", bar)
    uicorner.CornerRadius = UDim.new(0, 12)
    local label = Instance.new("TextLabel", bar)
    label.Size = UDim2.new(1, -16, 1, -12)
    label.Position = UDim2.new(0, 8, 0, 6)
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = Color3.fromRGB(255,255,255)
    label.TextSize = 14
    label.Font = Enum.Font.GothamBold
    label.TextWrapped = true
    bar.Position = UDim2.new(1, -260, 0, -50)
    bar.BackgroundTransparency = 1
    label.TextTransparency = 1
    local tweenIn = tweenService:Create(bar, TweenInfo.new(0.35, Enum.EasingStyle.Quad), {Position = UDim2.new(1, -260, 0, 18), BackgroundTransparency = 0.2})
    local tweenLabelIn = tweenService:Create(label, TweenInfo.new(0.25), {TextTransparency = 0})
    tweenIn:Play()
    tweenLabelIn:Play()
    tweenIn.Completed:Wait()
    wait(duration or 5)
    local tweenOut = tweenService:Create(bar, TweenInfo.new(0.35, Enum.EasingStyle.Quad), {Position = UDim2.new(1, -260, 0, -50), BackgroundTransparency = 1})
    local tweenLabelOut = tweenService:Create(label, TweenInfo.new(0.25), {TextTransparency = 1})
    tweenOut:Play()
    tweenLabelOut:Play()
    tweenOut.Completed:Wait()
    bar:Destroy()
end

coroutine.wrap(function()
    showAchievementBar("Welcome to Freeman HUB for Brookhaven!",4)
end)()