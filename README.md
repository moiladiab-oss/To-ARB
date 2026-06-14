-- [[ MOILA933 - FAZAA SCRIPT (فزعة لخويك) ]] --

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
-- Style helpers
-- ============================================================
local WHITE = Color3.fromRGB(255, 255, 255)
local BLUE_A = Color3.fromRGB(30, 100, 255)
local BLUE_B = Color3.fromRGB(10, 40, 160)
local DARK = Color3.fromRGB(3, 5, 22)
local GREEN = Color3.fromRGB(30, 180, 80)
local GREEN2 = Color3.fromRGB(15, 90, 40)

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

local function styleBtn(btn, c1, c2)
btn.BackgroundColor3 = c1
btn.Font = Enum.Font.GothamBold
btn.TextSize = 12
btn.AutoButtonColor = false
btn.BorderSizePixel = 0
btn.TextColor3 = WHITE
btn.TextStrokeColor3 = Color3.new(0, 0, 0)
btn.TextStrokeTransparency = 0.6
corner(btn, 8)
gradient(btn, c1, c2 or c1, 90)
stroke(btn, WHITE, 1, 0.6)

btn.MouseEnter:Connect(function()
TweenService:Create(btn, TweenInfo.new(0.12),
{BackgroundTransparency = 0.25}):Play()
end)
btn.MouseLeave:Connect(function()
TweenService:Create(btn, TweenInfo.new(0.12),
{BackgroundTransparency = 0}):Play()
end)
btn.MouseButton1Down:Connect(function()
TweenService:Create(btn, TweenInfo.new(0.07),
{BackgroundTransparency = 0.5}):Play()
end)
btn.MouseButton1Up:Connect(function()
TweenService:Create(btn, TweenInfo.new(0.12),
{BackgroundTransparency = 0}):Play()
end)
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
-- Rescue commands list
-- ============================================================
local RESCUE_CMDS = {
{ label = "🔓 unjail", cmd = ";unjail %s", tip = "يفتح الجيل" },
{ label = "🧊 unice", cmd = ";unice %s", tip = "يذوّب الثلج" },
{ label = "🔥 unfreeze", cmd = ";unfreeze %s", tip = "يحل الفريز" },
{ label = "💚 res", cmd = ";res %s", tip = "يرجعك للحياة" },
{ label = "🔫 unloopkill", cmd = ";unloopkill %s", tip = "يوقف اللووب كيل" },
{ label = "🌀 unwarp", cmd = ";unwarp %s", tip = "يلغي الوارب" },
{ label = "💨 unsmoke", cmd = ";unsmoke %s", tip = "يزيل السموك" },
{ label = "🩹 fix", cmd = ";fix %s", tip = "يصلح" },
{ label = "🩺 heal", cmd = ";heal %s", tip = "يعيد الHP كامل" },
{ label = "🛡️ god", cmd = ";god %s", tip = "يعطي الغود" },
{ label = "🌙 ungod", cmd = ";ungod %s", tip = "يلغي الغود" },
{ label = "⛓️ free", cmd = ";free %s", tip = "يحرر من كل شيء" },
{ label = "🚀 tp me→him", cmd = ";tp me %s", tip = "ينتقل إليه" },
{ label = "🚀 tp him→me", cmd = ";tp %s me", tip = "يجيبه عندك" },
}

-- ============================================================
-- ScreenGui
-- ============================================================
pcall(function()
LocalPlayer.PlayerGui:FindFirstChild("MOILA_Fazaa"):Destroy()
end)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MOILA_Fazaa"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.DisplayOrder = 9999
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- ============================================================
-- Main Frame 580 × 460
-- ============================================================
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 580, 0, 460)
MainFrame.Position = UDim2.new(0.5, -290, 0.5, -230)
MainFrame.BackgroundColor3 = DARK
MainFrame.BackgroundTransparency = 0.08
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Parent = ScreenGui
corner(MainFrame, 18)
gradient(MainFrame, Color3.fromRGB(4, 10, 36), Color3.fromRGB(2, 4, 16), 135)

