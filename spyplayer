local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Espião de Jogadores " .. Fluent.Version,
    SubTitle = "por juaozinfofo",
    TabWidth = 160,
    Size = UDim2.fromOffset(600, 480),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local MainTab = Window:AddTab({Title = "Espionagem", Icon = "user"})

local SpySection = MainTab:AddSection("Controle de Espionagem")

local playerDropdown = SpySection:AddDropdown("PlayerList", {
    Title = "Selecionar Jogador",
    Description = "Escolha um jogador para espionar",
    Values = {},
    Multi = false,
    Default = nil,
    AllowNull = true,
    Callback = function(value)
        local infoParagraph = SpySection:GetParagraph("Info")
        if value then
            if infoParagraph then
                infoParagraph:SetText("Jogador selecionado: " .. value)
            else
                SpySection:AddParagraph("Info", "Jogador selecionado: " .. value)
            end
        else
            if infoParagraph then
                infoParagraph:SetText("Nenhum jogador selecionado")
            else
                SpySection:AddParagraph("Info", "Nenhum jogador selecionado")
            end
        end
    end
})

local isSpying = false
local spyToggle = SpySection:AddToggle("SpyToggle", {
    Title = "Espionar Jogador",
    Description = "Ativa/desativa a câmera no jogador selecionado",
    Default = false,
    Callback = function(value)
        isSpying = value
        local camera = workspace.CurrentCamera
        
        if value then
            local selectedPlayer = playerDropdown.Value
            if selectedPlayer then
                local player = game:GetService("Players"):FindFirstChild(selectedPlayer)
                if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    camera.CameraType = Enum.CameraType.Scriptable
                    
                    spawn(function()
                        while isSpying and player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") do
                            camera.CFrame = CFrame.new(
                                player.Character.HumanoidRootPart.Position + Vector3.new(0, 5, 10),
                                player.Character.HumanoidRootPart.Position
                            )
                            game:GetService("RunService").Heartbeat:Wait()
                        end
                        if not isSpying then
                            camera.CameraType = Enum.CameraType.Custom
                            spyToggle:SetValue(false)
                        end
                    end)
                else
                    spyToggle:SetValue(false)
                    Window:Notify({
                        Title = "Erro",
                        Content = "Jogador inválido ou sem personagem!",
                        Duration = 3
                    })
                end
            else
                spyToggle:SetValue(false)
                Window:Notify({
                    Title = "Erro",
                    Content = "Nenhum jogador selecionado!",
                    Duration = 3
                })
            end
        else
            camera.CameraType = Enum.CameraType.Custom
        end
    end
})

local function updatePlayerList()
    local players = game:GetService("Players"):GetPlayers()
    local playerNames = {}
    
    for _, player in ipairs(players) do
        if player ~= game:GetService("Players").LocalPlayer then
            table.insert(playerNames, player.Name)
        end
    end
    
    playerDropdown:SetValues(playerNames)
    
    local currentValue = playerDropdown.Value
    local infoParagraph = SpySection:GetParagraph("Info")
    
    if #playerNames == 0 then
        playerDropdown:SetValue(nil)
        if infoParagraph then
            infoParagraph:SetText("Nenhum jogador disponível")
        else
            SpySection:AddParagraph("Info", "Nenhum jogador disponível")
        end
    elseif currentValue and not table.find(playerNames, currentValue) then
        playerDropdown:SetValue(nil)
        if infoParagraph then
            infoParagraph:SetText("Nenhum jogador selecionado")
        else
            SpySection:AddParagraph("Info", "Nenhum jogador selecionado")
        end
    end
end

game:GetService("Players").PlayerAdded:Connect(updatePlayerList)
game:GetService("Players").PlayerRemoving:Connect(updatePlayerList)

updatePlayerList()

Window:Notify({
    Title = "Bem-vindo!",
    Content = "Espião de Jogadores carregado com sucesso!",
    Duration = 5
})

Window:SelectTab(1)
