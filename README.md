local NormalWalkSpeed = 16
local SprintSpeed = 25
local CameraEffect = true

local cas = game:GetService("ContextActionService")
local Leftc = Enum.KeyCode.LeftControl
local RightC = Enum.KeyCode.RightControl
local player = game:GetService("Players").LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local Humanoid = char:WaitForChild("Humanoid")

local Camera = game.Workspace.CurrentCamera
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

-- Shift: Sprint PC
UIS.InputBegan:Connect(function(key, gameProcessed)
	if gameProcessed then return end
	if key.KeyCode == Enum.KeyCode.LeftShift then
		if CameraEffect then
			TweenService:Create(Camera, TweenInfo.new(0.5), {FieldOfView = 90}):Play()
		end
		Humanoid.WalkSpeed = SprintSpeed
	end
end)

UIS.InputEnded:Connect(function(key, gameProcessed)
	if gameProcessed then return end
	if key.KeyCode == Enum.KeyCode.LeftShift then
		if CameraEffect then
			TweenService:Create(Camera, TweenInfo.new(0.5), {FieldOfView = 70}):Play()
		end
		Humanoid.WalkSpeed = NormalWalkSpeed
	end
end)

-- Função para botão mobile
local function handleContext(name, state, input)
	if state == Enum.UserInputState.Begin then
		Humanoid.WalkSpeed = SprintSpeed
	else
		player.Character.Humanoid.Health -= 100
	end
end

-- Criar o botão
cas:BindAction("Sprint", handleContext, true, Leftc, RightC)
cas:SetTitle("Sprint", "Reset")
cas:SetPosition("Sprint", UDim2.new(0.2, 0, 0.5, 0))

-- Espera o botão existir antes de aplicar o tamanho (levemente aumentado)
task.spawn(function()
	repeat task.wait() until cas:GetButton("Sprint")
	cas:GetButton("Sprint").Size = UDim2.new(0.15, 0, 0.15, 0)
end)