local mStroke = stroke(MainFrame, WHITE, 2, 0.35)
task.spawn(function()
while MainFrame and MainFrame.Parent do
local p = (math.sin(tick() * 2) + 1) / 2
mStroke.Transparency = 0.08 + p * 0.72
task.wait(0.03)
end
end)

makeDraggable(MainFrame)

-- ---- Title Bar ----
local TBar = Instance.new("Frame")
TBar.Size = UDim2.new(1, 0, 0, 48)
TBar.BackgroundColor3 = Color3.fromRGB(8, 22, 78)
TBar.BackgroundTransparency = 0.18
TBar.BorderSizePixel = 0
TBar.Parent = MainFrame
corner(TBar, 18)
gradient(TBar, Color3.fromRGB(12, 38, 115), Color3.fromRGB(5, 15, 55), 90)

local TBarFix = Instance.new("Frame")
TBarFix.Size = UDim2.new(1, 0, 0.5, 0)
TBarFix.Position = UDim2.new(0, 0, 0.5, 0)
TBarFix.BackgroundColor3 = Color3.fromRGB(8, 22, 78)
TBarFix.BackgroundTransparency = 0.18
TBarFix.BorderSizePixel = 0
TBarFix.Parent = TBar

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -60, 1, 0)
TitleLabel.Position = UDim2.new(0, 16, 0, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "⚡ MOILA933 — فزعة لخويك ⚡"
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 15
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = TBar
task.spawn(function()
while TitleLabel and TitleLabel.Parent do
TitleLabel.TextColor3 = Color3.fromHSV((tick() * 0.32) % 1, 0.55, 1)
task.wait(0.03)
end
end)

makeDraggable(MainFrame, TBar)

-- ---- Close Button ----
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -38, 0, 9)
CloseBtn.BackgroundColor3 = Color3.fromRGB(180, 30, 30)
CloseBtn.Text = "✕"; CloseBtn.TextColor3 = WHITE
CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.TextSize = 15
CloseBtn.AutoButtonColor = false; CloseBtn.BorderSizePixel = 0
CloseBtn.ZIndex = 10; CloseBtn.Parent = MainFrame
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
MainFrame.Visible = false
end)

-- ============================================================
-- Left side — Player List
-- ============================================================
local LeftPanel = Instance.new("Frame")
LeftPanel.Size = UDim2.new(0, 155, 1, -60)
LeftPanel.Position = UDim2.new(0, 8, 0, 54)
LeftPanel.BackgroundColor3 = Color3.fromRGB(4, 8, 30)
LeftPanel.BackgroundTransparency = 0.3
LeftPanel.BorderSizePixel = 0
LeftPanel.Parent = MainFrame
corner(LeftPanel, 12)
stroke(LeftPanel, BLUE_A, 1, 0.4)

local PanelTitle = Instance.new("TextLabel")
PanelTitle.Size = UDim2.new(1, 0, 0, 28)
PanelTitle.BackgroundColor3 = BLUE_B
PanelTitle.BackgroundTransparency = 0.3
PanelTitle.BorderSizePixel = 0
PanelTitle.Text = "👥 اختر خويك"
PanelTitle.Font = Enum.Font.GothamBold
PanelTitle.TextSize = 11
PanelTitle.TextColor3 = WHITE
PanelTitle.Parent = LeftPanel
corner(PanelTitle, 12)
gradient(PanelTitle, Color3.fromRGB(15, 50, 160), Color3.fromRGB(8, 25, 90), 90)

local PlayerScroll = Instance.new("ScrollingFrame")
PlayerScroll.Size = UDim2.new(1, -6, 1, -36)
PlayerScroll.Position = UDim2.new(0, 3, 0, 32)
PlayerScroll.BackgroundTransparency = 1
PlayerScroll.BorderSizePixel = 0
PlayerScroll.ScrollBarThickness = 4
PlayerScroll.ScrollBarImageColor3 = BLUE_A
PlayerScroll.CanvasSize = UDim2.new()
PlayerScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
PlayerScroll.Parent = LeftPanel

