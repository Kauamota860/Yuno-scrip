--// Yuno Scripts Hub
--// ESP Team + Speed + Noclip
--// UI Inspirada na imagem enviada

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "YunoScripts"
ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

-- MAIN
local Main = Instance.new("Frame")
Main.Parent = ScreenGui
Main.Size = UDim2.new(0, 420, 0, 300)
Main.Position = UDim2.new(0.35, 0, 0.25, 0)
Main.BackgroundColor3 = Color3.fromRGB(10,10,10)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true

local UICorner = Instance.new("UICorner", Main)
UICorner.CornerRadius = UDim.new(0,12)

-- TITLE
local Title = Instance.new("TextLabel")
Title.Parent = Main
Title.Size = UDim2.new(1,0,0,45)
Title.BackgroundTransparency = 1
Title.Text = "Yuno Scripts"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 26

-- MINIMIZE
local Minimize = Instance.new("TextButton")
Minimize.Parent = Main
Minimize.Size = UDim2.new(0,35,0,35)
Minimize.Position = UDim2.new(1,-45,0,5)
Minimize.Text = "-"
Minimize.Font = Enum.Font.GothamBold
Minimize.TextSize = 25
Minimize.TextColor3 = Color3.fromRGB(255,255,255)
Minimize.BackgroundColor3 = Color3.fromRGB(30,30,30)

local MinCorner = Instance.new("UICorner", Minimize)
MinCorner.CornerRadius = UDim.new(1,0)

local Closed = false

Minimize.MouseButton1Click:Connect(function()
	Closed = not Closed
	
	for _,v in pairs(Main:GetChildren()) do
		if v:IsA("Frame") and v.Name ~= "TopBar" then
			v.Visible = not Closed
		end
	end
	
	if Closed then
		Main.Size = UDim2.new(0,420,0,50)
	else
		Main.Size = UDim2.new(0,420,0,300)
	end
end)

-- CONTENT
local Container = Instance.new("Frame")
Container.Parent = Main
Container.BackgroundTransparency = 1
Container.Position = UDim2.new(0,0,0,50)
Container.Size = UDim2.new(1,0,1,-50)

-- ESP BUTTON
local EspButton = Instance.new("TextButton")
EspButton.Parent = Container
EspButton.Size = UDim2.new(0,180,0,45)
EspButton.Position = UDim2.new(0,20,0,20)
EspButton.Text = "ESP PLAYERS"
EspButton.Font = Enum.Font.GothamBold
EspButton.TextSize = 18
EspButton.TextColor3 = Color3.new(1,1,1)
EspButton.BackgroundColor3 = Color3.fromRGB(255,90,90)

local EspCorner = Instance.new("UICorner", EspButton)
EspCorner.CornerRadius = UDim.new(0,8)

-- SPEED BUTTON
local SpeedButton = Instance.new("TextButton")
SpeedButton.Parent = Container
SpeedButton.Size = UDim2.new(0,180,0,45)
SpeedButton.Position = UDim2.new(0,20,0,80)
SpeedButton.Text = "SPEED: OFF"
SpeedButton.Font = Enum.Font.GothamBold
SpeedButton.TextSize = 18
SpeedButton.TextColor3 = Color3.new(1,1,1)
SpeedButton.BackgroundColor3 = Color3.fromRGB(255,90,90)

local SpeedCorner = Instance.new("UICorner", SpeedButton)
SpeedCorner.CornerRadius = UDim.new(0,8)

-- NOCLIP BUTTON
local NoclipButton = Instance.new("TextButton")
NoclipButton.Parent = Container
NoclipButton.Size = UDim2.new(0,180,0,45)
NoclipButton.Position = UDim2.new(0,20,0,140)
NoclipButton.Text = "NOCLIP: OFF"
NoclipButton.Font = Enum.Font.GothamBold
NoclipButton.TextSize = 18
NoclipButton.TextColor3 = Color3.new(1,1,1)
NoclipButton.BackgroundColor3 = Color3.fromRGB(255,90,90)

local NoclipCorner = Instance.new("UICorner", NoclipButton)
NoclipCorner.CornerRadius = UDim.new(0,8)

-- ESP
local ESPEnabled = false
local ESPs = {}

local function CreateESP(player)
	if player == LocalPlayer then return end
	
	local Highlight = Instance.new("Highlight")
	Highlight.Parent = player.Character or player.CharacterAdded:Wait()
	Highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	
	if player.Team == LocalPlayer.Team then
		Highlight.FillColor = Color3.fromRGB(0,170,255)
	else
		Highlight.FillColor = Color3.fromRGB(255,0,0)
	end
	
	Highlight.OutlineColor = Color3.new(1,1,1)
	
	ESPs[player] = Highlight
end

local function RemoveESP()
	for _,v in pairs(ESPs) do
		if v then
			v:Destroy()
		end
	end
	ESPs = {}
end

EspButton.MouseButton1Click:Connect(function()
	ESPEnabled = not ESPEnabled
	
	if ESPEnabled then
		for _,plr in pairs(Players:GetPlayers()) do
			if plr ~= LocalPlayer then
				CreateESP(plr)
			end
		end
		
		EspButton.Text = "ESP PLAYERS: ON"
	else
		RemoveESP()
		EspButton.Text = "ESP PLAYERS: OFF"
	end
end)

Players.PlayerAdded:Connect(function(plr)
	plr.CharacterAdded:Connect(function()
		if ESPEnabled then
			CreateESP(plr)
		end
	end)
end)

-- SPEED
local SpeedEnabled = false

SpeedButton.MouseButton1Click:Connect(function()
	SpeedEnabled = not SpeedEnabled
	
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("Humanoid") then
		if SpeedEnabled then
			char.Humanoid.WalkSpeed = 50
			SpeedButton.Text = "SPEED: ON"
		else
			char.Humanoid.WalkSpeed = 16
			SpeedButton.Text = "SPEED: OFF"
		end
	end
end)

-- NOCLIP
local Noclip = false

NoclipButton.MouseButton1Click:Connect(function()
	Noclip = not Noclip
	
	if Noclip then
		NoclipButton.Text = "NOCLIP: ON"
	else
		NoclipButton.Text = "NOCLIP: OFF"
	end
end)

RunService.Stepped:Connect(function()
	if Noclip and LocalPlayer.Character then
		for _,v in pairs(LocalPlayer.Character:GetDescendants()) do
			if v:IsA("BasePart") then
				v.CanCollide = false
			end
		end
	end
end)
