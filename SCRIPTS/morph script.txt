--Created by roiprizer
--FE code by Mokiros   
--Edited by XKxngSupremeX--
--Discord: OofCopSupreme#1765
game.Players.LocalPlayer.Character["LUAhEAD"].Handle.Mesh:Destroy() --Pink Hair
game.Players.LocalPlayer.Character["LongHairBeanie"].Handle.Mesh:Destroy()
game.Players.LocalPlayer.Character["Kate Hair"].Handle.Mesh:Destroy() --LavanderHair
game.Players.LocalPlayer.Character["LavanderHair"].Handle.Mesh:Destroy()
game.Players.LocalPlayer.Character["Robloxclassicred"].Handle.Mesh:Destroy() --Pink Hair
game.Players.LocalPlayer.Character["Pink Hair"].Handle.Mesh:Destroy() --Hat1
game.Players.LocalPlayer.Character["Hat1"].Handle.Mesh:Destroy()
game.Players.LocalPlayer.Character["Pal Hair"].Handle.Mesh:Destroy()
game.Players.LocalPlayer.Character["Necklace"].Handle.Mesh:Destroy() --Necklace
--hats what you will know 


local c = game.Players.LocalPlayer.Character
for i, v in pairs({"Right Arm", "Left Arm"}) do
    local arm = c[v]
    arm.Parent = nil
    arm.Transparency = 1
    arm.Parent = c
end

local c = game.Players.LocalPlayer.Character
for i, v in pairs({"Right Leg", "Left Leg"}) do
    local Leg = c[v]
    Leg.Parent = nil
    Leg.Transparency = 1
    Leg.Parent = c
end


local v3_net, v3_808 = Vector3.new(10000, 10000, 10000), Vector3.new(8, 0, 8)
		local function getNetlessVelocity(realPartVelocity)
			local mag = realPartVelocity.Magnitude
			if mag > 1 then
				local unit = realPartVelocity.Unit
				if (unit.Y > 0.25) or (unit.Y < -0.75) then
					return unit * (25.1 / unit.Y)
				end
			end 
			return v3_net + realPartVelocity * v3_808
		end
		local simradius = "shp" --simulation radius (net bypass) method
--simulation radius (net bypass) method
--"shp" - sethiddenproperty
--"ssr" - setsimulationradius
--false - disable
local antiragdoll = true --removes hingeConstraints and ballSocketConstraints from your character
local newanimate = false --disables the animate script and enables after reanimation
local discharscripts = true --disables all localScripts parented to your character before reanimation
local R15toR6 = true --tries to convert your character to r6 if its r15
local hatcollide = true --makes hats cancollide (only method 0)
local humState16 = true --enables collisions for limbs before the humanoid dies (using hum:ChangeState)
local addtools = false --puts all tools from backpack to character and lets you hold them after reanimation
local hedafterneck = false --disable aligns for head and enable after neck is removed
local loadtime = game:GetService("Players").RespawnTime + 0.5 --anti respawn delay
local method = 0 --reanimation method
--methods:
--0 - breakJoints (takes [loadtime] seconds to laod)
--1 - limbs
--2 - limbs + anti respawn
--3 - limbs + breakJoints after [loadtime] seconds
--4 - remove humanoid + breakJoints
--5 - remove humanoid + limbs
local alignmode = 3 --AlignPosition mode
--modes:
--1 - AlignPosition rigidity enabled true
--2 - 2 AlignPositions rigidity enabled both true and false
--3 - AlignPosition rigidity enabled false

healthHide = healthHide and ((method == 0) or (method == 2) or (method == 000)) and gp(c, "Head", "BasePart")

local lp = game:GetService("Players").LocalPlayer
local rs = game:GetService("RunService")
local stepped = rs.Stepped
local heartbeat = rs.Heartbeat
local renderstepped = rs.RenderStepped
local sg = game:GetService("StarterGui")
local ws = game:GetService("Workspace")
local cf = CFrame.new
local v3 = Vector3.new
local v3_0 = v3(0, 0, 0)
local inf = math.huge

local c = lp.Character

if not (c and c.Parent) then
	return
end

c.Destroying:Connect(function()
	c = nil
end)

local function gp(parent, name, className)
	if typeof(parent) == "Instance" then
		for i, v in pairs(parent:GetChildren()) do
			if (v.Name == name) and v:IsA(className) then
				return v
			end
		end
	end
	return nil
end

local function align(Part0, Part1)
	Part0.CustomPhysicalProperties = PhysicalProperties.new(0.0001, 0.0001, 0.0001, 0.0001, 0.0001)

	local att0 = Instance.new("Attachment", Part0)
	att0.Orientation = v3_0
	att0.Position = v3_0
	att0.Name = "att0_" .. Part0.Name
	local att1 = Instance.new("Attachment", Part1)
	att1.Orientation = v3_0
	att1.Position = v3_0
	att1.Name = "att1_" .. Part1.Name

	if (alignmode == 1) or (alignmode == 2) then
		local ape = Instance.new("AlignPosition", att0)
		ape.ApplyAtCenterOfMass = false
		ape.MaxForce = inf
		ape.MaxVelocity = inf
		ape.ReactionForceEnabled = false
		ape.Responsiveness = 200
		ape.Attachment1 = att1
		ape.Attachment0 = att0
		ape.Name = "AlignPositionRtrue"
		ape.RigidityEnabled = true
	end

	if (alignmode == 2) or (alignmode == 3) then
		local apd = Instance.new("AlignPosition", att0)
		apd.ApplyAtCenterOfMass = false
		apd.MaxForce = inf
		apd.MaxVelocity = inf
		apd.ReactionForceEnabled = false
		apd.Responsiveness = 200
		apd.Attachment1 = att1
		apd.Attachment0 = att0
		apd.Name = "AlignPositionRfalse"
		apd.RigidityEnabled = false
	end

	local ao = Instance.new("AlignOrientation", att0)
	ao.MaxAngularVelocity = inf
	ao.MaxTorque = inf
	ao.PrimaryAxisOnly = false
	ao.ReactionTorqueEnabled = false
	ao.Responsiveness = 200
	ao.Attachment1 = att1
	ao.Attachment0 = att0
	ao.RigidityEnabled = false

	if type(getNetlessVelocity) == "function" then
	    local realVelocity = v3_0
        local steppedcon = stepped:Connect(function()
            Part0.Velocity = realVelocity
        end)
        local heartbeatcon = heartbeat:Connect(function()
            realVelocity = Part0.Velocity
            Part0.Velocity = getNetlessVelocity(realVelocity)
        end)
        Part0.Destroying:Connect(function()
            Part0 = nil
            steppedcon:Disconnect()
            heartbeatcon:Disconnect()
        end)
    end
end

local function respawnrequest()
	local ccfr = ws.CurrentCamera.CFrame
	local c = lp.Character
	lp.Character = nil
	lp.Character = c
	local con = nil
	con = ws.CurrentCamera.Changed:Connect(function(prop)
	    if (prop ~= "Parent") and (prop ~= "CFrame") then
	        return
	    end
	    ws.CurrentCamera.CFrame = ccfr
	    con:Disconnect()
    end)
end

local destroyhum = (method == 4) or (method == 5)
local breakjoints = (method == 0) or (method == 4)
local antirespawn = (method == 0) or (method == 2) or (method == 3)

hatcollide = hatcollide and (method == 0)

addtools = addtools and gp(lp, "Backpack", "Backpack")

local fenv = getfenv()
local shp = fenv.sethiddenproperty or fenv.set_hidden_property or fenv.set_hidden_prop or fenv.sethiddenprop
local ssr = fenv.setsimulationradius or fenv.set_simulation_radius or fenv.set_sim_radius or fenv.setsimradius or fenv.set_simulation_rad or fenv.setsimulationrad

if shp and (simradius == "shp") then
	spawn(function()
		while c and heartbeat:Wait() do
			shp(lp, "SimulationRadius", inf)
		end
	end)
elseif ssr and (simradius == "ssr") then
	spawn(function()
		while c and heartbeat:Wait() do
			ssr(inf)
		end
	end)
end

antiragdoll = antiragdoll and function(v)
	if v:IsA("HingeConstraint") or v:IsA("BallSocketConstraint") then
		v.Parent = nil
	end
end

if antiragdoll then
	for i, v in pairs(c:GetDescendants()) do
		antiragdoll(v)
	end
	c.DescendantAdded:Connect(antiragdoll)
end

if antirespawn then
	respawnrequest()
end

if method == 0 then
	wait(loadtime)
	if not c then
		return
	end
end

if discharscripts then
	for i, v in pairs(c:GetChildren()) do
		if v:IsA("LocalScript") then
			v.Disabled = true
		end
	end
elseif newanimate then
	local animate = gp(c, "Animate", "LocalScript")
	if animate and (not animate.Disabled) then
		animate.Disabled = true
	else
		newanimate = false
	end
end

if addtools then
	for i, v in pairs(addtools:GetChildren()) do
		if v:IsA("Tool") then
			v.Parent = c
		end
	end
end

pcall(function()
	settings().Physics.AllowSleep = false
	settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
end)

local OLDscripts = {}

for i, v in pairs(c:GetDescendants()) do
	if v.ClassName == "Script" then
		table.insert(OLDscripts, v)
	end
end

local scriptNames = {}

for i, v in pairs(c:GetDescendants()) do
	if v:IsA("BasePart") then
		local newName = tostring(i)
		local exists = true
		while exists do
			exists = false
			for i, v in pairs(OLDscripts) do
				if v.Name == newName then
					exists = true
				end
			end
			if exists then
				newName = newName .. "_"    
			end
		end
		table.insert(scriptNames, newName)
		Instance.new("Script", v).Name = newName
	end
end

c.Archivable = true
local hum = c:FindFirstChildOfClass("Humanoid")
if hum then
	for i, v in pairs(hum:GetPlayingAnimationTracks()) do
		v:Stop()
	end
end
local cl = c:Clone()
if hum and humState16 then
    hum:ChangeState(Enum.HumanoidStateType.Physics)
    if destroyhum then
        wait(1.6)
    end
end
if hum and hum.Parent and destroyhum then
    hum:Destroy()
end

if not c then
    return
end

local head = gp(c, "Head", "BasePart")
local torso = gp(c, "Torso", "BasePart") or gp(c, "UpperTorso", "BasePart")
local root = gp(c, "HumanoidRootPart", "BasePart")
if hatcollide and c:FindFirstChildOfClass("Accessory") then
    local anything = c:FindFirstChildOfClass("BodyColors") or gp(c, "Health", "Script")
    if not (torso and root and anything) then
        return
    end
    torso:Destroy()
    root:Destroy()
    if shp then
        for i,v in pairs(c:GetChildren()) do
            if v:IsA("Accessory") then
                shp(v, "BackendAccoutrementState", 0)
            end 
        end
    end
    anything:Destroy()
    if head then
       
    end
end

for i, v in pairs(cl:GetDescendants()) do
	if v:IsA("BasePart") then
		v.Transparency = 1
		v.Anchored = false
	end
end

local model = Instance.new("Model", c)
model.Name = model.ClassName

model.Destroying:Connect(function()
	model = nil
end)

for i, v in pairs(c:GetChildren()) do
	if v ~= model then
		if addtools and v:IsA("Tool") then
			for i1, v1 in pairs(v:GetDescendants()) do
				if v1 and v1.Parent and v1:IsA("BasePart") then
					local bv = Instance.new("BodyVelocity", v1)
					bv.Velocity = v3_0
					bv.MaxForce = v3(1000, 1000, 1000)
					bv.P = 1250
					bv.Name = "bv_" .. v.Name
				end
			end
		end
		v.Parent = model
	end
end

if breakjoints then
	model:BreakJoints()
else
	if head and torso then
		for i, v in pairs(model:GetDescendants()) do
			if v:IsA("Weld") or v:IsA("Snap") or v:IsA("Glue") or v:IsA("Motor") or v:IsA("Motor6D") then
				local save = false
				if (v.Part0 == torso) and (v.Part1 == head) then
					save = true
				end
				if (v.Part0 == head) and (v.Part1 == torso) then
					save = true
				end
				if save then
					if hedafterneck then
						hedafterneck = v
					end
				else
					v:Destroy()
				end
			end
		end
	end
	if method == 3 then
		spawn(function()
			wait(loadtime)
			if model then
				model:BreakJoints()
			end
		end)
	end
end

cl.Parent = c
for i, v in pairs(cl:GetChildren()) do
	v.Parent = c
end
cl:Destroy()

local modelDes = {}
for i, v in pairs(model:GetDescendants()) do
	if v:IsA("BasePart") then
		i = tostring(i)
		v.Destroying:Connect(function()
			modelDes[i] = nil
		end)
		modelDes[i] = v
	end
end
local modelcolcon = nil
local function modelcolf()
	if model then
		for i, v in pairs(modelDes) do
			v.CanCollide = false
		end
	else
		modelcolcon:Disconnect()
	end
end
modelcolcon = stepped:Connect(modelcolf)
modelcolf()

for i, scr in pairs(model:GetDescendants()) do
	if (scr.ClassName == "Script") and table.find(scriptNames, scr.Name) then
		local Part0 = scr.Parent
		if Part0:IsA("BasePart") then
			for i1, scr1 in pairs(c:GetDescendants()) do
				if (scr1.ClassName == "Script") and (scr1.Name == scr.Name) and (not scr1:IsDescendantOf(model)) then
					local Part1 = scr1.Parent
					if (Part1.ClassName == Part0.ClassName) and (Part1.Name == Part0.Name) then
						align(Part0, Part1)
						break
					end
				end
			end
		end
	end
end

if (typeof(hedafterneck) == "Instance") and head then
	local aligns = {}
	local con = nil
	con = hedafterneck.Changed:Connect(function(prop)
	    if (prop == "Parent") and not hedafterneck.Parent then
	        con:Disconnect()
    		for i, v in pairs(aligns) do
    			v.Enabled = true
    		end
		end
	end)
	for i, v in pairs(head:GetDescendants()) do
		if v:IsA("AlignPosition") or v:IsA("AlignOrientation") then
			i = tostring(i)
			aligns[i] = v
			v.Destroying:Connect(function()
			    aligns[i] = nil
			end)
			v.Enabled = false
		end
	end
end

for i, v in pairs(c:GetDescendants()) do
	if v and v.Parent then
		if v.ClassName == "Script" then
			if table.find(scriptNames, v.Name) then
				v:Destroy()
			end
		elseif not v:IsDescendantOf(model) then
			if v:IsA("Decal") then
				v.Transparency = 1
			elseif v:IsA("ForceField") then
				v.Visible = false
			elseif v:IsA("Sound") then
				v.Playing = false
			elseif v:IsA("BillboardGui") or v:IsA("SurfaceGui") or v:IsA("ParticleEmitter") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Sparkles") then
				v.Enabled = false
			end
		end
	end
end

if newanimate then
	local animate = gp(c, "Animate", "LocalScript")
	if animate then
		animate.Disabled = false
	end
end

if addtools then
	for i, v in pairs(c:GetChildren()) do
		if v:IsA("Tool") then
			v.Parent = addtools
		end
	end
end

local hum0 = model:FindFirstChildOfClass("Humanoid")
if hum0 then
    hum0.Destroying:Connect(function()
        hum0 = nil
    end)
end

local hum1 = c:FindFirstChildOfClass("Humanoid")
if hum1 then
    hum1.Destroying:Connect(function()
        hum1 = nil
    end)
end

if hum1 then
	ws.CurrentCamera.CameraSubject = hum1
	local camSubCon = nil
	local function camSubFunc()
		camSubCon:Disconnect()
		if c and hum1 then
			ws.CurrentCamera.CameraSubject = hum1
		end
	end
	camSubCon = renderstepped:Connect(camSubFunc)
	if hum0 then
		hum0.Changed:Connect(function(prop)
			if hum1 and (prop == "Jump") then
				hum1.Jump = hum0.Jump
			end
		end)
	else
		respawnrequest()
	end
end

local rb = Instance.new("BindableEvent", c)
rb.Event:Connect(function()
	rb:Destroy()
	sg:SetCore("ResetButtonCallback", true)
	if destroyhum then
		c:BreakJoints()
		return
	end
	if hum0 and (hum0.Health > 0) then
		model:BreakJoints()
		hum0.Health = 0
	end
	if antirespawn then
	    respawnrequest()
	end
end)
sg:SetCore("ResetButtonCallback", rb)

spawn(function()
	while c do
		if hum0 and hum1 then
			hum1.Jump = hum0.Jump
		end
		wait()
	end
	sg:SetCore("ResetButtonCallback", true)
end)