local PlayerLayout = Instance.new("UIListLayout")
PlayerLayout.Padding = UDim.new(0, 4)
PlayerLayout.SortOrder = Enum.SortOrder.LayoutOrder
PlayerLayout.Parent = PlayerScroll

local PlayerPad = Instance.new("UIPadding")
PlayerPad.PaddingTop = UDim.new(0, 4)
PlayerPad.PaddingLeft = UDim.new(0, 2)
PlayerPad.PaddingRight = UDim.new(0, 2)
PlayerPad.Parent = PlayerScroll

-- ============================================================
-- Right side — Target label + Rescue buttons + FAZAA button
-- ============================================================
local RightPanel = Instance.new("Frame")
RightPanel.Size = UDim2.new(1, -173, 1, -60)
RightPanel.Position = UDim2.new(0, 167, 0, 54)
RightPanel.BackgroundTransparency = 1
RightPanel.BorderSizePixel = 0
RightPanel.Parent = MainFrame

-- Target label
local TargetLabel = Instance.new("TextLabel")
TargetLabel.Size = UDim2.new(1, 0, 0, 30)
TargetLabel.BackgroundColor3 = Color3.fromRGB(8, 22, 70)
TargetLabel.BackgroundTransparency = 0.2
TargetLabel.BorderSizePixel = 0
TargetLabel.Text = "🎯 المستهدف: لم يُختر بعد"
TargetLabel.Font = Enum.Font.GothamBold
TargetLabel.TextSize = 12
TargetLabel.TextColor3 = Color3.fromRGB(180, 220, 255)
TargetLabel.Parent = RightPanel
corner(TargetLabel, 8)
gradient(TargetLabel, Color3.fromRGB(12, 35, 110), Color3.fromRGB(5, 15, 60), 90)
local tLabelStroke = stroke(TargetLabel, WHITE, 1, 0.4)
task.spawn(function()
while TargetLabel and TargetLabel.Parent do
local p = (math.sin(tick() * 2.5) + 1) / 2
tLabelStroke.Transparency = 0.2 + p * 0.65
task.wait(0.04)
end
end)

-- Rescue buttons scroll
local BtnScroll = Instance.new("ScrollingFrame")
BtnScroll.Size = UDim2.new(1, 0, 1, -100)
BtnScroll.Position = UDim2.new(0, 0, 0, 36)
BtnScroll.BackgroundTransparency = 1
BtnScroll.BorderSizePixel = 0
BtnScroll.ScrollBarThickness = 4
BtnScroll.ScrollBarImageColor3 = BLUE_A
BtnScroll.CanvasSize = UDim2.new()
BtnScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
BtnScroll.Parent = RightPanel

local BtnGrid = Instance.new("UIGridLayout")
BtnGrid.CellSize = UDim2.new(0.5, -5, 0, 40)
BtnGrid.CellPadding = UDim2.new(0, 6, 0, 6)
BtnGrid.HorizontalAlignment = Enum.HorizontalAlignment.Center
BtnGrid.SortOrder = Enum.SortOrder.LayoutOrder
BtnGrid.Parent = BtnScroll

local BtnPad = Instance.new("UIPadding")
BtnPad.PaddingTop = UDim.new(0, 4)
BtnPad.PaddingBottom = UDim.new(0, 4)
BtnPad.PaddingLeft = UDim.new(0, 4)
BtnPad.PaddingRight = UDim.new(0, 4)
BtnPad.Parent = BtnScroll

-- ============================================================
-- Selected player state
-- ============================================================
local selectedPlayer = ""

-- ============================================================
-- Build rescue buttons
-- ============================================================
local btnColors = {
{Color3.fromRGB(20, 80, 200), Color3.fromRGB(10, 40, 120)},
{Color3.fromRGB(25, 140, 60), Color3.fromRGB(12, 70, 30)},
{Color3.fromRGB(160, 50, 180), Color3.fromRGB(90, 25, 110)},
{Color3.fromRGB(180, 120, 15), Color3.fromRGB(100, 65, 8)},
}

