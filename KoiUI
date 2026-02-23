-- ============================================
-- KoiUI v1 Edition
-- Clean Module for GitHub Loader
-- Mobile + PC | Draggable | Minimize to title bar
-- ============================================

local KoiUI = {}
KoiUI.__index = KoiUI

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")

function KoiUI.new(title)
	local self = setmetatable({}, KoiUI)
	
	local pg = Players.LocalPlayer:WaitForChild("PlayerGui")
	
	self.ScreenGui = Instance.new("ScreenGui")
	self.ScreenGui.Name = "KoiUI_WonderChase"
	self.ScreenGui.ResetOnSpawn = false
	self.ScreenGui.Parent = pg
	
	-- Main Frame (darker & more like screenshot)
	self.MainFrame = Instance.new("Frame")
	self.MainFrame.Size = UDim2.new(0, 430, 0, 495)
	self.MainFrame.Position = UDim2.new(0.5, -215, 0.5, -247.5)
	self.MainFrame.BackgroundColor3 = Color3.fromRGB(24, 12, 44)
	self.MainFrame.Parent = self.ScreenGui
	
	Instance.new("UICorner", self.MainFrame).CornerRadius = UDim.new(0, 18)
	
	local stroke = Instance.new("UIStroke")
	stroke.Color = Color3.fromRGB(145, 75, 255)
	stroke.Thickness = 2.8
	stroke.Parent = self.MainFrame
	
	local grad = Instance.new("UIGradient")
	grad.Color = ColorSequence.new{
		ColorSequenceKeypoint.new(0, Color3.fromRGB(38, 19, 62)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(19, 10, 35))
	}
	grad.Rotation = 35
	grad.Parent = self.MainFrame
	
	-- Title Bar
	self.TitleBar = Instance.new("Frame")
	self.TitleBar.Size = UDim2.new(1, 0, 0, 58)
	self.TitleBar.BackgroundColor3 = Color3.fromRGB(42, 21, 72)
	self.TitleBar.Parent = self.MainFrame
	
	Instance.new("UICorner", self.TitleBar).CornerRadius = UDim.new(0, 18)
	
	local titleGrad = Instance.new("UIGradient")
	titleGrad.Color = ColorSequence.new{
		ColorSequenceKeypoint.new(0, Color3.fromRGB(75, 40, 130)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(42, 21, 72))
	}
	titleGrad.Parent = self.TitleBar
	
	-- Skull Icon (matches screenshot style)
	local icon = Instance.new("TextLabel")
	icon.Size = UDim2.new(0, 36, 0, 36)
	icon.Position = UDim2.new(0, 16, 0.5, -18)
	icon.BackgroundTransparency = 1
	icon.Text = "☠"
	icon.TextColor3 = Color3.fromRGB(255, 110, 220)
	icon.TextSize = 30
	icon.Font = Enum.Font.GothamBold
	icon.Parent = self.TitleBar
	
	-- Editable Title
	self.TitleBox = Instance.new("TextBox")
	self.TitleBox.Size = UDim2.new(1, -190, 1, 0)
	self.TitleBox.Position = UDim2.new(0, 62, 0, 0)
	self.TitleBox.BackgroundTransparency = 1
	self.TitleBox.Text = title or "Wonder Chase V3"
	self.TitleBox.TextColor3 = Color3.new(1,1,1)
	self.TitleBox.Font = Enum.Font.GothamBold
	self.TitleBox.TextSize = 23
	self.TitleBox.TextXAlignment = Enum.TextXAlignment.Left
	self.TitleBox.ClearTextOnFocus = false
	self.TitleBox.Parent = self.TitleBar
	
	-- Minimize Button
	self.MinimizeBtn = Instance.new("TextButton")
	self.MinimizeBtn.Size = UDim2.new(0, 42, 0, 42)
	self.MinimizeBtn.Position = UDim2.new(1, -96, 0.5, -21)
	self.MinimizeBtn.BackgroundColor3 = Color3.fromRGB(65, 35, 105)
	self.MinimizeBtn.Text = "−"
	self.MinimizeBtn.TextColor3 = Color3.new(1,1,1)
	self.MinimizeBtn.TextSize = 30
	self.MinimizeBtn.Font = Enum.Font.GothamBold
	self.MinimizeBtn.Parent = self.TitleBar
	Instance.new("UICorner", self.MinimizeBtn).CornerRadius = UDim.new(0, 11)
	
	-- Close Button
	self.CloseBtn = Instance.new("TextButton")
	self.CloseBtn.Size = UDim2.new(0, 42, 0, 42)
	self.CloseBtn.Position = UDim2.new(1, -48, 0.5, -21)
	self.CloseBtn.BackgroundColor3 = Color3.fromRGB(205, 55, 55)
	self.CloseBtn.Text = "✕"
	self.CloseBtn.TextColor3 = Color3.new(1,1,1)
	self.CloseBtn.TextSize = 26
	self.CloseBtn.Font = Enum.Font.GothamBold
	self.CloseBtn.Parent = self.TitleBar
	Instance.new("UICorner", self.CloseBtn).CornerRadius = UDim.new(0, 11)
	
	-- Tab Bar
	self.TabBar = Instance.new("Frame")
	self.TabBar.Size = UDim2.new(1, -24, 0, 48)
	self.TabBar.Position = UDim2.new(0, 12, 0, 66)
	self.TabBar.BackgroundColor3 = Color3.fromRGB(33, 16, 55)
	self.TabBar.Parent = self.MainFrame
	Instance.new("UICorner", self.TabBar).CornerRadius = UDim.new(0, 12)
	
	local tabList = Instance.new("UIListLayout")
	tabList.FillDirection = Enum.FillDirection.Horizontal
	tabList.Padding = UDim.new(0, 8)
	tabList.SortOrder = Enum.SortOrder.LayoutOrder
	tabList.Parent = self.TabBar
	
	-- Content
	self.Content = Instance.new("Frame")
	self.Content.Size = UDim2.new(1, -24, 1, -126)
	self.Content.Position = UDim2.new(0, 12, 0, 122)
	self.Content.BackgroundTransparency = 1
	self.Content.Parent = self.MainFrame
	
	self.Pages = {}
	
	self:MakeDraggable()
	self:SetupMinimize()
	
	return self