R15toR6 = R15toR6 and hum1 and (hum1.RigType == Enum.HumanoidRigType.R15)
if R15toR6 then
    local part = gp(c, "HumanoidRootPart", "BasePart") or gp(c, "UpperTorso", "BasePart") or gp(c, "LowerTorso", "BasePart") or gp(c, "Head", "BasePart") or c:FindFirstChildWhichIsA("BasePart")
	if part then
	    local cfr = part.CFrame
		local R6parts = { 
			head = {
				Name = "Head",
				Size = v3(2, 1, 1),
				R15 = {
					Head = 0
				}
			},
			torso = {
				Name = "Torso",
				Size = v3(2, 2, 1),
				R15 = {
					UpperTorso = 0.2,
					LowerTorso = -100
				}
			},
			root = {
				Name = "HumanoidRootPart",
				Size = v3(2, 2, 1),
				R15 = {
					HumanoidRootPart = 0
				}
			},
			leftArm = {
				Name = "Left Arm",
				Size = v3(1, 2, 1),
				R15 = {
					LeftHand = -0.73,
					LeftLowerArm = -0.2,
					LeftUpperArm = 0.4
				}
			},
			rightArm = {
				Name = "Right Arm",
				Size = v3(1, 2, 1),
				R15 = {
					RightHand = -0.73,
					RightLowerArm = -0.2,
					RightUpperArm = 0.4
				}
			},
			leftLeg = {
				Name = "Left Leg",
				Size = v3(1, 2, 1),
				R15 = {
					LeftFoot = -0.73,
					LeftLowerLeg = -0.15,
					LeftUpperLeg = 0.6
				}
			},
			rightLeg = {
				Name = "Right Leg",
				Size = v3(1, 2, 1),
				R15 = {
					RightFoot = -0.73,
					RightLowerLeg = -0.15,
					RightUpperLeg = 0.6
				}
			}
		}
		for i, v in pairs(c:GetChildren()) do
			if v:IsA("BasePart") then
				for i1, v1 in pairs(v:GetChildren()) do
					if v1:IsA("Motor6D") then
						v1.Part0 = nil
					end
				end
			end
		end
		part.Archivable = true
		for i, v in pairs(R6parts) do
			local part = part:Clone()
			part:ClearAllChildren()
			part.Name = v.Name
			part.Size = v.Size
			part.CFrame = cfr
			part.Anchored = false
			part.Transparency = 1
			part.CanCollide = false
			for i1, v1 in pairs(v.R15) do
				local R15part = gp(c, i1, "BasePart")
				local att = gp(R15part, "att1_" .. i1, "Attachment")
				if R15part then
					local weld = Instance.new("Weld", R15part)
					weld.Name = "Weld_" .. i1
					weld.Part0 = part
					weld.Part1 = R15part
					weld.C0 = cf(0, v1, 0)
					weld.C1 = cf(0, 0, 0)
					R15part.Massless = true
					R15part.Name = "R15_" .. i1
					R15part.Parent = part
					if att then
						att.Parent = part
						att.Position = v3(0, v1, 0)
					end
				end
			end
			part.Parent = c
			R6parts[i] = part
		end
		local R6joints = {
			neck = {
				Parent = R6parts.torso,
				Name = "Neck",
				Part0 = R6parts.torso,
				Part1 = R6parts.head,
				C0 = cf(0, 1, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
				C1 = cf(0, -0.5, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
			},
			rootJoint = {
				Parent = R6parts.root,
				Name = "RootJoint" ,
				Part0 = R6parts.root,
				Part1 = R6parts.torso,
				C0 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
				C1 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
			},
			rightShoulder = {
				Parent = R6parts.torso,
				Name = "Right Shoulder",
				Part0 = R6parts.torso,
				Part1 = R6parts.rightArm,
				C0 = cf(1, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
				C1 = cf(-0.5, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			},
			leftShoulder = {
				Parent = R6parts.torso,
				Name = "Left Shoulder",
				Part0 = R6parts.torso,
				Part1 = R6parts.leftArm,
				C0 = cf(-1, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
				C1 = cf(0.5, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			},
			rightHip = {
				Parent = R6parts.torso,
				Name = "Right Hip",
				Part0 = R6parts.torso,
				Part1 = R6parts.rightLeg,
				C0 = cf(1, -1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
				C1 = cf(0.5, 1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			},
			leftHip = {
				Parent = R6parts.torso,
				Name = "Left Hip" ,
				Part0 = R6parts.torso,
				Part1 = R6parts.leftLeg,
				C0 = cf(-1, -1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
				C1 = cf(-0.5, 1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			}
		}
		for i, v in pairs(R6joints) do
			local joint = Instance.new("Motor6D")
			for prop, val in pairs(v) do
				joint[prop] = val
			end
			R6joints[i] = joint
		end
		hum1.RigType = Enum.HumanoidRigType.R6
		hum1.HipHeight = 0
	end
end



--find rig joints

local function fakemotor()
    return {C0=cf(), C1=cf()}
end

local torso = gp(c, "Torso", "BasePart")
local root = gp(c, "HumanoidRootPart", "BasePart")

local neck = gp(torso, "Neck", "Motor6D")
neck = neck or fakemotor()

local rootJoint = gp(root, "RootJoint", "Motor6D")
rootJoint = rootJoint or fakemotor()

local leftShoulder = gp(torso, "Left Shoulder", "Motor6D")
leftShoulder = leftShoulder or fakemotor()

local rightShoulder = gp(torso, "Right Shoulder", "Motor6D")
rightShoulder = rightShoulder or fakemotor()

local leftHip = gp(torso, "Left Hip", "Motor6D")
leftHip = leftHip or fakemotor()

local rightHip = gp(torso, "Right Hip", "Motor6D")
rightHip = rightHip or fakemotor()

--120 fps

local fps = 40
local event = Instance.new("BindableEvent", c)
event.Name = "120 fps"
local floor = math.floor
fps = 1 / fps
local tf = 0
local con = nil
con = game:GetService("RunService").RenderStepped:Connect(function(s)
	if not c then
		con:Disconnect()
		return
	end
    --tf += s
	if tf >= fps then
		for i=1, floor(tf / fps) do
			event:Fire(c)
		end
		tf = 0
	end
end)
local event = event.Event

local hedrot = v3(0, 5, 0)

local uis = game:GetService("UserInputService")
local function isPressed(key)
    return (not uis:GetFocusedTextBox()) and uis:IsKeyDown(Enum.KeyCode[key])
end

local biggesthandle = Back_AccAccessory
for i, v in pairs(c:GetChildren()) do
    if v:IsA("Accessory") then --Accessory
        local handle = gp(v, "Handle", "BasePart")
        if biggesthandle then
            if biggesthandle.Size.Magnitude < handle.Size.Magnitude then
                biggesthandle = handle
            end
        else
            biggesthandle = gp(v, "Handle", "BasePart")
        end
    end
end

if not biggesthandle then
    return
end

local handle1 = gp(gp(model, biggesthandle.Parent.Name, "Accessory"), "Handle", "BasePart")
if not handle1 then
    return
end

handle1.Destroying:Connect(function()
    handle1 = Back_AccAccessory
end)
biggesthandle.Destroying:Connect(function()
    biggesthandle = Back_AccAccessory
end)

biggesthandle:BreakJoints()
biggesthandle.Anchored = true

--for i, v in pairs(handle1:GetDescendants()) do
   -- if v:IsA("AlignOrientation") then
       -- v.Enabled = false
    --end
--nd

--local mouse = lp:GetMouse()
--local fling = false
--mouse.Button1Down:Connect(function()
    --fling = true
--end)
--mouse.Button1Up:Connect(function()
   -- fling = false
--end)
local function doForSignal(signal, vel)
    spawn(function()
        while signal:Wait() and c and handle1 and biggesthandle do
            if fling and mouse.Target then
                biggesthandle.Position = mouse.Hit.Position
            end
            --handle1.RotVelocity = vel
        end
    end)
end
doForSignal(stepped, v3(100, 100, 100))
doForSignal(renderstepped, v3(100, 100, 100))
doForSignal(heartbeat, v3(20000, 20000, 20000)) --https://web.roblox.com/catalog/63690008/Pal-Hair



local lp = game:GetService("Players").LocalPlayer
local rs = game:GetService("RunService")
local stepped = rs.Stepped
local heartbeat = rs.Heartbeat
local renderstepped = rs.RenderStepped
local sg = game:GetService("StarterGui")
local ws = game:GetService("Workspace")
local cf = CFrame.new
local v3 = Vector3.new
local v3_0 = Vector3.zero
local inf = math.huge

local cplayer = lp.Character

local v3 = Vector3.new

local function gp(parent, name, className)
    if typeof(parent) == "Instance" then
        for i, v in pairs(parent:GetChildren()) do
            if (v.Name == name) and v:IsA(className) then
                return v
            end
        end
    end
    return nil
end

local hat2 = gp(cplayer, "LUAhEAD", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Torso"]
att2.Position = Vector3.new(-0, -0, 0)
att2.Rotation = Vector3.new(90, 0, 0)


local hat2 = gp(cplayer, "LongHairBeanie", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Left Arm"]
att2.Position = Vector3.new(1, -0.5, 0)
att2.Rotation = Vector3.new(0, 0, 0) --Necklace

local hat2 = gp(cplayer, "Necklace", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Left Arm"]
att2.Position = Vector3.new(1, -2, 0)
att2.Rotation = Vector3.new(90, 0, 0)

local hat2 = gp(cplayer, "Kate Hair", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Right Arm"]
att2.Position = Vector3.new(-1, -0, 0)
att2.Rotation = Vector3.new(90, 0, 0)--LavanderHair Pink Hair

local hat2 = gp(cplayer, "Pink Hair", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Right Arm"]
att2.Position = Vector3.new(-1, -2, 0)
att2.Rotation = Vector3.new(90, 0, 0)

local hat2 = gp(cplayer, "LavanderHair", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Right Leg"]
att2.Position = Vector3.new(0, -0, 0) --Robloxclassicred
att2.Rotation = Vector3.new(90, 0, 0)

local hat2 = gp(cplayer, "Robloxclassicred", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Left Leg"]
att2.Position = Vector3.new(0, -0, 0) --Robloxclassicred
att2.Rotation = Vector3.new(90, 0, 0) --Surfboard Hat1

local hat2 = gp(cplayer, "Hat1", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Left Leg"]
att2.Position = Vector3.new(0, -2, 0) --Robloxclassicred
att2.Rotation = Vector3.new(90, 0, 0) --Pal Hair

local hat2 = gp(cplayer, "Pal Hair", "Accessory")
local handle2 = gp(hat2, "Handle", "BasePart")
local att2 = gp(handle2, "att1_Handle", "Attachment")
att2.Parent = cplayer["Right Leg"]
att2.Position = Vector3.new(0, -2, 0) --Robloxclassicred
att2.Rotation = Vector3.new(90, 0, 0)

		--local humanoid = char:WaitForChild("Humanoid")	
		

Player = game:GetService("Players").LocalPlayer 
--PlayerGui = Player.PlayerGui
Cam = workspace.CurrentCamera
--Backpack = Player.Backpack
Character = Player.Character
Humanoid = Character.Humanoid
Mouse = Player:GetMouse()
RootPart = Character["HumanoidRootPart"]
Torso = Character["Torso"]
Head = Character["Head"]
RightArm = Character["Right Arm"]
LeftArm = Character["Left Arm"]
RightLeg = Character["Right Leg"]
LeftLeg = Character["Left Leg"]
RootJoint = RootPart["RootJoint"]
Neck = Torso["Neck"]
RightShoulder = Torso["Right Shoulder"]
LeftShoulder = Torso["Left Shoulder"]
RightHip = Torso["Right Hip"]
LeftHip = Torso["Left Hip"]

KEYHOLD = false
IT = Instance.new
CF = CFrame.new
VT = Vector3.new
RAD = math.rad
C3 = Color3.new
UD2 = UDim2.new
BRICKC = BrickColor.new
ANGLES = CFrame.Angles
EULER = CFrame.fromEulerAnglesXYZ
COS = math.cos
ACOS = math.acos
SIN = math.sin
ASIN = math.asin
ABS = math.abs
MRANDOM = math.random
FLOOR = math.floor

Character = Player.Character
Humanoid = Character.Humanoid
---------
plr = game.Players.LocalPlayer
chara = plr.Character
mouse = plr:GetMouse()
Create = Instance.new
Huge = math.huge


Player = game:GetService("Players").LocalPlayer
PlayerGui = Player.PlayerGui
Cam = workspace.CurrentCamera
Backpack = Player.Backpack
Character = Player.Character
char = Player.Character
Humanoid = Character.Humanoid
Mouse = Player:GetMouse()
RootPart = Character["HumanoidRootPart"]
Torso = Character["Torso"]
Head = Character["Head"]
RightArm = Character["Right Arm"]
LeftArm = Character["Left Arm"]
RightLeg = Character["Right Leg"]
LeftLeg = Character["Left Leg"]
RootJoint = RootPart["RootJoint"]
Neck = Torso["Neck"]
RightShoulder = Torso["Right Shoulder"]
LeftShoulder = Torso["Left Shoulder"]
RightHip = Torso["Right Hip"]
LeftHip = Torso["Left Hip"]




function weld(a, b, acf)
	local w = Instance.new("Weld", a)
	w.Part0 = a
	w.Part1 = b
	w.C0 = acf
end
--------------------------------
char.Head.face.Texture = "rbxassetid://0"
--------------------------------

-------------------------------------------------------

local FavIDs = {
	340106355, --Nefl Crystals
	927529620, --Dimension
	876981900, --Fantasy
	398987889, --Ordinary Days
	1117396305, --Oh wait, it's you.
	885996042, --Action Winter Journey
	919231299, --Sprawling Idiot Effigy
	743466274, --Good Day Sunshine
	727411183, --Knife Fight
	1402748531, --The Earth Is Counting On You!
	595230126 --Robot Language
	}



--The reality of my life isn't real but a Universe -makhail07
wait(0.2)
local plr = game:service'Players'.LocalPlayer
print('Local User is '..plr.Name)
print('SCRIPTNAME Loaded')
print('SCRIPT DESCRIPTION')
local char = plr.Character
local hum = char.Humanoid
local hed = char.Head
local root = char.HumanoidRootPart
local rootj = root.RootJoint
local tors = char.Torso
local ra = char["Right Arm"]
local la = char["Left Arm"]
local rl = char["Right Leg"]
local ll = char["Left Leg"]
local neck = tors["Neck"]
local mouse = plr:GetMouse()
local RootCF = CFrame.fromEulerAnglesXYZ(-1.57, 0, 3.14)
local RHCF = CFrame.fromEulerAnglesXYZ(0, 1.6, 0)
local LHCF = CFrame.fromEulerAnglesXYZ(0, -1.6, 0)
local maincolor = BrickColor.new("Really black")



-------------------------------------------------------
--Start Good Stuff--
-------------------------------------------------------
cam = game.Workspace.CurrentCamera
CF = CFrame.new
angles = CFrame.Angles
attack = false
Euler = CFrame.fromEulerAnglesXYZ
Rad = math.rad
IT = Instance.new
BrickC = BrickColor.new
Cos = math.cos
Acos = math.acos
Sin = math.sin
Asin = math.asin
Abs = math.abs
Mrandom = math.random
Floor = math.floor



-------------------------------------------------------
--End Good Stuff--
-------------------------------------------------------
necko = CF(0, 1, 0, -1, -0, -0, 0, 0, 1, 0, 1, 0)
RSH, LSH = nil, nil 
RW = Instance.new("Weld") 
LW = Instance.new("Weld")
RH = tors["Right Hip"]
LH = tors["Left Hip"]
RSH = tors["Right Shoulder"] 
LSH = tors["Left Shoulder"] 
RSH.Parent = nil 
LSH.Parent = nil 
RW.Name = "RW"
RW.Part0 = tors 
RW.C0 = CF(1.5, 0.5, 0)
RW.C1 = CF(0, 0.5, 0) 
RW.Part1 = ra
RW.Parent = tors 
LW.Name = "LW"
LW.Part0 = tors 
LW.C0 = CF(-1.5, 0.5, 0)
LW.C1 = CF(0, 0.5, 0) 
LW.Part1 = la
LW.Parent = tors
Effects = {}
-------------------------------------------------------
--Start HeartBeat--
-------------------------------------------------------
ArtificialHB = Instance.new("BindableEvent", script)
ArtificialHB.Name = "Heartbeat"
script:WaitForChild("Heartbeat")

frame = 1 / 60
tf = 0
allowframeloss = false
tossremainder = false


lastframe = tick()
script.Heartbeat:Fire()


game:GetService("RunService").Heartbeat:connect(function(s, p)
	tf = tf + s
	if tf >= frame then
		if allowframeloss then
			script.Heartbeat:Fire()
			lastframe = tick()
		else
			for i = 1, math.floor(tf / frame) do
				script.Heartbeat:Fire()
			end
			lastframe = tick()
		end
		if tossremainder then
			tf = 0
		else
			tf = tf - frame * math.floor(tf / frame)
		end
	end
end)
-------------------------------------------------------
--End HeartBeat--
-------------------------------------------------------
        local joyemoji = Instance.new('ParticleEmitter', tors)
        joyemoji.VelocitySpread = 2000
        joyemoji.Lifetime = NumberRange.new(1)
        joyemoji.Speed = NumberRange.new(40)
joy= {}
for i=0, 19 do
  joy[#joy+ 1] = NumberSequenceKeypoint.new(i/19, math.random(1, 1))
end
joyemoji.Size = NumberSequence.new(joy)
        joyemoji.Rate = 0
        joyemoji.LockedToPart = false
        joyemoji.LightEmission = 0
        joyemoji.Texture = "rbxassetid://73623723"
        joyemoji.Color = ColorSequence.new(BrickColor.new("Institutional white").Color)

-------------------------------------------------------
--Start Important Functions--
-------------------------------------------------------


function swait(num)
	if num == 0 or num == nil then
		game:service("RunService").Stepped:wait(0)
	else
		for i = 0, num do
			game:service("RunService").Stepped:wait(0)
		end
	end
end
function thread(f)
	coroutine.resume(coroutine.create(f))
end
function clerp(a, b, t)
	local qa = {
		QuaternionFromCFrame(a)
	}
	local qb = {
		QuaternionFromCFrame(b)
	}
	local ax, ay, az = a.x, a.y, a.z
	local bx, by, bz = b.x, b.y, b.z
	local _t = 1 - t
	return QuaternionToCFrame(_t * ax + t * bx, _t * ay + t * by, _t * az + t * bz, QuaternionSlerp(qa, qb, t))
end
function QuaternionFromCFrame(cf)
	local mx, my, mz, m00, m01, m02, m10, m11, m12, m20, m21, m22 = cf:components()
	local trace = m00 + m11 + m22
	if trace > 0 then
		local s = math.sqrt(1 + trace)
		local recip = 0.5 / s
		return (m21 - m12) * recip, (m02 - m20) * recip, (m10 - m01) * recip, s * 0.5
	else
		local i = 0
		if m00 < m11 then
			i = 1
		end
		if m22 > (i == 0 and m00 or m11) then
			i = 2
		end
		if i == 0 then
			local s = math.sqrt(m00 - m11 - m22 + 1)
			local recip = 0.5 / s
			return 0.5 * s, (m10 + m01) * recip, (m20 + m02) * recip, (m21 - m12) * recip
		elseif i == 1 then
			local s = math.sqrt(m11 - m22 - m00 + 1)
			local recip = 0.5 / s
			return (m01 + m10) * recip, 0.5 * s, (m21 + m12) * recip, (m02 - m20) * recip
		elseif i == 2 then
			local s = math.sqrt(m22 - m00 - m11 + 1)
			local recip = 0.5 / s
			return (m02 + m20) * recip, (m12 + m21) * recip, 0.5 * s, (m10 - m01) * recip
		end
	end
end
function QuaternionToCFrame(px, py, pz, x, y, z, w)
	local xs, ys, zs = x + x, y + y, z + z
	local wx, wy, wz = w * xs, w * ys, w * zs
	local xx = x * xs
	local xy = x * ys
	local xz = x * zs
	local yy = y * ys
	local yz = y * zs
	local zz = z * zs
	return CFrame.new(px, py, pz, 1 - (yy + zz), xy - wz, xz + wy, xy + wz, 1 - (xx + zz), yz - wx, xz - wy, yz + wx, 1 - (xx + yy))
end
function QuaternionSlerp(a, b, t)
	local cosTheta = a[1] * b[1] + a[2] * b[2] + a[3] * b[3] + a[4] * b[4]
	local startInterp, finishInterp
	if cosTheta >= 1.0E-4 then
		if 1 - cosTheta > 1.0E-4 then
			local theta = math.acos(cosTheta)
			local invSinTheta = 1 / Sin(theta)
			startInterp = Sin((1 - t) * theta) * invSinTheta
			finishInterp = Sin(t * theta) * invSinTheta
		else
			startInterp = 1 - t
			finishInterp = t
		end
	elseif 1 + cosTheta > 1.0E-4 then
		local theta = math.acos(-cosTheta)
		local invSinTheta = 1 / Sin(theta)
		startInterp = Sin((t - 1) * theta) * invSinTheta
		finishInterp = Sin(t * theta) * invSinTheta
	else
		startInterp = t - 1
		finishInterp = t
	end
	return a[1] * startInterp + b[1] * finishInterp, a[2] * startInterp + b[2] * finishInterp, a[3] * startInterp + b[3] * finishInterp, a[4] * startInterp + b[4] * finishInterp
end
function rayCast(Position, Direction, Range, Ignore)
	return game:service("Workspace"):FindPartOnRay(Ray.new(Position, Direction.unit * (Range or 999.999)), Ignore)
end


function getRegion(point,range,ignore)
    return workspace:FindPartsInRegion3WithIgnoreList(Region3.new(point-Vector3.new(1,1,1)*range/2,point+Vector3.new(1,1,1)*range/2),ignore,100)
end

function GetTorso(char)
	return char:FindFirstChild'Torso' or char:FindFirstChild'UpperTorso' or char:FindFirstChild'LowerTorso' or char:FindFirstChild'HumanoidRootPart'
end

local M = {C=math.cos,R=math.rad,S=math.sin,P=math.pi,RNG=math.random,MRS=math.randomseed,H=math.huge,RRNG = function(min,max,div) return math.rad(math.random(min,max)/(div or 1)) end}
-------------------------------------------------------
--Start Damage Function--
-------------------------------------------------------
function Damage(Part, hit, minim, maxim, knockback, Type, Property, Delay, HitSound, HitPitch)
	if hit.Parent == nil then
		return
	end
	local h = hit.Parent:FindFirstChildOfClass("Humanoid")
	for _, v in pairs(hit.Parent:children()) do
		if v:IsA("Humanoid") then
			h = v
		end
	end
         if h ~= nil and hit.Parent.Name ~= char.Name and hit.Parent:FindFirstChild("UpperTorso") ~= nil then
	
         --hit.Parent:FindFirstChild("Head"):BreakJoints()
         end

	if h ~= nil and hit.Parent.Name ~= char.Name and hit.Parent:FindFirstChild("Torso") ~= nil then
		if hit.Parent:findFirstChild("DebounceHit") ~= nil then
			if hit.Parent.DebounceHit.Value == true then
				return
			end
		end
         if insta == true then
         --hit.Parent:FindFirstChild("Head"):BreakJoints()
         end
		local c = Create("ObjectValue"){
			Name = "creator",
			Value = game:service("Players").LocalPlayer,
			Parent = h,
		}
		game:GetService("Debris"):AddItem(c, .5)
		if HitSound ~= nil and HitPitch ~= nil then
			CFuncs.Sound.Create(HitSound, hit, 1, HitPitch) 
		end
		--local Damage = math.random(minim, maxim)
		--local blocked = false
		--local block = hit.Parent:findFirstChild("Block")
		if block ~= nil then
			if block.className == "IntValue" then
				if block.Value > 0 then
					blocked = true
					block.Value = block.Value - 1
					print(block.Value)
				end
			end
		end
		if blocked == false then
			h.Health = h.Health - Damage
			ShowDamage((Part.CFrame * CFrame.new(0, 0, (Part.Size.Z / 2)).p + Vector3.new(0, 1.5, 0)), -Damage, 1.5, tors.BrickColor.Color)
		else
			h.Health = h.Health - (Damage / 2)
			ShowDamage((Part.CFrame * CFrame.new(0, 0, (Part.Size.Z / 2)).p + Vector3.new(0, 1.5, 0)), -Damage, 1.5, tors.BrickColor.Color)
		end
		if Type == "Knockdown" then
			local hum = hit.Parent.Humanoid
			hum.PlatformStand = true
			coroutine.resume(coroutine.create(function(HHumanoid)
				swait(1)
				HHumanoid.PlatformStand = false
			end), hum)
			local angle = (hit.Position - (Property.Position + Vector3.new(0, 0, 0))).unit
			local bodvol = Create("BodyVelocity"){
				velocity = angle * knockback,
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			local rl = Create("BodyAngularVelocity"){
				P = 3000,
				maxTorque = Vector3.new(500000, 500000, 500000) * 50000000000000,
				angularvelocity = Vector3.new(math.random(-10, 10), math.random(-10, 10), math.random(-10, 10)),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodvol, .5)
			game:GetService("Debris"):AddItem(rl, .5)
		elseif Type == "Normal" then
			local vp = Create("BodyVelocity"){
				P = 500,
				maxForce = Vector3.new(math.huge, 0, math.huge),
				velocity = Property.CFrame.lookVector * knockback + Property.Velocity / 1.05,
			}
			if knockback > 0 then
				vp.Parent = hit.Parent.Torso
			end
			game:GetService("Debris"):AddItem(vp, .5)
		elseif Type == "Up" then
			local bodyVelocity = Create("BodyVelocity"){
				velocity = Vector3.new(0, 20, 0),
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodyVelocity, .5)
		elseif Type == "DarkUp" then
			coroutine.resume(coroutine.create(function()
				for i = 0, 1, 0.1 do
					swait()
					--Effects.Block.Create(BrickColor.new("Black"), hit.Parent.Torso.CFrame, 5, 5, 5, 1, 1, 1, .08, 1)
				end
			end))
			local bodyVelocity = Create("BodyVelocity"){
				velocity = Vector3.new(0, 20, 0),
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodyVelocity, 1)
		elseif Type == "Snare" then
			local bp = Create("BodyPosition"){
				P = 2000,
				D = 100,
				maxForce = Vector3.new(math.huge, math.huge, math.huge),
				position = hit.Parent.Torso.Position,
				Parent = hit.Parent.Torso,
			}
			game:GetService("Debris"):AddItem(bp, 1)
		elseif Type == "Freeze" then
			local BodPos = Create("BodyPosition"){
				P = 50000,
				D = 1000,
				maxForce = Vector3.new(math.huge, math.huge, math.huge),
				position = hit.Parent.Torso.Position,
				Parent = hit.Parent.Torso,
			}
			local BodGy = Create("BodyGyro") {
				maxTorque = Vector3.new(4e+005, 4e+005, 4e+005) * math.huge ,
				P = 20e+003,
				Parent = hit.Parent.Torso,
				cframe = hit.Parent.Torso.CFrame,
			}
			hit.Parent.Torso.Anchored = false
			coroutine.resume(coroutine.create(function(Part) 
				swait(1.5)
				Part.Anchored = false
			end), hit.Parent.Torso)
			game:GetService("Debris"):AddItem(BodPos, 3)
			game:GetService("Debris"):AddItem(BodGy, 3)
		end
		local debounce = Create("BoolValue"){
			Name = "DebounceHit",
			Parent = hit.Parent,
			Value = true,
		}
		game:GetService("Debris"):AddItem(debounce, Delay)
		c = Create("ObjectValue"){
			Name = "creator",
			Value = Player,
			Parent = h,
		}
		game:GetService("Debris"):AddItem(c, .5)
	end
end
-------------------------------------------------------
--End Damage Function--
-------------------------------------------------------

-------------------------------------------------------
--Start Damage Function Customization--
-------------------------------------------------------
function ShowDamage(Pos, Text, Time, Color)
	local Rate = (1 / 30)
	local Pos = (Pos or Vector3.new(0, 0, 0))
	local Text = (Text or "")
	local Time = (Time or 2)
	local Color = (Color or Color3.new(255, 255, 1))
	local EffectPart = CFuncs.Part.Create(workspace, "SmoothPlastic", 0, 1, BrickColor.new(Color), "Effect", Vector3.new(0, 0, 0))
	EffectPart.Anchored = true
	local BillboardGui = Create("BillboardGui"){
		Size = UDim2.new(3, 0, 3, 0),
		Adornee = EffectPart,
		Parent = EffectPart,
	}
	local TextLabel = Create("TextLabel"){
		BackgroundTransparency = 1,
		Size = UDim2.new(1, 0, 1, 0),
		Text = Text,
		Font = "scp - 096",
		TextColor3 = Color,
		TextScaled = true,
		TextStrokeColor3 = Color3.fromRGB(0,0,0),
		Parent = BillboardGui,
	}
	game.Debris:AddItem(EffectPart, (Time))
	EffectPart.Parent = game:GetService("Workspace")
	delay(0, function()
		local Frames = (Time / Rate)
		for Frame = 1, Frames do
			wait(Rate)
			local Percent = (Frame / Frames)
			EffectPart.CFrame = CFrame.new(Pos) + Vector3.new(0, Percent, 0)
			TextLabel.TextTransparency = Percent
		end
		if EffectPart and EffectPart.Parent then
			EffectPart:Destroy()
		end
	end)
end
-------------------------------------------------------
--End Damage Function Customization--
-------------------------------------------------------

function MagniDamage(Part, magni, mindam, maxdam, knock, Type)
  for _, c in pairs(workspace:children()) do
    local hum = c:findFirstChild("Humanoid")
    if hum ~= nil then
      local head = c:findFirstChild("Head")
      if head ~= nil then
        local targ = head.Position - Part.Position
        local mag = targ.magnitude
        if magni >= mag and c.Name ~= plr.Name then
          Damage(head, head, mindam, maxdam, knock, Type, root, 0.1, "http://www.roblox.com/asset/?id=0", 1.2)
        end
      end
    end
  end
end


CFuncs = {
	Part = {
		Create = function(Parent, Material, Reflectance, Transparency, BColor, Name, Size)
			local Part = Create("Part")({
				Parent = Parent,
				Reflectance = Reflectance,
				Transparency = Transparency,
				CanCollide = false,
				Locked = true,
				BrickColor = BrickColor.new(tostring(BColor)),
				Name = Name,
				Size = Size,
				Material = Material
			})
			RemoveOutlines(Part)
			return Part
		end
	},
	Mesh = {
		Create = function(Mesh, Part, MeshType, MeshId, OffSet, Scale)
			local Msh = Create(Mesh)({
				Parent = Part,
				Offset = OffSet,
				Scale = Scale
			})
			if Mesh == "SpecialMesh" then
				Msh.MeshType = MeshType
				Msh.MeshId = MeshId
			end
			return Msh
		end
	},
	Mesh = {
		Create = function(Mesh, Part, MeshType, MeshId, OffSet, Scale)
			local Msh = Create(Mesh)({
				Parent = Part,
				Offset = OffSet,
				Scale = Scale
			})
			if Mesh == "SpecialMesh" then
				Msh.MeshType = MeshType
				Msh.MeshId = MeshId
			end
			return Msh
		end
	},
	Weld = {
		Create = function(Parent, Part0, Part1, C0, C1)
			local Weld = Create("Weld")({
				Parent = Parent,
				Part0 = Part0,
				Part1 = Part1,
				C0 = C0,
				C1 = C1
			})
			return Weld
		end
	},
	Sound = {
		Create = function(id, par, vol, pit)
			coroutine.resume(coroutine.create(function()
				local S = Create("Sound")({
					Volume = vol,
					Pitch = pit or 1,
					SoundId = id,
					Parent = par or workspace
				})
				wait()
				S:play()
				game:GetService("Debris"):AddItem(S, 6)
			end))
		end
	},
	ParticleEmitter = {
		Create = function(Parent, Color1, Color2, LightEmission, Size, Texture, Transparency, ZOffset, Accel, Drag, LockedToPart, VelocityInheritance, EmissionDirection, Enabled, LifeTime, Rate, Rotation, RotSpeed, Speed, VelocitySpread)
			local fp = Create("ParticleEmitter")({
				Parent = Parent,
				Color = ColorSequence.new(Color1, Color2),
				LightEmission = LightEmission,
				Size = Size,
				Texture = Texture,
				Transparency = Transparency,
				ZOffset = ZOffset,
				Acceleration = Accel,
				Drag = Drag,
				LockedToPart = LockedToPart,
				VelocityInheritance = VelocityInheritance,
				EmissionDirection = EmissionDirection,
				Enabled = Enabled,
				Lifetime = LifeTime,
				Rate = Rate,
				Rotation = Rotation,
				RotSpeed = RotSpeed,
				Speed = Speed,
				VelocitySpread = VelocitySpread
			})
			return fp
		end
	}
}
function RemoveOutlines(part)
	part.TopSurface, part.BottomSurface, part.LeftSurface, part.RightSurface, part.FrontSurface, part.BackSurface = 10, 10, 10, 10, 10, 10
end
function CreatePart(FormFactor, Parent, Material, Reflectance, Transparency, BColor, Name, Size)
	local Part = Create("Part")({
		formFactor = FormFactor,
		Parent = Parent,
		Reflectance = Reflectance,
		Transparency = Transparency,
		CanCollide = false,
		Locked = true,
		BrickColor = BrickColor.new(tostring(BColor)),
		Name = Name,
		Size = Size,
		Material = Material
	})
	RemoveOutlines(Part)
	return Part
end
function CreateMesh(Mesh, Part, MeshType, MeshId, OffSet, Scale)
	local Msh = Create(Mesh)({
		Parent = Part,
		Offset = OffSet,
		Scale = Scale
	})
	if Mesh == "SpecialMesh" then
		Msh.MeshType = MeshType
		Msh.MeshId = MeshId
	end
	return Msh
end
function CreateWeld(Parent, Part0, Part1, C0, C1)
	local Weld = Create("Weld")({
		Parent = Parent,
		Part0 = Part0,
		Part1 = Part1,
		C0 = C0,
		C1 = C1
	})
	return Weld
end


-------------------------------------------------------
--Start Effect Function--
-------------------------------------------------------
EffectModel = Instance.new("Model", char)
Effects = {
  Block = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay, Type)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("BlockMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      if Type == 1 or Type == nil then
        table.insert(Effects, {
          prt,
          "Block1",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      elseif Type == 2 then
        table.insert(Effects, {
          prt,
          "Block2",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      else
        table.insert(Effects, {
          prt,
          "Block3",
          delay,
          x3,
          y3,
          z3,
          msh
        })
      end
    end
  },
  Sphere = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "Sphere", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Cylinder = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("CylinderMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Wave = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://20329976", Vector3.new(0, 0, 0), Vector3.new(x1 / 60, y1 / 60, z1 / 60))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3 / 60,
        y3 / 60,
        z3 / 60,
        msh
      })
    end
  },
  Ring = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://3270017", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
  Break = {
    Create = function(brickcolor, cframe, x1, y1, z1)
      local prt = CFuncs.Part.Create(EffectModel, "Neon", 0, 0, brickcolor, "Effect", Vector3.new(0.5, 0.5, 0.5))
      prt.Anchored = true
      prt.CFrame = cframe * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "Sphere", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      local num = math.random(10, 50) / 1000
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Shatter",
        num,
        prt.CFrame,
        math.random() - math.random(),
        0,
        math.random(50, 100) / 100
      })
    end
  },
Spiral = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://1051557", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  },
Push = {
    Create = function(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
      local prt = CFuncs.Part.Create(EffectModel, "SmoothPlastic", 0, 0, brickcolor, "Effect", Vector3.new())
      prt.Anchored = true
      prt.CFrame = cframe
      local msh = CFuncs.Mesh.Create("SpecialMesh", prt, "FileMesh", "rbxassetid://437347603", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
      game:GetService("Debris"):AddItem(prt, 10)
      table.insert(Effects, {
        prt,
        "Cylinder",
        delay,
        x3,
        y3,
        z3,
        msh
      })
    end
  }
}
function part(formfactor ,parent, reflectance, transparency, brickcolor, name, size)
	local fp = IT("Part")
	fp.formFactor = formfactor 
	fp.Parent = parent
	fp.Reflectance = reflectance
	fp.Transparency = transparency
	fp.CanCollide = false 
	fp.Locked = true
	fp.BrickColor = brickcolor
	fp.Name = name
	fp.Size = size
	fp.Position = tors.Position 
	RemoveOutlines(fp)
	fp.Material = "SmoothPlastic"
	fp:BreakJoints()
	return fp 
end 
 
function mesh(Mesh,part,meshtype,meshid,offset,scale)
	local mesh = IT(Mesh) 
	mesh.Parent = part
	if Mesh == "SpecialMesh" then
		mesh.MeshType = meshtype
	if meshid ~= "nil" then
		mesh.MeshId = "http://www.roblox.com/asset/?id="..meshid
		end
	end
	mesh.Offset = offset
	mesh.Scale = scale
	return mesh
end

function Magic(bonuspeed, type, pos, scale, value, color, MType)
	local type = type
	local rng = Instance.new("Part", char)
	rng.Anchored = true
	rng.BrickColor = color
	rng.CanCollide = false
	rng.FormFactor = 3
	rng.Name = "Ring"
	rng.Material = "Neon"
	rng.Size = Vector3.new(1, 1, 1)
	rng.Transparency = 0
	rng.TopSurface = 0
	rng.BottomSurface = 0
	rng.CFrame = pos
	local rngm = Instance.new("SpecialMesh", rng)
	rngm.MeshType = MType
	rngm.Scale = scale
	local scaler2 = 1
	if type == "Add" then
		scaler2 = 1 * value
	elseif type == "Divide" then
		scaler2 = 1 / value
	end
	coroutine.resume(coroutine.create(function()
		for i = 0, 10 / bonuspeed, 0.1 do
			swait()
			if type == "Add" then
				scaler2 = scaler2 - 0.01 * value / bonuspeed
			elseif type == "Divide" then
				scaler2 = scaler2 - 0.01 / value * bonuspeed
			end
			rng.Transparency = rng.Transparency + 0.01 * bonuspeed
			rngm.Scale = rngm.Scale + Vector3.new(scaler2 * bonuspeed, scaler2 * bonuspeed, scaler2 * bonuspeed)
		end
		rng:Destroy()
	end))
end

function Eviscerate(dude)
	if dude.Name ~= char then
		--local bgf = IT("BodyGyro", dude.Head)
		--bgf.CFrame = bgf.CFrame * CFrame.fromEulerAnglesXYZ(Rad(-90), 0, 0)
		--local val = IT("BoolValue", dude)
		--val.Name = "IsHit"
		local ds = coroutine.wrap(function()
			--dude:WaitForChild("Head"):BreakJoints()
			wait(0.5)
			target = nil
			coroutine.resume(coroutine.create(function()
				for i, v in pairs(dude:GetChildren()) do
					if v:IsA("Accessory") then
						
					end
					if v:IsA("Humanoid") then
						
					end
					if v:IsA("CharacterMesh") then
						
					end
					if v:IsA("Model") then
						
					end
					if v:IsA("Part") or v:IsA("MeshPart") then
						for x, o in pairs(v:GetChildren()) do
							if o:IsA("Decal") then
								
							end
						end
						coroutine.resume(coroutine.create(function()
							v.Material = "Neon"
							v.CanCollide = false
							local PartEmmit1 = IT("ParticleEmitter", reye)
							PartEmmit1.LightEmission = 1
							PartEmmit1.Texture = "rbxassetid://284205403"
							PartEmmit1.Color = ColorSequence.new(maincolor.Color)
							PartEmmit1.Rate = 150
							PartEmmit1.Lifetime = NumberRange.new(1)
							PartEmmit1.Size = NumberSequence.new({
								NumberSequenceKeypoint.new(0, 0.75, 0),
								NumberSequenceKeypoint.new(1, 0, 0)
							})
							PartEmmit1.Transparency = NumberSequence.new({
								NumberSequenceKeypoint.new(0, 0, 0),
								NumberSequenceKeypoint.new(1, 1, 0)
							})
							PartEmmit1.Speed = NumberRange.new(0, 0)
							PartEmmit1.VelocitySpread = 30000
							PartEmmit1.Rotation = NumberRange.new(-500, 500)
							PartEmmit1.RotSpeed = NumberRange.new(-500, 500)
							--local BodPoss = IT("BodyPosition", v)
							BodPoss.P = 3000
							BodPoss.D = 1000
							BodPoss.maxForce = Vector3.new(50000000000, 50000000000, 50000000000)
							BodPoss.position = v.Position + Vector3.new(Mrandom(-15, 15), Mrandom(-15, 15), Mrandom(-15, 15))
							v.Color = maincolor.Color
							coroutine.resume(coroutine.create(function()
								for i = 0, 49 do
									swait(1)
									--v.Transparency = v.Transparency + 0.08
								end
								wait(0.5)
								PartEmmit1.Enabled = false
								wait(3)
								--v:Destroy()
								--dude:Destroy()
							end))
						end))
					end
				end
			end))
		end)
		ds()
	end
end

function FindNearestHead(Position, Distance, SinglePlayer)
	if SinglePlayer then
		return Distance > (SinglePlayer.Torso.CFrame.p - Position).magnitude
	end
	local List = {}
	for i, v in pairs(workspace:GetChildren()) do
		if v:IsA("Model") and v:findFirstChild("Head") and v ~= char and Distance >= (v.Head.Position - Position).magnitude then
			table.insert(List, v)
		end
	end
	return List
end

function Aura(bonuspeed, FastSpeed, type, pos, x1, y1, z1, value, color, outerpos, MType)
	local type = type
	local rng = Instance.new("Part", char)
	rng.Anchored = true
	rng.BrickColor = color
	rng.CanCollide = false
	rng.FormFactor = 3
	rng.Name = "Ring"
	rng.Material = "Neon"
	rng.Size = Vector3.new(1, 1, 1)
	rng.Transparency = 0
	rng.TopSurface = 0
	rng.BottomSurface = 0
	rng.CFrame = pos
	rng.CFrame = rng.CFrame + rng.CFrame.lookVector * outerpos
	local rngm = Instance.new("SpecialMesh", rng)
	rngm.MeshType = MType
	rngm.Scale = Vector3.new(x1, y1, z1)
	local scaler2 = 1
	local speeder = FastSpeed
	if type == "Add" then
		scaler2 = 1 * value
	elseif type == "Divide" then
		scaler2 = 1 / value
	end
	coroutine.resume(coroutine.create(function()
		for i = 0, 10 / bonuspeed, 0.1 do
			swait()
			if type == "Add" then
				scaler2 = scaler2 - 0.01 * value / bonuspeed
			elseif type == "Divide" then
				scaler2 = scaler2 - 0.01 / value * bonuspeed
			end
			speeder = speeder - 0.01 * FastSpeed * bonuspeed
			rng.CFrame = rng.CFrame + rng.CFrame.lookVector * speeder * bonuspeed
			rng.Transparency = rng.Transparency + 0.01 * bonuspeed
			rngm.Scale = rngm.Scale + Vector3.new(scaler2 * bonuspeed, scaler2 * bonuspeed, 0)
		end
		rng:Destroy()
	end))
end



BTAUNT = Instance.new("Sound", char.Head)
BTAUNT.SoundId = "http://www.roblox.com/asset/?id=1700344165"
BTAUNT.Volume = 19
BTAUNT.Pitch = 1
BTAUNT.Looped = true

BTAUNT2 = Instance.new("Sound", char)
BTAUNT2.SoundId = "http://www.roblox.com/asset/?id=1394058517"
BTAUNT2.Volume = 20
BTAUNT2.Pitch = 1
BTAUNT2.Looped = true

BTAUNT3 = Instance.new("Sound", char)
BTAUNT3.SoundId = "http://www.roblox.com/asset/?id=1090127517"
BTAUNT3.Volume = 2
BTAUNT3.Pitch = 1
BTAUNT3.Looped = true

BTAUNT4 = Instance.new("Sound", char.Torso)
BTAUNT4.SoundId = "http://www.roblox.com/asset/?id=1470848774"
BTAUNT4.Volume = 5
BTAUNT4.Pitch = 3
BTAUNT4.Looped = true

BTAUNT5 = Instance.new("Sound", char.Torso)
BTAUNT5.SoundId = "http://www.roblox.com/asset/?id=1470848774"
BTAUNT5.Volume = 5
BTAUNT5.Pitch = 1
BTAUNT5.Looped = true

TEST = Instance.new("Sound", tors)
TEST.SoundId = "http://www.roblox.com/asset/?id=636494529"
TEST.Volume = 25
TEST.Pitch = 1
TEST.Looped = false
-------------------------------------------------------
--End Effect Function--
-------------------------------------------------------

function CreateMesh(MESH, PARENT, MESHTYPE, MESHID, TEXTUREID, SCALE, OFFSET)
	local NEWMESH = IT(MESH)
	if MESH == "SpecialMesh" then
		NEWMESH.MeshType = MESHTYPE
		if MESHID ~= "nil" and MESHID ~= "" then
			NEWMESH.MeshId = "http://www.roblox.com/asset/?id="..MESHID
		end
		if TEXTUREID ~= "nil" and TEXTUREID ~= "" then
			NEWMESH.TextureId = "http://www.roblox.com/asset/?id="..TEXTUREID
		end
	end
	NEWMESH.Offset = OFFSET or VT(0, 0, 0)
	NEWMESH.Scale = SCALE
	NEWMESH.Parent = PARENT
	return NEWMESH
end

function CreatePart(FORMFACTOR, PARENT, MATERIAL, REFLECTANCE, TRANSPARENCY, BRICKCOLOR, NAME, SIZE)
	local NEWPART = IT("Part")
	NEWPART.formFactor = FORMFACTOR
	NEWPART.Reflectance = REFLECTANCE
	NEWPART.Transparency = TRANSPARENCY
	NEWPART.CanCollide = false
	NEWPART.Locked = true
	NEWPART.BrickColor = BRICKC(tostring(BRICKCOLOR))
	NEWPART.Name = NAME
	NEWPART.Size = SIZE
	NEWPART.Position = Torso.Position
	NEWPART.Material = MATERIAL
	NEWPART:BreakJoints()
	NEWPART.Parent = PARENT
	return NEWPART
end

function MakeForm(PART,TYPE)
	local MSH = nil
	if TYPE == "Cyl" then
		MSH = IT("CylinderMesh",PART)
	elseif TYPE == "Ball" then
		MSH = IT("SpecialMesh",PART)
		MSH.MeshType = "Sphere"
	elseif TYPE == "Wedge" then
		MSH = IT("SpecialMesh",PART)
		MSH.MeshType = "Wedge"
	elseif TYPE == "Block" then
		MSH = IT("SpecialMesh",PART)
		MSH.MeshType = "Brick"
	end
	return MSH
end

function Cso(ID, PARENT, VOLUME, PITCH)
	local NSound = nil
	coroutine.resume(coroutine.create(function()
		NSound = IT("Sound", PARENT)
		NSound.Volume = VOLUME
		NSound.Pitch = PITCH
		NSound.SoundId = "http://www.roblox.com/asset/?id="..ID
		swait()
		NSound:play()
		game:GetService("Debris"):AddItem(NSound, 50)
	end))
	return NSound
end
function CameraEnshaking(Length, Intensity)
	coroutine.resume(coroutine.create(function()
		local intensity = 1 * Intensity
		local rotM = 0.01 * Intensity
		for i = 0, Length, 0.1 do
			swait()
			intensity = intensity - 0.05 * Intensity / Length
			rotM = rotM - 5.0E-4 * Intensity / Length
			hum.CameraOffset = Vector3.new(Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity)))
			cam.CFrame = cam.CFrame * CF(Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity)), Rad(Mrandom(-intensity, intensity))) * Euler(Rad(Mrandom(-intensity, intensity)) * rotM, Rad(Mrandom(-intensity, intensity)) * rotM, Rad(Mrandom(-intensity, intensity)) * rotM)
		end
		hum.CameraOffset = Vector3.new(0, 0, 0)
	end))
end


function Sink(position,radius)
	for i,v in ipairs(workspace:GetChildren()) do
	if v:FindFirstChild("Hit2By"..plr.Name) == nil then
		local body = v:GetChildren()
			for part = 1, #body do
				if(v:FindFirstChild("Hit2By"..plr.Name) == nil and (body[part].ClassName == "Part" or body[part].ClassName == "MeshPart") and v ~= char) then
					if(body[part].Position - position).Magnitude < radius then
						if v.ClassName == "Model" then
							v:FindFirstChildOfClass("Humanoid").Name = "Humanoid"
							if v:FindFirstChild("Humanoid") then
								local defence = Instance.new("BoolValue",v)
								defence.Name = ("Hit2By"..plr.Name)
								if v.Humanoid.Health ~= 0 then
									local TORS = v:FindFirstChild("HumanoidRootPart") or v:FindFirstChild("Torso") or v:FindFirstChild("UpperTorso")
									if TORS ~= nil then
										local HITFLOOR2, HITPOS2 = Raycast(TORS.Position, (CF(TORS.Position, TORS.Position + Vector3.new(0, -1, 0))).lookVector, 25 * TORS.Size.Y/2, v)
										coroutine.resume(coroutine.create(function()
											if HITFLOOR2 ~= nil then
												TORS.Anchored = true
												local Hole2 = CreatePart(3, EffectModel, "Neon", 0, 0, "Really black", "Hole", Vector3.new(TORS.Size.X*4,0,TORS.Size.X*4))
												Hole2.Color = Color3.new(0,0,0)
												local MESH = MakeForm(Hole2,"Cyl")
												MESH.Scale = Vector3.new(0,1,0)
												Hole2.CFrame = CF(HITPOS2)
												for i = 1, 10 do
													swait()
													MESH.Scale = MESH.Scale + Vector3.new(0.1,0,0.1)
												end
												--Cso("160440683", v:FindFirstChild("Head"), 10, .8)
												Cso("154955269", v:FindFirstChild("Head"), 10, 1)
												repeat
													swait()
													TORS.CFrame = TORS.CFrame * CF(0,-0.1,0)
													--MESH.Scale = MESH.Scale + Vector3.new(0,1.6,0)
												until TORS.Position.Y<position.Y-4

												for i = 1, 10 do
													swait()
													MESH.Scale = MESH.Scale - Vector3.new(0.1,0,0.1)
												end
												Hole2:remove()
											end
										end))
									end
								end
							end
						end
						--body[part].Velocity = CFrame.new(position,body[part].Position).lookVector*5*maxstrength
					end
				end
			end
		end	
	end
end
function Trail(Part)
	local TRAIL = Part:Clone()
	TRAIL.CanCollide = false
	TRAIL.Anchored = true
	TRAIL.Parent = EffectModel
	TRAIL.Name = "Trail"
	local TRANS = Part.Transparency
	coroutine.resume(coroutine.create(function()
		for i = 1, 20 do
			swait()
			TRAIL.Transparency = TRAIL.Transparency + ((1-TRANS)/20)
		end
		TRAIL:remove()
	end))
end
function getRegion(point,range,ignore)
    return workspace:FindPartsInRegion3WithIgnoreList(Region3.new(point-Vector3.new(1,1,1)*range/2,point+Vector3.new(1,1,1)*range/2),ignore,100)
end

function GetTorso(char)
	return char:FindFirstChild'Torso' or char:FindFirstChild'UpperTorso' or char:FindFirstChild'LowerTorso' or char:FindFirstChild'HumanoidRootPart'
end

local M = {C=math.cos,R=math.rad,S=math.sin,P=math.pi,RNG=math.random,MRS=math.randomseed,H=math.huge,RRNG = function(min,max,div) return math.rad(math.random(min,max)/(div or 1)) end}


function CreateSound(ID, PARENT, VOLUME, PITCH)
	local NSound = nil
	coroutine.resume(coroutine.create(function()
		NSound = Instance.new("Sound", PARENT)
		NSound.Volume = VOLUME
		NSound.Pitch = PITCH
		NSound.SoundId = "http://www.roblox.com/asset/?id="..ID
		swait()
		NSound:play()
		game:GetService("Debris"):AddItem(NSound, 10)
	end))
	return NSound
end

-------------------------------------------------------
--End Important Functions--
-------------------------------------------------------





-------------------------------------------------------
--Start Customization--
-------------------------------------------------------
local Player_Size = 1.7
if Player_Size ~= 0 then
root.Size = root.Size * Player_Size
tors.Size = tors.Size * Player_Size
hed.Size = hed.Size * Player_Size
ra.Size = ra.Size * Player_Size
la.Size = la.Size * Player_Size
rl.Size = rl.Size * Player_Size
ll.Size = ll.Size * Player_Size
----------------------------------------------------------------------------------
rootj.Parent = root
neck.Parent = tors
RW.Parent = tors
LW.Parent = tors
RH.Parent = tors
LH.Parent = tors
----------------------------------------------------------------------------------
rootj.C0 = RootCF * CF(0 * Player_Size, 0 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0))
rootj.C1 = RootCF * CF(0 * Player_Size, 0 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0))
neck.C0 = necko * CF(0 * Player_Size, -1 * Player_Size, 0 + ((1 * Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0))
neck.C1 = CF(0 * Player_Size, 0 * Player_Size, 0 * Player_Size) * angles(Rad(-90), Rad(0), Rad(180))
RW.C0 = CF(3 * Player_Size, 0.3 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0)) --* RIGHTSHOULDERC0
LW.C0 = CF(3 * Player_Size, -0.3 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(0)) --* LEFTSHOULDERC0
----------------------------------------------------------------------------------
RH.C0 = CF(0.3 * Player_Size, -1 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
LH.C0 = CF(-0.3 * Player_Size, -1 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(-90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
RH.C1 = CF(0.7 * Player_Size, 0.7 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
LH.C1 = CF(-0.7 * Player_Size, 0.7 * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(-90), Rad(0)) * angles(Rad(0), Rad(0), Rad(0))
--hat.Parent = Character
end
----------------------------------------------------------------------------------
----------------------------------------------------------------------------------
local equipped = false
local idle = 0
local change = 1
local val = 0
local toim = 0
local idleanim = 0.4
local sine = 0
local Sit = 1
----------------------------------------------------------------------------------
hum.WalkSpeed = 8
hum.JumpPower = 57
hum.Animator.Parent = nil
----------------------------------------------------------------------------------
local Hole = CreatePart(3, EffectModel, "Neon", 0, 0, "Really black", "Hole", Vector3.new(5,0,5))
local MESH = MakeForm(Hole,"Cyl")

		--local humanoid = char:WaitForChild("Humanoid")	
		--humanoid.HipHeight = 2

-------------------------------------------------------
--End Customization--
-------------------------------------------------------
local Blobby = Instance.new("Part", char)
Blobby.Name = "Blob"
Blobby.CanCollide = false
Blobby.BrickColor = BrickColor.new("Deep orange")
Blobby.Transparency = 0
Blobby.Material = "Neon"
Blobby.Size = Vector3.new(1, 1, 2)
Blobby.TopSurface = Enum.SurfaceType.Smooth
Blobby.BottomSurface = Enum.SurfaceType.Smooth



local M2 = Instance.new("SpecialMesh")
M2.Parent = Blobby
M2.MeshId = "rbxassetid://0"
M2.Scale = Vector3.new(0.015, 0.015, 0.015)

--[[local naeeym2 = Instance.new("BillboardGui",char)
naeeym2.AlwaysOnTop = true
naeeym2.Size = UDim2.new(5,35,2,15)
naeeym2.StudsOffset = Vector3.new(0, 3.5, 0)
naeeym2.Adornee = hed
naeeym2.Name = "Name"
--naeeym2.PlayerToHideFrom = Player
local tecks2 = Instance.new("TextLabel",naeeym2)
tecks2.BackgroundTransparency = 1
tecks2.TextScaled = true
tecks2.BorderSizePixel = 0
tecks2.Text = "Fight Me"
tecks2.Font = Enum.Font.Bodoni
tecks2.TextSize = 30
tecks2.TextStrokeTransparency = 0
tecks2.TextColor3 = Color3.new(0, 0, 0)
tecks2.TextStrokeColor3 = Color3.new(1, 1, 1)
tecks2.Size = UDim2.new(1,0,0.5,0)
tecks2.Parent = naeeym2]]
----------------------------------------------------------------------------------
local AddInstance = function(Object, ...)
local Obj = Instance.new(Object)
for i,v in next,(...) do
Obj[i] = v
end
return Obj
end
----------------------------------------------------
i_new = Instance.new

function nooutline(part)
		part.TopSurface,part.BottomSurface,part.LeftSurface,part.RightSurface,part.FrontSurface,part.BackSurface = 10,10,10,10,10,10
	end
function perts(formfactor,parent,material,reflectance,transparency,brickcolor,name,size)
		local fp=it("Part")
		fp.formFactor=formfactor
		fp.Parent=parent
		fp.Reflectance=reflectance
		fp.Transparency=transparency
		fp.CanCollide=false
		fp.Locked=true
		fp.BrickColor=BrickColor.new(tostring(brickcolor))
		fp.Name=name
		fp.Size=size
		fp.Position=Character.Torso.Position
		nooutline(fp)
		fp.Material=material
		fp:BreakJoints()
		return fp
	end
	
	function mush(Mesh,part,meshtype,meshid,offset,scale)
		local mush=it(Mesh)
		mush.Parent=part
		if Mesh=="SpecialMesh" then
		mush.MeshType=meshtype
		mush.MeshId=meshid
		end
		mush.Offset=offset
		mush.Scale=scale
		return mush
	end
	
	function wald2(parent,part0,part1,c0,c1)
		local wald2=it("Weld")
		wald2.Parent=parent
		wald2.Part0=part0
		wald2.Part1=part1
		wald2.C0=c0
		wald2.C1=c1
		return wald2
	end

function mesh(Mesh, part, meshtype, meshid, offset, scale)
	local mesh = i_new(Mesh)
	mesh.Parent = part
	if Mesh == "SpecialMesh" then
		mesh.MeshType = meshtype
		mesh.MeshId = meshid
	end
	mesh.Offset = offset
	mesh.Scale = scale
	return mesh
end

Head.Transparency = 1



Eyes = Instance.new("Decal",_Face)
Eyes.Texture = "rbxassetid://0"

Mouth = Instance.new("Decal",_Face)
Mouth.Texture = "rbxassetid://0"

Mouth.Color3 = Color3.new(1,0.8,0)


local Right_Arm = Character:FindFirstChild("Right Arm")
local Left_Arm = Character:FindFirstChild("Left Arm")
local Right_Leg = Character:FindFirstChild("Right Leg")
local Left_Leg = Character:FindFirstChild("Left Leg")
local Right_Shoulder = Torso:FindFirstChild("Right Shoulder")
local Left_Shoulder = Torso:FindFirstChild("Left Shoulder")
local Right_Hip = Torso:FindFirstChild("Right Hip")
local Left_Hip = Torso:FindFirstChild("Left Hip")

Clothes = Instance.new("Model",Character)
Clothes.Name = "Clothing"



local _Head2 = Instance.new("Part",Head)
_Head2.Name = "_Head2"
_Head2.Shape = Enum.PartType.Block
_Head2.CanCollide = false
_Head2.Color = Color3.new(0,0,0)
_Head2.Transparency = 0
_Head2.Material = "Metal"
_Head2.Size = Vector3.new(1.4, 1.2, 1)
_Head2.TopSurface = Enum.SurfaceType.Smooth
_Head2.BottomSurface = Enum.SurfaceType.Smooth	local Weld = Instance.new("Weld", _Head2)
Weld.Part0 = Head
Weld.Part1 = _Head2
Weld.C1 = CFrame.new(0,-0.2,0.2)
_HeadMesh = Instance.new("SpecialMesh",_Head2)
_HeadMesh.MeshType = "Sphere"
_HeadMesh.Scale = Vector3.new(0,0,0)

local Horn = Instance.new("Part",Head)
Horn.Name = "Horn"
Horn.Shape = Enum.PartType.Ball
Horn.CanCollide = false
Horn.Color = Color3.new(0,0,0)
Horn.Transparency = 0
Horn.Material = "SmoothPlastic"
Horn.Size = Vector3.new(0.1, 0.1, 0.1)
Horn.TopSurface = Enum.SurfaceType.Smooth
Horn.BottomSurface = Enum.SurfaceType.Smooth	
local Weld = Instance.new("Weld", Horn)
Weld.Part0 = Head
Weld.Part1 = Horn
Weld.C1 = CFrame.new(-1.05,-0.6,0.1)*CFrame.fromEulerAnglesXYZ(math.rad(-5),math.rad(5),math.rad(-15))
HornMesh = Instance.new("FileMesh",Horn)
HornMesh.MeshId = "http://www.roblox.com/asset/?id=0"
HornMesh.Scale = Vector3.new(1,0.8,0.8)

local Horn = Instance.new("Part",Head)
Horn.Name = "Horn"
Horn.Shape = Enum.PartType.Ball
Horn.CanCollide = false
Horn.Color = Color3.new(0,0,0)
Horn.Transparency = 0
Horn.Material = "SmoothPlastic"
Horn.Size = Vector3.new(0.1, 0.1, 0.1)
Horn.TopSurface = Enum.SurfaceType.Smooth
Horn.BottomSurface = Enum.SurfaceType.Smooth	
local Weld = Instance.new("Weld", Horn)
Weld.Part0 = Head
Weld.Part1 = Horn
Weld.C1 = CFrame.new(-1.05,-0.6,-0.1)*CFrame.fromEulerAnglesXYZ(math.rad(5),math.rad(175),math.rad(15))
HornMesh = Instance.new("FileMesh",Horn)
HornMesh.MeshId = "http://www.roblox.com/asset/?id=0"
HornMesh.Scale = Vector3.new(1,0.8,0.8)



		local Reaper2 = AddInstance("Part",{
			Parent = tors,
			CFrame = tors.CFrame,
			formFactor = "Symmetric",
			Size = Vector3.new(0, 0, 0),
			CanCollide = false,
			TopSurface = "Smooth",
			BottomSurface = "Smooth",
			Locked = true,
		})
		local Weld = AddInstance("Weld",{
			Parent = Reaper2,
			Part0 = tors,
			C0 = CFrame.new(0, 0, 0)*CFrame.Angles(0, 0, 0),
			Part1 = Reaper2,
		})
		local Mesh = AddInstance("SpecialMesh",{
			Parent = Reaper2,
			MeshId = "rbxassetid://0",
			TextureId = "rbxassetid://0",
			Scale = Vector3.new(1.8, 1, 1.7),
			VertexColor = Vector3.new(0, 0, 0),
		})


local Blobby = Instance.new("Part", char)
Blobby.Name = "Blob"
Blobby.CanCollide = false
Blobby.BrickColor = BrickColor.new("Deep orange")
Blobby.Transparency = 0
Blobby.Material = "Neon"
Blobby.Size = Vector3.new(1, 1, 2)
Blobby.TopSurface = Enum.SurfaceType.Smooth
Blobby.BottomSurface = Enum.SurfaceType.Smooth

local Weld = Instance.new("Weld", Blobby)
Weld.Part0 = tors
Weld.Part1 = Blobby
Weld.C1 = CFrame.new(0, -0.8, 0.7)
Weld.C0 = CFrame.Angles(Rad(0),0,0.2)

local M2 = Instance.new("SpecialMesh")
M2.Parent = Blobby
M2.MeshId = "rbxassetid://0"
M2.Scale = Vector3.new(0.005, 0.005, 0.005)

local Particle = IT("ParticleEmitter",nil)
Particle.Enabled = false
--Particle.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0,1),NumberSequenceKeypoint.new(0.3,0.95),NumberSequenceKeypoint.new(1,1)})
Particle.LightEmission = 0.5
Particle.Rate = 150
Particle.ZOffset = 1
Particle.Rotation = NumberRange.new(-180, 180)
Particle.RotSpeed = NumberRange.new(-180, 180)
Particle.Texture = "http://www.roblox.com/asset/?id="
Particle.Color = ColorSequence.new(Color3.new(0,0,0),Color3.new(0,0,0))

function ParticleEmitter(Table)
	local PRTCL = Particle:Clone()
	local Speed = Table.Speed or 5
	local Drag = Table.Drag or 0
	local Size1 = Table.Size1 or 1
	local Size2 = Table.Size2 or 5
	local Lifetime1 = Table.Lifetime1 or 1
	local Lifetime2 = Table.Lifetime2 or 1.5
	local Parent = Table.Parent or tors
	local Emit = Table.Emit or 100
	local Offset = Table.Offset or 360
	local Acel = Table.Acel or Vector3.new(0,-50,0)
	local Enabled = Table.Enabled or false
	PRTCL.Parent = Parent
	PRTCL.Size = NumberSequence.new(Size1,Size2)
	PRTCL.Lifetime = NumberRange.new(Lifetime1,Lifetime2)
	PRTCL.Speed = NumberRange.new(Speed)
	PRTCL.VelocitySpread = Offset
	PRTCL.Drag = Drag
	PRTCL.Acceleration = Acel
	if Enabled == false then
		PRTCL:Emit(Emit)
		game:GetService("Debris"):AddItem(PRTCL,Lifetime2)
	else
		PRTCL.Enabled = true
	end
	return PRTCL
end
local PRT = ParticleEmitter({Speed = 0.3, Drag = 3, Size1 = 0.5, Size2 = 0.3, Lifetime1 = 0.2, Lifetime2 = 1, Parent = rl, Emit = 900, Offset = 360, Enabled = true})
--PRT.LockedToPart = true
local PRT = ParticleEmitter({Speed = 0.3, Drag = 3, Size1 = 0.5, Size2 = 0.3, Lifetime1 = 0.2, Lifetime2 = 1, Parent = ll, Emit = 900, Offset = 360, Enabled = true})
--PRT.LockedToPart = true
local PRT = ParticleEmitter({Speed = 0.3, Drag = 3, Size1 = 0.5, Size2 = 0.3, Lifetime1 = 0.3, Lifetime2 = 1.5, Parent = tors, Emit = 900, Offset = 360, Enabled = true})
--PRT.LockedToPart = true
local PRT = ParticleEmitter({Speed = 0.3, Drag = 3, Size1 = 0.5, Size2 = 0.3, Lifetime1 = 0.2, Lifetime2 = 1, Parent = ra, Emit = 900, Offset = 360, Enabled = true})
--PRT.LockedToPart = true
local PRT = ParticleEmitter({Speed = 0.3, Drag = 3, Size1 = 0.5, Size2 = 0.3, Lifetime1 = 0.2, Lifetime2 = 1, Parent = la, Emit = 900, Offset = 360, Enabled = true})
--PRT.LockedToPart = true


char.Torso.roblox.Texture = "http://www.roblox.com/asset/?id=6477494"

leg1 = Instance.new("SpecialMesh", LeftLeg)
leg1.MeshType = "FileMesh"
leg1.Scale = Vector3.new(1, 1, 1)
leg1.MeshId,leg1.TextureId = 'http://www.roblox.com/asset/?id=68241558','http://www.roblox.com/asset/?id=6477494'

leg2 = Instance.new("CharacterMesh",Character)
leg2.MeshId = 319336155
leg2.BodyPart = "RightLeg"

torso1 = Instance.new("CharacterMesh",Character)
torso1.MeshId = 717339870
torso1.BodyPart = "Torso"

arm1 = Instance.new("CharacterMesh",Character)
arm1.MeshId = 27111419
arm1.BodyPart = "LeftArm"

arm2 = Instance.new("CharacterMesh",Character)
arm2.MeshId = 279174886
arm2.BodyPart = "RightArm"


-------------------------------------------------------
wait(1)
plr = game.Players.LocalPlayer
char = plr.Character
mouse = plr:GetMouse()
whitecolor = Color3.new(255,255,1)
epicmode = false
normal = true
for i,v in pairs(char:GetChildren()) do
   if v.ClassName == "Shirt" or v.ClassName == "Pants" or v.ClassName == "ShirtGraphic" then
      v:Destroy()
     end
end
local shirt = Instance.new("Shirt",char)
shirt.ShirtTemplate = "rbxassetid://0"
local pants = Instance.new("Pants",char)
pants.PantsTemplate = "rbxassetid://"
local bdycolors = char["Body Colors"]
bdycolors.HeadColor3 = whitecolor
bdycolors.LeftArmColor3 = whitecolor
bdycolors.LeftLegColor3 = whitecolor
bdycolors.RightArmColor3 = whitecolor
bdycolors.RightLegColor3 = whitecolor
bdycolors.TorsoColor3 = whitecolor
for i,v in pairs(char:GetChildren()) do
    if v.ClassName == "Hat" or v.ClassName == "Accessory" then
        
    end
end


local BC = Character["Body Colors"]
BC.HeadColor = BrickColor.new("Institutional white")
BC.LeftArmColor = BrickColor.new("Really black")
BC.LeftLegColor = BrickColor.new("Really black")
BC.RightArmColor = BrickColor.new("Really black")
BC.RightLegColor = BrickColor.new("Really black")
BC.TorsoColor = BrickColor.new("Really black")
-------------------------------------------------------
--Start Attacks N Stuff--
-------------------------------------------------------


local naeeym2 = Instance.new("BillboardGui",char)
naeeym2.AlwaysOnTop = true
naeeym2.Size = UDim2.new(5,35,2,35)
naeeym2.StudsOffset = Vector3.new(0,2,0)
naeeym2.Adornee = hed
naeeym2.Name = "Name"

local tecks2 = Instance.new("TextLabel",naeeym2)
tecks2.BackgroundTransparency = 1
tecks2.TextScaled = true
tecks2.BorderSizePixel = 0
tecks2.Text = "scp - 096"
tecks2.Font = "Antique"
tecks2.TextSize = 30
tecks2.TextStrokeTransparency = 0
tecks2.TextColor3 = BrickColor.new('Institutional white').Color
tecks2.TextStrokeColor3 = BrickColor.new('Really black').Color
tecks2.Size = UDim2.new(1,0,0.5,0)
tecks2.Parent = naeeym2
textfag = tecks2
tecks2.Text = "scp - 096"
BTAUNT2:Play()
coroutine.resume(coroutine.create(function()
    while textfag ~= nil do
        swait()
        textfag.Position = UDim2.new(math.random(-0.1,0.1),math.random(-3,3),.05,math.random(-3,3))  
        textfag.Rotation = math.random(-0.1,0.1)
    end
end))

function CreateMagicCircle()
	local sinkhole = IT("Part")
	sinkhole.Size = Vector3.new(0,0,0)
	sinkhole.Parent = EffectModel
	sinkhole.Material = "Plastic"
	sinkhole.Color = Color3.new(0,0,0)
	sinkhole.Anchored = true
	sinkhole.CanCollide = false
	sinkhole.Transparency = 1
	local decal = IT("Decal",sinkhole)
	decal.Face = "Top"
	decal.Texture = "http://www.roblox.com/asset/?id=845483336"
	local decal2 = IT("Decal",sinkhole)
	decal2.Face = "Bottom"
	decal2.Texture = "http://www.roblox.com/asset/?id=2025608249"
	return sinkhole
end
function AoEKill(position,radius)
	for i,v in ipairs(workspace:GetChildren()) do
	if v:FindFirstChild("HitBy"..plr.Name) == nil then
		local body = v:GetChildren()
			for part = 1, #body do
				if(v:FindFirstChild("HitBy"..plr.Name) == nil and (body[part].ClassName == "Part" or body[part].ClassName == "MeshPart") and v ~= char) then
					if(body[part].Position - position).Magnitude < radius then
						if v.ClassName == "Model" then
							if v:FindFirstChild("Humanoid") then
								if v.Humanoid.Health ~= 0 then
									if body[part].Position.Y < position.Y+12 then
										print("Got "..v.Name)
										--local defence = Instance.new("BoolValue",v)
										--defence.Name = ("HitBy"..plr.Name)
										--game:GetService("Debris"):AddItem(defence, 0.01)
										--local TORSO = v:FindFirstChild("HumanoidRootPart") or v:FindFirstChild("Torso") or v:FindFirstChild("UpperTorso")
										--local HEAD = v:FindFirstChild("Head")
										--HEAD:Remove()
									end
								end
							end
						end
						--body[part].Velocity = CFrame.new(position,body[part].Position).lookVector*5*maxstrength
					end
				end
			end
		end
	end
end
function Die()
	attack = true
	for i = 1, 45 do
	swait()
	--Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	--Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	--Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
	RW.C0 = clerp(RW.C0, CFrame.new(1.5, 0.5, 0) * angles(math.rad(165), math.rad(0), math.rad(10)), 0.1)
        LW.C0 = clerp(LW.C0, CFrame.new(-1.5, 0.5, 0) * angles(math.rad(-165), math.rad(0), math.rad(-10)), 0.3)
		for _,v in next, char:GetDescendants() do
			if(v:IsA'DataModelMesh')then
				v.Offset = Vector3.new(math.random(-50,50)/100,math.random(-50,50)/100,math.random(-50,50)/100)
			end
		end
		end
		for _,v in next, char:GetDescendants() do
			if(v:IsA'DataModelMesh')then
				v.Offset = Vector3.new(0,0,0)
			end
		end
		for i = 1, 15 do
	--Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	--Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	--Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
		RW.C0 = clerp(RW.C0, CFrame.new(1.5, 0.5, 0) * angles(math.rad(0), math.rad(0), math.rad(90)), 0.1)
                LW.C0 = clerp(LW.C0, CFrame.new(-1.5, 0.5, 0) * angles(math.rad(-0), math.rad(0), math.rad(-90)), 0.3)
		swait()
		end
	local RING = CreateMagicCircle()
	RING.CFrame = CF(root.Position)*CF(0,-2.8,0)
	for i = 1, 200 do
		swait()
		RING.CFrame = RING.CFrame * angles(Rad(0),Rad(i/15),Rad(0))
		RING.Size = RING.Size + Vector3.new(.6,0,.6)
	end
	AoEKill(RING.Position,RING.Size.X/2)
	coroutine.resume(coroutine.create(function()
		swait(75)
		for i = 1, 50 do
			swait()
			RING.CFrame = RING.CFrame * angles(Rad(0),Rad(-i/2),Rad(0))
			RING.Size = RING.Size - Vector3.new(4,0,4)
		end
		RING:remove()
	end))
	attack = false
end
function sammyvoice()
print('>:3')
	attack = true
	hum.WalkSpeed = 0
        TEST:Remove()
        TEST:Play()
        BTAUNT:stop()
        repeat
	for i = 0,4,0.1 do
		swait()
                TEST.Parent = tors
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 12)) * angles(Rad(0), Rad(0), Rad(20)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-2.5 * Sin(sine / 30)), Rad(0), Rad(-20)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1* Player_Size, 0.5 + 0.05 * Cos(sine / 12)* Player_Size, -0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-76)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1* Player_Size, 0.5 + 0.05 * Cos(sine / 12)* Player_Size, -0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(56)), 0.1)
	end
        until TEST.Playing == false
        TEST:Stop()
        TEST:Play()
        TEST:Remove()
        BTAUNT:play()
		attack = false
		hum.WalkSpeed = 16
	end
function InkyWarp()
	attack = true
	attack = true
	root.Anchored = true
	for i = 0, 4, 0.1 do
		swait()
                --Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, 0 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(25 - 6.5 * Cos(sine / 12)), Rad(0), Rad(20 * Cos(sine / 12))), 0.3)
		RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(25 + 4.5 * Sin(sine / 12))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(7 + 4.5 * Sin(sine / 12)), Rad(0 + 2.5 * Sin(sine / 12)), Rad(-13 - 4.5 * Sin(sine / 12))), 0.1)
	end
	for i = 0, 2, 0.1 do
		swait()
                --Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, -15 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(25 - 6.5 * Cos(sine / 12)), Rad(0), Rad(20 * Cos(sine / 12))), 0.3)
		RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(25 + 4.5 * Sin(sine / 12))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(7 + 4.5 * Sin(sine / 12)), Rad(0 + 2.5 * Sin(sine / 12)), Rad(-13 - 4.5 * Sin(sine / 12))), 0.1)
	end
	for i = 1, 50 do
		swait()
		Trail(Hole)
		MESH.Scale = MESH.Scale - Vector3.new(0.02,0,0.02)
	end
	local ORIGINPOS = root.Position
	root.CFrame = CF(Vector3.new(mouse.Hit.p.X,root.Position.Y,mouse.Hit.p.Z),ORIGINPOS)
	Cso("154955269", tors, 10, .8)
	for i = 1, 50 do
		swait()
		MESH.Scale = MESH.Scale + Vector3.new(0.02,0,0.02)
	end
	for i = 0, 4, 0.1 do
		swait()
                --Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, 0 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(25 - 6.5 * Cos(sine / 12)), Rad(0), Rad(20 * Cos(sine / 12))), 0.3)
		RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(25 + 4.5 * Sin(sine / 12))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(7 + 4.5 * Sin(sine / 12)), Rad(0 + 2.5 * Sin(sine / 12)), Rad(-13 - 4.5 * Sin(sine / 12))), 0.1)
	end
	attack = false
	hum.WalkSpeed = 16
	root.Anchored = false
