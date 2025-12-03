-- Script Cartola Hub Mobile v1.0
-- Compatível com Roblox Mobile

local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Configurações padrão
local Settings = {
    Aimbot = {
        Enabled = false,
        Smoothness = 0.5,
        MaxDistance = 500,
        TeamCheck = true,
        FOV = 50
    },
    ESP = {
        Enabled = false,
        Box = true,
        Names = true,
        Health = true,
        Distance = true,
        TeamColor = Color3.fromRGB(0, 255, 0),
        EnemyColor = Color3.fromRGB(255, 0, 0)
    },
    Hitbox = {
        Enabled = false,
        Size = 1.5
    }
}

-- Elementos da UI
local UI = {}
local Components = {}

-- Criar a interface principal
function UI:CreateMainFrame()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "CartolaHubUI"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    -- Frame principal
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0.9, 0, 0.95, 0)
    MainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
    MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    MainFrame.BackgroundTransparency = 0.15
    MainFrame.BorderSizePixel = 0
    MainFrame.ClipsDescendants = true
    
    -- Sombras e efeitos
    local UIStroke = Instance.new("UIStroke")
    UIStroke.Color = Color3.fromRGB(100, 65, 165)
    UIStroke.Thickness = 2
    UIStroke.Transparency = 0.3
    UIStroke.Parent = MainFrame
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 15)
    UICorner.Parent = MainFrame
    
    -- Header
    local Header = Instance.new("Frame")
    Header.Name = "Header"
    Header.Size = UDim2.new(1, 0, 0.08, 0)
    Header.BackgroundTransparency = 1
    Header.Parent = MainFrame
    
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(1, 0, 1, 0)
    Title.BackgroundTransparency = 1
    Title.Text = "🎮 CARTOLA HUB"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 28
    Title.Font = Enum.Font.GothamBold
    Title.Parent = Header
    
    local Subtitle = Instance.new("TextLabel")
    Subtitle.Name = "Subtitle"
    Subtitle.Size = UDim2.new(1, 0, 0.5, 0)
    Subtitle.Position = UDim2.new(0, 0, 0.5, 0)
    Subtitle.BackgroundTransparency = 1
    Subtitle.Text = "Mobile Edition v1.0"
    Subtitle.TextColor3 = Color3.fromRGB(200, 200, 200)
    Subtitle.TextSize = 14
    Subtitle.Font = Enum.Font.Gotham
    Subtitle.Parent = Header
    
    -- Botão de fechar
    local CloseButton = Instance.new("TextButton")
    CloseButton.Name = "CloseButton"
    CloseButton.Size = UDim2.new(0.1, 0, 1, 0)
    CloseButton.Position = UDim2.new(0.9, 0, 0, 0)
    CloseButton.BackgroundTransparency = 1
    CloseButton.Text = "×"
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    CloseButton.TextSize = 30
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Parent = Header
    
    -- Container de categorias
    local CategoryContainer = Instance.new("Frame")
    CategoryContainer.Name = "CategoryContainer"
    CategoryContainer.Size = UDim2.new(1, 0, 0.82, 0)
    CategoryContainer.Position = UDim2.new(0, 0, 0.08, 0)
    CategoryContainer.BackgroundTransparency = 1
    CategoryContainer.Parent = MainFrame
    
    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.Padding = UDim.new(0, 8)
    UIListLayout.Parent = CategoryContainer
    
    -- Botão flutuante de toggle
    local ToggleButton = Instance.new("ImageButton")
    ToggleButton.Name = "ToggleButton"
    ToggleButton.Size = UDim2.new(0, 60, 0, 60)
    ToggleButton.Position = UDim2.new(1, -70, 0.5, -30)
    ToggleButton.AnchorPoint = Vector2.new(0, 0.5)
    ToggleButton.BackgroundColor3 = Color3.fromRGB(100, 65, 165)
    ToggleButton.Image = "rbxassetid://3926305904"
    ToggleButton.ImageRectOffset = Vector2.new(644, 204)
    ToggleButton.ImageRectSize = Vector2.new(36, 36)
    ToggleButton.ImageColor3 = Color3.fromRGB(255, 255, 255)
    
    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCorner.Parent = ToggleButton
    
    local ToggleStroke = Instance.new("UIStroke")
    ToggleStroke.Color = Color3.fromRGB(255, 255, 255)
    ToggleStroke.Thickness = 2
    ToggleStroke.Parent = ToggleButton
    
    ToggleButton.Parent = ScreenGui
    MainFrame.Parent = ScreenGui
    ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
    
    -- Estados
    local isVisible = false
    MainFrame.Visible = false
    
    -- Animações
    local TweenService = game:GetService("TweenService")
    
    CloseButton.MouseButton1Click:Connect(function()
        UI:Hide()
    end)
    
    ToggleButton.MouseButton1Click:Connect(function()
        if isVisible then
            UI:Hide()
        else
            UI:Show()
        end
    end)
    
    function UI:Show()
        MainFrame.Visible = true
        isVisible = true
        
        local tween = TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Position = UDim2.new(0.5, 0, 0.5, 0),
            Size = UDim2.new(0.9, 0, 0.95, 0)
        })
        tween:Play()
        
        ToggleButton.Visible = false
    end
    
    function UI:Hide()
        isVisible = false
        
        local tween = TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
            Position = UDim2.new(0.5, 0, 1.5, 0)
        })
        tween:Play()
        
        task.wait(0.3)
        MainFrame.Visible = false
        ToggleButton.Visible = true
    end
    
    return {
        ScreenGui = ScreenGui,
        MainFrame = MainFrame,
        CategoryContainer = CategoryContainer,
        ToggleButton = ToggleButton
    }
