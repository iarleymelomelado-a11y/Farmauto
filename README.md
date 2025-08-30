--[[ 
AutoMine Script para Roblox Executor
Procura por objetos "ore", "coal" e "mine" e os minera automaticamente usando o item "Pick".
]]

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local backpack = player:WaitForChild("Backpack")
local pick = backpack:FindFirstChild("Pick") or character:FindFirstChild("Pick")

-- Função para teletransportar o personagem
local function teleportTo(part)
    if character and character:FindFirstChild("HumanoidRootPart") then
        character.HumanoidRootPart.CFrame = part.CFrame + Vector3.new(0, 3, 0)
    end
end

-- Função para minerar
local function mineOre(ore)
    teleportTo(ore)
    -- Equipar a Pick, se não estiver equipada
    if pick and not character:FindFirstChild("Pick") then
        pick.Parent = character
    end
    -- Simular clique/ação
    local tool = character:FindFirstChild("Pick")
    if tool and tool:FindFirstChild("Activate") then
        tool:Activate()
    else
        -- Tenta simular clique via RemoteEvent
        for _, v in pairs(tool:GetChildren()) do
            if v:IsA("RemoteEvent") then
                v:FireServer()
            elseif v:IsA("RemoteFunction") then
                v:InvokeServer()
            end
        end
    end
    wait(0.5) -- Aguarda um pouco para não bugar
end

-- Loop principal
while wait(1) do
    for _, obj in pairs(workspace:GetChildren()) do
        if obj.Name:lower():find("ore") or obj.Name:lower():find("coal") or obj.Name:lower():find("mine") then
            mineOre(obj)
            wait(0.5)
        end
    end
end