end
function dead()
	attack = true
	hum.WalkSpeed = 8
	Magic(5, "Add", Mouse.Hit * CFrame.new(0, -2.9, 0), Vector3.new(0, 0, 0), 1, maincolor, "Sphere")
	Magic(10, "Add", Mouse.Hit * CFrame.new(0, -2.9, 0), Vector3.new(0, 0, 0), 2, maincolor, "Sphere")
	Magic(1, "Add", Mouse.Hit, Vector3.new(1, 100000, 1), 0.5, maincolor, "Sphere")
	Magic(1, "Add", Mouse.Hit, Vector3.new(1, 1, 1), 0.75, maincolor, "Sphere")
	--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
coroutine.resume(coroutine.create(function()
    while textfag ~= nil do
        swait()
        textfag.Position = UDim2.new(math.random(-.2,.2),math.random(-3,3),.05,math.random(-3,3))  
        textfag.Rotation = math.random(-3,3)
    end
end))                  
	--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
--Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)

--Effects.Ring.Create(BrickColor.new("Institutional black"), tors.CFrame, 2, 2, 2, 7.6, 7.6, 7.6, 0.03)
	for i, v in pairs(FindNearestHead(Mouse.Hit.p, 14.5)) do
		if v:FindFirstChild("Head") then
			Eviscerate(v)
		end
	end
	attack = false
