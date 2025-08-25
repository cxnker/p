wait = task.wait
local a = game:GetService("Players").LocalPlayer.Character or game:GetService("Players").LocalPlayer.CharacterAdded:wait()

while wait() do
    local b = a:FindFirstChildOfClass("Humanoid") or a:FindFirstChildOfClass("AnimationController")
    if not b or not a then continue end
    for c, d in next, b:GetPlayingAnimationTracks() do
        d:AdjustSpeed(15)
    end
end
