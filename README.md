local menuAberto = false
local limiteAtivo = false
local velocidadeMaxMS = 80 / 3.6 -- Converte 80 km/h para m/s
local opcaoSelecionada = 1

-- Lista de opções do menu
local opcoes = {
    "Ativar Limite (80 km/h)",
    "Desativar Limite",
    "Fechar Menu"
}

-- Função para desenhar texto na tela (Interface do Menu)
local function DesenharTexto(texto, x, y, escala, r, g, b, a, centralizado)
    SetTextFont(0)
    SetTextProportional(1)
    SetTextScale(escala, escala)
    SetTextColour(r, g, b, a)
    SetTextDropShadow()
    SetTextEdge(1, 0, 0, 0, 255)
    SetTextDropShadow()
    SetTextOutline()
    if centralizado then SetTextCentre(true) end
    BeginTextCommandDisplayText("STRING")
    AddTextComponentSubstringPlayerName(texto)
    EndTextCommandDisplayText(x, y)
end

-- Thread Principal: Controla o Menu e as Teclas
CreateThread(function()
    while true do
        local sleep = 500
        
        -- Abre o menu ao pressionar F9 (Altere a tecla se desejar)
        if IsControlJustPressed(0, 56) then 
            local playerPed = PlayerPedId()
            if IsPedInAnyVehicle(playerPed, false) then
                menuAberto = not menuAberto
            else
                print("Voce precisa estar em um veiculo para abrir o menu!")
            end
        end

        if menuAberto then
            sleep = 0
            -- Desenha o Fundo do Menu
            DrawRect(0.15, 0.4, 0.2, 0.25, 0, 0, 0, 180)
            DesenharTexto("MENU DE VELOCIDADE", 0.06, 0.30, 0.45, 255, 255, 255, 255, false)
            DesenharTexto("----------------------", 0.06, 0.33, 0.40, 255, 255, 255, 255, false)

            -- Desenha os botões/opções
            for i, v in ipairs(opcoes) do
                if i == opcaoSelecionada then
                    DesenharTexto("> " .. v .. " <", 0.15, 0.32 + (i * 0.035), 0.38, 0, 255, 0, 255, true)
                else
                    DesenharTexto(v, 0.15, 0.32 + (i * 0.035), 0.35, 255, 255, 255, 200, true)
                end
            end

            -- Controles de Navegação (Seta para Cima / Seta para Baixo)
            if IsControlJustPressed(0, 172) then -- SETA PARA CIMA
                opcaoSelecionada = opcaoSelecionada - 1
                if opcaoSelecionada < 1 then opcaoSelecionada = #opcoes end
            elseif IsControlJustPressed(0, 173) then -- SETA PARA BAIXO
                opcaoSelecionada = opcaoSelecionada + 1
                if opcaoSelecionada > #opcoes then opcaoSelecionada = 1 end
            end

            -- Botão de Confirmar (Teclado: ENTER / Controle: A)
            if IsControlJustPressed(0, 191) then 
                if opcaoSelecionada == 1 then
                    limiteAtivo = true
                    print("Limitador ativado: 80 km/h")
                elseif opcaoSelecionada == 2 then
                    limiteAtivo = false
                    local vehicle = GetVehiclePedIsIn(PlayerPedId(), false)
                    if vehicle ~= 0 then
                        local maxSpeed = GetVehicleHandlingFloat(vehicle, 'CHandlingData', 'fInitialDriveMaxFlatVel')
                        SetEntityMaxSpeed(vehicle, maxSpeed)
                    end
                    print("Limitador desativado")
                elseif opcaoSelecionada == 3 then
                    menuAberto = false
                end
            end
        end
        Wait(sleep)
    end
end)

-- Thread de Força da Velocidade: Monitora o motor em tempo real
CreateThread(function()
    while true do
        local sleep = 1000
        if limiteAtivo then
            local playerPed = PlayerPedId()
            local vehicle = GetVehiclePedIsIn(playerPed, false)

            if vehicle ~= 0 and GetPedInVehicleSeat(vehicle, -1) == playerPed then
                sleep = 0
                -- Trava a velocidade do carro se tentar ultrapassar o equivalente a 80 km/h
                if GetEntitySpeed(vehicle) > velocidadeMaxMS then
                    SetEntityMaxSpeed(vehicle, velocidadeMaxMS)
                end
            else
                -- Desativa se o jogador for ejetado ou sair do banco do motorista
                limiteAtivo = false
            end
        end
        Wait(sleep)
    end
end)
local menuAberto = false
local limiteAtivo = false
local velocidadeMaxMS = 80 / 3.6 -- Converte 80 km/h para m/s
local opcaoSelecionada = 1