end
function Chain2()
	if Mouse.Target.Parent ~= char and Mouse.Target.Parent.Parent ~= char and Mouse.Target.Parent:FindFirstChildOfClass("Humanoid") ~= nil then
	local HUM = Mouse.Target.Parent:FindFirstChildOfClass("Humanoid")
	local TORSO = HUM.Parent:FindFirstChild("Torso") or HUM.Parent:FindFirstChild("UpperTorso")
	local HEAD = HUM.Parent:FindFirstChild("Head")
	local RIGHTARM = HUM.Parent:FindFirstChild("Right Arm") or HUM.Parent:FindFirstChild("RightLowerArm")
	local LEFTARM = HUM.Parent:FindFirstChild("Left Arm") or HUM.Parent:FindFirstChild("LeftLowerArm")
	if HEAD and TORSO and HUM.Health > 0 then
	local GYRO = IT("BodyGyro",root)
	GYRO.D = 275
	GYRO.P = 20000
	GYRO.MaxTorque = Vector3.new(0,40000,0)
	attack = true
	hum.WalkSpeed = 0
	local hit,pos,hummie;
	local Hook2 = Part(EffectModel, Color3.new(),Enum.Material.Neon,Vector3.new(.05,.05,.05),root.CFrame,true,false)
	Hook2.Transparency = 1
	local A2 = NewInstance("Attachment",Hook2)
	local B2 = NewInstance("Attachment",la,{Position = Vector3.new(0,-ra.Size.Y/2,0)})
	local Chain2 = NewInstance("Beam",Hook2,{Attachment0 = A2,Attachment1=B2,Color = Color3.fromRGB(138,138,138),FaceCamera=true,LightInfluence=0,Texture="rbxassetid://24419398",TextureLength=5,Transparency=NumberSequence.new(0),TextureSpeed=0,CurveSize0=0,CurveSize1=0,FaceCamera=true,Segments=10,Width0=1,Width1=1})
	for i = 0, 2.3, .1 do
		swait()
		GYRO.cframe = CF(root.Position,TORSO.Position)
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, 0 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(25 - 6.5 * Cos(sine / 12)), Rad(0), Rad(20 * Cos(sine / 12))), 0.3)
				RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(25 + 4.5 * Sin(sine / 12))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
			end
	for i = 0, 5, .1 do
		if(hit)then break end
		swait()
		GYRO.cframe = CF(root.Position,TORSO.Position)
		Hook2.CFrame = TORSO.CFrame
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, 0 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(25 - 6.5 * Cos(sine / 12)), Rad(0), Rad(20 * Cos(sine / 12))), 0.3)
				RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(25 + 4.5 * Sin(sine / 12))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
			end
        chatfunc("your soul is mine.", BrickColor.new("Really black").Color)
        wait(3)
	Cso("464600985", ra, 5, .8)
	GYRO:remove()
	--TORSO:BreakJoints()
	for i = 0, 6, .1 do
		swait()
		Hook2.CFrame = Hook2.CFrame:lerp(tors.CFrame * CF(0, 0, -1), .2)
		if(hit)then hit.CFrame = Hook2.CFrame; hit.Velocity = Vector3.new() 
		end
		if((Hook2.CFrame.p-tors.CFrame.p).magnitude < 2)then
			break
		end
		Chain2.TextureLength = 4
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, 0 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(25 - 6.5 * Cos(sine / 12)), Rad(0), Rad(20 * Cos(sine / 12))), 0.3)
				RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.35 + 0.15 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(25 + 4.5 * Sin(sine / 12))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
			end
		hum.WalkSpeed = 16
		attack = false
		Hook2:Destroy()
		end
	end
