-- Voo-hub
local hub = Instance.new("ScreenGui")
hub.Name = "Hub"
hub.Parent = game.Players.LocalPlayer.PlayerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 250) -- Aumenta o tamanho do frame
frame.Position = UDim2.new(0.5, -100, 0.5, -125)
frame.BackgroundColor3 = Color3.new(0.8, 0.8, 0.8)
frame.Parent = hub

local titulo = Instance.new("TextLabel")
titulo.Size = UDim2.new(1, 0, 0.2, 0)
titulo.Text = "Hub de Comandos"
titulo.BackgroundTransparency = 1
titulo.Parent = frame

local botaoTeleporte = Instance.new("TextButton")
botaoTeleporte.Size = UDim2.new(0.8, 0, 0.2, 0)
botaoTeleporte.Position = UDim2.new(0.1, 0, 0.3, 0)
botaoTeleporte.Text = "Teletransporte"
botaoTeleporte.Parent = frame

local botaoVoar = Instance.new("TextButton")
botaoVoar.Size = UDim2.new(0.8, 0, 0.2, 0)
botaoVoar.Position = UDim2.new(0.1, 0, 0.5, 0)
botaoVoar.Text = "Voar"
botaoVoar.Parent = frame

local botaoFechar = Instance.new("TextButton")
botaoFechar.Size = UDim2.new(0.2, 0, 0.2, 0)
botaoFechar.Position = UDim2.new(0.8, 0, 0, 0)
botaoFechar.Text = "Fechar"
botaoFechar.Parent = frame

-- Função para fechar o hub
botaoFechar.MouseButton1Click:Connect(function()
    hub.Enabled = false
end)

-- Desativa o hub no início
hub.Enabled = false

-- Função para exibir o hub
local function exibirHub(mensagem, jogador)
    if mensagem == "/hub" then
        hub.Enabled = true
    end
end

-- Conecta a função ao evento de chat
game.Players.LocalPlayer.Chatted:Connect(exibirHub)

-- Função para dar o item de teletransporte
local function darTeleporte(jogador)
    local itemTeleporte = Instance.new("Tool")
    itemTeleporte.Name = "Teleportador"
    itemTeleporte.Parent = jogador.Backpack

    local scriptTeleporte = Instance.new("Script")
    scriptTeleporte.Parent = itemTeleporte
    scriptTeleporte.Source = [[
        local ferramenta = script.Parent
        local destino = workspace.DestinoDoTeleporte -- Substitua pelo nome da parte de destino

        ferramenta.Activated:Connect(function()
            local jogador = ferramenta.Parent.Parent
            if jogador:IsA("Player") then
                jogador.Character.HumanoidRootPart.CFrame = destino.CFrame
            end
        end)
    ]]
end

-- Conecta a função ao botão de teletransporte
botaoTeleporte.MouseButton1Click:Connect(function()
    darTeleporte(game.Players.LocalPlayer)
end)

-- Função para ativar/desativar o modo de voo
local voando = false

local function voar(jogador)
    voando = not voando
    local humanoide = jogador.Character:FindFirstChild("Humanoid")
    if humanoide then
        if voando then
            humanoide.WalkSpeed = 0
            local bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.Parent = jogador.Character.HumanoidRootPart
            bodyVelocity.MaxForce = Vector3.new(0, 1000, 0)
            bodyVelocity.Velocity = Vector3.new(0, 20, 0)
            jogador.Character.Humanoid.PlatformStand = true
        else
            humanoide.WalkSpeed = 16
            local bodyVelocity = jogador.Character.HumanoidRootPart:FindFirstChild("BodyVelocity")
            if bodyVelocity then
                bodyVelocity:Destroy()
            end
            jogador.Character.Humanoid.PlatformStand = false
        end
    end
end

-- Conecta a função ao botão de voar
botaoVoar.MouseButton1Click:Connect(function()
    voar(game.Players.LocalPlayer)
end)