-- Lista de opções do menu
local opcoes = {
    "Ativar Limite (80 km/h)",
    "Desativar Limite",
    "Fechar Menu"
}

-- Função para desenhar texto na tela (Interface do Menu)
local function DesenharTexto(texto, x, y, escala, r, g, b, a, centralizado)
    SetTextFont(0)
    SetTextProportional(1)
    SetTextScale(escala, escala)
    SetTextColour(r, g, b, a)
    SetTextDropShadow()
    SetTextEdge(1, 0, 0, 0, 255)
    SetTextDropShadow()
    SetTextOutline()
    if centralizado then SetTextCentre(true) end
    BeginTextCommandDisplayText("STRING")
    AddTextComponentSubstringPlayerName(texto)
    EndTextCommandDisplayText(x, y)
end

-- Thread Principal: Controla o Menu e as Teclas
CreateThread(function()
    while true do
        local sleep = 500
        
        -- Abre o menu ao pressionar F9 (Altere a tecla se desejar)
        if IsControlJustPressed(0, 56) then 
            local playerPed = PlayerPedId()
            if IsPedInAnyVehicle(playerPed, false) then
                menuAberto = not menuAberto
            else
                print("Voce precisa estar em um veiculo para abrir o menu!")
            end
        end

        if menuAberto then
            sleep = 0
            -- Desenha o Fundo do Menu
            DrawRect(0.15, 0.4, 0.2, 0.25, 0, 0, 0, 180)
            DesenharTexto("MENU DE VELOCIDADE", 0.06, 0.30, 0.45, 255, 255, 255, 255, false)
            DesenharTexto("----------------------", 0.06, 0.33, 0.40, 255, 255, 255, 255, false)

            -- Desenha os botões/opções
            for i, v in ipairs(opcoes) do
                if i == opcaoSelecionada then
                    DesenharTexto("> " .. v .. " <", 0.15, 0.32 + (i * 0.035), 0.38, 0, 255, 0, 255, true)
                else
                    DesenharTexto(v, 0.15, 0.32 + (i * 0.035), 0.35, 255, 255, 255, 200, true)
                end
            end

            -- Controles de Navegação (Seta para Cima / Seta para Baixo)
            if IsControlJustPressed(0, 172) then -- SETA PARA CIMA
                opcaoSelecionada = opcaoSelecionada - 1
                if opcaoSelecionada < 1 then opcaoSelecionada = #opcoes end
            elseif IsControlJustPressed(0, 173) then -- SETA PARA BAIXO
                opcaoSelecionada = opcaoSelecionada + 1
                if opcaoSelecionada > #opcoes then opcaoSelecionada = 1 end
            end

            -- Botão de Confirmar (Teclado: ENTER / Controle: A)
            if IsControlJustPressed(0, 191) then 
                if opcaoSelecionada == 1 then
                    limiteAtivo = true
                    print("Limitador ativado: 80 km/h")
                elseif opcaoSelecionada == 2 then
                    limiteAtivo = false
                    local vehicle = GetVehiclePedIsIn(PlayerPedId(), false)
                    if vehicle ~= 0 then
                        local maxSpeed = GetVehicleHandlingFloat(vehicle, 'CHandlingData', 'fInitialDriveMaxFlatVel')
                        SetEntityMaxSpeed(vehicle, maxSpeed)
                    end
                    print("Limitador desativado")
                elseif opcaoSelecionada == 3 then
                    menuAberto = false
                end
            end
        end
        Wait(sleep)
    end
end)

-- Thread de Força da Velocidade: Monitora o motor em tempo real
CreateThread(function()
    while true do
        local sleep = 1000
        if limiteAtivo then
            local playerPed = PlayerPedId()
            local vehicle = GetVehiclePedIsIn(playerPed, false)

            if vehicle ~= 0 and GetPedInVehicleSeat(vehicle, -1) == playerPed then
                sleep = 0
                -- Trava a velocidade do carro se tentar ultrapassar o equivalente a 80 km/h
                if GetEntitySpeed(vehicle) > velocidadeMaxMS then
                    SetEntityMaxSpeed(vehicle, velocidadeMaxMS)
                end
            else
                -- Desativa se o jogador for ejetado ou sair do banco do motorista
                limiteAtivo = false
            end
        end
        Wait(sleep)
    end
end)
# Bleninlocal menuAberto = false
local limiteAtivo = false
local velocidadeMaxMS = 80 / 3.6 -- Converte 80 km/h para m/s
local opcaoSelecionada = 1

