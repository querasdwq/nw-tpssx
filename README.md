local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = char:WaitForChild("HumanoidRootPart")

local charging = false
local power = 0
local maxPower = 9999
local chargeSpeed = 80 -- ne kadar hızlı dolsun

UIS.InputBegan:Connect(function(input, gp)
	if gp then return end
	if input.KeyCode == Enum.KeyCode.E then
		charging = true
		power = 0
	end
end)

UIS.InputEnded:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.E then
		charging = false
		
		-- Server'a şut bilgisini gönder
		game.ReplicatedStorage.PowerShot:FireServer(power)
	end
end)

game:GetService("RunService").RenderStepped:Connect(function(dt)
	if charging then
		power += chargeSpeed * dt
		if power > maxPower then
			power = maxPower
		end
	end
end)
