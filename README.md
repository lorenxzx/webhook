local HttpService = game:GetService("HttpService")
local webhookURL = "https://discord.com/api/webhooks/1327991479125151875/zWq-0Hrti7OfhXmtliZz-biL8Vzkd2qD0TG8rGtB_-Kr198gpZFz88EvN3Og4wbeUIKe"

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