for i, data in ipairs(RESCUE_CMDS) do
local col = btnColors[((i - 1) % #btnColors) + 1]

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(0.5, -5, 0, 40)
btn.Text = data.label
btn.LayoutOrder = i
btn.Parent = BtnScroll
styleBtn(btn, col[1], col[2])

btn.MouseButton1Click:Connect(function()
if selectedPlayer == "" then
-- flash red on target label
TweenService:Create(TargetLabel, TweenInfo.new(0.1),
{BackgroundColor3 = Color3.fromRGB(160, 20, 20)}):Play()
task.delay(0.25, function()
TweenService:Create(TargetLabel, TweenInfo.new(0.2),
{BackgroundColor3 = Color3.fromRGB(8, 22, 70)}):Play()
end)
TargetLabel.Text = "⚠️ اختر لاعب أولاً!"
return
end
local finalCmd = string.format(data.cmd, selectedPlayer)
fireCommand(finalCmd)
-- flash green
TweenService:Create(btn, TweenInfo.new(0.08),
{BackgroundColor3 = GREEN}):Play()
task.delay(0.2, function()
TweenService:Create(btn, TweenInfo.new(0.25),
{BackgroundColor3 = col[1]}):Play()
end)
end)
end

-- ============================================================
-- FAZAA Button (يطلق جميع الأوامر دفعة واحدة)
-- ============================================================
local FazaaBtn = Instance.new("TextButton")
FazaaBtn.Size = UDim2.new(1, 0, 0, 48)
FazaaBtn.Position = UDim2.new(0, 0, 1, -52)
FazaaBtn.Text = "🆘 فزعة كاملة — اطلق الكل دفعة وحدة"
FazaaBtn.Parent = RightPanel
styleBtn(FazaaBtn, Color3.fromRGB(200, 30, 30), Color3.fromRGB(120, 15, 15))
FazaaBtn.TextSize = 13

-- Pulsing glow on fazaa button
local fazaaStroke = stroke(FazaaBtn, Color3.fromRGB(255, 80, 80), 2, 0.2)
task.spawn(function()
while FazaaBtn and FazaaBtn.Parent do
local p = (math.sin(tick() * 3) + 1) / 2
fazaaStroke.Transparency = 0.0 + p * 0.7
FazaaBtn.BackgroundColor3 = Color3.fromRGB(
math.floor(160 + p * 60),
math.floor(20 + p * 20),
math.floor(20 + p * 10)
)
task.wait(0.03)
end
end)

FazaaBtn.MouseButton1Click:Connect(function()
if selectedPlayer == "" then
TargetLabel.Text = "⚠️ اختر لاعب أولاً!"
TweenService:Create(TargetLabel, TweenInfo.new(0.1),
{BackgroundColor3 = Color3.fromRGB(160, 20, 20)}):Play()
task.delay(0.3, function()
TweenService:Create(TargetLabel, TweenInfo.new(0.2),
{BackgroundColor3 = Color3.fromRGB(8, 22, 70)}):Play()
end)
return
end

FazaaBtn.Text = "⏳ جاري الفزعة..."
for _, data in ipairs(RESCUE_CMDS) do
local finalCmd = string.format(data.cmd, selectedPlayer)
fireCommand(finalCmd)
task.wait(0.12)
end
FazaaBtn.Text = "✅ تمت الفزعة!"
task.delay(2, function()
if FazaaBtn and FazaaBtn.Parent then
FazaaBtn.Text = "🆘 فزعة كاملة — اطلق الكل دفعة وحدة"
end
end)
end)

-- ============================================================
-- Player List Builder
-- ============================================================
local function refreshPlayers()
for _, v in pairs(PlayerScroll:GetChildren()) do
if v:IsA("TextButton") then v:Destroy() end
end

for i, p in ipairs(Players:GetPlayers()) do
if p == LocalPlayer then continue end

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, -4, 0, 42)
btn.BackgroundColor3 = Color3.fromRGB(6, 14, 50)
btn.BackgroundTransparency = 0.3
btn.BorderSizePixel = 0
btn.LayoutOrder = i
btn.Parent = PlayerScroll
corner(btn, 8)
gradient(btn, Color3.fromRGB(8, 20, 70), Color3.fromRGB(4, 10, 40), 90)

local pStroke = stroke(btn, BLUE_A, 1, 0.6)

-- Avatar thumbnail
local avatar = Instance.new("ImageLabel")
avatar.Size = UDim2.new(0, 34, 0, 34)
avatar.Position = UDim2.new(0, 4, 0.5, -17)
avatar.BackgroundColor3 = Color3.fromRGB(5, 10, 35)
avatar.BorderSizePixel = 0
avatar.Image = ""
avatar.Parent = btn
corner(avatar, 999)

task.spawn(function()
local ok, uid = pcall(function()
return Players:GetUserIdFromNameAsync(p.Name)
end)
if ok and uid and avatar and avatar.Parent then
local ok2, thumb = pcall(function()
return Players:GetUserThumbnailAsync(
uid,
Enum.ThumbnailType.HeadShot,
Enum.ThumbnailSize.Size100x100
)
end)
if ok2 and thumb and avatar and avatar.Parent then
avatar.Image = thumb
end
end
end)

local nameLabel = Instance.new("TextLabel")
nameLabel.Size = UDim2.new(1, -44, 0, 20)
nameLabel.Position = UDim2.new(0, 42, 0, 4)
nameLabel.BackgroundTransparency = 1
nameLabel.Text = p.Name
nameLabel.Font = Enum.Font.GothamBold
nameLabel.TextSize = 10
nameLabel.TextColor3 = Color3.fromRGB(180, 215, 255)
nameLabel.TextXAlignment = Enum.TextXAlignment.Left
nameLabel.TextTruncate = Enum.TextTruncate.AtEnd
nameLabel.Parent = btn

local dispLabel = Instance.new("TextLabel")
dispLabel.Size = UDim2.new(1, -44, 0, 16)
dispLabel.Position = UDim2.new(0, 42, 0, 22)
dispLabel.BackgroundTransparency = 1
dispLabel.Text = p.DisplayName
dispLabel.Font = Enum.Font.Gotham
dispLabel.TextSize = 9
dispLabel.TextColor3 = Color3.fromRGB(100, 140, 200)
dispLabel.TextXAlignment = Enum.TextXAlignment.Left
dispLabel.TextTruncate = Enum.TextTruncate.AtEnd
dispLabel.Parent = btn

btn.MouseButton1Click:Connect(function()
selectedPlayer = p.Name
TargetLabel.Text = "🎯 المستهدف: " .. p.Name

-- highlight selected
for _, v2 in pairs(PlayerScroll:GetChildren()) do
if v2:IsA("TextButton") then
v2.BackgroundColor3 = Color3.fromRGB(6, 14, 50)
local s2 = v2:FindFirstChildOfClass("UIStroke")
if s2 then s2.Color = BLUE_A; s2.Thickness = 1 end
end
end
btn.BackgroundColor3 = Color3.fromRGB(15, 50, 160)
pStroke.Color = WHITE
pStroke.Thickness = 2

-- flash target label green
TweenService:Create(TargetLabel, TweenInfo.new(0.1),
{BackgroundColor3 = GREEN}):Play()
task.delay(0.25, function()
TweenService:Create(TargetLabel, TweenInfo.new(0.3),
{BackgroundColor3 = Color3.fromRGB(8, 22, 70)}):Play()
end)
end)
end
end

refreshPlayers()
Players.PlayerAdded:Connect(refreshPlayers)
Players.PlayerRemoving:Connect(refreshPlayers)

-- ============================================================
-- Bottom branding
-- ============================================================
local BottomNote = Instance.new("TextLabel")
BottomNote.Size = UDim2.new(1, -20, 0, 16)
BottomNote.Position = UDim2.new(0, 10, 1, -20)
BottomNote.BackgroundTransparency = 1
BottomNote.Text = "made by MOILA933 ✨ | فزعة Script"
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
ToggleBtn.Text = "⚡"
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