end

function Taunt2()
	attack = true
	hum.WalkSpeed = 0
BTAUNT3:Stop()
BTAUNT:Stop()
	Cso("982463837", hed, 10, 1)
	for i = 0, 2, 0.1 do
		swait()
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 12)) * angles(Rad(20), Rad(0), Rad(0)), 0.1)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 6.5 * Sin(sine / 12)), Rad(20), Rad(Mrandom(-15, 25))), 0.1)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 + 0.15 * Cos(sine / 20)* Player_Size, -0.1* Player_Size) * angles(Rad(0), Rad(76), Rad(0)) * angles(Rad(-8.5), Rad(0), Rad(20)), 0.1)
				LH.C0 = clerp(LH.C0, CF(-1.1* Player_Size, -0.9 + 0.15 * Cos(sine / 20)* Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-76), Rad(0)) * angles(Rad(-8.5), Rad(15), Rad(-20)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34) * angles(Rad(-65), Rad(Mrandom(-15, 25)), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, -0.34) * angles(Rad(-65), Rad(Mrandom(-15, 25)), Rad(-13) + la.RotVelocity.Y / 75), 0.15)
	end
	for i = 0, 6, 0.1 do
		swait()
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.7 * Player_Size) * angles(Rad(30), Rad(0), Rad(0)), 0.1)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 6.5 * Sin(sine / 12)), Rad(30), Rad(Mrandom(-45, 25))), 0.1)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, 0.07 * Player_Size, -0.1* Player_Size) * angles(Rad(0), Rad(76), Rad(0)) * angles(Rad(-8.5), Rad(0), Rad(35)), 0.1)
				LH.C0 = clerp(LH.C0, CF(-1.1* Player_Size, 0.07 * Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-76), Rad(0)) * angles(Rad(-8.5), Rad(15), Rad(-30)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34) * angles(Rad(-75), Rad(Mrandom(-35, 35)), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, -0.34) * angles(Rad(-75), Rad(Mrandom(-35, 35)), Rad(-13) + la.RotVelocity.Y / 75), 0.15)
	end
	attack = false
	hum.WalkSpeed = 8
end

function Taunt()
BTAUNT2:Stop()
BTAUNT3:Stop()
	attack = true
	hum.WalkSpeed = 0
	Cso("982463837", hed, 10, 1)
	for i = 0, 2, 0.1 do
		swait()
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 12)) * angles(Rad(20), Rad(0), Rad(0)), 0.1)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 6.5 * Sin(sine / 12)), Rad(20), Rad(Mrandom(-15, 25))), 0.1)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 + 0.15 * Cos(sine / 20)* Player_Size, -0.1* Player_Size) * angles(Rad(0), Rad(76), Rad(0)) * angles(Rad(-8.5), Rad(0), Rad(20)), 0.1)
				LH.C0 = clerp(LH.C0, CF(-1.1* Player_Size, -0.9 + 0.15 * Cos(sine / 20)* Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-76), Rad(0)) * angles(Rad(-8.5), Rad(15), Rad(-20)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34) * angles(Rad(-65), Rad(Mrandom(-15, 25)), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, -0.34) * angles(Rad(-65), Rad(Mrandom(-15, 25)), Rad(-13) + la.RotVelocity.Y / 75), 0.15)
	end
	for i = 0, 6, 0.1 do
		swait()
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.7 * Player_Size) * angles(Rad(30), Rad(0), Rad(0)), 0.1)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 6.5 * Sin(sine / 12)), Rad(30), Rad(Mrandom(-45, 25))), 0.1)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, 0.07 * Player_Size, -0.1* Player_Size) * angles(Rad(0), Rad(76), Rad(0)) * angles(Rad(-8.5), Rad(0), Rad(35)), 0.1)
				LH.C0 = clerp(LH.C0, CF(-1.1* Player_Size, 0.07 * Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-76), Rad(0)) * angles(Rad(-8.5), Rad(15), Rad(-30)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34) * angles(Rad(-75), Rad(Mrandom(-35, 35)), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, -0.34) * angles(Rad(-75), Rad(Mrandom(-35, 35)), Rad(-13) + la.RotVelocity.Y / 75), 0.15)
	end
	attack = false
	hum.WalkSpeed = 49
BTAUNT:Play()
end

function Taunt3()
BTAUNT2:Stop()
BTAUNT:Stop()
	attack = true
	hum.WalkSpeed = 0
	Cso("982463837", hed, 10, 1)
	for i = 0, 2, 0.1 do
		swait()
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 12)) * angles(Rad(20), Rad(0), Rad(0)), 0.1)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 6.5 * Sin(sine / 12)), Rad(20), Rad(Mrandom(-15, 25))), 0.1)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 + 0.15 * Cos(sine / 20)* Player_Size, -0.1* Player_Size) * angles(Rad(0), Rad(76), Rad(0)) * angles(Rad(-8.5), Rad(0), Rad(20)), 0.1)
				LH.C0 = clerp(LH.C0, CF(-1.1* Player_Size, -0.9 + 0.15 * Cos(sine / 20)* Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-76), Rad(0)) * angles(Rad(-8.5), Rad(15), Rad(-20)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34) * angles(Rad(-65), Rad(Mrandom(-15, 25)), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, -0.34) * angles(Rad(-65), Rad(Mrandom(-15, 25)), Rad(-13) + la.RotVelocity.Y / 75), 0.15)
	end
	for i = 0, 6, 0.1 do
		swait()
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.7 * Player_Size) * angles(Rad(30), Rad(0), Rad(0)), 0.1)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 6.5 * Sin(sine / 12)), Rad(30), Rad(Mrandom(-45, 25))), 0.1)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, 0.07 * Player_Size, -0.1* Player_Size) * angles(Rad(0), Rad(76), Rad(0)) * angles(Rad(-8.5), Rad(0), Rad(35)), 0.1)
				LH.C0 = clerp(LH.C0, CF(-1.1* Player_Size, 0.07 * Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-76), Rad(0)) * angles(Rad(-8.5), Rad(15), Rad(-30)), 0.1)
			RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34) * angles(Rad(-75), Rad(Mrandom(-35, 35)), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
			LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, -0.34) * angles(Rad(-75), Rad(Mrandom(-35, 35)), Rad(-13) + la.RotVelocity.Y / 75), 0.15)
	end
	attack = false
	hum.WalkSpeed = 49
