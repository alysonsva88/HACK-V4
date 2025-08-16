-- LocalScript (StarterPlayerScripts)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- Criar GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "RiotHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 120)
frame.Position = UDim2.new(0.05, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 255)
title.Text = "🚀 Riot Hub"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextScaled = true
title.Parent = frame

-- Botões
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(1, 0, 0, 30)
toggleBtn.Position = UDim2.new(0, 0, 0, 35)
toggleBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
toggleBtn.Text = "Ativar Plataforma"
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextScaled = true
toggleBtn.Parent = frame

local speedBtn = Instance.new("TextButton")
speedBtn.Size = UDim2.new(1, 0, 0, 30)
speedBtn.Position = UDim2.new(0, 0, 0, 70)
speedBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
speedBtn.Text = "Boost Speed: OFF"
speedBtn.TextColor3 = Color3.new(1, 1, 1)
speedBtn.Font = Enum.Font.GothamBold
speedBtn.TextScaled = true
speedBtn.Parent = frame

-- Criar plataforma
local platform = Instance.new("Part")
platform.Size = Vector3.new(6, 1, 6)
platform.Anchored = true
platform.Color = Color3.fromRGB(0, 0, 255)
platform.Material = Enum.Material.Neon
platform.CanCollide = true
platform.Transparency = 0
platform.Parent = workspace
platform.Name = "RiotPlatform"
platform.Position = Vector3.new(0, -500, 0) -- começa escondida

local ativo = false
local speedActive = false
local conn

-- Atualiza humanoid ao respawn
player.CharacterAdded:Connect(function(char)
    character = char
    hrp = character:WaitForChild("HumanoidRootPart")
    humanoid = character:WaitForChild("Humanoid")
end)

-- Toggle plataforma
local function togglePlataforma()
    ativo = not ativo
    if ativo then
        toggleBtn.Text = "Desativar Plataforma"
        conn = RunService.RenderStepped:Connect(function()
            if hrp and hrp.Parent then
                local pos = hrp.Position
                platform.Position = Vector3.new(pos.X, pos.Y - 3, pos.Z)
            end
        end)
    else
        toggleBtn.Text = "Ativar Plataforma"
        if conn then conn:Disconnect() end
        platform.Position = Vector3.new(0, -500, 0)
    end
end

toggleBtn.MouseButton1Click:Connect(togglePlataforma)

-- Função Speed disfarçada (leve aumento)
local normalSpeed = humanoid.WalkSpeed
local boostSpeed = normalSpeed + 4 -- leve aumento, discreto

speedBtn.MouseButton1Click:Connect(function()
    speedActive = not speedActive
    if speedActive then
        humanoid.WalkSpeed = boostSpeed
        speedBtn.Text = "Boost Speed: ON"
    else
        humanoid.WalkSpeed = normalSpeed
        speedBtn.Text = "Boost Speed: OFF"
    end
end) 
