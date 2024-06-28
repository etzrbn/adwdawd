-- Constants for the outline color and thickness
local OUTLINE_COLOR = Color3.new(1, 0, 0) -- Red color
local OUTLINE_THICKNESS = 0.1

-- Function to add outline effect to a part
local function addOutline(part)
    local outline = Instance.new("SelectionBox")
    outline.Adornee = part
    outline.Color3 = OUTLINE_COLOR
    outline.LineThickness = OUTLINE_THICKNESS
    outline.Parent = part
end

-- Function to create a nametag above the player
local function createNameTag(player)
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        local existingTag = player.Character:FindFirstChild("NameTag")
        if existingTag then
            existingTag:Destroy()
        end
        
        local nameTag = Instance.new("BillboardGui")
        nameTag.Name = "NameTag"
        nameTag.AlwaysOnTop = true
        nameTag.Size = UDim2.new(3, 0, 1, 0) -- Adjust size as needed
        nameTag.StudsOffset = Vector3.new(0, 3, 0) -- Adjust offset as needed

        local nameLabel = Instance.new("TextLabel")
        nameLabel.Text = player.Name
        nameLabel.Size = UDim2.new(1, 0, 1, 0)
        nameLabel.TextScaled = true
        nameLabel.Parent = nameTag

        nameTag.Parent = player.Character.Head
    end
end

-- Function to handle updating outlines and nametags for all players
local function refreshPlayerData()
    for _, player in ipairs(game.Players:GetPlayers()) do
        local character = player.Character
        if character then
            for _, part in ipairs(character:GetDescendants()) do
                if part:IsA("BasePart") then
                    local existingOutline = part:FindFirstChildOfClass("SelectionBox")
                    if existingOutline then
                        existingOutline:Destroy()
                    end
                    addOutline(part)
                end
            end
            createNameTag(player)
        end
    end
end

-- Connect player added event
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        refreshPlayerData()
    end)
end)

-- Initial setup for players already in the game
for _, player in ipairs(game.Players:GetPlayers()) do
    player.CharacterAdded:Connect(function(character)
        refreshPlayerData()
    end)
end

-- Start periodic refresh (every 5 seconds, adjust as needed)
while true do
    refreshPlayerData()
    wait(5) -- Wait 5 seconds before refreshing again
end