BTAUNT3:Play()
end

function THUNDERCLAP()
	attack = true
	for i = 0, 15, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(-25), Rad(0), Rad(0)), 0.3)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-20 - 2.5 * Sin(sine / 20)), Rad(Mrandom(-15, 15)), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(85), Rad(0)) * angles(Rad(-5), Rad(0), Rad(-25)), 0.3)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-85), Rad(0)) * angles(Rad(-5), Rad(0), Rad(25)), 0.3)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-25), Rad(Mrandom(-15, 15)), Rad(65 - 4.5 * Sin(sine / 20))), 0.3)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-25), Rad(Mrandom(-15, 15)), Rad(-65 + 4.5 * Sin(sine / 20))), 0.3)
	end
	CFuncs.Sound.Create("rbxassetid://907528019", root, 1.85, 1)
	for i = 0, 7, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(25), Rad(0), Rad(0)), 0.3)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20 - 2.5 * Sin(sine / 20)), Rad(Mrandom(-15, 15)), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(85), Rad(0)) * angles(Rad(-5), Rad(0), Rad(25)), 0.3)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-85), Rad(0)) * angles(Rad(-5), Rad(0), Rad(-25)), 0.3)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, -0.6* Player_Size) * angles(Rad(85), Rad(Mrandom(-15, 15)), Rad(45 - 4.5 * Sin(sine / 20))), 0.3)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, -0.6* Player_Size) * angles(Rad(85), Rad(Mrandom(-15, 15)), Rad(-45 + 4.5 * Sin(sine / 20))), 0.3)
	end

	Magic(1, "Add", root.CFrame, Vector3.new(50, 100, 50), 4, BrickC("Really black"), "Sphere")
	Magic(1, "Add", root.CFrame, Vector3.new(30, 60, 30), 4, BrickC("Really black"), "Sphere")
	Magic(1, "Add", root.CFrame, Vector3.new(3, 600, 3), 4, BrickC("Really black"), "Sphere")
	for i, v in pairs(FindNearestHead(tors.CFrame.p, 500000)) do
		if v:FindFirstChild("Head") then
			Eviscerate(v)
		end
	end
	CFuncs["Sound"].Create("rbxassetid://138213851", char, 2,1.2)
	CFuncs["Sound"].Create("rbxassetid://239000203", char, 2,1.2)
	CFuncs["Sound"].Create("rbxassetid://919941001", char, 3,1.05)
	for i = 0, 7, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(25), Rad(0), Rad(0)), 0.3)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(20 - 2.5 * Sin(sine / 20)), Rad(Mrandom(-15, 15)), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(85), Rad(0)) * angles(Rad(-5), Rad(0), Rad(25)), 0.3)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-85), Rad(0)) * angles(Rad(-5), Rad(0), Rad(-25)), 0.3)
		RW.C0 = clerp(RW.C0, CF(1* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, -0.6* Player_Size) * angles(Rad(85), Rad(Mrandom(-15, 15)), Rad(-45 - 4.5 * Sin(sine / 20))), 0.3)
		LW.C0 = clerp(LW.C0, CF(-1* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, -0.6* Player_Size) * angles(Rad(85), Rad(Mrandom(-15, 15)), Rad(45 + 4.5 * Sin(sine / 20))), 0.3)
	end
	attack = false
end

function eee()
	attack = true
BTAUNT:Stop()
wait(0.8)
BTAUNT5:Play()
        Effects.Ring.Create(BrickC("Really black"), root.CFrame * CF(0, -2.3, 0) * angles(Rad(90),Rad(-1),Rad(0)), 2.5, 2.5, 40, 3, 3, 45, 0.01)

  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 7, 10, 15, -0.1, 15, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really red"), root.CFrame * CF(0, -2, 0), 10, 6, 10, 15, -0.1, 15, 0.02)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 4, 10, 15, -0.1, 15, 0.01)

	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
for i = 0, 30, 0.1 do
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		                neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0 - 255.45 * i)), 0.15)
				RH.C0 = clerp(RH.C0, CF(.8* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, .2* Player_Size) * angles(Rad(0), Rad(45), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-75), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.4 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-20 - 6.5 * Sin(sine / 22)), Rad(-20 - 6.5 * Sin(sine / 22)), Rad(13)), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.4 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(20 - 6.5 * Sin(sine / 22)), Rad(20 - 6.5 * Sin(sine / 22)), Rad(-13)), 0.1)
end
for i = 0, 30, 0.1 do
swait()
                rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -1.5 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(10 + Mrandom(-65,65)), Rad(0 + Mrandom(-65,65)), Rad(0 + Mrandom(-65,65))), 0.08)
                tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(15 - 2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.08)
                RH.C0 = clerp(RH.C0, CF(1* Player_Size, 0.7 - 0.1 * Cos(sine / 20)* Player_Size, -0.5* Player_Size) * angles(Rad(0), Rad(80), Rad(0)) * angles(Rad(-10.5 + Mrandom(-65,65)), Rad(0 + Mrandom(-65,65)), Rad(10 + Mrandom(-65,65))), 0.08)
                LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-80), Rad(0)) * angles(Rad(-10.5 + Mrandom(-65,65)), Rad(0 + Mrandom(-65,65)), Rad(70 + Mrandom(-65,65))), 0.08)
                RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(170 + 4.5 * Sin(sine / 5) + Mrandom(-65,65)), Rad(0 + 4.5 * Sin(sine / 5) + Mrandom(-35,35)), Rad(-25 + 4.5 * Sin(sine / 5) + Mrandom(-65,65))), 0.08)
                LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(170 - 4.5 * Sin(sine / 5) + Mrandom(-65,65)), Rad(0 - 4.5 * Sin(sine / 5) + Mrandom(-65,65)), Rad(25 - 4.5 * Sin(sine / 5) + Mrandom(-65,65))), 0.08)
end
BTAUNT5:Stop()
Cso("199978176", hed, 10, 1)
        Effects.Ring.Create(BrickC("Really black"), root.CFrame * CF(0, -2.3, 0) * angles(Rad(90),Rad(-1),Rad(0)), 2.5, 2.5, 40, 3, 3, 45, 0.01)

  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 7, 10, 15, -0.1, 15, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really red"), root.CFrame * CF(0, -2, 0), 10, 6, 10, 15, -0.1, 15, 0.02)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 4, 10, 15, -0.1, 15, 0.01)

	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
wait(0.1)
Cso("199978176", hed, 9, 0.9)
        Effects.Ring.Create(BrickC("Really black"), root.CFrame * CF(0, -2.3, 0) * angles(Rad(90),Rad(-1),Rad(0)), 2.5, 2.5, 40, 3, 3, 45, 0.01)

  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 7, 10, 15, -0.1, 15, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really red"), root.CFrame * CF(0, -2, 0), 10, 6, 10, 15, -0.1, 15, 0.02)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 4, 10, 15, -0.1, 15, 0.01)

	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
wait(0.1)
Cso("199978176", hed, 8, 0.8)
        Effects.Ring.Create(BrickC("Really black"), root.CFrame * CF(0, -2.3, 0) * angles(Rad(90),Rad(-1),Rad(0)), 2.5, 2.5, 40, 3, 3, 45, 0.01)

  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 7, 10, 15, -0.1, 15, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really red"), root.CFrame * CF(0, -2, 0), 10, 6, 10, 15, -0.1, 15, 0.02)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 4, 10, 15, -0.1, 15, 0.01)

	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(2,7,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1.5,9.5,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)
	PixelBlock(1,12,"Add",tors.CFrame*CFrame.Angles(math.rad(math.random(-360,360)),math.rad(math.random(-360,360)),math.rad(math.random(-360,360))),2,2,2,0.3,maincolor,0)

BTAUNT4:Play()
BTAUNT:Stop()
	attack = false
tecks2.TextColor3 = BrickColor.new('Really black').Color
tecks2.TextStrokeColor3 = BrickColor.new('Really red').Color
tecks2.Text = "Xander..??"
tecks2.Font = "Antique"
Aa.MeshId = "rbxassetid://432256490"
Aa.TextureId = "rbxassetid://432256526"

end

function ooo()
Aa.MeshId = "rbxassetid://0"
Aa.TextureId = "rbxassetid://0"
	attack = true
BTAUNT4:Stop()
BTAUNT:Play()
	attack = false
tecks2.Text = "Mr. Xander"
tecks2.TextColor3 = BrickColor.new('Really black').Color
tecks2.TextStrokeColor3 = BrickColor.new('Institutional white').Color
tecks2.Font = "Fantasy"
end


function Sie_alle_sterben()
	attack = true
	movelegs = true
	local orb = Instance.new("Part", char)
	orb.Anchored = true
	orb.BrickColor = BrickC("Really red")
	orb.CanCollide = false
	orb.FormFactor = 3
	orb.Name = "Ring"
	orb.Material = "Granite"
	orb.Size = Vector3.new(1, 1, 1)
	orb.Transparency = 0
	orb.TopSurface = 0
	orb.BottomSurface = 0
	local orbm = Instance.new("SpecialMesh", orb)
	orbm.MeshType = "Sphere"
	orbm.Name = "SizeMesh"
	orbm.Scale = Vector3.new(0, 0, 0)
	local scaled = 0.1
	local posid = 0
	local RoaringLaugh = Cso("0", char, 5, 0.8)
	swait(2)
	for i = 0, 30, 0.1 do
		swait()
		scaled = scaled + 0.006
		posid = posid - scaled
		orb.CFrame = la.CFrame * CF(0, -0.1 + posid / 1.05, 0)
		orbm.Scale = orbm.Scale + Vector3.new(scaled, scaled, scaled)
		--Magic(5, "Add", root.CFrame * CFrame.new(0, -2.9, 0), Vector3.new(0, 0, 0), 1, maincolor, "Sphere")
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(20)), 0.15)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
                RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(80), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.08)
                LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-80), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.08)
		RW.C0 = clerp(RW.C0, CF(1.5 * Player_Size, 0.5 + 0.1 * Cos(sine / 20) * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(35 + 2.5 * Sin(sine / 20))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(180), Rad(0 - 5 * Sin(sine / 20)), Rad(-20)), 0.1)
	end
	for i = 0, 10, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(20)), 0.15)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
                RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(80), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.08)
                LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-80), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.08)
		RW.C0 = clerp(RW.C0, CF(1.5 * Player_Size, 0.5 + 0.1 * Cos(sine / 20) * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(35 + 2.5 * Sin(sine / 20))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(225), Rad(0 - 5 * Sin(sine / 20)), Rad(-20)), 0.1)
	end
	coroutine.resume(coroutine.create(function()
		orb.Anchored = false
		CFuncs.Sound.Create("rbxassetid://907528019", root, 1.85, 1)
		local a = Instance.new("Part", workspace)
		a.Name = "Direction"
		a.Anchored = true
		a.BrickColor = BrickC("Royal purple")
		a.Material = "Neon"
		a.Transparency = 1
		a.CanCollide = false
		local ray = Ray.new(orb.CFrame.p, (mouse.Hit.p - orb.CFrame.p).unit * 500)
		local ignore = orb
		local hit, position, normal = workspace:FindPartOnRay(ray, ignore)
		a.BottomSurface = 10
		a.TopSurface = 10
		local distance = (orb.CFrame.p - position).magnitude
		a.Size = Vector3.new(0.1, 0.1, 0.1)
		a.CFrame = CFrame.new(orb.CFrame.p, position) * CFrame.new(0, 0, 0)
		orb.CFrame = a.CFrame
		a:Destroy()
		local bv = Instance.new("BodyVelocity")
		bv.maxForce = Vector3.new(1000000000, 1000000000, 1000000000)
		bv.velocity = orb.CFrame.lookVector * 125
		bv.Parent = orb
		local hitted = false
		game:GetService("Debris"):AddItem(orb, 15)
		swait()
		local hit = orb.Touched:connect(function(hit)
			if hitted == false then
				hitted = true
				CameraEnshaking(10, 20)
				CFuncs.Sound.Create("rbxassetid://304490261", char, 5, 0.7)
				for i, v in pairs(FindNearestHead(orb.CFrame.p, 100)) do
					if v:FindFirstChild("Head") then
						Eviscerate(v)
					end
				end
				Magic(1, "Add", orb.CFrame, Vector3.new(orbm.Scale.x, orbm.Scale.y, orbm.Scale.z), 1, BrickC("Crimson"), "Sphere")
				Magic(2, "Add", orb.CFrame, Vector3.new(orbm.Scale.x, orbm.Scale.y, orbm.Scale.z), 2, maincolor, "Sphere")
				orb.Anchored = true
				orb.Transparency = 1
				wait(8)
				orb:Destroy()
			end
		end)
	end))
	for i = 0, 10, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(20)), 0.15)
		neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0 - 5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.1)
                RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(80), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.08)
                LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-80), Rad(0)) * angles(Rad(-2.5), Rad(0), Rad(0)), 0.08)
		RW.C0 = clerp(RW.C0, CF(1.5 * Player_Size, 0.5 + 0.1 * Cos(sine / 20) * Player_Size, 0 * Player_Size) * angles(Rad(0), Rad(0), Rad(35 + 2.5 * Sin(sine / 20))), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.1 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(135), Rad(0 - 5 * Sin(sine / 20)), Rad(-20)), 0.1)
	end
	attack = false
end

function Chain2()
	if Mouse.Target.Parent ~= char and Mouse.Target.Parent.Parent ~= char and Mouse.Target.Parent:FindFirstChildOfClass("Humanoid") ~= nil then
	local HUM = Mouse.Target.Parent:FindFirstChildOfClass("Humanoid")
	local TORSO = HUM.Parent:FindFirstChild("Torso") or HUM.Parent:FindFirstChild("UpperTorso")
	local HEAD = HUM.Parent:FindFirstChild("Head")
	local RIGHTARM = HUM.Parent:FindFirstChild("Right Arm") or HUM.Parent:FindFirstChild("RightLowerArm")
	local LEFTARM = HUM.Parent:FindFirstChild("Left Arm") or HUM.Parent:FindFirstChild("LeftLowerArm")
	if HEAD and TORSO and HUM.Health > 0 then
	local GYRO = IT("BodyGyro",root)
	GYRO.D = 275
	GYRO.P = 20000
	GYRO.MaxTorque = Vector3.new(0,40000,0)
	attack = true
	hum.WalkSpeed = 0
	local hit,pos,hummie;
	local Hook2 = Part(EffectModel, Color3.new(),Enum.Material.Neon,Vector3.new(.05,.05,.05),root.CFrame,true,false)
	Hook2.Transparency = 1
	local A2 = NewInstance("Attachment",Hook2)
	local B2 = NewInstance("Attachment",la,{Position = Vector3.new(0,-ra.Size.Y/2,0)})
	local Chain2 = NewInstance("Beam",Hook2,{Attachment0 = A2,Attachment1=B2,Color = Color3.fromRGB(138,138,138),FaceCamera=true,LightInfluence=0,Texture="rbxassetid://73042633",TextureLength=5,Transparency=NumberSequence.new(0),TextureSpeed=0,CurveSize0=0,CurveSize1=0,FaceCamera=true,Segments=10,Width0=1,Width1=1})
	for i = 0, 2.3, .1 do
		swait()
		GYRO.cframe = CF(root.Position,TORSO.Position)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.1 + 0.1* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(-7)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(7)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34 * Cos(sine / 7* Player_Size)) * angles(Rad(110)  * Cos(sine / 7) , Rad(0), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
	end
	Cso("169105657", ra, 7, 1.2)
	for i = 0, 5, .1 do
		if(hit)then break end
		swait()
		GYRO.cframe = CF(root.Position,TORSO.Position)
		Hook2.CFrame = TORSO.CFrame
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.1 + 0.1* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(7)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(-7)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34 * Cos(sine / 7* Player_Size)) * angles(Rad(110)  * Cos(sine / 7) , Rad(0), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, -0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
	end
	Cso("169105657", ra, 5, .8)
        Cso("0", char, 7, 1) 
	GYRO:remove()
	--TORSO:BreakJoints()
	for i = 0, 6, .1 do
		swait()
		Hook2.CFrame = Hook2.CFrame:lerp(tors.CFrame * CF(0, 0, -1), .2)
		if(hit)then hit.CFrame = Hook2.CFrame; hit.Velocity = Vector3.new() 
		end
		if((Hook2.CFrame.p-tors.CFrame.p).magnitude < 2)then
			break
		end
		Chain2.TextureLength = 4
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.1 + 0.1* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(90)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(7)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34 * Cos(sine / 7* Player_Size)) * angles(Rad(110)  * Cos(sine / 7) , Rad(0), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
	end
		hum.WalkSpeed = 16
		attack = false
		Hook2:Destroy()
		end
	end
end


function DRAG_THEM_TO_HELL()
	if mouse.Target.Parent ~= char and mouse.Target.Parent.Parent ~= char and mouse.Target.Parent:FindFirstChildOfClass("Humanoid") ~= nil then
	local HUM = mouse.Target.Parent:FindFirstChildOfClass("Humanoid")
	local TORSO = HUM.Parent:FindFirstChild("Torso") or HUM.Parent:FindFirstChild("UpperTorso")
	local HEAD = HUM.Parent:FindFirstChild("Head")
	if HEAD and TORSO and HUM.Health > 0 then
	local GYRO = IT("BodyGyro",root)
	GYRO.D = 275
	GYRO.P = 20000
	GYRO.MaxTorque = Vector3.new(0,40000,0)
	attack = true
	hum.WalkSpeed = 0
	local hit,pos,hummie;
	local Hook = Part(EffectModel, Color3.new(),Enum.Material.Neon,Vector3.new(.05,.05,.05),root.CFrame,true,false)
	Hook.Transparency = 1
	local A = NewInstance("Attachment",Hook)
	local B = NewInstance("Attachment",la,{Position = Vector3.new(0,-ra.Size.Y/2,0)})
	local Chain = NewInstance("Beam",Hook,{Attachment0 = A,Attachment1=B,Color = Color3.fromRGB(138,138,138),FaceCamera=true,LightInfluence=0,Texture="rbxassetid://73042633",TextureLength=5,Transparency=NumberSequence.new(0),TextureSpeed=0,CurveSize0=0,CurveSize1=0,FaceCamera=true,Segments=10,Width0=1,Width1=1})
	local POS = mouse.Hit.p
	local CHAINS = false
	local CHAINLINKS = {}
	local A = IT("Attachment",la)
	A.Position = Vector3.new(1,-1,0)*Player_Size
	A.Orientation = Vector3.new(-90, -89.982, 0)
	local B = IT("Attachment",la)
	B.Position = Vector3.new(-1,-1,0)*Player_Size
	B.Orientation = Vector3.new(-90, 89.988, 0)
	local C = IT("Attachment",la)
	C.Position = Vector3.new(0.5,-1.3,0)*Player_Size
	C.Orientation = Vector3.new(-90, -89.982, 0)
	local D = IT("Attachment",la)
	D.Position = Vector3.new(-0.5,-1.3,0)*Player_Size
	D.Orientation = Vector3.new(-90, 89.988, 0)
	local LIGHT = IT("Attachment",la)
	LIGHT.Position = Vector3.new(0,-1,0)*Player_Size
	local LIGHT2 = IT("PointLight",LIGHT)
	LIGHT2.Range = 7
	LIGHT2.Brightness = 5
	LIGHT2.Color = Color3.new(0,0,0)
	for i = 1, 2 do
		local TWIST = -2
		local START = A
		local END = B
		if i == 1 then
			START = B
			END = A
		end
		local ChainLink = IT("Beam",tors)
		ChainLink.Texture = "rbxassetid://73042633"
		ChainLink.Color = ColorSequence.new(Color3.fromRGB(138,138,138))
		ChainLink.TextureSpeed = 1
		ChainLink.Width0 = 1
		ChainLink.Width1 = 1
		ChainLink.TextureLength = 2.5
		ChainLink.Attachment0 = START
		ChainLink.Attachment1 = END
		ChainLink.CurveSize0 = TWIST
		ChainLink.CurveSize1 = TWIST
		--ChainLink.FaceCamera = true
		ChainLink.Segments = 45
		ChainLink.Transparency = NumberSequence.new(0.25)
		table.insert(CHAINLINKS,ChainLink)
	end
	for i = 1, 2 do
		local TWIST = -1
		local START = C
		local END = D
		if i == 1 then
			START = D
			END = C
		end
		local ChainLink = IT("Beam",tors)
		ChainLink.Texture = "rbxassetid://73042633"
		ChainLink.Color = ColorSequence.new(Color3.fromRGB(138,138,138))
		ChainLink.TextureSpeed = 0
		ChainLink.Width0 = 1
		ChainLink.Width1 = 1
		ChainLink.TextureLength = 2
		ChainLink.Attachment0 = START
		ChainLink.Attachment1 = END
		ChainLink.CurveSize0 = TWIST
		ChainLink.CurveSize1 = TWIST
		--ChainLink.FaceCamera = true
		ChainLink.Segments = 25
		ChainLink.LightEmission = 0.5
		ChainLink.Transparency = NumberSequence.new(0.25)
		table.insert(CHAINLINKS,ChainLink)
	end
	for i = 0, 2.3, .1 do
		swait()
		GYRO.cframe = CF(root.Position,TORSO.Position)
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.1 + 0.1* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(-7)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(7)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34 * Cos(sine / 7* Player_Size)) * angles(Rad(110)  * Cos(sine / 7) , Rad(0), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
	end
	Cso("169105657", ra, 7, 1.2)
	for i = 0, 4, .1 do
		if(hit)then break end
		swait()
		GYRO.cframe = CF(root.Position,TORSO.Position)
		Hook.CFrame = HEAD.CFrame
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.1 + 0.1* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(7)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(-7)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.05 * Sin(sine / 30)* Player_Size, 0.34 * Cos(sine / 7* Player_Size)) * angles(Rad(110)  * Cos(sine / 7) , Rad(0), Rad(13) - ra.RotVelocity.Y / 75), 0.15)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, -0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(-25)), 0.1)
	end
	for _,v in next, getRegion(Hook.Position,1,{char}) do
			if(v.Parent and GetTorso(v.Parent) and v.Parent:FindFirstChildOfClass'Humanoid')then
				hit = GetTorso(v.Parent);
				hummie = v.Parent:FindFirstChildOfClass'Humanoid';
			break;
		end
	end
	Cso("169105657", ra, 5, .8)
	Cso("0", tors, 2, 1.1)
	GYRO:remove()
	for i = 0, 3, .1 do
		swait()
		HUM.PlatformStand = true
		Hook.CFrame = Hook.CFrame:lerp(ra.CFrame * CF(0, 0, -1), .2)
		if(hit)then hit.CFrame = Hook.CFrame; hit.Velocity = Vector3.new() 
		end
		if((Hook.CFrame.p-ra.CFrame.p).magnitude < 2)then
			break
		end
		Chain.TextureLength = 4
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.1 + 0.1* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-5 - 2.5 * Sin(sine / 30)), Rad(0), Rad(45)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(10)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(10)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0.4* Player_Size) * angles(Rad(90), Rad(-.6), Rad(45)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(30), Rad(-.6), Rad(-25)), 0.1)
	end
		hum.WalkSpeed = 16
		attack = false
		Hook:Destroy()
		A:remove()
		B:remove()
		C:remove()
		D:remove()
		end
	end
