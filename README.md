local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local LockRadius = 35 -- Adjust the lock-on radius as needed
local DPadUpButton = Enum.KeyCode.DPadUp -- Adjust the DPad Up button as needed

local function FindNearestPlayer()
    local MaxDistance = math.huge
    local Target = nil

    for _, Player in ipairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            local CharacterRootPart = Player.Character:FindFirstChild("HumanoidRootPart")
            local Distance = (CharacterRootPart.Position - Camera.CFrame.Position).Magnitude
            if Distance < MaxDistance and Distance <= LockRadius then
                MaxDistance = Distance
                Target = Player
            end
        end
    end

    return Target
end

local function TeleportBehindPlayer(Player)
    if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
        local CharacterRootPart = Player.Character:FindFirstChild("HumanoidRootPart")
        if CharacterRootPart then
            local CharacterDirection = CharacterRootPart.CFrame.LookVector
            local TeleportPosition = CharacterRootPart.Position + (CharacterDirection * -5) -- Teleport 10 units behind the player
            LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(TeleportPosition, CharacterRootPart.Position)
        end
    end
end

local function HandleTeleport()
    local TargetPlayer = FindNearestPlayer()
    if TargetPlayer then
        TeleportBehindPlayer(TargetPlayer)
    end
end

game:GetService("ContextActionService"):BindAction("TeleportAction", HandleTeleport, false, DPadUpButton)
p
