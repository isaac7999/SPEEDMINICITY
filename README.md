-- Script: Aumentar 2 e diminuir 1 de speed pelo chat
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")
local speed = 0 -- Começa normal

-- Atualiza se o personagem morrer
player.CharacterAdded:Connect(function(char)
    character = char
    humanoidRootPart = char:WaitForChild("HumanoidRootPart")
    humanoid = char:WaitForChild("Humanoid")
end)

-- Monitorar o chat
player.Chatted:Connect(function(message)
    message = message:lower()
    if message == "+" then
        speed = speed + 2
        game.StarterGui:SetCore("SendNotification", {
            Title = "Speed",
            Text = "Velocidade aumentada! ("..speed..")",
            Duration = 2
        })
    elseif message == "-" then
        speed = math.max(0, speed - 1) -- Diminui 1, não deixa ficar negativo
        game.StarterGui:SetCore("SendNotification", {
            Title = "Speed",
            Text = "Velocidade diminuída! ("..speed..")",
            Duration = 2
        })
    end
end)

-- Loop para aplicar o speed real
game:GetService("RunService").Heartbeat:Connect(function()
    if humanoidRootPart and humanoidRootPart.Parent and humanoid.MoveDirection.Magnitude > 0 then
        humanoidRootPart.CFrame = humanoidRootPart.CFrame + (humanoid.MoveDirection * speed)
    end
end)