end

function Attack()
	attack = true
	for i = 0, 0.6, 0.1 do
		swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1* Player_Size * Cos(sine / 20)) * angles(Rad(-40), Rad(0), Rad(-60)), 0.2)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-7.5 * Sin(sine / 30)), Rad(0), Rad(60)), 0.2)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9* Player_Size - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(-40)), 0.2)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9* Player_Size - 0.1 * Cos(sine / 20)* Player_Size, -.6* Player_Size) * angles(Rad(0), Rad(-84), Rad(0)) * angles(Rad(-6.5), Rad(0), Rad(40)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(160), Rad(-.6), Rad(13)), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(15), Rad(-6), Rad(-25 - 4.5 * Sin(sine / 20))), 0.2)
	end
	Cso("982475423", tors, 10, 1)
	CameraEnshaking(2, 15)
 	for i, v in pairs(FindNearestHead(Blobby.CFrame.p, 9.5)) do
		if v:FindFirstChild("Head") then
			Eviscerate(v)
Cso("1744093986", tors, 10, 1)
		end
	end
	for i = 0, 3, 0.1 do
		swait()
        rootj.C0 = clerp(rootj.C0, RootCF * CF(0, -0.5, 0) * angles(Rad(40), Rad(0), Rad(30)), 0.3)
        tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.1)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.3 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(90), Rad(-.6), Rad(30)), 0.3)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(15), Rad(-6), Rad(-25 - 4.5 * Sin(sine / 20))), 0.3)
        RH.C0 = clerp(RH.C0, CF(1, -1, 0) * RHCF * angles(Rad(0), Rad(-45), Rad(40)), 0.3)
        LH.C0 = clerp(LH.C0, CF(-1, -1, 0) * LHCF * angles(Rad(0), Rad(-0), Rad(-40)), 0.3)
	end
	attack = false
end

function Blood_ball()
	 Cso("2545010175", hed, 10, 1)
	local orb = Instance.new("Part", char)
	orb.Anchored = true
	orb.BrickColor = BrickC("Really red")
	orb.CanCollide = false
	orb.FormFactor = 3
	orb.Name = "Ring"
	orb.Material = "Sand"
	orb.Size = Vector3.new(1, 1, 1)
	orb.Transparency = 0
	orb.TopSurface = 0
	orb.BottomSurface = 0
	local orbm = Instance.new("SpecialMesh", orb)
	orbm.MeshType = "Sphere"
	orbm.Name = "SizeMesh"
	orbm.Scale = Vector3.new(0, 0, 0)
	local scaled = 0.1
	local posid = 0
	for i = 0, 109, 0.1 do
		swait()
		scaled = scaled + 0.001
		posid = posid - scaled
		orb.CFrame = ra.CFrame * CF(0, -0.1 + posid / 1.05, 0)
		orbm.Scale = orbm.Scale + Vector3.new(scaled, scaled, scaled)
                rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
                tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-2.5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(79), Rad(0)) * angles(Rad(-10), Rad(0), Rad(-10)), 0.2)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-79), Rad(0)) * angles(Rad(-15), Rad(0), Rad(10)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(125), Rad(-7.5 * Sin(sine / 20)), Rad(40)), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-25), Rad(7.5 * Sin(sine / 20)), Rad(-25)), 0.2)
	end
	coroutine.resume(coroutine.create(function()
		orb.Anchored = false
		--CFuncs.Sound.Create("rbxassetid://260433768", root, 1.25, 1)
		mes("CALAMITY ORB COMING IN EVERYTHING WILL BE DESTROYED",0.05)
		local a = Instance.new("Part", workspace)
		a.Name = "Direction"
		a.Anchored = true
		a.BrickColor = BrickC("Crimson")
		a.Material = "Neon"
		a.Transparency = 1
		a.CanCollide = false
		local ray = Ray.new(orb.CFrame.p, (root.CFrame.lookVector - orb.CFrame.p).unit * 500)
		local ignore = orb
		local hit, position, normal = workspace:FindPartOnRay(ray, ignore)
		a.BottomSurface = 10
		a.TopSurface = 10
		local distance = (orb.CFrame.p - position).magnitude
		a.Size = Vector3.new(0.1, 0.1, 0.1)
		a.CFrame = CF(orb.CFrame.p, position) * CF(0, 0, 0)
		orb.CFrame = a.CFrame
		a:Destroy()
		local bv = Instance.new("BodyVelocity")
		bv.maxForce = Vector3.new(1000000000, 1000000000, 1000000000)
		bv.velocity = orb.CFrame.lookVector * 125
		bv.Parent = orb
		local hitted = false
		game:GetService("Debris"):AddItem(orb, 15)
		wait()
		local hit = orb.Touched:connect(function(hit)
			if hitted == false then
				hitted = true
				coroutine.resume(coroutine.create(function() 
		for i = 0,1.8,0.1 do
			swait()
			hum.CameraOffset = Vector3.new(Mrandom(-1,1),0,Mrandom(-1,1))
		end
		for i = 0,1.8,0.1 do
			swait()
		hum.CameraOffset = Vector3.new(0,0,0)
		end
	end))
				CFuncs.Sound.Create("rbxassetid://151304356", orb, 5, 1)
					for i, v in pairs(FindNearestHead(orb.CFrame.p, 50000)) do
		if v:FindFirstChild("Head") then
			Eviscerate(v)
		end
	end
				Magic(1, "Add", orb.CFrame, Vector3.new(orbm.Scale.x, orbm.Scale.y, orbm.Scale.z), 1, BrickC("Royal Purple"), "Sphere")
				Magic(2, "Add", orb.CFrame, Vector3.new(orbm.Scale.x, orbm.Scale.y, orbm.Scale.z), 2, BrickC("Royal Purple"), "Sphere")
				for i = 0, 9 do
					--Aura(1, 2.5, "Add", orb.CFrame * angles(Rad(Mrandom(-360, 360)), Rad(Mrandom(-360, 360)), Rad(Mrandom(-360, 360))), 5, 5, 50, -0.05, BrickColor.new("Royal purple"), 0, "Sphere")
					--Aura(2, 5, "Add", orb.CFrame * angles(Rad(Mrandom(-360, 360)), Rad(Mrandom(-360, 360)), Rad(Mrandom(-360, 360))), 5, 5, 50, -0.05, BrickColor.new("Royal purple"), 0, "Sphere")
				end
				orb.Anchored = true
				orb.Transparency = 1
				wait(8)
				orb:Destroy()
			end
		end)
	end))
	for i = 0, 2, 0.1 do
		swait()
                rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
                tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(-2.5 * Sin(sine / 20)), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(79), Rad(0)) * angles(Rad(-10), Rad(0), Rad(-10)), 0.2)
		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.9 - 0.1 * Cos(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(0), Rad(-79), Rad(0)) * angles(Rad(-15), Rad(0), Rad(10)), 0.2)
		RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(45), Rad(-7.5 * Sin(sine / 20)), Rad(40)), 0.2)
		LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(-25), Rad(7.5 * Sin(sine / 20)), Rad(-25)), 0.2)
	end
	attack = false
end

function ByeBye()
	local target = nil
	local targettorso = nil
	if mouse.Target.Parent ~= char and mouse.Target.Parent.Parent ~= char and mouse.Target.Parent:FindFirstChild("Humanoid") ~= nil then
		if mouse.Target.Parent.Humanoid.PlatformStand == false then
			target = mouse.Target.Parent.Humanoid
			targettorso = mouse.Target.Parent:FindFirstChild("Torso") or mouse.Target.Parent:FindFirstChild("UpperTorso")
			targethead = mouse.Target.Parent:FindFirstChild("Head")
		end
	end
	if target ~= nil then
		targettorso.Anchored = true
		attack = true
		hum.WalkSpeed = 0
		root.CFrame = targettorso.CFrame * CF(0,0,2)
		for i = 0,4.2,0.1 do
			swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(0)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(0)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-5), Rad(0), Rad(-0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.1, 0.7 + 0.05 * Sin(sine / 30), -.6 + 0.025 * Cos(sine / 20)) * angles(Rad(115), Rad(0), Rad(-15)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(20), Rad(0), Rad(-25)), 0.1)
		end
		local bloody = Instance.new("ParticleEmitter",targettorso)
		bloody.Color = ColorSequence.new(Color3.new(1, 0, 0), Color3.new(.5, 0, 0))
		bloody.LightEmission = .1
		bloody.Size = NumberSequence.new(0.5, 0)
		bloody.Texture = "http://www.roblox.com/asset/?ID=24419398"
		aaa = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2),NumberSequenceKeypoint.new(1, 5)})
		bbb = NumberSequence.new({NumberSequenceKeypoint.new(0, 1),NumberSequenceKeypoint.new(0.0636, 0), NumberSequenceKeypoint.new(1, 1)})
		bloody.Transparency = bbb
		bloody.Size = aaa
		bloody.ZOffset = -.9
		bloody.Acceleration = Vector3.new(0, -5, 0)
		bloody.LockedToPart = false
		bloody.Lifetime = NumberRange.new(0.8)
		bloody.Rate = 255
		bloody.Rotation = NumberRange.new(-100, 100)
		bloody.RotSpeed = NumberRange.new(-100, 100)
		bloody.Speed = NumberRange.new(6)
		bloody.VelocitySpread = 0
		bloody.Enabled=true
		--targethead:Remove()
		CreateSound("429400881", targettorso, 5, .8)
		CreateSound("1093102664", targettorso, 10, 1)
		for i = 0,6.2,0.1 do
			swait()
		rootj.C0 = clerp(rootj.C0, RootCF * CF(0, 0, -0.1 + 0.1 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(110)), 0.15)
		tors.Neck.C0 = clerp(tors.Neck.C0, necko * angles(Rad(0), Rad(0), Rad(-110)), 0.3)
		RH.C0 = clerp(RH.C0, CF(1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * RHCF * angles(Rad(-5), Rad(0), Rad(0)), 0.15)
		LH.C0 = clerp(LH.C0, CF(-1, -0.9 - 0.1 * Cos(sine / 20), 0.025 * Cos(sine / 20)) * LHCF * angles(Rad(-5), Rad(0), Rad(-0)), 0.15)
		RW.C0 = clerp(RW.C0, CF(1.3, 0.7 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(100), Rad(0), Rad(-15)), 0.1)
		LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 0.05 * Sin(sine / 30), 0.025 * Cos(sine / 20)) * angles(Rad(0), Rad(0), Rad(-5)), 0.1)
		end
		targettorso.Anchored = false
		attack = false
		hum.WalkSpeed = 16
		root.CFrame = targettorso.CFrame * CF(0,0,3)
	end
end

function AAA()
	attack = true
	hum.WalkSpeed = 0
	Magic(5, "Add", Mouse.Hit * CFrame.new(0, -2.9, 0), Vector3.new(0, 0, 0), 1, maincolor, "Sphere")
	Magic(10, "Add", Mouse.Hit * CFrame.new(0, -2.9, 0), Vector3.new(0, 0, 0), 2, maincolor, "Sphere")
	Magic(1, "Add", Mouse.Hit, Vector3.new(1, 100000, 1), 0.5, maincolor, "Sphere")
	Magic(1, "Add", Mouse.Hit, Vector3.new(1, 1, 1), 0.75, maincolor, "Sphere")
	Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
	Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 7, 10, 15, -0.1, 15, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 6, 10, 15, -0.1, 15, 0.02)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 4, 10, 15, -0.1, 15, 0.01)
coroutine.resume(coroutine.create(function()
    while textfag ~= nil do
        swait()
        textfag.Position = UDim2.new(math.random(-.2,.2),math.random(-3,3),.05,math.random(-3,3))  
        textfag.Rotation = math.random(-9,3)
    end
end))                  
	Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
  	Effects.Wave.Create(BrickColor.new("Really black"), tors.CFrame * CF(0, -5, 0) * angles(math.rad(0), math.rad(math.random(0, 180)), math.rad(0)), 550.5, 100.5, 550.5, 200, 20, 200, 0.05)
	Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 7, 10, 15, -0.1, 15, 0.04)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 6, 10, 15, -0.1, 15, 0.02)
  	Effects.Sphere.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0), 10, 4, 10, 15, -0.1, 15, 0.01)
Effects.Ring.Create(BrickColor.new("Really black"), root.CFrame * CF(0, -2, 0) * angles(math.rad(90), math.rad(0), math.rad(0)), 0.5, 0.5, 0.1, 2, 2, 0, 0.04)
Effects.Block.Create(BrickColor.new("Institutional black"), tors.CFrame, 2, 2, 2, 3.6, 3.6, 3.6, 0.05)
    Effects.Block.Create(BrickColor.new("Neon"), tors.CFrame, 2, 2, 2, 3.4, 3.4, 3.4, 0.03)
Effects.Block.Create(BrickColor.new("Institutional black"), tors.CFrame, 2, 2, 2, 6.6, 6.6, 6.6, 0.05)
    Effects.Block.Create(BrickColor.new("Neon"), tors.CFrame, 2, 2, 2, 6.4, 6.4, 6.4, 0.05)
 Effects.Block.Create(BrickColor.new("Neon"), tors.CFrame, 2, 2, 2, 10.5, 10.5, 10.5, 0.05)

Effects.Ring.Create(BrickColor.new("Institutional black"), tors.CFrame, 2, 2, 2, 7.6, 7.6, 7.6, 0.03)
Effects.Sphere.Create(maincolor, tors.CFrame, 2, 2, 2, 17.6, 17.6, 17.6, 0.02)
Effects.Sphere.Create(BrickColor.new("Really black"), tors.CFrame, 2, 2, 2, 10.6, 10.6, 10.6, 0.02)
Effects.Sphere.Create(BrickColor.new("Really black"), tors.CFrame, 2, 2, 2, 14.6, 14.6, 14.6, 0.02)
Effects.Sphere.Create(BrickColor.new("Really black"), tors.CFrame, 2, 2, 2, 16.6, 16.6, 16.6, 0.02)
Effects.Sphere.Create(BrickColor.new("Really black"), tors.CFrame, 2, 2, 2, 5.6, 5.6, 5.6, 0.02)
	for i, v in pairs(FindNearestHead(Mouse.Hit.p, 14.5)) do
		if v:FindFirstChild("Head") then
			Eviscerate(v)
		end
	end
	attack = false
	hum.WalkSpeed = 16
end

-------------------------------------------------------
--End Attacks N Stuff--
-------------------------------------------------------

