local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- Aguarda o personagem carregar
local function getCharacter()
    return LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
end

-- Ativar noclip
local noclip = false

-- Função para remover restrições/bypasses simples (como reativação de CanCollide)
local function antiBypass(char)
    for _, obj in pairs(char:GetDescendants()) do
        if obj:IsA("BasePart") then
            obj.CanCollide = false
            obj:GetPropertyChangedSignal("CanCollide"):Connect(function()
                if noclip then
                    obj.CanCollide = false
                end
            end)
        end
    end
end

-- Função principal de noclip
RunService.Stepped:Connect(function()
    local char = getCharacter()
    if noclip then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)

-- Interface simples para Delta Executor
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "NoclipApex"
gui.ResetOnSpawn = false

local btn = Instance.new("TextButton", gui)
btn.Size = UDim2.new(0, 160, 0, 40)
btn.Position = UDim2.new(0, 10, 0, 10)
btn.Text = "Noclip: OFF"
btn.BackgroundColor3 = Color3.fromRGB(0, 0, 80)
btn.TextColor3 = Color3.new(1, 1, 1)
btn.TextScaled = true

btn.MouseButton1Click:Connect(function()
    noclip = not noclip
    btn.Text = noclip and "Noclip: ON" or "Noclip: OFF"
    btn.BackgroundColor3 = noclip and Color3.fromRGB(0, 120, 0) or Color3.fromRGB(80, 0, 0)

    if noclip then
        antiBypass(getCharacter())
    end
end)
