local loadstring_script = [[
    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local Workspace = game:GetService("Workspace")
    local Ball = Workspace:FindFirstChild("Ball")
    
    -- Auto GoalKeeper
    local GOAL_AREA = Region3.new(Vector3.new(-10, 0, -10), Vector3.new(10, 10, 10))  -- Ajuste para seu gol

    local function isInGoalArea(position)
        return GOAL_AREA:ContainsPoint(position)
    end

    local function onBallTouched(hit)
        if hit and hit.Parent and hit.Parent:FindFirstChild("Humanoid") then
            local player = Players:GetPlayerFromCharacter(hit.Parent)
            if player and player.Character and player.Character.PrimaryPart and isInGoalArea(player.Character.PrimaryPart.Position) then
                Ball.Velocity = Vector3.new(0, 0, 0)
                Ball.Position = Vector3.new(0, 0, 0)
            end
        end
    end

    if Ball then
        Ball.Touched:Connect(onBallTouched)
    end

    -- Lock on the Ball
    local function onPlayerAdded(player)
        local camera = Workspace.CurrentCamera

        RunService.RenderStepped:Connect(function()
            if player.Character and player.Character:FindFirstChild("Head") and Ball then
                camera.CFrame = CFrame.new(player.Character.Head.Position, Ball.Position)
            end
        end)
    end

    Players.PlayerAdded:Connect(onPlayerAdded)

    -- ESP
    local function createESP(part, color)
        local esp = Instance.new("BoxHandleAdornment")
        esp.Size = part.Size
        esp.Color3 = color
        esp.AlwaysOnTop = true
        esp.Adornee = part
        esp.Transparency = 0.5
        esp.ZIndex = 0
        esp.Parent = part
    end

    local function onESPPlayerAdded(player)
        player.CharacterAdded:Connect(function(character)
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    createESP(part, Color3.new(1, 0, 0))  -- Cor vermelha para o ESP
                end
            end
        end)
    end

    Players.PlayerAdded:Connect(onESPPlayerAdded)
]]

loadstring(loadstring_script)()
