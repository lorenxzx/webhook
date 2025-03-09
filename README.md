local HttpService = game:GetService("HttpService")
local webhookURL = "COLE_AQUI_SUA_URL_DO_WEBHOOK"

-- Função para enviar dados ao Discord
local function logToDiscord(player)
    local data = {
        content = "O jogador **" .. player.Name .. "** (ID: " .. player.UserId .. ") executou o script!",
        username = "Log do Roblox"
    }
    
    local success, response = pcall(function()
        HttpService:PostAsync(webhookURL, HttpService:JSONEncode(data))
    end)
    
    if not success then
        warn("Erro ao enviar log para o Discord:", response)
    end
end

-- Detectar quando o script é executado (exemplo com um Tool)
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        local tool = character:WaitForChild("Tool") -- Substitua pelo nome da sua ferramenta/script
        tool.Activated:Connect(function()
            logToDiscord(player)
        end)
    end)
end)
