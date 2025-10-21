--[[
Minh Hub Full Script v2.0 – Fly Menu nhỏ gọn tích hợp
LocalScript, Studio / Local testing only
--]]

-- Load Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "Menu tổng hợp Minh Hub",
    LoadingTitle = "Minh Hub",
    LoadingSubtitle = "Menu Tổng Hợp - Local/Test Only",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "MinhHubConfig"
    },
    KeySystem = false
})

-- === Globals ===
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInput = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- === Fly Menu Small GUI ===
local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local root = character:WaitForChild("HumanoidRootPart")

local fly = {enabled=false, speed=50, bv=nil, bg=nil}

local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "FlyMenuMini"
gui.ResetOnSpawn=false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0,220,0,120)
frame.Position = UDim2.new(0.7,0,0.7,0)
frame.BackgroundColor3 = Color3.fromRGB(0,0,0)
frame.BackgroundTransparency = 0.4
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(0.8,0,0.2,0)
title.BackgroundTransparency=1
title.Text="✈️ Fly Menu"
title.TextColor3=Color3.fromRGB(255,255,255)
title.Font=Enum.Font.GothamBold
title.TextScaled=true

local btnX = Instance.new("TextButton", frame)
btnX.Size=UDim2.new(0.2,0,0.4,0)
btnX.Position=UDim2.new(0.8,0,0,0)
btnX.BackgroundColor3=Color3.fromRGB(255,0,0)
btnX.Text="X"
btnX.TextColor3=Color3.fromRGB(255,255,255)
btnX.Font=Enum.Font.GothamBold
btnX.TextScaled=true
btnX.MouseButton1Click:Connect(function()
    gui:Destroy()
    fly.enabled=false
    if fly.bg then fly.bg:Destroy() end
    if fly.bv then fly.bv:Destroy() end
    humanoid.PlatformStand=false
end)

local toggle = Instance.new("TextButton", frame)
toggle.Size=UDim2.new(0.4,0,0.25,0)
toggle.Position=UDim2.new(0.05,0,0.3,0)
toggle.BackgroundColor3=Color3.fromRGB(255,0,0)
toggle.Text="OFF"
toggle.TextColor3=Color3.fromRGB(255,255,255)
toggle.Font=Enum.Font.GothamBold
toggle.TextScaled=true

local speedLabel = Instance.new("TextLabel", frame)
speedLabel.Size=UDim2.new(0.5,0,0.2,0)
speedLabel.Position=UDim2.new(0.45,0,0.3,0)
speedLabel.BackgroundTransparency=1
speedLabel.Text="Speed: "..fly.speed
speedLabel.TextColor3=Color3.fromRGB(255,255,255)
speedLabel.Font=Enum.Font.Gotham
speedLabel.TextScaled=true

local plus = Instance.new("TextButton", frame)
plus.Size=UDim2.new(0.4,0,0.25,0)
plus.Position=UDim2.new(0.05,0,0.6,0)
plus.BackgroundColor3=Color3.fromRGB(0,170,255)
plus.Text="+ Speed"
plus.TextColor3=Color3.fromRGB(255,255,255)
plus.Font=Enum.Font.GothamBold
plus.TextScaled=true

local minus = Instance.new("TextButton", frame)
minus.Size=UDim2.new(0.4,0,0.25,0)
minus.Position=UDim2.new(0.55,0,0.6,0)
minus.BackgroundColor3=Color3.fromRGB(255,170,0)
minus.Text="- Speed"
minus.TextColor3=Color3.fromRGB(255,255,255)
minus.Font=Enum.Font.GothamBold
minus.TextScaled=true

toggle.MouseButton1Click:Connect(function()
    fly.enabled = not fly.enabled
    if fly.enabled then
        toggle.BackgroundColor3=Color3.fromRGB(0,255,0)
        toggle.Text="ON"
        fly.bg=Instance.new("BodyGyro")
        fly.bg.P=9e5
        fly.bg.MaxTorque=Vector3.new(9e9,9e9,9e9)
        fly.bg.CFrame=root.CFrame
        fly.bg.Parent=root

        fly.bv=Instance.new("BodyVelocity")
        fly.bv.MaxForce=Vector3.new(9e9,9e9,9e9)
        fly.bv.Velocity=Vector3.zero
        fly.bv.Parent=root

        humanoid.PlatformStand=true
    else
        toggle.BackgroundColor3=Color3.fromRGB(255,0,0)
        toggle.Text="OFF"
        if fly.bg then fly.bg:Destroy() end
        if fly.bv then fly.bv:Destroy() end
        humanoid.PlatformStand=false
    end
end)

