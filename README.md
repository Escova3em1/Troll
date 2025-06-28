		local uis = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local player = game.Players.LocalPlayer

-- Main GUI
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
ScreenGui.Name = "SimpleGuiV1"
ScreenGui.ResetOnSpawn = false

-- Intro Text
local intro = Instance.new("TextLabel", ScreenGui)
intro.Size = UDim2.new(0.5, 0, 0.2, 0)
intro.Position = UDim2.new(0.25, 0, 0.4, 0)
intro.BackgroundTransparency = 1
intro.Text = "Thank You For Using Our Script"
intro.Font = Enum.Font.GothamBold
intro.TextColor3 = Color3.fromRGB(255, 255, 255)
intro.TextScaled = true
intro.ZIndex = 10

TweenService:Create(intro, TweenInfo.new(2, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
	TextTransparency = 0
}):Play()

task.wait(2.5)
intro:Destroy()

-- Main Frame
local frame = Instance.new("Frame", ScreenGui)
frame.Size = UDim2.new(0, 550, 0, 400)
frame.Position = UDim2.new(0.5, -275, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(255, 165, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Visible = true

-- Title
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
title.Text = "Simple GUI V1"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

-- Tabs
local tabNames = {"Basics", "Trolling", "Hubs"}
local pages = {}

local tabBar = Instance.new("Frame", frame)
tabBar.Size = UDim2.new(1, 0, 0, 40)
tabBar.Position = UDim2.new(0, 0, 0, 40)
tabBar.BackgroundColor3 = Color3.fromRGB(255, 200, 100)

for i, tabName in ipairs(tabNames) do
	local tab = Instance.new("TextButton", tabBar)
	tab.Size = UDim2.new(1/#tabNames, 0, 1, 0)
	tab.Position = UDim2.new((i-1)/#tabNames, 0, 0, 0)
	tab.Text = tabName
	tab.Font = Enum.Font.GothamSemibold
	tab.TextColor3 = Color3.new(1, 1, 1)
	tab.BackgroundColor3 = Color3.fromRGB(255, 170, 80)
	tab.TextScaled = true

	local page = Instance.new("Frame", frame)
	page.Size = UDim2.new(1, 0, 1, -80)
	page.Position = UDim2.new(0, 0, 0, 80)
	page.BackgroundColor3 = Color3.fromRGB(245, 245, 245)
	page.Visible = (i == 1)
	pages[tabName] = page

	tab.MouseEnter:Connect(function()
		TweenService:Create(tab, TweenInfo.new(0.2), {Position = UDim2.new(tab.Position.X.Scale, 0, -0.05, 0)}):Play()
	end)
	tab.MouseLeave:Connect(function()
		TweenService:Create(tab, TweenInfo.new(0.2), {Position = UDim2.new(tab.Position.X.Scale, 0, 0, 0)}):Play()
	end)
	tab.MouseButton1Click:Connect(function()
		for _, p in pairs(pages) do p.Visible = false end
		page.Visible = true
	end)
end

-- Function to make buttons
local function makeButton(parent, name, scriptCode, isExternal, hasInput)
	local btn = Instance.new("TextButton", parent)
	btn.Size = UDim2.new(0.9, 0, 0, 40)
	btn.Position = UDim2.new(0.05, 0, 0, (#parent:GetChildren()-1)*50)
	btn.Text = name
	btn.Font = Enum.Font.Gotham
	btn.TextColor3 = Color3.new(0, 0, 0)
	btn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	btn.TextScaled = true

	local hoverAnim = TweenService:Create(btn, TweenInfo.new(0.1), {Position = btn.Position + UDim2.new(0, 5, 0, 0)})

	btn.MouseEnter:Connect(function() hoverAnim:Play() end)
	btn.MouseLeave:Connect(function()
		TweenService:Create(btn, TweenInfo.new(0.1), {Position = btn.Position - UDim2.new(0, 5, 0, 0)}):Play()
	end)

	if hasInput then
		local input = Instance.new("TextBox", parent)
		input.PlaceholderText = "Enter value"
		input.Size = UDim2.new(0.9, 0, 0, 30)
		input.Position = UDim2.new(0.05, 0, 0, (#parent:GetChildren()-1)*50)
		input.Text = ""
		input.BackgroundColor3 = Color3.fromRGB(240, 240, 240)
		input.TextColor3 = Color3.new(0, 0, 0)
		input.Font = Enum.Font.Gotham
		input.TextScaled = true

		btn.MouseButton1Click:Connect(function()
			local val = tonumber(input.Text)
			if val then
				loadstring(scriptCode:gsub("{{val}}", val))()
			end
		end)
	else
		btn.MouseButton1Click:Connect(function()
			if isExternal then
				loadstring(game:HttpGet(scriptCode))()
			else
				loadstring(scriptCode)()
			end
		end)
	end
end

-- Tab 1: Basics
makeButton(pages["Basics"], "Fly GUI V3", "https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt", true)
makeButton(pages["Basics"], "Speed", [[game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = {{val}}]], false, true)
makeButton(pages["Basics"], "Jump Power", [[local char = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")
hum.UseJumpPower = true
hum:SetStateEnabled(Enum.HumanoidStateType.Jumping, true)
hum.Jump = true
hum.JumpPower = {{val}}]], false, true)

-- Tab 2: Trolling
makeButton(pages["Trolling"], "Punch Fling", "https://scriptblox.com/script/Universal-Script-punch-fling-actually-works-unlike-others-25569", true)
makeButton(pages["Trolling"], "Jerk Off Tool", "https://pastefy.app/YZoglOyJ/raw", true)
makeButton(pages["Trolling"], "Neko", "https://raw.githubusercontent.com/Gazer-Ha/Neko-v1/main/Extremely%20Broken", true)

-- Tab 3: Hubs
makeButton(pages["Hubs"], "TROLLING GUI V5", "https://pastefy.app/rmdi1m55/raw", true)
makeButton(pages["Hubs"], "Unknown Hub", "https://pastebin.com/raw/tvZ99LCQ", true)
makeButton(pages["Hubs"], "Lazy Hub", "https://raw.githubusercontent.com/LioK251/RbScripts/main/loader.lua", true)
makeButton(pages["Hubs"], "Ban Hammer Inside Ghost Hub", "https://rawscripts.net/raw/Universal-Script-GhostHub-16534", true)

-- RightShift Toggle
local guiVisible = true
uis.InputBegan:Connect(function(key)
	if key.KeyCode == Enum.KeyCode.RightShift then
		guiVisible = not guiVisible
		frame.Visible = guiVisible
	end
end)
