-- LocalScript para StarterPlayerScripts

local p = game.Players.LocalPlayer
local m = p:GetMouse()
local u = game:GetService("UserInputService")
local tE, iE, cd = false, false, false

local function c() return p.Character or p.CharacterAdded:Wait() end
local function r() return c():FindFirstChild("HumanoidRootPart") end
local function inv(e)
    for _,v in ipairs(c():GetChildren()) do
        if v:IsA("BasePart") or v:IsA("Decal") then v.Transparency = e and 1 or 0 end
    end
end

local sg = Instance.new("ScreenGui", p:WaitForChild("PlayerGui"))
local f = Instance.new("Frame", sg)
f.Size = UDim2.new(0,220,0,120)
f.Position = UDim2.new(0.5,-110,0.85,0)
f.BackgroundColor3 = Color3.fromRGB(30,30,30)
f.Active = true

local drag, dI, dS, sP
f.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        drag, dS, sP = true, i.Position, f.Position
        i.Changed:Connect(function() if i.UserInputState==Enum.UserInputState.End then drag=false end end)
    end
end)
f.InputChanged:Connect(function(i) if i.UserInputType==Enum.UserInputType.MouseMovement then dI=i end end)
u.InputChanged:Connect(function(i)
    if drag and i==dI then
        local d = i.Position-dS
        f.Position = UDim2.new(sP.X.Scale,sP.X.Offset+d.X,sP.Y.Scale,sP.Y.Offset+d.Y)
    end
end)

local tb = Instance.new("TextButton", f)
tb.Size = UDim2.new(0.85,0,0.35,0)
tb.Position = UDim2.new(0.075,0,0.1,0)
tb.Text = "Ativar Teleporte (H)"
tb.BackgroundColor3 = Color3.fromRGB(50,180,255)
tb.TextColor3 = Color3.fromRGB(255,255,255)
tb.Font = Enum.Font.SourceSansBold
tb.TextSize = 22

tb.MouseButton1Click:Connect(function()
    tE = not tE
    tb.Text = tE and "Desativar Teleporte (H)" or "Ativar Teleporte (H)"
    tb.BackgroundColor3 = tE and Color3.fromRGB(60,230,150) or Color3.fromRGB(50,180,255)
end)

local ib = Instance.new("TextButton", f)
ib.Size = UDim2.new(0.85,0,0.35,0)
ib.Position = UDim2.new(0.075,0,0.55,0)
ib.Text = "Ativar Invisibilidade"
ib.BackgroundColor3 = Color3.fromRGB(180,50,255)
ib.TextColor3 = Color3.fromRGB(255,255,255)
ib.Font = Enum.Font.SourceSansBold
ib.TextSize = 22

ib.MouseButton1Click:Connect(function()
    iE = not iE
    inv(iE)
    ib.Text = iE and "Desativar Invisibilidade" or "Ativar Invisibilidade"
    ib.BackgroundColor3 = iE and Color3.fromRGB(230,60,150) or Color3.fromRGB(180,50,255)
end)

p.CharacterAdded:Connect(function() if iE then inv(true) end end)

u.InputBegan:Connect(function(i,g)
    if g then return end
    if tE and i.KeyCode==Enum.KeyCode.H and not cd then
        cd = true
        local rp = r()
        if rp then rp.CFrame = CFrame.new(m.Hit.p) end
        task.wait(1)
        cd = false
    end
end)# test1