end

-- Criar mira do Aimbot
function UI:CreateAimCrosshair()
    local Crosshair = Instance.new("ScreenGui")
    Crosshair.Name = "AimbotCrosshair"
    Crosshair.ResetOnSpawn = false
    Crosshair.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    local OuterCircle = Instance.new("Frame")
    OuterCircle.Name = "OuterCircle"
    OuterCircle.Size = UDim2.new(0, 30, 0, 30)
    OuterCircle.Position = UDim2.new(0.5, -15, 0.5, -15)
    OuterCircle.BackgroundTransparency = 1
    OuterCircle.BorderSizePixel = 0
    
    local Circle1 = Instance.new("Frame")
    Circle1.Name = "Circle1"
    Circle1.Size = UDim2.new(1, 0, 1, 0)
    Circle1.BackgroundTransparency = 0.8
    Circle1.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    Circle1.BorderSizePixel = 0
    
    local UICorner1 = Instance.new("UICorner")
    UICorner1.CornerRadius = UDim.new(1, 0)
    UICorner1.Parent = Circle1
    
    local UIStroke1 = Instance.new("UIStroke")
    UIStroke1.Color = Color3.fromRGB(255, 255, 255)
    UIStroke1.Thickness = 2
    UIStroke1.Parent = Circle1
    
    local InnerCircle = Instance.new("Frame")
    InnerCircle.Name = "InnerCircle"
    InnerCircle.Size = UDim2.new(0, 8, 0, 8)
    InnerCircle.Position = UDim2.new(0.5, -4, 0.5, -4)
    InnerCircle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    InnerCircle.BorderSizePixel = 0
    
    local UICorner2 = Instance.new("UICorner")
    UICorner2.CornerRadius = UDim.new(1, 0)
    UICorner2.Parent = InnerCircle
    
    InnerCircle.Parent = OuterCircle
    Circle1.Parent = OuterCircle
    OuterCircle.Parent = Crosshair
    Crosshair.Parent = LocalPlayer:WaitForChild("PlayerGui")
    
    OuterCircle.Visible = false
    
    return OuterCircle