-- Lista de opções do menu
local opcoes = {
    "Ativar Limite (80 km/h)",
    "Desativar Limite",
    "Fechar Menu"
}

-- Função para desenhar texto na tela (Interface do Menu)
local function DesenharTexto(texto, x, y, escala, r, g, b, a, centralizado)
    SetTextFont(0)
    SetTextProportional(1)
    SetTextScale(escala, escala)
    SetTextColour(r, g, b, a)
    SetTextDropShadow()
    SetTextEdge(1, 0, 0, 0, 255)
    SetTextDropShadow()
    SetTextOutline()
    if centralizado then SetTextCentre(true) end
    BeginTextCommandDisplayText("STRING")
    AddTextComponentSubstringPlayerName(texto)
    EndTextCommandDisplayText(x, y)
end

-- Thread Principal: Controla o Menu e as Teclas
CreateThread(function()
    while true do
        local sleep = 500
        
        -- Abre o menu ao pressionar F9 (Altere a tecla se desejar)
        if IsControlJustPressed(0, 56) then 
            local playerPed = PlayerPedId()
            if IsPedInAnyVehicle(playerPed, false) then
                menuAberto = not menuAberto
            else
                print("Voce precisa estar em um veiculo para abrir o menu!")
            end
        end

        if menuAberto then
            sleep = 0
            -- Desenha o Fundo do Menu
            DrawRect(0.15, 0.4, 0.2, 0.25, 0, 0, 0, 180)
            DesenharTexto("MENU DE VELOCIDADE", 0.06, 0.30, 0.45, 255, 255, 255, 255, false)
            DesenharTexto("----------------------", 0.06, 0.33, 0.40, 255, 255, 255, 255, false)

            -- Desenha os botões/opções
            for i, v in ipairs(opcoes) do
                if i == opcaoSelecionada then
                    DesenharTexto("> " .. v .. " <", 0.15, 0.32 + (i * 0.035), 0.38, 0, 255, 0, 255, true)
                else
                    DesenharTexto(v, 0.15, 0.32 + (i * 0.035), 0.35, 255, 255, 255, 200, true)
                end
            end

            -- Controles de Navegação (Seta para Cima / Seta para Baixo)
            if IsControlJustPressed(0, 172) then -- SETA PARA CIMA
                opcaoSelecionada = opcaoSelecionada - 1
                if opcaoSelecionada < 1 then opcaoSelecionada = #opcoes end
            elseif IsControlJustPressed(0, 173) then -- SETA PARA BAIXO
                opcaoSelecionada = opcaoSelecionada + 1
                if opcaoSelecionada > #opcoes then opcaoSelecionada = 1 end
            end

            -- Botão de Confirmar (Teclado: ENTER / Controle: A)
            if IsControlJustPressed(0, 191) then 
                if opcaoSelecionada == 1 then
                    limiteAtivo = true
                    print("Limitador ativado: 80 km/h")
                elseif opcaoSelecionada == 2 then
                    limiteAtivo = false
                    local vehicle = GetVehiclePedIsIn(PlayerPedId(), false)
                    if vehicle ~= 0 then
                        local maxSpeed = GetVehicleHandlingFloat(vehicle, 'CHandlingData', 'fInitialDriveMaxFlatVel')
                        SetEntityMaxSpeed(vehicle, maxSpeed)
                    end
                    print("Limitador desativado")
                elseif opcaoSelecionada == 3 then
                    menuAberto = false
                end
            end
        end
        Wait(sleep)
    end
end)

-- Thread de Força da Velocidade: Monitora o motor em tempo real
CreateThread(function()
    while true do
        local sleep = 1000
        if limiteAtivo then
            local playerPed = PlayerPedId()
            local vehicle = GetVehiclePedIsIn(playerPed, false)

            if vehicle ~= 0 and GetPedInVehicleSeat(vehicle, -1) == playerPed then
                sleep = 0
                -- Trava a velocidade do carro se tentar ultrapassar o equivalente a 80 km/h
                if GetEntitySpeed(vehicle) > velocidadeMaxMS then
                    SetEntityMaxSpeed(vehicle, velocidadeMaxMS)
                end
            else
                -- Desativa se o jogador for ejetado ou sair do banco do motorista
                limiteAtivo = false
            end
        end
        Wait(sleep)
    end
end)
local menuAberto = false
local limiteAtivo = false
local velocidadeMaxMS = 80 / 3.6 -- Converte 80 km/h para m/s
local opcaoSelecionada = 1

