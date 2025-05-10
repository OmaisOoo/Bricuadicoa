local Sounds = {
    DeepBoom = "rbxassetid://138186576", -- Batida grave de tensão
    HorrorDrone = "rbxassetid://9121628917", -- Dronagem sombria
    CreepyLaugh = "rbxassetid://9118820214", -- Risada arrepiante
    Whispering = "rbxassetid://540220229", -- Sussurros assustadores
    ScaryAmbience = "rbxassetid://183763463" -- Ambiente de terror
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "FE_TerrorSounds"
gui.ResetOnSpawn = false

local yOffset = 10
for name, soundId in pairs(Sounds) do
    local btn = Instance.new("TextButton", gui)
    btn.Size = UDim2.new(0, 160, 0, 40)
    btn.Position = UDim2.new(0, 10, 0, yOffset)
    btn.Text = "Play: " .. name
    btn.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.TextScaled = true

    btn.MouseButton1Click:Connect(function()
        local sound = Instance.new("Sound")
        sound.SoundId = soundId
        sound.Volume = 5
        sound.Looped = false
        sound.Parent = workspace
        sound:Play()
        game:GetService("Debris"):AddItem(sound, 10)
    end)

    yOffset = yOffset + 50
end
