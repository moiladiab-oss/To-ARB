-- [[ MOILA933 - SKINS CIRCLE HUB ]] --

local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- ============================================================
-- Remotes
-- ============================================================
local HDAdminRemote, DataServiceRemote

pcall(function()
HDAdminRemote = ReplicatedStorage
:WaitForChild("HDAdminHDClient", 5)
:WaitForChild("Signals", 5)
:WaitForChild("RequestCommandModification", 5)
end)
pcall(function()
DataServiceRemote = ReplicatedStorage
:WaitForChild("RemoteEvents", 5)
:WaitForChild("DataService", 5)
end)

local function fireCommand(cmd)
if cmd == "" then return end
task.spawn(function()
pcall(function()
if HDAdminRemote then
if HDAdminRemote:IsA("RemoteFunction") then
HDAdminRemote:InvokeServer(cmd)
else
HDAdminRemote:FireServer(cmd)
end
end
end)
end)
task.spawn(function()
pcall(function()
if DataServiceRemote then
DataServiceRemote:FireServer(cmd)
end
end)
end)
end

-- ============================================================
-- Helpers
-- ============================================================
local WHITE = Color3.fromRGB(255, 255, 255)
local BLUE_A = Color3.fromRGB(30, 100, 255)
local BLUE_B = Color3.fromRGB(10, 40, 160)
local DARK = Color3.fromRGB(3, 5, 22)

local function corner(p, r)
local c = Instance.new("UICorner", p)
c.CornerRadius = UDim.new(0, r or 10)
end

local function stroke(p, col, t, tr)
local s = Instance.new("UIStroke", p)
s.Color = col; s.Thickness = t or 1
s.Transparency = tr or 0
s.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
return s
end

local function gradient(p, c1, c2, rot)
local g = Instance.new("UIGradient", p)
g.Color = ColorSequence.new(c1, c2)
g.Rotation = rot or 90
end

local function makeDraggable(frame, handle)
local dragging, dragInput, dragStart, startPos
handle = handle or frame
handle.InputBegan:Connect(function(inp)
if inp.UserInputType == Enum.UserInputType.MouseButton1
or inp.UserInputType == Enum.UserInputType.Touch then
dragging = true
dragStart = inp.Position
startPos = frame.Position
inp.Changed:Connect(function()
if inp.UserInputState == Enum.UserInputState.End then
dragging = false
end
end)
end
end)
handle.InputChanged:Connect(function(inp)
if inp.UserInputType == Enum.UserInputType.MouseMovement
or inp.UserInputType == Enum.UserInputType.Touch then
dragInput = inp
end
end)
UIS.InputChanged:Connect(function(inp)
if inp == dragInput and dragging then
local d = inp.Position - dragStart
frame.Position = UDim2.new(
startPos.X.Scale, startPos.X.Offset + d.X,
startPos.Y.Scale, startPos.Y.Offset + d.Y
)
end
end)
end

-- ============================================================
-- All skins (14 دائرة)
-- ============================================================
local SKINS = {
{ name = "moila933", cmd = ";char me moila933" },
{ name = "atiti20255", cmd = ";char me atiti20255" },
{ name = "coreddddd_core", cmd = ";char me coreddddd_core" },
{ name = "xx_tr22", cmd = ";char me xx_tr22" },
{ name = "fh_556", cmd = ";char me fh_556" },
{ name = "lr_2e", cmd = ";char me lr_2e" },
{ name = "rrily7", cmd = ";char me rrily7" },
{ name = "Ahmad_00435A", cmd = ";char me Ahmad_00435A" },
{ name = "omare3leloo", cmd = ";char me omare3leloo" },
{ name = "xd_99121", cmd = ";char me xd_99121" },
{ name = "saaaaa9924", cmd = ";char me saaaaa9924" },
{ name = "pa_989", cmd = ";char me pa_989" },
{ name = "lolo_yota1", cmd = ";char me lolo_yota1" },
{ name = "malal_816", cmd = ";char me malal_816" },
}

