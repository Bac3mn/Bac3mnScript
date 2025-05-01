-- Interface simples
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Toggle = Instance.new("TextButton")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.Name = "ESP_UI"

Frame.Size = UDim2.new(0, 150, 0, 50)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
Frame.Parent = ScreenGui

Toggle.Size = UDim2.new(1, 0, 1, 0)
Toggle.Text = "ESP: OFF"
Toggle.BackgroundColor3 = Color3.new(0.3, 0.3, 0.3)
Toggle.TextColor3 = Color3.new(1, 1, 1)
Toggle.Parent = Frame

-- Variáveis
local espEnabled = false

-- Função para criar ESP
function createESP(player)
    local Billboard = Instance.new("BillboardGui", player.Character:FindFirstChild("Head"))
    Billboard.Name = "ESP"
    Billboard.Size = UDim2.new(0, 100, 0, 40)
    Billboard.AlwaysOnTop = true

    local nameLabel = Instance.new("TextLabel", Billboard)
    nameLabel.Size = UDim2.new(1, 0, 1, 0)
    nameLabel.Text = player.Name
    nameLabel.TextColor3 = Color3.new(1, 0, 0)
    nameLabel.BackgroundTransparency = 1
end

-- Ativador do ESP
function toggleESP()
    espEnabled = not espEnabled
    Toggle.Text = espEnabled and "ESP: ON" or "ESP: OFF"

    for _, player in ipairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local isSameTeam = player.Team == game.Players.LocalPlayer.Team
            local hasESP = player.Character and player.Character:FindFirstChild("Head") and player.Character.Head:FindFirstChild("ESP")

            if espEnabled and not isSameTeam and not hasESP then
                createESP(player)
            elseif not espEnabled and hasESP then
                player.Character.Head:FindFirstChild("ESP"):Destroy()
            end
        end
    end
end

-- Botão
Toggle.MouseButton1Click:Connect(toggleESP)
