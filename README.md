local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Function to add highlight to a character
local function addHighlight(character)
    if not character:FindFirstChild("HumanoidRootPart") then return end
    
    local highlight = Instance.new("Highlight")
    highlight.Parent = character
    highlight.Adornee = character
    highlight.FillTransparency = 1 -- Make the fill transparent
    highlight.OutlineColor = Color3.new(1, 0, 0) -- Red color
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop -- Ensure it's visible through walls
end

-- Function to remove highlight from a character
local function removeHighlight(character)
    local highlight = character:FindFirstChildOfClass("Highlight")
    if highlight then
        highlight:Destroy()
    end
end

-- Function to handle new players
local function onPlayerAdded(player)
    if player == LocalPlayer then return end

    -- Handle the player's character when it's added
    player.CharacterAdded:Connect(function(character)
        addHighlight(character)
    end)
    
    -- Handle the player's character if it already exists
    if player.Character then
        addHighlight(player.Character)
    end
end

-- Connect existing players
for _, player in pairs(Players:GetPlayers()) do
    onPlayerAdded(player)
end

-- Connect new players
Players.PlayerAdded:Connect(onPlayerAdded)

-- Remove highlights if players leave
Players.PlayerRemoving:Connect(function(player)
    if player.Character then
        removeHighlight(player.Character)
    end
end)