function MagicBlock(brickcolor, cframe, x1, y1, z1, x3, y3, z3, delay)
	local prt = part(3, char, 0, 0, brickcolor, "Effect", Vector3.new(0.5, 0.5, 0.5))
	prt.Anchored = true
	prt.Material = "Neon"
	prt.CFrame = cframe
	prt.CFrame = prt.CFrame * Euler(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
local msh = mesh("BlockMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, z1))
	game:GetService("Debris"):AddItem(prt, 5)
	coroutine.resume(coroutine.create(function(Part, Mesh)
		for i = 0, 1, delay do
			swait()
			Part.CFrame = Part.CFrame * Euler(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
			Part.Transparency = i
			Mesh.Scale = Mesh.Scale + Vector3.new(x3, y3, z3)
		end
		Part.Parent = nil
	end), prt, msh)
end


function MagicShockAlt(brickcolor, cframe, x1, y1, x3, y3, delay, rottype)
	local prt = part(3, char, 0, 0, brickcolor, "Effect", Vector3.new(0.5, 0.5, 0.5))
	prt.Anchored = true
	prt.Material = "Neon"
	prt.CFrame = cframe
local msh = mesh("BlockMesh", prt, "", "", Vector3.new(0, 0, 0), Vector3.new(x1, y1, 0.01))
	game:GetService("Debris"):AddItem(prt, 5)
	coroutine.resume(coroutine.create(function(Part, Mesh)
		local rtype = rottype
		for i = 0, 1, delay do
			swait()
			if rtype == 1 then
				prt.CFrame = prt.CFrame * CFrame.Angles(0, 0, 0.1)
			elseif rtype == 2 then
				prt.CFrame = prt.CFrame * CFrame.Angles(0, 0, -0.1)
			end
			prt.Transparency = i
			Mesh.Scale = Mesh.Scale + Vector3.new(x3, y3, 0)
		end
		Part.Parent = nil
	end), prt, msh)
end

function chatfunc(text, color)
	local chat = coroutine.wrap(function()
		if char:FindFirstChild("TalkingBillBoard") ~= nil then
			char:FindFirstChild("TalkingBillBoard"):destroy()
		end
		local naeeym2 = Instance.new("BillboardGui", char)
		naeeym2.Size = UDim2.new(0, 100, 0, 40)
		naeeym2.StudsOffset = Vector3.new(0, 3, 0)
		naeeym2.Adornee = hed
		naeeym2.Name = "TalkingBillBoard"
		local tecks2 = Instance.new("TextLabel", naeeym2)
		tecks2.BackgroundTransparency = 1
		tecks2.BorderSizePixel = 0
		tecks2.Text = ""
		tecks2.Font = "Fantasy"
		tecks2.TextSize = 30
		tecks2.TextStrokeTransparency = 0
		tecks2.TextColor3 = color
		tecks2.TextStrokeColor3 = Color3.new(0, 0, 0)
		tecks2.Size = UDim2.new(1, 0, 0.5, 0)
		local tecks3 = Instance.new("TextLabel", naeeym2)
		tecks3.BackgroundTransparency = 1
		tecks3.BorderSizePixel = 0
		tecks3.Text = ""
		tecks3.Font = "SciFi"
		tecks3.TextSize = 30
		tecks3.TextStrokeTransparency = 0
		tecks3.TextColor3 = Color3.new(0, 0, 0)
		tecks3.TextStrokeColor3 = color
		tecks3.Size = UDim2.new(1, 0, 0.5, 0)
		coroutine.resume(coroutine.create(function()
			while true do
				swait(1)
				tecks2.Position = UDim2.new(0, math.random(-5, 5), 0, math.random(-5, 5))
				tecks3.Position = UDim2.new(0, math.random(-5, 5), 0, math.random(-5, 5))
				tecks2.Rotation = math.random(-5, 5)
				tecks3.Rotation = math.random(-5, 5)
			end
		end))
		for i = 1, string.len(text) do
			CFuncs.Sound.Create("rbxassetid://274118116", char, 0.25, 0.115)
			tecks2.Text = string.sub(text, 1, i)
			tecks3.Text = string.sub(text, 1, i)
			swait(1)
		end
		wait(1)
		local randomrot = math.random(1, 2)
		if randomrot == 1 then
			for i = 1, 50 do
				swait()
				tecks2.Rotation = tecks2.Rotation - 0.75
				tecks2.TextStrokeTransparency = tecks2.TextStrokeTransparency + 0.04
				tecks2.TextTransparency = tecks2.TextTransparency + 0.04
				tecks3.Rotation = tecks2.Rotation + 0.75
				tecks3.TextStrokeTransparency = tecks2.TextStrokeTransparency + 0.04
				tecks3.TextTransparency = tecks2.TextTransparency + 0.04
			end
		elseif randomrot == 2 then
			for i = 1, 50 do
				swait()
				tecks2.Rotation = tecks2.Rotation + 0.75
				tecks2.TextStrokeTransparency = tecks2.TextStrokeTransparency + 0.04
				tecks2.TextTransparency = tecks2.TextTransparency + 0.04
				tecks3.Rotation = tecks2.Rotation - 0.75
				tecks3.TextStrokeTransparency = tecks2.TextStrokeTransparency + 0.04
				tecks3.TextTransparency = tecks2.TextTransparency + 0.04
			end
		end
		naeeym2:Destroy()
	end)
	chat()
end

mouse.KeyDown:connect(function(key)
	if attack == false then
		if key == "q" then
                          --DRAG_THEM_TO_HELL()   
   
                     elseif key == 't' then
                     Taunt()

                     elseif key == 'r' then
                     Taunt3()

                     elseif key == 'y' then
BTAUNT:Stop()
BTAUNT2:Play()
                     Taunt2()

		elseif key == 'x' then
			--ByeBye()

		elseif key == 'v' then
			--Blood_ball()

		elseif key == 'n' then
			--eee()
 		end
	end
end)

mouse.KeyDown:connect(function(key)
	if attack == false then
		if key == "f" then
                       Die()
		elseif key == "x" then
                       InkyWarp()
		elseif key == "c" then
                       dead()
		elseif key == "e" then
                       Chain2()
 		end
	end
end)


mouse.Button1Down:connect(function(key)
	if attack == false then
		Attack()
	end
end)

function Part(parent,color,material,size,cframe,anchored,cancollide)
	local part = Instance.new("Part")
	part[typeof(color) == 'BrickColor' and 'BrickColor' or 'Color'] = color or Color3.new(0,0,0)
	part.Material = material or Enum.Material.SmoothPlastic
	part.TopSurface,part.BottomSurface=10,10
	part.Size = size or Vector3.new(1,1,1)
	part.CFrame = cframe or CF(0,0,0)
	part.Anchored = anchored or true
	part.CanCollide = cancollide or false
	part.Parent = parent or char
	return part
end

NewInstance = function(instance,parent,properties)
	local inst = Instance.new(instance)
	inst.Parent = parent
	if(properties)then
		for i,v in next, properties do
			pcall(function() inst[i] = v end)
		end
	end
	return inst;
end
-------------------------------------------------------
--Start Damage Function--
-------------------------------------------------------
function PixelBlock(bonuspeed,FastSpeed,type,pos,x1,y1,z1,value,color,outerpos) --Thanks, Star Glitcher!
local type = type
local rng = Instance.new("Part", char)
        rng.Anchored = true
        rng.BrickColor = color
        rng.CanCollide = false
        rng.FormFactor = 3
        rng.Name = "Ring"
        rng.Material = "Neon"
        rng.Size = Vector3.new(1, 1, 1)
        rng.Transparency = 0
        rng.TopSurface = 0
        rng.BottomSurface = 0
        rng.CFrame = pos
rng.CFrame = rng.CFrame + rng.CFrame.lookVector*outerpos
        local rngm = Instance.new("SpecialMesh", rng)
        rngm.MeshType = "Brick"
if rainbowmode == true then
rng.Color = Color3.new(r/255,g/255,b/255)
end
local scaler2 = 1
local speeder = FastSpeed/10
if type == "Add" then
scaler2 = 1*value
elseif type == "Divide" then
scaler2 = 1/value
end
coroutine.resume(coroutine.create(function()
for i = 0,10/bonuspeed,0.1 do
swait()
if type == "Add" then
scaler2 = scaler2 - 0.01*value/bonuspeed
elseif type == "Divide" then
scaler2 = scaler2 - 0.01/value*bonuspeed
end
speeder = speeder - 0.01*FastSpeed*bonuspeed/10
rng.CFrame = rng.CFrame + rng.CFrame.lookVector*speeder*bonuspeed
rng.Transparency = rng.Transparency + 0.01*bonuspeed
end
rng:Destroy()
end))
end

function Damage(Part, hit, minim, maxim, knockback, Type, Property, Delay, HitSound, HitPitch)
	if hit.Parent == nil then
		return
	end
	local h = hit.Parent:FindFirstChildOfClass("Humanoid")
	for _, v in pairs(hit.Parent:children()) do
		if v:IsA("Humanoid") then
			h = v
		end
	end
         if h ~= nil and hit.Parent.Name ~= char.Name and hit.Parent:FindFirstChild("UpperTorso") ~= nil then
	
         --hit.Parent:FindFirstChild("Head"):BreakJoints()
         end

	if h ~= nil and hit.Parent.Name ~= char.Name and hit.Parent:FindFirstChild("Torso") ~= nil then
		if hit.Parent:findFirstChild("DebounceHit") ~= nil then
			if hit.Parent.DebounceHit.Value == true then
				return
			end
		end
         if insta == true then
        -- hit.Parent:FindFirstChild("Head"):BreakJoints()
         end
		local c = Create("ObjectValue"){
			Name = "creator",
			Value = game:service("Players").LocalPlayer,
			Parent = h,
		}
		game:GetService("Debris"):AddItem(c, .5)
		if HitSound ~= nil and HitPitch ~= nil then
			CFuncs.Sound.Create(HitSound, hit, 1, HitPitch) 
		end
		local Damage = math.random(minim, maxim)
		local blocked = false
		local block = hit.Parent:findFirstChild("Block")
		if block ~= nil then
			if block.className == "IntValue" then
				if block.Value > 0 then
					blocked = true
					block.Value = block.Value - 1
					print(block.Value)
				end
			end
		end
		if blocked == false then
			h.Health = h.Health - Damage
			ShowDamage((Part.CFrame * CFrame.new(0, 0, (Part.Size.Z / 2)).p + Vector3.new(0, 1.5, 0)), -Damage, 1.5, tors.BrickColor.Color)
		else
			h.Health = h.Health - (Damage / 2)
			ShowDamage((Part.CFrame * CFrame.new(0, 0, (Part.Size.Z / 2)).p + Vector3.new(0, 1.5, 0)), -Damage, 1.5, tors.BrickColor.Color)
		end
		if Type == "Knockdown" then
			local hum = hit.Parent.Humanoid
			hum.PlatformStand = true
			coroutine.resume(coroutine.create(function(HHumanoid)
				swait(1)
				HHumanoid.PlatformStand = false
			end), hum)
			local angle = (hit.Position - (Property.Position + Vector3.new(0, 0, 0))).unit
			local bodvol = Create("BodyVelocity"){
				velocity = angle * knockback,
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			local rl = Create("BodyAngularVelocity"){
				P = 3000,
				maxTorque = Vector3.new(500000, 500000, 500000) * 50000000000000,
				angularvelocity = Vector3.new(math.random(-10, 10), math.random(-10, 10), math.random(-10, 10)),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodvol, .5)
			game:GetService("Debris"):AddItem(rl, .5)
		elseif Type == "Normal" then
			local vp = Create("BodyVelocity"){
				P = 500,
				maxForce = Vector3.new(math.huge, 0, math.huge),
				velocity = Property.CFrame.lookVector * knockback + Property.Velocity / 1.05,
			}
			if knockback > 0 then
				vp.Parent = hit.Parent.Torso
			end
			game:GetService("Debris"):AddItem(vp, .5)
		elseif Type == "Up" then
			local bodyVelocity = Create("BodyVelocity"){
				velocity = Vector3.new(0, 20, 0),
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodyVelocity, .5)
		elseif Type == "DarkUp" then
			coroutine.resume(coroutine.create(function()
				for i = 0, 1, 0.1 do
					swait()
					Effects.Block.Create(BrickColor.new("Royal purple"), hit.Parent.Torso.CFrame, 5, 5, 5, 1, 1, 1, .08, 1)
				end
			end))
			local bodyVelocity = Create("BodyVelocity"){
				velocity = Vector3.new(0, 20, 0),
				P = 5000,
				maxForce = Vector3.new(8e+003, 8e+003, 8e+003),
				Parent = hit,
			}
			game:GetService("Debris"):AddItem(bodyVelocity, 1)
		elseif Type == "Snare" then
			local bp = Create("BodyPosition"){
				P = 2000,
				D = 100,
				maxForce = Vector3.new(math.huge, math.huge, math.huge),
				position = hit.Parent.Torso.Position,
				Parent = hit.Parent.Torso,
			}
			game:GetService("Debris"):AddItem(bp, 1)
		elseif Type == "Freeze" then
			local BodPos = Create("BodyPosition"){
				P = 50000,
				D = 1000,
				maxForce = Vector3.new(math.huge, math.huge, math.huge),
				position = hit.Parent.Torso.Position,
				Parent = hit.Parent.Torso,
			}
			local BodGy = Create("BodyGyro") {
				maxTorque = Vector3.new(4e+005, 4e+005, 4e+005) * math.huge ,
				P = 20e+003,
				Parent = hit.Parent.Torso,
				cframe = hit.Parent.Torso.CFrame,
			}
			hit.Parent.Torso.Anchored = true
			coroutine.resume(coroutine.create(function(Part) 
				swait(1.5)
				Part.Anchored = false
			end), hit.Parent.Torso)
			game:GetService("Debris"):AddItem(BodPos, 3)
			game:GetService("Debris"):AddItem(BodGy, 3)
		end
		local debounce = Create("BoolValue"){
			Name = "DebounceHit",
			Parent = hit.Parent,
			Value = true,
		}
		game:GetService("Debris"):AddItem(debounce, Delay)
		c = Create("ObjectValue"){
			Name = "creator",
			Value = Player,
			Parent = h,
		}
		game:GetService("Debris"):AddItem(c, .5)
	end
end

function damage(range,mindam,maxdam,pos)
	for i,v in ipairs(workspace:GetChildren()) do
		if v:IsA("Model") then
			if v.Name ~= Player.Name then
				if v:FindFirstChildOfClass("Humanoid") then
					if v:FindFirstChild("Head") then
						if (v:FindFirstChild("Head").Position - pos).magnitude < 10 then
							if v:FindFirstChildOfClass("Humanoid").Health > 5000 then v:FindFirstChildOfClass("Humanoid").Health = 0 else
								v:FindFirstChildOfClass("Humanoid").Health = v:FindFirstChildOfClass("Humanoid").Health - math.random(mindam,maxdam)
							end
						end
					end
				end
			end
		end
	end
end
-------------------------------------------------------
--End Damage Function--
-------------------------------------------------------

-------------------------------------------------------
--Start Animations--
-------------------------------------------------------
print("By Makhail07")
coroutine.resume(coroutine.create(function()
	while wait() do
	if hitfloor ~= nil then
		Hole.CFrame = CF(posfloor)
	end
	Sink(Hole.Position, Hole.Size.X/2.2 * MESH.Scale.X)
	Hole.Color = Color3.new(0,0,0)
	Trail(Hole)
	end
end))
while true do
	swait()
	sine = sine + change
	local torvel = (root.Velocity * Vector3.new(1, 0, 1)).magnitude
	local velderp = root.Velocity.y
	hitfloor, posfloor = rayCast(root.Position, CFrame.new(root.Position, root.Position - Vector3.new(0, 1, 0)).lookVector, 4* Player_Size, char)
	if equipped == true or equipped == false then
		if attack == false then
			idle = idle + 1
		else
			idle = 0
		end
		if 1 < root.Velocity.y and hitfloor == nil then
			Anim = "Jump"
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, 0.9 + 0.5* Player_Size * Cos(sine / -15)) * angles(Rad(0), Rad(0), Rad(0)), 0.17)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(10 - 2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -.2 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * RHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -.9 - 0.1 * Cos(sine / 20), -.5* Player_Size) * LHCF * angles(Rad(-2.5), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(25), Rad(-.6), Rad(13 + 4.5 * Sin(sine / 20))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(25), Rad(-.6), Rad(-13 - 4.5 * Sin(sine / 20))), 0.1)
			end
		elseif -1 > root.Velocity.y and hitfloor == nil then
			Anim = "Fall"
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.1 + 0.1 * Cos(sine / 20)* Player_Size) * angles(Rad(24), Rad(0), Rad(0)), 0.15)
				neck.C0 = clerp(neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(10 - 2.5 * Sin(sine / 30)), Rad(0), Rad(0)), 0.3)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -1 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * RHCF * angles(Rad(-3.5), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -.8 - 0.1 * Cos(sine / 20)* Player_Size, -.3* Player_Size) * LHCF * angles(Rad(-3.5), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(65), Rad(-.6), Rad(45 + 4.5 * Sin(sine / 20))), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5* Player_Size, 0.5 + 0.02 * Sin(sine / 20)* Player_Size, 0* Player_Size) * angles(Rad(55), Rad(-.6), Rad(-45 - 4.5 * Sin(sine / 20))), 0.1)
			end
		elseif torvel < 1 and hitfloor ~= nil then
			Anim = "Idle"
			change = 1
 			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0 - 0.04 * Sin(sine / 24) * Player_Size, 0 + 0.04 * Sin(sine / 12) * Player_Size, 0 + 0.05 * Player_Size * Cos(sine / 12)) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(0 - 2.5 * Sin(sine / 24)), Rad(0)), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-6.5 * Cos(sine / 12)), Rad(10), Rad(0)), 0.3)
				RH.C0 = clerp(RH.C0, CF(1 * Player_Size, -1 * Player_Size + 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(84), Rad(0)) * angles(Rad(-6 - 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				LH.C0 = clerp(LH.C0, CF(-1 * Player_Size, -1 * Player_Size - 0.06 * Sin(sine / 24) - 0.05 * Player_Size * Cos(sine / 12), -0.01 * Player_Size) * angles(Rad(0 - 2.5 * Sin(sine / 12)), Rad(-84), Rad(0)) * angles(Rad(-6 + 2.5 * Sin(sine / 24)), Rad(0), Rad(0)), 0.15)
				RW.C0 = clerp(RW.C0, CF(1.4* Player_Size, 0.75 + 0 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(-6 + 4.5 * Sin(sine / 12)), Rad(25 + 2.5 * Sin(sine / 12)), Rad(5)), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.4* Player_Size, 0.75 + 0 * Cos(sine / 12)* Player_Size, 0* Player_Size) * angles(Rad(7 + 4.5 * Sin(sine / 12)), Rad(0 + 2.5 * Sin(sine / 12)), Rad(-5)), 0.1)
			end
		elseif torvel > 2 and torvel < 25 and hitfloor ~= nil then
			Anim = "Walk"
			change = 0.5
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.175 + 0.13 * Cos(sine / 3.5) + -Sin(sine / 3.5) / 7* Player_Size) * angles(Rad(3 - 2.5 * Cos(sine / 3.5)), Rad(0) - root.RotVelocity.Y / 75, Rad(15 * Cos(sine / 7))), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(0), Rad(0), Rad(0) - hed.RotVelocity.Y / 15), 0.3)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.8 - 0.5 * Cos(sine / 7) / 2* Player_Size, 0.6 * Cos(sine / 7) / 2* Player_Size)  * angles(Rad(-10 - 25 * Cos(sine / 7)) - rl.RotVelocity.Y / 75 + -Sin(sine / 7) / 2.5, Rad(90 - 15 * Cos(sine / 7)), Rad(0)) * angles(Rad(0 + 2 * Cos(sine / 7)), Rad(0), Rad(0)), 0.3)
         		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.8 + 0.5 * Cos(sine / 7) / 2* Player_Size, -0.6 * Cos(sine / 7) / 2* Player_Size) * angles(Rad(-10 + 25 * Cos(sine / 7)) + ll.RotVelocity.Y / 75 + Sin(sine / 7) / 2.5, Rad(-90 - 15 * Cos(sine / 7)), Rad(0)) * angles(Rad(0 - 2 * Cos(sine / 7)), Rad(0), Rad(0)), 0.3)
				RW.C0 = clerp(RW.C0, CF(1.4* Player_Size, 0.75 + 0.05 * Sin(sine / 7)* Player_Size, 0* Player_Size) * angles(Rad(47)  * Cos(sine / 7) , Rad(10 * Cos(sine / 7)), Rad(0) - ra.RotVelocity.Y / 75), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.4* Player_Size, 0.75+ 0.05 * Sin(sine / 7)* Player_Size, 0* Player_Size) * angles(Rad(-47)  * Cos(sine / 7) , Rad(10 * Cos(sine / 7)) ,	Rad(0) + la.RotVelocity.Y / 75), 0.1)
			end
		elseif torvel >= 25 and hitfloor ~= nil then
			Anim = "Sprint"
			change = 1.35
			if attack == false then
				rootj.C0 = clerp(rootj.C0, RootCF * CF(0* Player_Size, 0* Player_Size, -0.175 + 0.7 * Cos(sine / 3.5) + -Sin(sine / 3.5) / 7* Player_Size) * angles(Rad(20 - 5.5 * Cos(sine / 3.5)), Rad(-14 - 19 * Cos(sine / 7)) - root.RotVelocity.Y / 75, Rad(-17 + 19 * Cos(sine / 7))), 0.15)
				tors.Neck.C0 = clerp(tors.Neck.C0, necko* CF(0, 0, 0 + ((1* Player_Size) - 1)) * angles(Rad(-20), Rad(17 + 19 * Cos(sine / 7)), Rad(17) - hed.RotVelocity.Y / 15), 0.3)
				RH.C0 = clerp(RH.C0, CF(1* Player_Size, -0.8 - 1 * Cos(sine / 7) / 2* Player_Size, 0.6 * Cos(sine / 7) / 0.5* Player_Size)  * angles(Rad(0 - 5 * Cos(sine / 7)) - rl.RotVelocity.Y / 75 + -Sin(sine / 7) / 1, Rad(90 - 15 * Cos(sine / 7)), Rad(0)) * angles(Rad(-14 - 2 * Cos(sine / 7)), Rad(0), Rad(-20)), 0.3)
         		LH.C0 = clerp(LH.C0, CF(-1* Player_Size, -0.8 + 1 * Cos(sine / 7) / 2* Player_Size, -0.6 * Cos(sine / 7) / 0.8* Player_Size) * angles(Rad(5 + 5 * Cos(sine / 7)) + ll.RotVelocity.Y / 75 + Sin(sine / 7) / 1.5, Rad(-90 - 15 * Cos(sine / 7)), Rad(0)) * angles(Rad(10 + 2 * Cos(sine / 7)), Rad(0), Rad(20)), 0.3)
				RW.C0 = clerp(RW.C0, CF(1.3* Player_Size, 1 + 0.9 * Sin(sine / 7)* Player_Size, 0* Player_Size) * angles(Rad(50)  , Rad(90), Rad(10) - ra.RotVelocity.Y / 75), 0.1)
				LW.C0 = clerp(LW.C0, CF(-1.5, 0.5 + 1 * Sin(sine / 7), 0.15 * Cos(sine / 7)) * angles(Rad(130 + 30.5 * Cos(sine / 7)) , Rad(-90),	Rad(28) + la.RotVelocity.Y / 75), 0.1)
			end
		end
	end
	if 0 < #Effects then
		for e = 1, #Effects do
			if Effects[e] ~= nil then
				local Thing = Effects[e]
				if Thing ~= nil then
					local Part = Thing[1]
					local Mode = Thing[2]
					local Delay = Thing[3]
					local IncX = Thing[4]
					local IncY = Thing[5]
					local IncZ = Thing[6]
					if 1 >= Thing[1].Transparency then
						if Thing[2] == "Block1" then
							Thing[1].CFrame = Thing[1].CFrame * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50))
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Block2" then
							Thing[1].CFrame = Thing[1].CFrame + Vector3.new(0, 0, 0)
							local Mesh = Thing[7]
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Block3" then
							Thing[1].CFrame = Thing[1].CFrame * CFrame.fromEulerAnglesXYZ(math.random(-50, 50), math.random(-50, 50), math.random(-50, 50)) + Vector3.new(0, 0.15, 0)
							local Mesh = Thing[7]
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Cylinder" then
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Blood" then
							local Mesh = Thing[7]
							Thing[1].CFrame = Thing[1].CFrame * Vector3.new(0, 0.5, 0)
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[4], Thing[5], Thing[6])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Elec" then
							local Mesh = Thing[1].Mesh
							Mesh.Scale = Mesh.Scale + Vector3.new(Thing[7], Thing[8], Thing[9])
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Disappear" then
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
						elseif Thing[2] == "Shatter" then
							Thing[1].Transparency = Thing[1].Transparency + Thing[3]
							Thing[4] = Thing[4] * CFrame.new(0, Thing[7], 0)
							Thing[1].CFrame = Thing[4] * CFrame.fromEulerAnglesXYZ(Thing[6], 0, 0)
							Thing[6] = Thing[6] + Thing[5]
						end
					else
						Part.Parent = nil
						table.remove(Effects, e)
					end
				end
			end
		end
	end
end
-----------------------------------------------------3