plus.MouseButton1Click:Connect(function()
    fly.speed=math.clamp(fly.speed+10,10,300)
    speedLabel.Text="Speed: "..fly.speed
end)

minus.MouseButton1Click:Connect(function()
    fly.speed=math.clamp(fly.speed-10,10,300)
    speedLabel.Text="Speed: "..fly.speed
end)

RunService.RenderStepped:Connect(function()
    if fly.enabled and fly.bg and fly.bv then
        local move = humanoid.MoveDirection
        local cam = workspace.CurrentCamera
        fly.bg.CFrame = cam.CFrame
        if move.Magnitude>0 then
            local x=move.X*fly.speed
            local z=move.Z*fly.speed
            local y=cam.CFrame.LookVector.Y*move.Magnitude*fly.speed
            fly.bv.Velocity=Vector3.new(x,y,z)
        else
            fly.bv.Velocity=Vector3.new(0,0,0)
        end
    end
end)

-- === Tab 1 – Main ===
local TabMain = Window:CreateTab("Main",448336245)
local SecMain = TabMain:CreateSection("Main Functions")
SecMain:CreateToggle({
    Name="Aim NPC (Local)",
    CurrentValue=false,
    Flag="AimNPC",
    Callback=function(v)
        if v then
            -- enable local Aim NPC
            if not _G.AimConn then
                _G.AimConn = RunService.RenderStepped:Connect(function()
                    local nearest,nDist = nil,math.huge
                    for _,m in pairs(workspace:GetDescendants()) do
                        if m:IsA("Model") and m:FindFirstChild("Humanoid") and m:FindFirstChild("HumanoidRootPart") then
                            local owner = Players:GetPlayerFromCharacter(m)
                            if not owner then
                                local d = (LocalPlayer.Character.HumanoidRootPart.Position - m.HumanoidRootPart.Position).Magnitude
                                if d<nDist then nearest,nDist=m,d end
                            end
                        end
                    end
                    if nearest then
                        local cam=workspace.CurrentCamera
                        local look=CFrame.new(cam.CFrame.Position, nearest.HumanoidRootPart.Position)
                        cam.CFrame=cam.CFrame:Lerp(look,0.08)
                    end
                end)
            end
        else
            if _G.AimConn then _G.AimConn:Disconnect(); _G.AimConn=nil end
        end
    end
})

-- === Tab 2 – Support ===
local TabSup = Window:CreateTab("Support",448336245)
local SecSup = TabSup:CreateSection("Player Tools")

local noclip=false
SecSup:CreateToggle({
    Name="Noclip",
    CurrentValue=false,
    Flag="Noclip",
    Callback=function(v)
        noclip=v
        local char=LocalPlayer.Character
        if not char then return end
        for _,p in pairs(char:GetDescendants()) do
            if p:IsA("BasePart") then
                p.CanCollide=not v
                p.Material=v and Enum.Material.Neon or Enum.Material.SmoothPlastic
                p.Color=v and Color3.fromRGB(0,170,255) or Color3.fromRGB(0,0,0)
            end
        end
    end
})

local speedPlayer=16
SecSup:CreateSlider({
    Name="Speed",
    Range={10,1000},
    Increment=5,
    CurrentValue=speedPlayer,
    Flag="Speed",
    Callback=function(v)
        speedPlayer=v
        local humanoid=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
        if humanoid then humanoid.WalkSpeed=v end
    end
})

local infinityJump=false
SecSup:CreateToggle({
    Name="Infinity Jump",
    CurrentValue=false,
    Flag="InfinityJump",
    Callback=function(v) infinityJump=v end
})
UserInput.InputBegan:Connect(function(input)
    if infinityJump and input.UserInputType==Enum.UserInputType.Keyboard then
        local char=LocalPlayer.Character
        if char and char:FindFirstChild("Humanoid") then
            char.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

local jumpPower=50
SecSup:CreateSlider({
    Name="Jump Power",
    Range={10,10000},
    Increment=10,
    CurrentValue=jumpPower,
    Flag="JumpPower",
    Callback=function(v)
        jumpPower=v
        local humanoid=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid")
        if humanoid then humanoid.JumpPower=v end
    end
})

-- === Tab 3 – Settings ===
local TabSet = Window:CreateTab("Settings",448336245)
local SecSet = TabSet:CreateSection("Configuration & UI")

SecSet:CreateButton({
    Name="Save Setting",
    Callback=function() Rayfield:SaveConfiguration() print("Saved") end
})
SecSet:CreateButton({
    Name="Load Setting",
    Callback=function() Rayfield:LoadConfiguration() print("Loaded") end
})

SecSet:CreateDropdown({
    Name="Menu Color",
    Options
