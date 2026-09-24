local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportEvent = ReplicatedStorage:WaitForChild("TeleportPetEvent")

local button = script.Parent

-- Defina o nome exato do pet que este botão representa
local PET_NAME = "Dragao" 

button.MouseButton1Click:Connect(function()
    -- Dispara o pedido ao servidor
    TeleportEvent:FireServer(PET_NAME)
end)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportEvent = ReplicatedStorage:WaitForChild("TeleportPetEvent")

-- Função para localizar a posição do ovo no mapa
local function getEggCFrame()
    local egg = workspace:FindFirstChild("OvoEspecial") -- Nome do modelo do ovo na Workspace
    if egg then
        if egg.PrimaryPart then
            return egg.PrimaryPart.CFrame
        else
            return egg:GetPivot()
        end
    end
    return nil
end

-- Recebe a solicitação da interface do jogador
TeleportEvent.OnServerEvent:Connect(function(player, petName)
    local eggCFrame = getEggCFrame()
    
    if not eggCFrame then
        warn("Ovo não encontrado no Workspace!")
        return
    end

    local character = player.Character
    if not character then return end

    -- Procura o pet dentro do personagem ou no modelo de pets do jogador
    local pet = character:FindFirstChild(petName)
    
    if pet then
        -- Teleporta o pet para 3 blocos ao lado do ovo
        local targetPosition = eggCFrame * CFrame.new(3, 0, 0)
        
        if pet:IsA("Model") then
            pet:PivotTo(targetPosition)
        elseif pet:IsA("BasePart") then
            pet.CFrame = targetPosition
        end
    else
        warn("Pet '" .. tostring(petName) .. "' não foi encontrado com o jogador.")
    end
end)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportEvent = Instance.new("RemoteEvent")
TeleportEvent.Name = "TeleportPetEvent"
TeleportEvent.Parent = ReplicatedStorage

-- Função para encontrar o ovo no cenário
local function getEggPosition()
    local egg = workspace:FindFirstChild("OvoEspecial") -- Nome do modelo do ovo no Workspace
    if egg then
        return egg.PrimaryPart and egg.PrimaryPart.CFrame or egg:GetPivot()
    end
    return nil
end

TeleportEvent.OnServerEvent:Connect(function(player, petName)
    local eggCFrame = getEggPosition()
    
    if not eggCFrame then
        warn("Ovo não encontrado no cenário!")
        return
    end

    -- Procura o pet do jogador (assumindo que o pet esteja anexado ao personagem)
    local character = player.Character
    if character then
        local pet = character:FindFirstChild(petName)
        if pet then
            -- Move o pet para perto do ovo
            pet:PivotTo(eggCFrame * CFrame.new(0, 2, 0))
        end
    end
end)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportEvent = ReplicatedStorage:WaitForChild("TeleportPetEvent")

local button = script.Parent
local selectedPetName = "Dragao" -- Nome do pet configurado no botão

button.MouseButton1Click:Connect(function()
    -- Envia o pedido de teleportar o pet para o servidor
    TeleportEvent:FireServer(selectedPetName)
end)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportEvent = ReplicatedStorage:WaitForChild("TeleportPetEvent")

local button = script.Parent
local cooldown = false
local TEMPO_DE_ESPERA = 3 -- Segundos

button.MouseButton1Click:Connect(function()
    if cooldown then return end
    
    cooldown = true
    button.AutoButtonColor = false
    
    TeleportEvent:FireServer("Dragao")
    
    task.wait(TEMPO_DE_ESPERA)
    cooldown = false
    button.AutoButtonColor = true
end)
TeleportEvent.OnServerEvent:Connect(function(player)
    local eggCFrame = getEggCFrame()
    if not eggCFrame then return end

    local character = player.Character
    if not character then return end

    -- Pasta onde os pets equipados ficam (ajuste o caminho se necessário)
    local petsFolder = character:FindFirstChild("Pets") or character

    local offset = 0
    for _, child in ipairs(petsFolder:GetChildren()) do
        -- Verifica se o item é um pet (ajuste conforme seu sistema de tags ou nomes)
        if child:IsA("Model") and child.Name ~= character.Name then
            -- Organiza os pets em fileira ao lado do ovo
            local targetCFrame = eggCFrame * CFrame.new(offset, 0, 3)
            child:PivotTo(targetCFrame)
            offset = offset + 2.5
        end
    end
end)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TeleportEvent = ReplicatedStorage:WaitForChild("TeleportPetEvent")

local button = script.Parent

-- Defina o nome exato do pet que este botão representa
local PET_NAME = "Dragao" 

button.MouseButton1Click:Connect(function()
    -- Dispara o pedido ao servidor
    TeleportEvent:FireServer(PET_NAME)
end)