end

-- Componente: Botão com toggle
function Components:CreateToggleButton(config)
    local ToggleFrame = Instance.new("Frame")
    ToggleFrame.Name = "ToggleFrame"
    ToggleFrame.Size = UDim2.new(1, 0, 0, 60)
    ToggleFrame.BackgroundTransparency = 1
    
    local Background = Instance.new("Frame")
    Background.Name = "Background"
    Background.Size = UDim2.new(1, 0, 1, 0)
    Background.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    Background.BackgroundTransparency = 0.1
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 10)
    UICorner.Parent = Background
    
    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Name = "Title"
    TitleLabel.Size = UDim2.new(0.7, 0, 1, 0)
    TitleLabel.Position = UDim2.new(0.05, 0, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = config.Title
    TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    TitleLabel.TextSize = 18
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.Font = Enum.Font.GothamBold
    
    local IconLabel = Instance.new("TextLabel")
    IconLabel.Name = "Icon"
    IconLabel.Size = UDim2.new(0.1, 0, 1, 0)
    IconLabel.Position = UDim2.new(0.85, 0, 0, 0)
    IconLabel.BackgroundTransparency = 1
    IconLabel.Text = config.Icon or "⚙️"
    IconLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    IconLabel.TextSize = 24
    IconLabel.Font = Enum.Font.GothamBold
    
    local ToggleButton = Instance.new("TextButton")
    ToggleButton.Name = "Toggle"
    ToggleButton.Size = UDim2.new(0.2, 0, 0.6, 0)
    ToggleButton.Position = UDim2.new(0.75, 0, 0.2, 0)
    ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
    ToggleButton.Text = ""
    ToggleButton.AutoButtonColor = false
    
    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(0.5, 0)
    ToggleCorner.Parent = ToggleButton
    
    local ToggleIndicator = Instance.new("Frame")
    ToggleIndicator.Name = "Indicator"
    ToggleIndicator.Size = UDim2.new(0.4, 0, 0.8, 0)
    ToggleIndicator.Position = UDim2.new(0.05, 0, 0.1, 0)
    ToggleIndicator.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    ToggleIndicator.BorderSizePixel = 0
    
    local IndicatorCorner = Instance.new("UICorner")
    IndicatorCorner.CornerRadius = UDim.new(0.5, 0)
    IndicatorCorner.Parent = ToggleIndicator
    
    ToggleIndicator.Parent = ToggleButton
    
    local function updateToggle(state)
        if state then
            TweenService:Create(ToggleIndicator, TweenInfo.new(0.2), {
                Position = UDim2.new(0.55, 0, 0.1, 0),
                BackgroundColor3 = Color3.fromRGB(50, 255, 50)
            }):Play()
        else
            TweenService:Create(ToggleIndicator, TweenInfo.new(0.2), {
                Position = UDim2.new(0.05, 0, 0.1, 0),
                BackgroundColor3 = Color3.fromRGB(255, 50, 50)
            }):Play()
        end
    end
    
    ToggleButton.MouseButton1Click:Connect(function()
        config.Callback(not config.State)
        config.State = not config.State
        updateToggle(config.State)
    end)
    
    updateToggle(config.State)
    
    ToggleButton.Parent = Background
    IconLabel.Parent = Background
    TitleLabel.Parent = Background
    Background.Parent = ToggleFrame
    
    return ToggleFrame
end

-- Componente: Slider
function Components:CreateSlider(config)
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Name = "SliderFrame"
    SliderFrame.Size = UDim2.new(1, 0, 0, 80)
    SliderFrame.BackgroundTransparency = 1
    
    local Background = Instance.new("Frame")
    Background.Name = "Background"
    Background.Size = UDim2.new(1, 0, 1, 0)
    Background.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
    Background.BackgroundTransparency = 0.1
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 10)
    UICorner.Parent = Background
    
    local TitleLabel = Instance.new("TextLabel")
    TitleLabel.Name = "Title"
    TitleLabel.Size = UDim2.new(0.9, 0, 0.4, 0)
    TitleLabel.Position = UDim2.new(0.05, 0, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = config.Title
    TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    TitleLabel.TextSize = 16
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
    TitleLabel.Font = Enum.Font.GothamBold
    
    local ValueLabel = Instance.new("TextLabel")
    ValueLabel.Name = "Value"
    ValueLabel.Size = UDim2.new(0.1, 0, 0.4, 0)
    ValueLabel.Position = UDim2.new(0.85, 0, 0, 0)
    ValueLabel.BackgroundTransparency = 1
    ValueLabel.Text = tostring(config.Value)
    ValueLabel.TextColor3 = Color3.fromRGB(200, 200, 255)
    ValueLabel.TextSize = 16
    ValueLabel.Font = Enum.Font.GothamBold
    
    local SliderTrack = Instance.new("Frame")
    SliderTrack.Name = "Track"
    SliderTrack.Size = UDim2.new(0.9, 0, 0.2, 0)
    SliderTrack.Position = UDim2.new(0.05, 0, 0.6, 0)
    SliderTrack.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
    SliderTrack.BorderSizePixel = 0
    
    local TrackCorner = Instance.new("UICorner")
    TrackCorner.CornerRadius = UDim.new(0, 5)
    TrackCorner.Parent = SliderTrack
    
    local SliderFill = Instance.new("Frame")
    SliderFill.Name = "Fill"
    SliderFill.Size = UDim2.new(
        (config.Value - config.Min) / (config.Max - config.Min),
        0, 1, 0
    )
    SliderFill.BackgroundColor3 = Color3.fromRGB(100, 65, 165)
    SliderFill.BorderSizePixel = 0
    
    local FillCorner = Instance.new("UICorner")
    FillCorner.CornerRadius = UDim.new(0, 5)
    FillCorner.Parent = SliderFill
    
    local SliderButton = Instance.new("TextButton")
    SliderButton.Name = "SliderButton"
    SliderButton.Size = UDim2.new(0, 20, 0, 20)
    SliderButton.Position = UDim2.new(
        (config.Value - config.Min) / (config.Max - config.Min),
        -10, 0.5, -10
    )
    SliderButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    SliderButton.Text = ""
    SliderButton.AutoButtonColor = false
    
    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0.5, 0)
    ButtonCorner.Parent = SliderButton
    
    SliderFill.Parent = SliderTrack
    SliderButton.Parent = SliderTrack
    
    local dragging = false
    
    local function updateSlider(value)
        local percent = math.clamp((value - config.Min) / (config.Max - config.Min), 0, 1)
        SliderFill.Size = UDim2.new(percent, 0, 1, 0)
        SliderButton.Position = UDim2.new(percent, -10, 0.5, -10)
        ValueLabel.Text = string.format("%.1f", value)
        config.Callback(value)
    end
    
    SliderButton.MouseButton1Down:Connect(function()
        dragging = true
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    RunService.RenderStepped:Connect(function()
        if dragging then
            local mousePos = UserInputService:GetMouseLocation()
            local trackPos = SliderTrack.AbsolutePosition
            local trackSize = SliderTrack.AbsoluteSize
            
            local relativeX = math.clamp((mousePos.X - trackPos.X) / trackSize.X, 0, 1)
            local value = config.Min + relativeX * (config.Max - config.Min)
            updateSlider(value)
        end
    end)
    
    ValueLabel.Parent = Background
    TitleLabel.Parent = Background
    SliderTrack.Parent = Background
    Background.Parent = SliderFrame
    
    return SliderFrame
end

-- Sistema de Aimbot
local AimSystem = {}
AimSystem.Crosshair = UI:CreateAimCrosshair()

function AimSystem:GetClosestEnemy()
    local closestPlayer = nil
    local closestDistance = Settings.Aimbot.MaxDistance
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            if Settings.Aimbot.TeamCheck then
                if player.Team and LocalPlayer.Team and player.Team == LocalPlayer.Team then
                    continue
                end
            end
            
            local character = player.Character
            local head = character:FindFirstChild("Head")
            local root = character:FindFirstChild("HumanoidRootPart")
            
            if head and root then
                local screenPoint, visible = Camera:WorldToViewportPoint(head.Position)
                local distance = (head.Position - Camera.CFrame.Position).Magnitude
                
                if visible and distance <= closestDistance then
                    local mousePos = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
                    local targetPos = Vector2.new(screenPoint.X, screenPoint.Y)
                    local distance2D = (targetPos - mousePos).Magnitude
                    
                    if distance2D <= Settings.Aimbot.FOV then
                        closestPlayer = player
                        closestDistance = distance
                    end
                end
            end
        end
    end
    
    return closestPlayer
end

function AimSystem:AimAtTarget(target)
    if not target or not target.Character then return end
    
    local head = target.Character:FindFirstChild("Head")
    if not head then return end
    
    local cameraPos = Camera.CFrame.Position
    local targetPos = head.Position
    local direction = (targetPos - cameraPos).Unit
    
    local currentCF = Camera.CFrame
    local targetCF = CFrame.lookAt(cameraPos, cameraPos + direction)
    
    local smoothness = math.clamp(Settings.Aimbot.Smoothness, 0.1, 1)
    local smoothedCF = currentCF:Lerp(targetCF, 1 - smoothness)
    
    Camera.CFrame = smoothedCF
end

-- Sistema ESP
local ESPSystem = {}
ESPSystem.ESPObjects = {}

function ESPSystem:CreateESP(player)
    if ESPSystem.ESPObjects[player] then return end
    
    local ESPHolder = Instance.new("Folder")
    ESPHolder.Name = player.Name .. "_ESP"
    ESPSystem.ESPObjects[player] = ESPHolder
    
    local function updateESP()
        if not player.Character then return end
        
        local character = player.Character
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        local head = character:FindFirstChild("Head")
        local root = character:FindFirstChild("HumanoidRootPart")
        
        if not humanoid or not head or not root then return end
        
        -- Box ESP
        local box = ESPHolder:FindFirstChild("Box")
        if not box then
            box = Instance.new("BoxHandleAdornment")
            box.Name = "Box"
            box.Adornee = root
            box.Size = Vector3.new(4, 6, 1)
            box.Transparency = 0.7
            box.ZIndex = 1
            box.AlwaysOnTop = true
            box.Parent = ESPHolder
        end
        
        -- Health ESP
        local healthBar = ESPHolder:FindFirstChild("HealthBar")
        if not healthBar then
            healthBar = Instance.new("Frame")
            healthBar.Name = "HealthBar"
            healthBar.Size = UDim2.new(0, 50, 0, 4)
            healthBar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            healthBar.BorderSizePixel = 0
            healthBar.Parent = ESPHolder
        end
        
        local healthFill = healthBar:FindFirstChild("Fill")
        if not healthFill then
            healthFill = Instance.new("Frame")
            healthFill.Name = "Fill"
            healthFill.Size = UDim2.new(1, 0, 1, 0)
            healthFill.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
            healthFill.BorderSizePixel = 0
            healthFill.Parent = healthBar
        end
        
        -- Name Label
        local nameLabel = ESPHolder:FindFirstChild("Name")
        if not nameLabel then
            nameLabel = Instance.new("BillboardGui")
            nameLabel.Name = "Name"
            nameLabel.Size = UDim2.new(0, 200, 0, 50)
            nameLabel.StudsOffset = Vector3.new(0, 3.5, 0)
            nameLabel.AlwaysOnTop = true
            nameLabel.MaxDistance = 1000
            nameLabel.Parent = 