local GLOW = {
Color3.fromRGB(30, 100, 255), Color3.fromRGB(140, 40, 220),
Color3.fromRGB(20, 180, 100), Color3.fromRGB(220, 160, 20),
Color3.fromRGB(200, 40, 40), Color3.fromRGB(20, 180, 200),
Color3.fromRGB(220, 80, 160), Color3.fromRGB(80, 200, 40),
}

-- ============================================================
-- ScreenGui
-- ============================================================
pcall(function()
LocalPlayer.PlayerGui:FindFirstChild("MOILA_SkinsHub"):Destroy()
end)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MOILA_SkinsHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.DisplayOrder = 9999
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- ============================================================
-- Main Frame 560 × 500
-- ============================================================
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 560, 0, 500)
MainFrame.Position = UDim2.new(0.5, -280, 0.5, -250)
MainFrame.BackgroundColor3 = DARK
MainFrame.BackgroundTransparency = 0.1
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Parent = ScreenGui
corner(MainFrame, 18)
gradient(MainFrame, Color3.fromRGB(5, 10, 38), Color3.fromRGB(2, 4, 18), 135)

local mainStroke = stroke(MainFrame, WHITE, 2, 0.3)
task.spawn(function()
while MainFrame and MainFrame.Parent do
local p = (math.sin(tick() * 2) + 1) / 2
mainStroke.Transparency = 0.08 + p * 0.72
task.wait(0.03)
end
end)

makeDraggable(MainFrame)

-- ---- Title Bar ----
local TBar = Instance.new("Frame")
TBar.Size = UDim2.new(1, 0, 0, 46)
TBar.BackgroundColor3 = Color3.fromRGB(8, 20, 75)
TBar.BackgroundTransparency = 0.2
TBar.BorderSizePixel = 0
TBar.Parent = MainFrame
corner(TBar, 18)
gradient(TBar, Color3.fromRGB(12, 35, 110), Color3.fromRGB(5, 14, 55), 90)

-- fix bottom corners of title bar
local TBarFix = Instance.new("Frame")
TBarFix.Size = UDim2.new(1, 0, 0.5, 0)
TBarFix.Position = UDim2.new(0, 0, 0.5, 0)
TBarFix.BackgroundColor3 = Color3.fromRGB(8, 20, 75)
TBarFix.BackgroundTransparency = 0.2
TBarFix.BorderSizePixel = 0
TBarFix.Parent = TBar

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -60, 1, 0)
TitleLabel.Position = UDim2.new(0, 16, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "☢️ MOILA933 — Skins Hub ☢️"
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 14
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TBar
task.spawn(function()
while TitleLabel and TitleLabel.Parent do
TitleLabel.TextColor3 = Color3.fromHSV((tick() * 0.3) % 1, 0.6, 1)
task.wait(0.03)
end
end)

makeDraggable(MainFrame, TBar)

-- ---- Close Button ----
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -38, 0, 8)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 30, 30)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = WHITE
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 15
CloseBtn.AutoButtonColor = false
CloseBtn.BorderSizePixel = 0
CloseBtn.ZIndex = 10
CloseBtn.Parent = MainFrame
corner(CloseBtn, 8)

