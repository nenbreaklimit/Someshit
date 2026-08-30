local player = game:GetService("Players").LocalPlayer

-- 1. TẠO UI (GIAO DIỆN) TRÊN MÀN HÌNH
local ScreenGui = Instance.new("ScreenGui")
local TextButton = Instance.new("TextButton")

-- Tương thích với các Executor bảo vệ UI (tránh bị phát hiện)
if gethui then
	ScreenGui.Parent = gethui()
elseif syn and syn.protect_gui then
	syn.protect_gui(ScreenGui)
	ScreenGui.Parent = game:GetService("CoreGui")
else
	ScreenGui.Parent = game:GetService("CoreGui")
end

-- 2. THIẾT KẾ NÚT BẤM (CÓ THỂ KÉO THẢ ĐƯỢC)
TextButton.Parent = ScreenGui
TextButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50) -- Màu đỏ (Đang tắt)
TextButton.Position = UDim2.new(0.5, -50, 0.2, 0) -- Nằm ở giữa phía trên màn hình
TextButton.Size = UDim2.new(0, 120, 0, 40)
TextButton.Font = Enum.Font.GothamBold
TextButton.Text = "CARRY: OFF"
TextButton.TextColor3 = Color3.fromRGB(255, 255, 255)
TextButton.TextSize = 14
TextButton.Active = true
TextButton.Draggable = true -- Cho phép kéo thả nút đi chỗ khác

local isToggled = false

-- 3. LOGIC KHI BẤM NÚT
TextButton.MouseButton1Click:Connect(function()
	isToggled = not isToggled
	
	if isToggled then
		-- TRẠNG THÁI: BẬT
		TextButton.Text = "CARRY: ON"
		TextButton.BackgroundColor3 = Color3.fromRGB(50, 255, 50) -- Đổi màu xanh lá
		
		-- Chạy code Carry Bật
		local args = {
			{
				["\194\162"] = {
					player.Character, -- Đã đổi tự động lấy nhân vật của bạn thay vì "HollyPADERd"
					true
				},
				["\194\145"] = {
					game:GetService("ReplicatedStorage"):WaitForChild("Assets"):WaitForChild("Sounds"):WaitForChild("Combat"):WaitForChild("Swing"):WaitForChild("Light"):WaitForChild("0013"),
					{
						ParentFolder = game:GetService("ReplicatedStorage"):WaitForChild("Assets"):WaitForChild("Sounds"):WaitForChild("Combat"):WaitForChild("Swing"):WaitForChild("Light"),
						RollOffMode = Enum.RollOffMode.Inverse,
						Looped = false,
						MinDistance = 5000,
						MaxDistance = 5000,
						Name = "",
						Volume = 0.5,
						RandomizePlaybackSpeed = true,
						PlayOnRemove = true
					}
				}
			},
			{}
		}
		game:GetService("ReplicatedStorage"):WaitForChild("RedEvent"):FireServer(unpack(args))
		
	else
		-- TRẠNG THÁI: TẮT
		TextButton.Text = "CARRY: OFF"
		TextButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50) -- Đổi lại màu đỏ
		
		-- Chạy code Carry Tắt (Drop)
		local args = {
			{
				["\194\162"] = {
					player.Character, -- Đã đổi tự động lấy nhân vật của bạn
					false
				},
				["\194\145"] = {
					game:GetService("ReplicatedStorage"):WaitForChild("Assets"):WaitForChild("Sounds"):WaitForChild("Combat"):WaitForChild("Swing"):WaitForChild("Light"):WaitForChild("0013"),
					{
						ParentFolder = game:GetService("ReplicatedStorage"):WaitForChild("Assets"):WaitForChild("Sounds"):WaitForChild("Combat"):WaitForChild("Swing"):WaitForChild("Light"),
						RollOffMode = Enum.RollOffMode.Inverse,
						Looped = false,
						MinDistance = 5000,
						MaxDistance = 5000,
						Name = "",
						Volume = 0.5,
						RandomizePlaybackSpeed = true,
						PlayOnRemove = true
					}
				}
			},
			{}
		}
		game:GetService("ReplicatedStorage"):WaitForChild("RedEvent"):FireServer(unpack(args))
	end
end)
