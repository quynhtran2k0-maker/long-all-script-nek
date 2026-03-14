# long-all-script-nek-- Simple GUI Hub (learning example)

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player.PlayerGui)
gui.Name = "LongAllScriptHub"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0,200,0,220)
frame.Position = UDim2.new(0,20,0,100)
frame.BackgroundColor3 = Color3.fromRGB(30,30,30)

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1,0,0,30)
title.Text = "Long-All-Script Hub"
title.TextColor3 = Color3.new(1,1,1)
title.BackgroundTransparency = 1

-- Button creator
function createButton(name, y, callback)
    local btn = Instance.new("TextButton", frame)
    btn.Size = UDim2.new(1,-10,0,30)
    btn.Position = UDim2.new(0,5,0,y)
    btn.Text = name
    btn.MouseButton1Click:Connect(callback)
end

-- Fly
local flying = false
createButton("Fly",40,function()
    flying = not flying
    if flying then
        local body = Instance.new("BodyVelocity")
        body.MaxForce = Vector3.new(9e9,9e9,9e9)
        body.Velocity = Vector3.new(0,50,0)
        body.Parent = player.Character.HumanoidRootPart
    else
        for _,v in pairs(player.Character.HumanoidRootPart:GetChildren()) do
            if v:IsA("BodyVelocity") then v:Destroy() end
        end
    end
end)

-- NoClip
local noclip = false
createButton("NoClip",80,function()
    noclip = not noclip
end)

game:GetService("RunService").Stepped:Connect(function()
    if noclip and player.Character then
        for _,v in pairs(player.Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = false
            end
        end
    end
end)

-- Anti AFK
createButton("Anti AFK",120,function()
    local vu = game:GetService("VirtualUser")
    player.Idled:Connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end)
end)

-- Simple ESP players
createButton("ESP Player",160,function()
    for _,p in pairs(game.Players:GetPlayers()) do
        if p ~= player and p.Character and not p.Character:FindFirstChild("Highlight") then
            local h = Instance.new("Highlight")
            h.FillColor = Color3.fromRGB(255,0,0)
            h.Parent = p.Character
        end
    end
end)