CloseBtn.MouseEnter:Connect(function()
TweenService:Create(CloseBtn, TweenInfo.new(0.15),
{BackgroundColor3 = Color3.fromRGB(220, 50, 50)}):Play()
end)
CloseBtn.MouseLeave:Connect(function()
TweenService:Create(CloseBtn, TweenInfo.new(0.15),
{BackgroundColor3 = Color3.fromRGB(180, 30, 30)}):Play()
end)
CloseBtn.MouseButton1Click:Connect(function()
TweenService:Create(MainFrame,
TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
{Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play()
task.wait(0.28)
ScreenGui:Destroy()
end)

-- ---- Hint ----
local HintLabel = Instance.new("TextLabel")
HintLabel.Size = UDim2.new(1, -20, 0, 18)
HintLabel.Position = UDim2.new(0, 10, 0, 48)
HintLabel.BackgroundTransparency = 1
HintLabel.Text = "اضغط على الدائرة لتطبيق السكن 👇"
HintLabel.TextColor3 = Color3.fromRGB(120, 170, 255)
HintLabel.Font = Enum.Font.GothamBold
HintLabel.TextSize = 11
HintLabel.Parent = MainFrame

-- ============================================================
-- ScrollingFrame (يحمل جميع الدوائر)
-- ============================================================
local ScrollArea = Instance.new("ScrollingFrame")
ScrollArea.Size = UDim2.new(1, -16, 1, -80)
ScrollArea.Position = UDim2.new(0, 8, 0, 70)
ScrollArea.BackgroundTransparency = 1
ScrollArea.BorderSizePixel = 0
ScrollArea.ScrollBarThickness = 5
ScrollArea.ScrollBarImageColor3 = BLUE_A
ScrollArea.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollArea.AutomaticCanvasSize = Enum.AutomaticSize.Y
ScrollArea.ScrollingDirection = Enum.ScrollingDirection.Y
ScrollArea.Parent = MainFrame

local GridLayout = Instance.new("UIGridLayout")
GridLayout.CellSize = UDim2.new(0, 110, 0, 145)
GridLayout.CellPadding = UDim2.new(0, 10, 0, 10)
GridLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
GridLayout.VerticalAlignment = Enum.VerticalAlignment.Top
GridLayout.SortOrder = Enum.SortOrder.LayoutOrder
GridLayout.Parent = ScrollArea

local GridPad = Instance.new("UIPadding")
GridPad.PaddingTop = UDim.new(0, 8)
GridPad.PaddingBottom = UDim.new(0, 8)
GridPad.PaddingLeft = UDim.new(0, 8)
GridPad.PaddingRight = UDim.new(0, 8)
GridPad.Parent = ScrollArea

-- ============================================================
-- Build Circles
-- ============================================================
for i, skin in ipairs(SKINS) do
local glowColor = GLOW[((i - 1) % #GLOW) + 1]

-- Card
local Card = Instance.new("TextButton")
Card.Size = UDim2.new(0, 110, 0, 145)
Card.BackgroundColor3 = Color3.fromRGB(5, 10, 38)
Card.BackgroundTransparency = 0.3
Card.BorderSizePixel = 0
Card.Text = ""
Card.AutoButtonColor = false
Card.LayoutOrder = i
Card.Parent = ScrollArea
corner(Card, 14)
gradient(Card, Color3.fromRGB(8, 18, 60), Color3.fromRGB(3, 6, 25), 135)

local cStroke = stroke(Card, glowColor, 1.5, 0.3)
local idx = i
task.spawn(function()
while Card and Card.Parent do
local p = (math.sin(tick() * 2.4 + idx * 0.9) + 1) / 2
cStroke.Transparency = 0.1 + p * 0.75
task.wait(0.04)
end
end)

-- Circle (avatar)
local Circle = Instance.new("ImageButton")
Circle.Size = UDim2.new(0, 88, 0, 88)
Circle.Position = UDim2.new(0.5, -44, 0, 8)
Circle.BackgroundColor3 = Color3.fromRGB(8, 18, 60)
Circle.BorderSizePixel = 0
Circle.Image = ""
Circle.AutoButtonColor = false
Circle.ClipsDescendants = true
Circle.ZIndex = 3
Circle.Parent = Card
corner(Circle, 999)

local circStroke = stroke(Circle, glowColor, 2.5, 0.2)
local cIdx = i
task.spawn(function()
while Circle and Circle.Parent do
local p = (math.sin(tick() * 3 + cIdx * 1.1) + 1) / 2
circStroke.Transparency = p * 0.5
circStroke.Color = Color3.fromHSV((tick() * 0.18 + cIdx * 0.12) % 1, 0.85, 1)
task.wait(0.04)
end
end)

-- Loading placeholder
local Placeholder = Instance.new("TextLabel")
Placeholder.Size = UDim2.new(1, 0, 1, 0)
Placeholder.BackgroundTransparency = 1
Placeholder.Text = "⏳"
Placeholder.TextColor3 = Color3.fromRGB(120, 160, 220)
Placeholder.Font = Enum.Font.GothamBold
Placeholder.TextSize = 28
Placeholder.ZIndex = 4
Placeholder.Parent = Circle

-- Load avatar
task.spawn(function()
local ok, uid = pcall(function()
return Players:GetUserIdFromNameAsync(skin.name)
end)
if ok and uid and Circle and Circle.Parent then
local ok2, thumb = pcall(function()
return Players:GetUserThumbnailAsync(
uid,
Enum.ThumbnailType.AvatarBust,
Enum.ThumbnailSize.Size420x420
)
end)
if ok2 and thumb and Circle and Circle.Parent then
Circle.Image = thumb
if Placeholder and Placeholder.Parent then
Placeholder:Destroy()
end
end
end
end)

-- Name label
local NameLabel = Instance.new("TextLabel")
NameLabel.Size = UDim2.new(1, -6, 0, 28)
NameLabel.Position = UDim2.new(0, 3, 0, 100)
NameLabel.BackgroundTransparency = 1
NameLabel.Text = skin.name
NameLabel.Font = Enum.Font.GothamBold
NameLabel.TextSize = 10
NameLabel.TextWrapped = true
NameLabel.ZIndex = 3
NameLabel.Parent = Card
task.spawn(function()
while NameLabel and NameLabel.Parent do
NameLabel.TextColor3 = Color3.fromHSV(
(tick() * 0.25 + idx * 0.14) % 1, 0.7, 1)
task.wait(0.04)
end
end)

-- Status icon
local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, -6, 0, 16)
StatusLabel.Position = UDim2.new(0, 3, 0, 126)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "✅ char"
StatusLabel.TextColor3 = Color3.fromRGB(80, 200, 80)
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.TextSize = 9
StatusLabel.ZIndex = 3
StatusLabel.Parent = Card

-- Click handler
local function applyEffect()
TweenService:Create(Circle, TweenInfo.new(0.08),
{BackgroundColor3 = WHITE}):Play()
task.delay(0.12, function()
TweenService:Create(Circle, TweenInfo.new(0.25),
{BackgroundColor3 = Color3.fromRGB(8, 18, 60)}):Play()
end)
fireCommand(skin.cmd)
end

Card.MouseButton1Click:Connect(applyEffect)
Circle.MouseButton1Click:Connect(applyEffect)
end

-- ============================================================
-- Bottom branding
-- ============================================================
local BottomNote = Instance.new("TextLabel")
BottomNote.Size = UDim2.new(1, -20, 0, 18)
BottomNote.Position = UDim2.new(0, 10, 1, -22)
BottomNote.BackgroundTransparency = 1
BottomNote.Text = "made by MOILA933 ✨ | Skins Circle Hub"
BottomNote.Font = Enum.Font.GothamBold
BottomNote.TextSize = 10
BottomNote.Parent = MainFrame
task.spawn(function()
while BottomNote and BottomNote.Parent do
BottomNote.TextColor3 = Color3.fromHSV((tick() * 0.22) % 1, 0.5, 0.8)
task.wait(0.04)
end
end)

-- ============================================================
-- Toggle Button (دائرة التشغيل والإلغاء)
-- ============================================================
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Name = "ToggleBtn"
ToggleBtn.Size = UDim2.new(0, 50, 0, 50)
ToggleBtn.Position = UDim2.new(0, 20, 0.5, -25)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(15, 50, 160)
ToggleBtn.Text = "☢️"
ToggleBtn.TextColor3 = WHITE
ToggleBtn.Font = Enum.Font.GothamBold
ToggleBtn.TextSize = 20
ToggleBtn.AutoButtonColor = true
ToggleBtn.Parent = ScreenGui

corner(ToggleBtn, 50)
stroke(ToggleBtn, WHITE, 2, 0)
makeDraggable(ToggleBtn)

ToggleBtn.MouseButton1Click:Connect(function()
MainFrame.Visible = not MainFrame.Visible
end)