-- Lista de opções do menu
local opcoes = {
    "Ativar Limite (80 km/h)",
    "Desativar Limite",
    "Fechar Menu"
}

-- Função para desenhar texto na tela (Interface do Menu)
local function DesenharTexto(texto, x, y, escala, r, g, b, a, centralizado)
    SetTextFont(0)
    SetTextProportional(1)
    SetTextScale(escala, escala)
    SetTextColour(r, g, b, a)
    SetTextDropShadow()
    SetTextEdge(1, 0, 0, 0, 255)
    SetTextDropShadow()
    SetTextOutline()
    if centralizado then SetTextCentre(true) end
    BeginTextCommandDisplayText("STRING")
    AddTextComponentSubstringPlayerName(texto)
    EndTextCommandDisplayText(x, y)
end

-- Thread Principal: Controla o Menu e as Teclas
CreateThread(function()
    while true do
        local sleep = 500
        
        -- Abre o menu ao pressionar F9 (Altere a tecla se desejar)
        if IsControlJustPressed(0, 56) then 
            local playerPed = PlayerPedId()
            if IsPedInAnyVehicle(playerPed, false) then
                menuAberto = not menuAberto
            else
                print("Voce precisa estar em um veiculo para abrir o menu!")
            end
        end

        if menuAberto then
            sleep = 0
            -- Desenha o Fundo do Menu
            DrawRect(0.15, 0.4, 0.2, 0.25, 0, 0, 0, 180)
            DesenharTexto("MENU DE VELOCIDADE", 0.06, 0.30, 0.45, 255, 255, 255, 255, false)
            DesenharTexto("----------------------", 0.06, 0.33, 0.40, 255, 255, 255, 255, false)

            -- Desenha os botões/opções
            for i, v in ipairs(opcoes) do
                if i == opcaoSelecionada then
                    DesenharTexto("> " .. v .. " <", 0.15, 0.32 + (i * 0.035), 0.38, 0, 255, 0, 255, true)
                else
                    DesenharTexto(v, 0.15, 0.32 + (i * 0.035), 0.35, 255, 255, 255, 200, true)
                end
            end

            -- Controles de Navegação (Seta para Cima / Seta para Baixo)
            if IsControlJustPressed(0, 172) then -- SETA PARA CIMA
                opcaoSelecionada = opcaoSelecionada - 1
                if opcaoSelecionada < 1 then opcaoSelecionada = #opcoes end
            elseif IsControlJustPressed(0, 173) then -- SETA PARA BAIXO
                opcaoSelecionada = opcaoSelecionada + 1
                if opcaoSelecionada > #opcoes then opcaoSelecionada = 1 end
            end

            -- Botão de Confirmar (Teclado: ENTER / Controle: A)
            if IsControlJustPressed(0, 191) then 
                if opcaoSelecionada == 1 then
                    limiteAtivo = true
                    print("Limitador ativado: 80 km/h")
                elseif opcaoSelecionada == 2 then
                    limiteAtivo = false
                    local vehicle = GetVehiclePedIsIn(PlayerPedId(), false)
                    if vehicle ~= 0 then
                        local maxSpeed = GetVehicleHandlingFloat(vehicle, 'CHandlingData', 'fInitialDriveMaxFlatVel')
                        SetEntityMaxSpeed(vehicle, maxSpeed)
                    end
                    print("Limitador desativado")
                elseif opcaoSelecionada == 3 then
                    menuAberto = false
                end
            end
        end
        Wait(sleep)
    end
end)

-- Thread de Força da Velocidade: Monitora o motor em tempo real
CreateThread(function()
    while true do
        local sleep = 1000
        if limiteAtivo then
            local playerPed = PlayerPedId()
            local vehicle = GetVehiclePedIsIn(playerPed, false)

            if vehicle ~= 0 and GetPedInVehicleSeat(vehicle, -1) == playerPed then
                sleep = 0
                -- Trava a velocidade do carro se tentar ultrapassar o equivalente a 80 km/h
                if GetEntitySpeed(vehicle) > velocidadeMaxMS then
                    SetEntityMaxSpeed(vehicle, velocidadeMaxMS)
                end
            else
                -- Desativa se o jogador for ejetado ou sair do banco do motorista
                limiteAtivo = false
            end
        end
        Wait(sleep)
    end
end)
# Blenin
