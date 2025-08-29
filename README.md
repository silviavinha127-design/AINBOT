# AINBOT
EFDAWDAS
-- // Script Roblox Lua
-- // Autor: ChatGPT
-- // Funções: Prediction, ShowAllNicks, Silent
-- // Toggle na tecla Q

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local BotEnabled = false
local Target = nil

-- // Função de Prediction (simples exemplo)
local function Prediction(target, velocity, time)
    if not target or not target.Character or not target.Character:FindFirstChild("HumanoidRootPart") then
        return nil
    end
    local root = target.Character.HumanoidRootPart
    local predictedPosition = root.Position + (velocity * time)
    return predictedPosition
end

-- // Mostrar todos os nicks
local function ShowAllNicks()
    for _, player in pairs(Players:GetPlayers()) do
        print("[NICK] -> " .. player.Name)
    end
end

-- // Silent (desliga/religa prints)
local silent = false
local oldPrint = print

local function Silent(toggle)
    silent = toggle
    if silent then
        warn("Silent ativado: prints desligados.")
        print = function() end
    else
        warn("Silent desativado: prints funcionando novamente.")
        print = oldPrint
    end
end

-- // Toggle com tecla Q
UIS.InputBegan:Connect(function(input, isProcessed)
    if isProcessed then return end
    if input.KeyCode == Enum.KeyCode.Q then
        BotEnabled = not BotEnabled
        if BotEnabled then
            -- Exemplo: pega o segundo player como alvo (não o LocalPlayer)
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer then
                    Target = player
                    break
                end
            end
            print("BOT ATIVADO em -> " .. (Target and Target.Name or "Nenhum"))
        else
            Target = nil
            print("BOT DESATIVADO")
        end
    end
end)

-- // Loop principal (Prediction rodando quando o bot está ativo)
RunService.RenderStepped:Connect(function()
    if BotEnabled and Target then
        local predicted = Prediction(Target, Vector3.new(5,0,0), 2)
        if predicted then
            -- Exemplo: só printa no output
            print("Prevendo posição de " .. Target.Name .. ": ", predicted)
        end
    end
end)

-- // Exemplos de uso:
-- ShowAllNicks()    -- Lista todos os jogadores
-- Silent(true)      -- Liga o silent
-- Silent(false)     -- Desliga o silent
