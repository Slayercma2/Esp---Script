-- Script para destacar jogadores e visualizar times
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local teamPirates = "Piratas"
local teamMarines = "Marinheiros"

local markers = {}
local active = true

local function createMarker(player, color)
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Size = UDim2.new(1, 0, 1, 0)
    billboardGui.Adornee = player.Character.Head
    billboardGui.Parent = player.Character.Head
    
    local frame = Instance.new("Frame", billboardGui)
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundTransparency = 0.5
    frame.BackgroundColor3 = color
    
    markers[player] = billboardGui
end

local function removeMarker(player)
    if markers[player] then
        markers[player]:Destroy()
        markers[player] = nil
    end
end

local function updateMarkers()
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Team then
            if player.Team.Name == teamPirates then
                createMarker(player, Color3.fromRGB(255, 0, 0))
            elseif player.Team.Name == teamMarines then
                createMarker(player, Color3.fromRGB(0, 0, 255))
            else
                removeMarker(player)
            end
        end
    end
end

local function toggleMarkers()
    active = not active
    if active then
        updateMarkers()
    else
        for _, marker in pairs(markers) do
            marker:Destroy()
        end
        markers = {}
    end
end

-- Ativa a atualização dos marcadores
RunService.RenderStepped:Connect(function()
    if active then
        updateMarkers()
    end
end)

-- Define a tecla "T" para ativar e desativar o script
game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.T then
        toggleMarkers()
    end
end)