end

function KoiUI:MakeDraggable()
	local dragging, dragStart, startPos
	
	self.TitleBar.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			dragging = true
			dragStart = input.Position
			startPos = self.MainFrame.Position
		end
	end)
	
	self.TitleBar.InputChanged:Connect(function(input)
		if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
			local delta = input.Position - dragStart
			self.MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
		end
	end)
	
	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
			dragging = false
		end
	end)
end

function KoiUI:SetupMinimize()
	self.Minimized = false
	self.OriginalSize = self.MainFrame.Size
	
	self.MinimizeBtn.MouseButton1Click:Connect(function()
		self.Minimized = not self.Minimized
		if self.Minimized then
			TweenService:Create(self.MainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Size = UDim2.new(0, 430, 0, 58)}):Play()
			self.Content.Visible = false
			self.TabBar.Visible = false
			self.MinimizeBtn.Text = "+"
		else
			TweenService:Create(self.MainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quint, Enum.EasingDirection.Out), {Size = self.OriginalSize}):Play()
			self.Content.Visible = true
			self.TabBar.Visible = true
			self.MinimizeBtn.Text = "−"
		end
	end)
	
	self.CloseBtn.MouseButton1Click:Connect(function()
		self.ScreenGui:Destroy()
	end)
end

function KoiUI:CreateTab(tabName)
	local tabBtn = Instance.new("TextButton")
	tabBtn.Size = UDim2.new(0, 115, 1, -8)
	tabBtn.BackgroundColor3 = Color3.fromRGB(55, 28, 85)
	tabBtn.Text = tabName
	tabBtn.TextColor3 = Color3.new(1,1,1)
	tabBtn.Font = Enum.Font.GothamSemibold
	tabBtn.TextSize = 16
	tabBtn.Parent = self.TabBar
	Instance.new("UICorner", tabBtn).CornerRadius = UDim.new(0, 10)
	
	local page = Instance.new("ScrollingFrame")
	page.Size = UDim2.new(1,0,1,0)
	page.BackgroundTransparency = 1
	page.ScrollBarThickness = 5
	page.ScrollBarImageColor3 = Color3.fromRGB(145, 75, 255)
	page.Visible = #self.Pages == 0
	page.Parent = self.Content
	
	local list = Instance.new("UIListLayout")
	list.Padding = UDim.new(0, 12)
	list.SortOrder = Enum.SortOrder.LayoutOrder
	list.Parent = page
	
	Instance.new("UIPadding", page).PaddingLeft = UDim.new(0, 10)
	
	tabBtn.MouseButton1Click:Connect(function()
		for _, t in ipairs(self.Pages) do
			t.Page.Visible = false
			t.Button.BackgroundColor3 = Color3.fromRGB(55, 28, 85)
		end
		page.Visible = true
		tabBtn.BackgroundColor3 = Color3.fromRGB(145, 75, 255)
	end)
	
	table.insert(self.Pages, {Button = tabBtn, Page = page})
	
	local Tab = {}
	Tab.Page = page
	
	function Tab:AddButton(text, callback)
		local btn = Instance.new("TextButton")
		btn.Size = UDim2.new(1, -10, 0, 52)
		btn.BackgroundColor3 = Color3.fromRGB(55, 30, 90)
		btn.Text = text
		btn.TextColor3 = Color3.new(1,1,1)
		btn.Font = Enum.Font.GothamSemibold
		btn.TextSize = 17
		btn.Parent = page
		Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 12)
		btn.MouseButton1Click:Connect(callback or function() end)
	end
	
	function Tab:AddToggle(text, default, callback)
		local frame = Instance.new("Frame")
		frame.Size = UDim2.new(1, -10, 0, 52)
		frame.BackgroundColor3 = Color3.fromRGB(38, 20, 65)
		frame.Parent = page
		Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)
		
		local label = Instance.new("TextLabel")
		label.Size = UDim2.new(0.65, 0, 1, 0)
		label.BackgroundTransparency = 1
		label.Text = text
		label.TextColor3 = Color3.new(1,1,1)
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.Position = UDim2.new(0, 18, 0, 0)
		label.Font = Enum.Font.GothamSemibold
		label.TextSize = 17
		label.Parent = frame
		
		local toggle = Instance.new("Frame")
		toggle.Size = UDim2.new(0, 58, 0, 28)
		toggle.Position = UDim2.new(1, -72, 0.5, -14)
		toggle.BackgroundColor3 = default and Color3.fromRGB(95, 225, 140) or Color3.fromRGB(70, 70, 85)
		toggle.Parent = frame
		Instance.new("UICorner", toggle).CornerRadius = UDim.new(1, 0)
		
		local knob = Instance.new("Frame")
		knob.Size = UDim2.new(0, 24, 0, 24)
		knob.Position = default and UDim2.new(1, -27, 0.5, -12) or UDim2.new(0, 2, 0.5, -12)
		knob.BackgroundColor3 = Color3.new(1,1,1)
		knob.Parent = toggle
		Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
		
		local on = default
		toggle.InputBegan:Connect(function(i)
			if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then
				on = not on
				if on then
					TweenService:Create(toggle, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(95, 225, 140)}):Play()
					TweenService:Create(knob, TweenInfo.new(0.25), {Position = UDim2.new(1, -27, 0.5, -12)}):Play()
				else
					TweenService:Create(toggle, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(70, 70, 85)}):Play()
					TweenService:Create(knob, TweenInfo.new(0.25), {Position = UDim2.new(0, 2, 0.5, -12)}):Play()
				end
				if callback then callback(on) end
			end
		end)
	end
	
	function Tab:AddSlider(text, min, max, default, callback)
		local frame = Instance.new("Frame")
		frame.Size = UDim2.new(1, -10, 0, 72)
		frame.BackgroundColor3 = Color3.fromRGB(38, 20, 65)
		frame.Parent = page
		Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)
		
		local label = Instance.new("TextLabel")
		label.Size = UDim2.new(1, -20, 0, 24)
		label.Position = UDim2.new(0, 12, 0, 8)
		label.BackgroundTransparency = 1
		label.Text = text .. ": " .. default
		label.TextColor3 = Color3.new(1,1,1)
		label.Font = Enum.Font.Gotham
		label.TextSize = 16
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.Parent = frame
		
		local bar = Instance.new("Frame")
		bar.Size = UDim2.new(1, -32, 0, 10)
		bar.Position = UDim2.new(0, 16, 0, 42)
		bar.BackgroundColor3 = Color3.fromRGB(55, 30, 85)
		bar.Parent = frame
		Instance.new("UICorner", bar).CornerRadius = UDim.new(1, 0)
		
		local fill = Instance.new("Frame")
		fill.Size = UDim2.new(0, 0, 1, 0)
		fill.BackgroundColor3 = Color3.fromRGB(145, 75, 255)
		fill.Parent = bar
		Instance.new("UICorner", fill).CornerRadius = UDim.new(1, 0)
		
		local knob = Instance.new("Frame")
		knob.Size = UDim2.new(0, 20, 0, 20)
		knob.BackgroundColor3 = Color3.new(1,1,1)
		knob.Parent = bar
		Instance.new("UICorner", knob).CornerRadius = UDim.new(1, 0)
		
		local val = default
		local function update(x)
			local percent = math.clamp((x - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
			val = math.floor(min + percent * (max - min))
			fill.Size = UDim2.new(percent, 0, 1, 0)
			knob.Position = UDim2.new(percent, -10, 0.5, -10)
			label.Text = text .. ": " .. val
			if callback then callback(val) end
		end
		
		local dragging = false
		knob.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
		UserInputService.InputChanged:Connect(function(i) if dragging and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then update(i.Position.X) end end)
		UserInputService.InputEnded:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
		
		-- Initial
		local init = (default - min) / (max - min)
		fill.Size = UDim2.new(init, 0, 1, 0)
		knob.Position = UDim2.new(init, -10, 0.5, -10)
	end
	
	function Tab:AddTextbox(text, default, placeholder, callback)
		local frame = Instance.new("Frame")
		frame.Size = UDim2.new(1, -10, 0, 54)
		frame.BackgroundColor3 = Color3.fromRGB(38, 20, 65)
		frame.Parent = page
		Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)
		
		local label = Instance.new("TextLabel")
		label.Size = UDim2.new(0.42, 0, 1, 0)
		label.BackgroundTransparency = 1
		label.Text = text
		label.TextColor3 = Color3.new(1,1,1)
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.Position = UDim2.new(0, 18, 0, 0)
		label.Font = Enum.Font.GothamSemibold
		label.TextSize = 16
		label.Parent = frame
		
		local box = Instance.new("TextBox")
		box.Size = UDim2.new(0.55, 0, 0, 36)
		box.Position = UDim2.new(0.43, 0, 0.5, -18)
		box.Text = default or ""
		box.PlaceholderText = placeholder or ""
		box.BackgroundColor3 = Color3.fromRGB(55, 30, 90)
		box.TextColor3 = Color3.new(1,1,1)
		box.Font = Enum.Font.Gotham
		box.TextSize = 16
		box.Parent = frame
		Instance.new("UICorner", box).CornerRadius = UDim.new(0, 10)
		
		box.FocusLost:Connect(function() if callback then callback(box.Text) end end)
	end
	
	function Tab:AddDropdown(text, options, default, callback)
		local frame = Instance.new("Frame")
		frame.Size = UDim2.new(1, -10, 0, 54)
		frame.BackgroundColor3 = Color3.fromRGB(38, 20, 65)
		frame.Parent = page
		Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 12)
		
		local label = Instance.new("TextLabel")
		label.Size = UDim2.new(0.38, 0, 1, 0)
		label.BackgroundTransparency = 1
		label.Text = text
		label.TextColor3 = Color3.new(1,1,1)
		label.TextXAlignment = Enum.TextXAlignment.Left
		label.Position = UDim2.new(0, 18, 0, 0)
		label.Font = Enum.Font.GothamSemibold
		label.TextSize = 16
		label.Parent = frame
		
		local btn = Instance.new("TextButton")
		btn.Size = UDim2.new(0.58, 0, 0, 38)
		btn.Position = UDim2.new(0.4, 0, 0.5, -19)
		btn.BackgroundColor3 = Color3.fromRGB(55, 30, 90)
		btn.Text = default or options[1]
		btn.TextColor3 = Color3.new(1,1,1)
		btn.Font = Enum.Font.Gotham
		btn.TextSize = 16
		btn.Parent = frame
		Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)
		
		local list = Instance.new("Frame")
		list.Size = UDim2.new(1, 0, 0, 0)
		list.Position = UDim2.new(0, 0, 1, 6)
		list.BackgroundColor3 = Color3.fromRGB(30, 15, 50)
		list.Visible = false
		list.Parent = btn
		Instance.new("UICorner", list).CornerRadius = UDim.new(0, 10)
		local ll = Instance.new("UIListLayout", list)
		ll.SortOrder = Enum.SortOrder.LayoutOrder
		
		local open = false
		btn.MouseButton1Click:Connect(function()
			open = not open
			list.Visible = open
			list.Size = UDim2.new(1, 0, 0, #options * 42)
		end)
		
		for _, opt in ipairs(options) do
			local optBtn = Instance.new("TextButton")
			optBtn.Size = UDim2.new(1, 0, 0, 40)
			optBtn.BackgroundColor3 = Color3.fromRGB(48, 25, 75)
			optBtn.Text = opt
			optBtn.TextColor3 = Color3.new(1,1,1)
			optBtn.Parent = list
			Instance.new("UICorner", optBtn).CornerRadius = UDim.new(0, 8)
			optBtn.MouseButton1Click:Connect(function()
				btn.Text = opt
				open = false
				list.Visible = false
				if callback then callback(opt) end
			end)
		end
	end
	
	return Tab
end

return KoiUI
