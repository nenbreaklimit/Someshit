local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

getgenv().AutoFarmDelivery = false

local gui = Instance.new("ScreenGui")
gui.Name = "made by orochimaru"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.DisplayOrder = 999999
gui.Parent = PlayerGui

local main = Instance.new("Frame")
main.Size = UDim2.fromOffset(330, 210)
main.Position = UDim2.new(0.5, -165, 0.5, -105)
main.BackgroundColor3 = Color3.fromRGB(24, 24, 30)
main.BorderSizePixel = 0
main.Parent = gui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 14)
mainCorner.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -50, 0, 45)
title.Position = UDim2.fromOffset(10, 0)
title.BackgroundTransparency = 1
title.Text = "RINTAMA HUB"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = main

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.fromOffset(30, 30)
minimizeBtn.Position = UDim2.new(1, -38, 0, 8)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(38, 38, 48)
minimizeBtn.BorderSizePixel = 0
minimizeBtn.Text = "-"
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 22
minimizeBtn.Parent = main

local minimizeCorner = Instance.new("UICorner")
minimizeCorner.CornerRadius = UDim.new(0, 8)
minimizeCorner.Parent = minimizeBtn

-- Nút tròn nhỏ dùng để mở lại Hub khi bị ẩn
local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.fromOffset(45, 45)
openBtn.Position = UDim2.new(0, 15, 0.5, -22)
openBtn.BackgroundColor3 = Color3.fromRGB(24, 24, 30)
openBtn.BorderSizePixel = 0
openBtn.Text = "HUB"
openBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
openBtn.Font = Enum.Font.GothamBold
openBtn.TextSize = 13
openBtn.Visible = false
openBtn.Parent = gui

local openCorner = Instance.new("UICorner")
openCorner.CornerRadius = UDim.new(0, 12)
openCorner.Parent = openBtn

-- Sự kiện ẩn / mở giao diện
minimizeBtn.MouseButton1Click:Connect(function()
	main.Visible = false
	openBtn.Visible = true
end)

openBtn.MouseButton1Click:Connect(function()
	main.Visible = true
	openBtn.Visible = false
end)

local moneyLabel = Instance.new("TextLabel")
moneyLabel.Size = UDim2.new(1, -20, 0, 45)
moneyLabel.Position = UDim2.fromOffset(10, 48)
moneyLabel.BackgroundTransparency = 1
moneyLabel.Text = "Money: ..."
moneyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
moneyLabel.Font = Enum.Font.GothamBold
moneyLabel.TextSize = 25
moneyLabel.Parent = main

local function updateMoney()
	local money = player:FindFirstChild("Money")

	if money then
		moneyLabel.Text = "Money: " .. tostring(money.Value)
	else
		moneyLabel.Text = "Money: ..."
	end
end

task.spawn(function()
	while gui.Parent do
		updateMoney()
		task.wait(0.2)
	end
end)

local toggle = Instance.new("TextButton")
toggle.Size = UDim2.new(1, -40, 0, 55)
toggle.Position = UDim2.fromOffset(20, 105)
toggle.BackgroundColor3 = Color3.fromRGB(170, 55, 55)
toggle.BorderSizePixel = 0
toggle.Text = "AUTO FARM: OFF"
toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
toggle.Font = Enum.Font.GothamBold
toggle.TextSize = 18
toggle.Parent = main

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(0, 10)
toggleCorner.Parent = toggle

-- Hàm tải và thực thi script farm
local function executeFarmScript()
	pcall(function()
		loadstring(game:HttpGet("https://raw.githubusercontent.com/nenbreaklimit/Someshit/refs/heads/main/Rintamahigh"))()
	end)
end

local function setToggle(state)
	getgenv().AutoFarmDelivery = state

	if state then
		toggle.Text = "AUTO FARM: ON"
		toggle.BackgroundColor3 = Color3.fromRGB(55, 170, 85)
		
		-- Chạy script mỗi khi bật nút
		task.spawn(executeFarmScript)
	else
		toggle.Text = "AUTO FARM: OFF"
		toggle.BackgroundColor3 = Color3.fromRGB(170, 55, 55)
	end
end

toggle.MouseButton1Click:Connect(function()
	setToggle(not getgenv().AutoFarmDelivery)
end)

-- Tự động chạy lại script khi nhân vật hồi sinh (nếu Auto Farm đang BẬT)
player.CharacterAdded:Connect(function()
	task.wait(1.5)
	if getgenv().AutoFarmDelivery then
		task.spawn(executeFarmScript)
	end
end)

local dragging = false
local dragStart
local startPosition

title.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPosition = main.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if not dragging then
		return
	end

	if input.UserInputType == Enum.UserInputType.MouseMovement
		or input.UserInputType == Enum.UserInputType.Touch then

		local delta = input.Position - dragStart

		main.Position = UDim2.new(
			startPosition.X.Scale,
			startPosition.X.Offset + delta.X,
			startPosition.Y.Scale,
			startPosition.Y.Offset + delta.Y
		)
	end
end)
