local MacLib = loadstring(game:HttpGet("https://github.com/biggaboy212/Maclib/releases/latest/download/maclib.txt"))()

local Window = MacLib:Window({
	Title = "Nesux X MacUI",
	Subtitle = "Bởi Muori Bản Trả Phí.",
	Size = UDim2.fromOffset(868, 650),
	DragStyle = 1,
	DisabledWindowControls = {},
	ShowUserInfo = true,
	Keybind = Enum.KeyCode.RightControl,
	AcrylicBlur = true,
})

local globalSettings = {
	UIBlurToggle = Window:GlobalSetting({
		Name = "Làm mờ giao diện người dùng",
		Default = Window:GetAcrylicBlurState(),
		Callback = function(bool)
			Window:SetAcrylicBlurState(bool)
			Window:Notify({
				Title = Window.Settings.Title,
				Description = (bool and "Bật" or "Tắt") .. " Làm mờ giao diện người dùng",
				Lifetime = 5
			})
		end,
	}),
	NotificationToggler = Window:GlobalSetting({
		Name = "Thông báo",
		Default = Window:GetNotificationsState(),
		Callback = function(bool)
			Window:SetNotificationsState(bool)
			Window:Notify({
				Title = Window.Settings.Title,
				Description = (bool and "Bật" or "Tắt") .. " Thông báo",
				Lifetime = 5
			})
		end,
	}),
	ShowUserInfo = Window:GlobalSetting({
		Name = "Hiển thị thông tin người dùng",
		Default = Window:GetUserInfoState(),
		Callback = function(bool)
			Window:SetUserInfoState(bool)
			Window:Notify({
				Title = Window.Settings.Title,
				Description = (bool and "Đang hiển thị" or "Đã che lại") .. " Hiển thị thông tin người dùng",
				Lifetime = 5
			})
		end,
	})
}

local tabGroups = {
	TabGroup1 = Window:TabGroup()
}

local tabs = {
	Main = tabGroups.TabGroup1:Tab({ Name = "Thông báo Update", Image = "rbxassetid://81478297548754" }),
	Esp = tabGroups.TabGroup1:Tab({ Name = "EspPlayer", Image = "rbxassetid://84727577248856" }),
		EspM = tabGroups.TabGroup1:Tab({ Name = "EspM", Image = "rbxassetid://84727577248856" }),
	Aim = tabGroups.TabGroup1:Tab({ Name = "Aimbot", Image = "rbxassetid://130939958971532" }),
	Uni = tabGroups.TabGroup1:Tab({ Name = "Visual", Image = "rbxassetid://122760395538267" }),
	Pla = tabGroups.TabGroup1:Tab({ Name = "Player", Image = "rbxassetid://122760395538267" }),
	Sha = tabGroups.TabGroup1:Tab({ Name = "Đồ họa", Image = "rbxassetid://87916652848972" }),
	Settings = tabGroups.TabGroup1:Tab({ Name = "Cài đặt", Image = "rbxassetid://75791170485208" })
}

local sections = {
	MainSection1 = tabs.Main:Section({ Side = "Left" }),
	Main2Section1 = tabs.Main:Section({ Side = "Right" }),
	EspSection1 = tabs.Esp:Section({ Side = "Left" }),
	Esp2Section1 = tabs.Esp:Section({ Side = "Right" }),
	EspMSection1 = tabs.EspM:Section({ Side = "Left" }),
	AimSection1 = tabs.Aim:Section({ Side = "Left" }),
	Aim2Section1 = tabs.Aim:Section({ Side = "Right" }),
	UniSection1 = tabs.Uni:Section({ Side = "Left" }),
	Uni2Section1 = tabs.Uni:Section({ Side = "Right" }),
	PlaSection1 = tabs.Pla:Section({ Side = "Left" }),
	Pla2Section1 = tabs.Pla:Section({ Side = "Right" }),
	ShaSection1 = tabs.Sha:Section({ Side = "Left" }),
}

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local player = Players.LocalPlayer
local VirtualUser = game:GetService("VirtualUser")
------------------------------------------------------------------------------------------------------------
sections.EspSection1:Header({
	Name = "ESP Tên Người Chơi"
})

local ShowNamesEnabled = false
local NameSize = 14
local TeamCheckEnabled = false
local NameColor = Color3.fromRGB(255, 255, 255)
local SelectedFont = Enum.Font.SourceSansBold
local YOffset = 3 -- Giá trị dương để hiển thị trên đầu

local fontOptions = {
    "SourceSans",
    "SourceSansBold",
    "SourceSansLight",
    "SourceSansItalic",
    "Highway",
    "Arcade",
    "Arial",
    "ArialBold",
    "Bodoni",
    "Cartoon",
    "Code",
    "Creepster",
    "DenkOne",
    "Fantasy",
    "Garamond",
    "Gotham",
    "GothamBlack",
    "GothamBold",
    "GothamMedium",
    "GrenzeGotisch",
    "IndieFlower",
    "JosefinSans",
    "Kalam",
    "Legacy",
    "LuckiestGuy",
    "Merriweather",
    "Michroma",
    "Nunito",
    "Oswald",
    "PatrickHand",
    "PermanentMarker",
    "Roboto",
    "RobotoCondensed",
    "RobotoMono",
    "Salvador",
    "Sarpanch",
    "SpecialElite",
    "TitilliumWeb",
    "Ubuntu"
}

local function getFontEnum(fontName)
    return Enum.Font[fontName]
end

local function isEnemyPlayer(player)
    if not TeamCheckEnabled then return true end
    if not game.Players.LocalPlayer.Team then return true end 
    if not player.Team then return true end
    return player.Team ~= game.Players.LocalPlayer.Team
end

local function UpdateCustomName(player)
    local function updateForCharacter(character)
        if character then
            local humanoid = character:WaitForChild("Humanoid")
            if humanoid then
                humanoid.DisplayDistanceType = ShowNamesEnabled and Enum.HumanoidDisplayDistanceType.None or Enum.HumanoidDisplayDistanceType.Viewer
            end
            
            local head = character:WaitForChild("Head")
            if head then
                local nameGui = head:FindFirstChild("NameGui") or Instance.new("BillboardGui")
                nameGui.Name = "NameGui"
                nameGui.AlwaysOnTop = true
                nameGui.Size = UDim2.new(0, 100, 0, 40)
                nameGui.StudsOffset = Vector3.new(0, YOffset, 0) -- Đã bỏ dấu trừ để hiển thị trên đầu
                nameGui.Parent = head

                local nameLabel = nameGui:FindFirstChild("NameLabel") or Instance.new("TextLabel")
                nameLabel.Name = "NameLabel"
                nameLabel.Size = UDim2.new(1, 0, 1, 0)
                nameLabel.BackgroundTransparency = 1
                nameLabel.TextColor3 = TeamCheckEnabled and player.TeamColor.Color or NameColor
                nameLabel.TextStrokeTransparency = 0
                nameLabel.TextSize = NameSize
                nameLabel.Font = SelectedFont
                nameLabel.Parent = nameGui

                if ShowNamesEnabled and isEnemyPlayer(player) then
                    nameGui.Enabled = true
                    nameLabel.Text = player.Name
                else
                    nameGui.Enabled = false
                end
            end
        end
    end

    if player.Character then
        updateForCharacter(player.Character)
    end

    player.CharacterAdded:Connect(updateForCharacter)
end

local function PlayerAdded(player)
    if player ~= game.Players.LocalPlayer then
        UpdateCustomName(player)
        player:GetPropertyChangedSignal("Team"):Connect(function()
            UpdateCustomName(player)
        end)
    end
end

-- Kết nối các sự kiện
game.Players.PlayerAdded:Connect(PlayerAdded)

-- Xử lý người chơi hiện có
for _, player in ipairs(game.Players:GetPlayers()) do
    PlayerAdded(player)
end

-- Tạo hàm để thay đổi YOffset
local function SetYOffset(value)
    YOffset = value
    -- Cập nhật lại tất cả ESP hiện có
    for _, player in ipairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            UpdateCustomName(player)
        end
    end
end


-- Toggle hiển thị tên
sections.EspSection1:Toggle({
    Name = "Hiện Tên",
    Default = false,
    Callback = function(value)
        ShowNamesEnabled = value
        if value then
            for _, player in pairs(game.Players:GetPlayers()) do
                if player ~= game.Players.LocalPlayer then
                    UpdateCustomName(player)
                end
            end
        end
        
        game.Players.PlayerAdded:Connect(PlayerAdded)

        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                PlayerAdded(player)
            end
        end
    end,
}, "ShowNames")

-- Toggle kiểm tra đồng đội
sections.EspSection1:Toggle({
    Name = "Team Check",
    Default = false,
    Callback = function(value)
        TeamCheckEnabled = value
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                UpdateCustomName(player)
            end
        end
    end,
}, "TeamCheck")

-- Slider kích thước tên
sections.EspSection1:Slider({
    Name = "Kích thước tên",
    Default = 14,
    Minimum = 1,
    Maximum = 100,
    DisplayMethod = "Value", -- Hiển thị giá trị số
    Precision = 0,
    Callback = function(value)
        NameSize = value
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head then
                    local nameGui = head:FindFirstChild("NameGui")
                    if nameGui then
                        local nameLabel = nameGui:FindFirstChild("NameLabel")
                        if nameLabel then
                            nameLabel.TextSize = value
                        end
                    end
                end
            end
        end
    end
}, "NameSize")

-- Colorpicker cho màu tên
sections.EspSection1:Colorpicker({
    Name = "Màu tên",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(color)
        NameColor = color
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head then
                    local nameGui = head:FindFirstChild("NameGui")
                    if nameGui then
                        local nameLabel = nameGui:FindFirstChild("NameLabel")
                        if nameLabel and not TeamCheckEnabled then
                            nameLabel.TextColor3 = color
                        end
                    end
                end
            end
        end
    end,
}, "NameColor")

sections.EspSection1:Dropdown({
    Name = "Phông chữ",
    Multi = false,
    Required = true,
    Options = fontOptions,
    Default = "SourceSansBold",
    Callback = function(fontName)
        SelectedFont = getFontEnum(fontName)
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head then
                    local nameGui = head:FindFirstChild("NameGui")
                    if nameGui then
                        local nameLabel = nameGui:FindFirstChild("NameLabel")
                        if nameLabel then
                            nameLabel.Font = SelectedFont
                        end
                    end
                end
            end
        end
    end,
}, "NameFont")

-----------------------------------------
sections.Esp2Section1:Header({
	Name = "ESP Thanh Máu Và %"
})

local ESP = {
    Enabled = false,
    TeamCheck = false,
    YOffset = -5 -- Thêm YOffset âm để ESP hiển thị phía trên đầu
}
local espConnections = {}

sections.Esp2Section1:Toggle({
    Name = "Hiện Máu",
    Default = false,
    Callback = function(value)
        ESP.Enabled = value
        
        -- Cleanup function để xóa toàn bộ ESP
        local function cleanupESP()
            for _, esp in ipairs(espConnections) do
                if esp.container then esp.container:Remove() end
                if esp.healthBar then esp.healthBar:Remove() end
                if esp.glowEffect then esp.glowEffect:Remove() end
                if esp.healthText then esp.healthText:Remove() end
                if esp.connection then esp.connection:Disconnect() end
                
                -- Set tất cả các đối tượng về nil
                esp.container = nil
                esp.healthBar = nil
                esp.glowEffect = nil
                esp.healthText = nil
                esp.connection = nil
            end
            table.clear(espConnections)
        end

        if not value then
            cleanupESP()
            return
        end

        local function CreateESP(player)
            if player == game.Players.LocalPlayer then return end

            local function IsTeammate()
                if not ESP.TeamCheck then return false end
                return player.Team == game.Players.LocalPlayer.Team
            end

            local container = Drawing.new("Square")
            container.Thickness = 1
            container.Filled = true
            container.Transparency = 0.8
            container.Color = Color3.fromRGB(25, 25, 25)
            container.Visible = false

            local healthBar = Drawing.new("Square")
            healthBar.Thickness = 1
            healthBar.Filled = true
            healthBar.Transparency = 1
            healthBar.Color = Color3.fromRGB(0, 255, 200)
            healthBar.Visible = false

            local glowEffect = Drawing.new("Square")
            glowEffect.Thickness = 1
            glowEffect.Filled = true
            glowEffect.Transparency = 0.4
            glowEffect.Color = Color3.fromRGB(0, 255, 200)
            glowEffect.Visible = false

            local healthText = Drawing.new("Text")
            healthText.Center = false
            healthText.Outline = true
            healthText.Color = Color3.fromRGB(255, 255, 255)
            healthText.Font = 3
            healthText.Size = 12
            healthText.Visible = false

            local previousHealth = 100
            local isAnimating = false
            local flashDuration = 0
            local originalTransparency = healthBar.Transparency

            local function AnimateHealthChange()
                if isAnimating then return end
                isAnimating = true
                flashDuration = 0

                local connection = game:GetService("RunService").RenderStepped:Connect(function(delta)
                    if not isAnimating or not ESP.Enabled then 
                        isAnimating = false
                        connection:Disconnect()
                        return 
                    end
                    
                    flashDuration = flashDuration + delta
                    if flashDuration >= 0.5 then
                        isAnimating = false
                        healthBar.Transparency = originalTransparency
                        glowEffect.Transparency = 0.4
                        healthText.Transparency = 1
                        connection:Disconnect()
                        return
                    end

                    local wave = math.abs(math.sin(flashDuration * 30))
                    healthBar.Transparency = originalTransparency * (0.5 + 0.5 * wave)
                    glowEffect.Transparency = 0.4 * (0.5 + 0.5 * wave)
                    healthText.Transparency = 1 * (0.5 + 0.5 * wave)
                    
                    local shake = math.random(-1, 1)
                    healthBar.Position = healthBar.Position + Vector2.new(shake, shake)
                    glowEffect.Position = glowEffect.Position + Vector2.new(shake, shake)
                    healthText.Position = healthText.Position + Vector2.new(shake, shake)
                end)
            end

            local function UpdateESP()
                if not ESP.Enabled then
                    container.Visible = false
                    healthBar.Visible = false
                    glowEffect.Visible = false
                    healthText.Visible = false
                    return
                end
                
                if IsTeammate() then
                    container.Visible = false
                    healthBar.Visible = false
                    glowEffect.Visible = false
                    healthText.Visible = false
                    return
                end
                
                local character = player.Character
                if not character or not character:FindFirstChild("Humanoid") or not character:FindFirstChild("HumanoidRootPart") or not character:FindFirstChild("Head") then
                    container.Visible = false
                    healthBar.Visible = false
                    glowEffect.Visible = false
                    healthText.Visible = false
                    return
                end

                local humanoid = character.Humanoid
                local head = character.Head

                local verticalOffset = 2.5
                local headPos = head.Position + Vector3.new(0, ESP.YOffset, 0) -- Sử dụng ESP.YOffset
    local headVec, onScreen = workspace.CurrentCamera:WorldToViewportPoint(headPos)
                
                if not onScreen then
                    container.Visible = false
                    healthBar.Visible = false
                    glowEffect.Visible = false
                    healthText.Visible = false
                    return
                end

                local barWidth = 65
                local barHeight = 6
                local padding = 2
                local health = humanoid.Health / humanoid.MaxHealth
                local healthPercent = math.floor(health * 100)

                if healthPercent < previousHealth then
                    AnimateHealthChange()
                end
                previousHealth = healthPercent

                local containerPos = Vector2.new(headVec.X - barWidth/2, headVec.Y - barHeight/2)
                
                container.Size = Vector2.new(barWidth, barHeight)
                container.Position = containerPos
                container.Visible = true

                local targetWidth = (barWidth - padding * 2) * health
                local currentWidth = healthBar.Size.X or 0
                healthBar.Size = Vector2.new(
                    currentWidth + (targetWidth - currentWidth) * 0.1,
                    barHeight - padding * 2
                )
                healthBar.Position = Vector2.new(containerPos.X + padding, containerPos.Y + padding)
                healthBar.Visible = true

                glowEffect.Size = Vector2.new(healthBar.Size.X, 2)
                glowEffect.Position = Vector2.new(containerPos.X + padding, containerPos.Y)
                glowEffect.Visible = true

                local textVerticalOffset = 15
                healthText.Text = healthPercent .. "%"
                healthText.Position = Vector2.new(
                    containerPos.X - 2,
                    containerPos.Y - textVerticalOffset
                )
                healthText.Visible = true
            end

            local renderConnection = game:GetService("RunService").RenderStepped:Connect(function()
                if not ESP.Enabled then 
                    container.Visible = false
                    healthBar.Visible = false
                    glowEffect.Visible = false
                    healthText.Visible = false
                    return 
                end
                
                if player.Parent == game:GetService("Players") then
                    UpdateESP()
                else
                    container:Remove()
                    healthBar:Remove()
                    glowEffect:Remove()
                    healthText:Remove()
                    renderConnection:Disconnect()
                end
            end)

            table.insert(espConnections, {
                container = container,
                healthBar = healthBar,
                glowEffect = glowEffect,
                healthText = healthText,
                connection = renderConnection
            })
        end

        -- Tạo ESP cho người chơi hiện tại
        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player ~= game.Players.LocalPlayer then
                CreateESP(player)
            end
        end

        -- Theo dõi người chơi mới
        game:GetService("Players").PlayerAdded:Connect(function(player)
            if player ~= game.Players.LocalPlayer then
                CreateESP(player)
            end
        end)
    end
})


sections.Esp2Section1:Toggle({
    Name = "Team Check", 
    Default = false,
    Callback = function(value)
        ESP.TeamCheck = value
    end
})
--------------------------------------------
sections.EspSection1:Header({
	Name = "ESP Tracer"
})

_G.Settings = {
    Enabled = false,
    TeamCheck = false,
    TracerThickness = 1.5,
    DefaultColor = Color3.fromRGB(255, 0, 0),
    TracerPosition = "Center",
    AnimationSpeed = 0.5
}

sections.EspSection1:Colorpicker({
    Name = "Màu Đường Kẻ",
    Default = Color3.fromRGB(255, 0, 0),
    Callback = function(color)
        _G.Settings.DefaultColor = color
        if _G.TracerObjects then
            for _, tracers in pairs(_G.TracerObjects) do
                tracers.main.Color = color
            end
        end
    end
})

sections.EspSection1:Dropdown({
    Name = "Vị Trí Đường Kẻ",
    Default = "Center",
    Options = {"Center", "Top", "Bottom"},
    Callback = function(Value)
        _G.Settings.TracerPosition = Value
    end
})

sections.EspSection1:Toggle({
    Name = "Hiện Đường Kẻ",
    Default = false,
    Callback = function(Value)
        _G.Settings.Enabled = Value
        
        if not _G.TracerInitialized then
            _G.TracerInitialized = true
            
            if not Drawing then
                Window:Notify({
                    Title = "Error"
                })
                return
            end
            
            local RunService = game:GetService("RunService")
            local Players = game:GetService("Players")
            local Camera = workspace.CurrentCamera
            local LocalPlayer = Players.LocalPlayer
            
            _G.TracerObjects = {}
            
            local function CreatePlayerTracers(player)
                if player ~= LocalPlayer then
                    if _G.TracerObjects[player.Name] then
                        _G.TracerObjects[player.Name].main:Remove()
                        _G.TracerObjects[player.Name].glow:Remove()
                    end
                    
                    local tracer = Drawing.new("Line")
                    tracer.Thickness = _G.Settings.TracerThickness
                    tracer.Color = _G.Settings.DefaultColor
                    tracer.Transparency = 0.6
                    tracer.ZIndex = 1
                    
                    local glow = Drawing.new("Line")
                    glow.Thickness = _G.Settings.TracerThickness + 2
                    glow.Color = Color3.new(1, 1, 1)
                    glow.Transparency = 0.3
                    glow.ZIndex = 0
                    
                    _G.TracerObjects[player.Name] = {
                        main = tracer,
                        glow = glow,
                        currentPos = Vector2.new(),
                        targetPos = Vector2.new()
                    }
                end
            end

            local function lerp(a, b, t)
                return a + (b - a) * t
            end

            local function IsTeamMate(player)
                if _G.Settings.TeamCheck then
                    return player.Team == LocalPlayer.Team
                end
                return false
            end
            
            local function UpdateTracers()
                if not _G.Settings.Enabled then
                    for _, tracers in pairs(_G.TracerObjects) do
                        tracers.main.Visible = false
                        tracers.glow.Visible = false
                    end
                    return
                end
                
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer and not _G.TracerObjects[player.Name] then
                        CreatePlayerTracers(player)
                    end
                end
                
                local viewportSize = Camera.ViewportSize
                local startPos
                
                if _G.Settings.TracerPosition == "Center" then
                    startPos = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
                elseif _G.Settings.TracerPosition == "Top" then
                    startPos = Vector2.new(viewportSize.X / 2, 0)
                elseif _G.Settings.TracerPosition == "Bottom" then
                    startPos = Vector2.new(viewportSize.X / 2, viewportSize.Y)
                end
                
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer then
                        local tracers = _G.TracerObjects[player.Name]
                        if not tracers then
                            CreatePlayerTracers(player)
                            tracers = _G.TracerObjects[player.Name]
                        end
                        
                        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            if IsTeamMate(player) then
                                tracers.main.Visible = false
                                tracers.glow.Visible = false
                                continue
                            end

                            local hrp = player.Character.HumanoidRootPart
                            local vector, onScreen = Camera:WorldToViewportPoint(hrp.Position)
                            
                            if onScreen then
                                local endPos = Vector2.new(vector.X, vector.Y)
                                tracers.targetPos = endPos
                                
                                tracers.currentPos = Vector2.new(
                                    lerp(tracers.currentPos.X, tracers.targetPos.X, _G.Settings.AnimationSpeed),
                                    lerp(tracers.currentPos.Y, tracers.targetPos.Y, _G.Settings.AnimationSpeed)
                                )
                                
                                tracers.main.From = startPos
                                tracers.main.To = tracers.currentPos
                                tracers.main.Visible = true
                                tracers.main.Color = _G.Settings.DefaultColor
                                
                                tracers.glow.From = startPos
                                tracers.glow.To = tracers.currentPos
                                tracers.glow.Visible = true
                            else
                                tracers.main.Visible = false
                                tracers.glow.Visible = false
                            end
                        else
                            tracers.main.Visible = false
                            tracers.glow.Visible = false
                        end
                    end
                end
            end
            
            Players.PlayerRemoving:Connect(function(player)
                local tracers = _G.TracerObjects[player.Name]
                if tracers then
                    tracers.main:Remove()
                    tracers.glow:Remove()
                    _G.TracerObjects[player.Name] = nil
                end
            end)

            Players.PlayerAdded:Connect(function(player)
                CreatePlayerTracers(player)
            end)
            
            for _, player in pairs(Players:GetPlayers()) do
                CreatePlayerTracers(player)
            end
            
            RunService.RenderStepped:Connect(UpdateTracers)
        end
    end
})

sections.EspSection1:Toggle({
    Name = "Team Check",
    Default = false,
    Callback = function(Value)
        _G.Settings.TeamCheck = Value
    end
})

-------------------------------------
sections.Esp2Section1:Header({
    Name = "ESP Chams"
})

_G.ChamsSettings = {
    Enabled = false,
    TeamCheck = false,
    RoleCheck = false,
    FillTransparency = 0.5,
    OutlineTransparency = 0,
    
    Colors = {
        Fill = Color3.fromRGB(0, 255, 0),
        Outline = Color3.fromRGB(255, 255, 255),
        Roles = {
            ["Murderer"] = Color3.fromRGB(255, 0, 0),
            ["Sheriff"] = Color3.fromRGB(0, 0, 255),
            ["Innocent"] = Color3.fromRGB(0, 255, 0),
        }
    },
    
    Animation = {
        Duration = 1,
        Style = Enum.EasingStyle.Sine,
        Direction = Enum.EasingDirection.Out
    }
}

-- Cache để lưu role của người chơi
local PlayerRoles = {}
local GameState = {
    IsRoundActive = false
}

local function MonitorGameState()
    local function checkRoundStatus()
        local murderer = false
        local sheriff = false
        
        for _, player in ipairs(Players:GetPlayers()) do
            if player.Character then
                local hasKnife = player.Character:FindFirstChild("Knife") or (player.Backpack and player.Backpack:FindFirstChild("Knife"))
                local hasGun = player.Character:FindFirstChild("Gun") or (player.Backpack and player.Backpack:FindFirstChild("Gun"))
                
                if hasKnife then murderer = true end
                if hasGun then sheriff = true end
            end
        end
        
        if not (murderer and sheriff) then
            if GameState.IsRoundActive then
                GameState.IsRoundActive = false
                -- Reset tất cả role khi round kết thúc
                table.clear(PlayerRoles)
            end
        else
            GameState.IsRoundActive = true
        end
    end
    
    RunService.Heartbeat:Connect(function()
        checkRoundStatus()
    end)
end

local function UpdatePlayerRole(Player)
    if game.PlaceId ~= 142823291 then return end
    if not GameState.IsRoundActive then return "Innocent" end
    
    local function CheckTools()
        if not Player.Character then return "Innocent" end
        if not Player.Backpack then return "Innocent" end
        
        -- Check trong character
        for _, tool in pairs(Player.Character:GetChildren()) do
            if tool:IsA("Tool") then
                if tool.Name == "Knife" then
                    return "Murderer"
                elseif tool.Name == "Gun" then
                    return "Sheriff"
                end
            end
        end
        
        -- Check trong backpack
        for _, tool in pairs(Player.Backpack:GetChildren()) do
            if tool:IsA("Tool") then
                if tool.Name == "Knife" then
                    return "Murderer"
                elseif tool.Name == "Gun" then
                    return "Sheriff"
                end
            end
        end
        
        return "Innocent"
    end
    
    PlayerRoles[Player.UserId] = CheckTools()
    
    -- Theo dõi thay đổi trong Backpack
        if not Player.Backpack:FindFirstChild("RoleMonitor") then
        local monitor = Instance.new("BoolValue")
        monitor.Name = "RoleMonitor"
        monitor.Parent = Player.Backpack
        
        Player.Backpack.ChildAdded:Connect(function(child)
            if child:IsA("Tool") and GameState.IsRoundActive then
                PlayerRoles[Player.UserId] = CheckTools()
            end
        end)
        
        Player.Backpack.ChildRemoved:Connect(function(child)
            if child:IsA("Tool") and GameState.IsRoundActive then
                PlayerRoles[Player.UserId] = CheckTools()
            end
        end)
    end
    
    if Player.Character and not Player.Character:FindFirstChild("RoleMonitor") then
        local monitor = Instance.new("BoolValue")
        monitor.Name = "RoleMonitor"
        monitor.Parent = Player.Character
        
        Player.Character.ChildAdded:Connect(function(child)
            if child:IsA("Tool") and GameState.IsRoundActive then
                PlayerRoles[Player.UserId] = CheckTools()
            end
        end)
        
        Player.Character.ChildRemoved:Connect(function(child)
            if child:IsA("Tool") and GameState.IsRoundActive then
                PlayerRoles[Player.UserId] = CheckTools()
            end
        end)
    end
end

MonitorGameState()

local function GetPlayerRole(Player)
    if game.PlaceId ~= 142823291 then return "Unknown" end
    
    if not PlayerRoles[Player.UserId] then
        UpdatePlayerRole(Player)
    end
    
    return PlayerRoles[Player.UserId] or "Innocent"
end

local function IsTeammate(Player)
    if not _G.ChamsSettings.TeamCheck then return false end
    return Player.Team == LocalPlayer.Team
end

local function ApplyChams(Player)
    local Connections = {}
    local CurrentHighlighter
    
    -- Khởi tạo role monitoring
    UpdatePlayerRole(Player)
    
    local function Setup(Character)
        if not Character then return end
        
        if CurrentHighlighter then
            CurrentHighlighter:Destroy()
        end
        
        local Highlighter = Instance.new("Highlight")
        CurrentHighlighter = Highlighter
        Highlighter.Parent = Character
        
        Highlighter.FillTransparency = 1
        Highlighter.OutlineTransparency = 1
        
        local AnimationInfo = TweenInfo.new(
            _G.ChamsSettings.Animation.Duration,
            _G.ChamsSettings.Animation.Style,
            _G.ChamsSettings.Animation.Direction
        )
        
        local FillTween = TweenService:Create(Highlighter, AnimationInfo, {
            FillTransparency = _G.ChamsSettings.FillTransparency
        })
        
        local OutlineTween = TweenService:Create(Highlighter, AnimationInfo, {
            OutlineTransparency = _G.ChamsSettings.OutlineTransparency
        })
        
        FillTween:Play()
        OutlineTween:Play()
        
        local function Update()
            if not _G.ChamsSettings.Enabled then
                Highlighter.Enabled = false
                return
            end
            
            if IsTeammate(Player) then
                Highlighter.Enabled = false
                return
            end
            
            Highlighter.Enabled = true
            
            if _G.ChamsSettings.RoleCheck then
                local role = GetPlayerRole(Player)
                local roleColor = _G.ChamsSettings.Colors.Roles[role]
                
                if roleColor then
                    Highlighter.FillColor = roleColor
                    Highlighter.OutlineColor = roleColor
                    print(Player.Name .. " is " .. role) -- Debug
                else
                    Highlighter.FillColor = _G.ChamsSettings.Colors.Fill
                    Highlighter.OutlineColor = _G.ChamsSettings.Colors.Outline
                end
            else
                Highlighter.FillColor = _G.ChamsSettings.Colors.Fill
                Highlighter.OutlineColor = _G.ChamsSettings.Colors.Outline
            end
        end
        
        local UpdateConnection = RunService.RenderStepped:Connect(Update)
        table.insert(Connections, UpdateConnection)
        
        -- Monitor character changes
        Character.ChildAdded:Connect(function(child)
            if child:IsA("Tool") then
                UpdatePlayerRole(Player)
            end
        end)
        
        Character.ChildRemoved:Connect(function(child)
            if child:IsA("Tool") then
                UpdatePlayerRole(Player)
            end
        end)
        
        local Humanoid = Character:FindFirstChild("Humanoid")
        if Humanoid then
            table.insert(Connections, Humanoid.Died:Connect(function()
                local FadeOutTween = TweenService:Create(Highlighter, TweenInfo.new(0.5), {
                    FillTransparency = 1,
                    OutlineTransparency = 1
                })
                FadeOutTween.Completed:Connect(function()
                    Highlighter:Destroy()
                end)
                FadeOutTween:Play()
            end))
        end
    end
    
    if Player.Character then
        Setup(Player.Character)
    end
    
    local CharacterAddedConnection = Player.CharacterAdded:Connect(function(Character)
        task.wait()
        Setup(Character)
    end)
    
    table.insert(Connections, CharacterAddedConnection)
    
    local function Cleanup()
        for _, Connection in pairs(Connections) do
            Connection:Disconnect()
        end
        if CurrentHighlighter then
            CurrentHighlighter:Destroy()
        end
        PlayerRoles[Player.UserId] = nil
    end
    
    table.insert(Connections, Player.AncestryChanged:Connect(function(_, parent)
        if not parent then
            Cleanup()
        end
    end))
    
    return Cleanup
end

-- UI Controls remain the same as before
sections.Esp2Section1:Toggle({
    Name = "Hiện Chams",
    Default = false,
    Callback = function(Value)
        _G.ChamsSettings.Enabled = Value
        
        if Value then
            for _, Player in pairs(Players:GetPlayers()) do
                if Player ~= LocalPlayer then
                    task.spawn(function()
                        task.wait(math.random() * 0.3)
                        ApplyChams(Player)
                    end)
                end
            end
            
            if not _G.ChamsInitialized then
                _G.ChamsInitialized = true
                Players.PlayerAdded:Connect(function(Player)
                    if Player ~= LocalPlayer then
                        ApplyChams(Player)
                    end
                end)
            end
        end
    end
})

sections.Esp2Section1:Toggle({
    Name = "Role Check",
    Default = false,
    Callback = function(Value)
        _G.ChamsSettings.RoleCheck = Value
    end
})

sections.Esp2Section1:Toggle({
    Name = "Team Check",
    Default = false,
    Callback = function(Value)
        _G.ChamsSettings.TeamCheck = Value
    end
})

sections.Esp2Section1:Colorpicker({
    Name = "Màu Chính Chams",
    Default = Color3.fromRGB(0, 255, 0),
    Callback = function(color)
        _G.ChamsSettings.Colors.Fill = color
    end
})

sections.Esp2Section1:Colorpicker({
    Name = "Màu Viền Chams",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(color)
        _G.ChamsSettings.Colors.Outline = color
    end
})

------------------------------------------
sections.Esp2Section1:Header({
	Name = "ESP Skeleton"
})

sections.Esp2Section1:Paragraph({
	Header = "Không Nên Bật Vì Nó Sẽ Gây Lag Máy Bạn: Chỉ Dùng Cho PC",
	Body = "Lag"
})


local SkeletonTeamCheck = false
local SkeletonColor = Color3.fromRGB(255, 255, 255)

sections.Esp2Section1:Toggle({
    Name = "Hiện Xương",
    Default = false,
    Callback = function(value)
        if value then
            RunService:BindToRenderStep("SkeletonESP", Enum.RenderPriority.Camera.Value, function()
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= LocalPlayer then
                        if SkeletonTeamCheck and player.Team == LocalPlayer.Team then
                            continue
                        end

                        local character = player.Character
                        if character then
                            for _, part in pairs(character:GetChildren()) do
                                if part:IsA("BasePart") then
                                    for _, motor in pairs(part:GetChildren()) do
                                        if motor:IsA("Motor6D") then
                                            local part0 = motor.Part0
                                            local part1 = motor.Part1
                                            
                                            if part0 and part1 then
                                                local p0 = workspace.CurrentCamera:WorldToViewportPoint(part0.Position)
                                                local p1 = workspace.CurrentCamera:WorldToViewportPoint(part1.Position)
                                                
                                                if p0.Z > 0 and p1.Z > 0 then
                                                    local line = Drawing.new("Line")
                                                    line.Visible = true
                                                    line.From = Vector2.new(p0.X, p0.Y)
                                                    line.To = Vector2.new(p1.X, p1.Y)
                                                    line.Color = SkeletonColor
                                                    line.Thickness = 1
                                                    line.Transparency = 1
                                                    
                                                    task.delay(1/60, function()
                                                        line:Remove()
                                                    end)
                                                end
                                            end
                                        end
                                    end
                                end
                            end
                        end
                    end
                end
            end)
        else
            RunService:UnbindFromRenderStep("SkeletonESP")
        end
    end,
}, "SkeletonESP")

sections.Esp2Section1:Toggle({
    Name = "Team Check",
    Default = false,
    Callback = function(value)
        SkeletonTeamCheck = value
    end,
}, "SkeletonTeamCheck")

sections.Esp2Section1:Colorpicker({
    Name = "Màu Xương",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(color)
        SkeletonColor = color
    end,
}, "SkeletonColor")
-------------------------------------
sections.EspSection1:Header({
	Name = "ESP Box"
})

-- ESP Settings
local ESP = {
    Enabled = false,
    TeamCheck = false,
    BoxColor = Color3.fromRGB(255, 255, 255),
    BoxThickness = 1.5,
    BoxTransparency = 1,
    CornerSize = 5,
    GlowColor = Color3.fromRGB(255, 255, 200),
    GlowThickness = 3.5,
    GlowTransparency = 0.5
}

-- Create ESP boxes table
local espBoxes = {}

-- ESP Function
local function CreateEsp(player)
    local Box = {}
    local Corners = {}
    local Glow = {}
    
    -- Create glow effect
    for i = 1, 12 do
        local glow = Drawing.new("Line")
        glow.Thickness = ESP.GlowThickness
        glow.Color = ESP.GlowColor
        glow.Transparency = ESP.GlowTransparency
        Glow[i] = glow
    end
    
    -- Create box lines
    for i = 1, 12 do
        local line = Drawing.new("Line")
        line.Thickness = ESP.BoxThickness
        line.Color = ESP.BoxColor
        line.Transparency = ESP.BoxTransparency
        if i <= 8 then
            Box[i] = line
        else
            Corners[i-8] = line
        end
    end
    
    -- Update ESP box
    local function UpdateEsp()
        local connection
        connection = RunService.RenderStepped:Connect(function()
            if not ESP.Enabled then
                for _, v in pairs(Box) do v.Visible = false end
                for _, v in pairs(Corners) do v.Visible = false end
                for _, v in pairs(Glow) do v.Visible = false end
                return
            end

            if ESP.TeamCheck and player.Team == LocalPlayer.Team then
                for _, v in pairs(Box) do v.Visible = false end
                for _, v in pairs(Corners) do v.Visible = false end
                for _, v in pairs(Glow) do v.Visible = false end
                return
            end

            if not player or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") or not player.Character:FindFirstChild("Humanoid") or player.Character.Humanoid.Health <= 0 then
                for _, v in pairs(Box) do v.Visible = false end
                for _, v in pairs(Corners) do v.Visible = false end
                for _, v in pairs(Glow) do v.Visible = false end
                return
            end

            local character = player.Character
            local HumanoidRootPart = character:FindFirstChild("HumanoidRootPart")
            local Head = character:FindFirstChild("Head")

            if HumanoidRootPart and Head then
                local Vector, OnScreen = Camera:WorldToViewportPoint(HumanoidRootPart.Position)
                
                if OnScreen then
                    local Size = (Camera:WorldToViewportPoint(Head.Position - Vector3.new(0, 3, 0)).Y - Camera:WorldToViewportPoint(Head.Position + Vector3.new(0, 2.5, 0)).Y) / 2
                    local BoxSize = Vector2.new(Size * 1, Size * 1.5)
                    
                    local Top1 = Vector2.new(Vector.X - BoxSize.X, Vector.Y - BoxSize.Y)
                    local Top2 = Vector2.new(Vector.X + BoxSize.X, Vector.Y - BoxSize.Y)
                    local Bottom1 = Vector2.new(Vector.X - BoxSize.X, Vector.Y + BoxSize.Y)
                    local Bottom2 = Vector2.new(Vector.X + BoxSize.X, Vector.Y + BoxSize.Y)
                    
                    -- Main box
                    Box[1].From = Top1
                    Box[1].To = Top2
                    Box[2].From = Bottom1
                    Box[2].To = Bottom2
                    Box[3].From = Top1
                    Box[3].To = Bottom1
                    Box[4].From = Top2
                    Box[4].To = Bottom2
                    
                    -- Glow effect
                    Glow[1].From = Top1
                    Glow[1].To = Top2
                    Glow[2].From = Bottom1
                    Glow[2].To = Bottom2
                    Glow[3].From = Top1
                    Glow[3].To = Bottom1
                    Glow[4].From = Top2
                    Glow[4].To = Bottom2
                    
                    -- Corner decorations
                    Corners[1].From = Top1
                    Corners[1].To = Top1 + Vector2.new(ESP.CornerSize, 0)
                    Corners[2].From = Top1
                    Corners[2].To = Top1 + Vector2.new(0, ESP.CornerSize)
                    Corners[3].From = Top2
                    Corners[3].To = Top2 + Vector2.new(-ESP.CornerSize, 0)
                    Corners[4].From = Top2
                    Corners[4].To = Top2 + Vector2.new(0, ESP.CornerSize)
                    
                    -- Update colors
                    for _, v in pairs(Box) do
                        v.Visible = true
                        v.Color = ESP.BoxColor
                    end
                    for _, v in pairs(Corners) do
                        v.Visible = true
                        v.Color = ESP.BoxColor
                    end
                    for _, v in pairs(Glow) do
                        v.Visible = true
                        v.Color = ESP.GlowColor
                    end
                else
                    for _, v in pairs(Box) do v.Visible = false end
                    for _, v in pairs(Corners) do v.Visible = false end
                    for _, v in pairs(Glow) do v.Visible = false end
                end
            end
        end)
    end
    
    UpdateEsp()
    espBoxes[player] = {Box = Box, Corners = Corners, Glow = Glow}
end

-- UI Section
sections.EspSection1:Toggle({
    Name = "Hiện Khung Viền",
    Default = false,
    Callback = function(value)
        ESP.Enabled = value
    end
})

sections.EspSection1:Toggle({
    Name = "Team Check",
    Default = false,
    Callback = function(value)
        ESP.TeamCheck = value
    end
})

sections.EspSection1:Colorpicker({
    Name = "Màu Khung Viền",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(color)
        ESP.BoxColor = color
    end
})

-- Create ESP for existing players
for _, player in pairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        CreateEsp(player)
    end
end

-- Handle new players joining
Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        CreateEsp(player)
    end
end)

-- Handle players leaving
Players.PlayerRemoving:Connect(function(player)
    if espBoxes[player] then
        for _, v in pairs(espBoxes[player].Box) do v:Remove() end
        for _, v in pairs(espBoxes[player].Corners) do v:Remove() end
        for _, v in pairs(espBoxes[player].Glow) do v:Remove() end
        espBoxes[player] = nil
    end
end)
-----------------------------------
sections.AimSection1:Header({
    Name = "Aimbot Cơ Bản"
})

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local AimbotEnabled = false
local FOVHidden = false
local FOVSize = 200
local AimbotTarget = "Head"
local AimbotRange = 50
local FOVColor = Color3.fromRGB(255, 255, 255)
local TeamCheck = false
local WallCheck = false
local LockedPlayer = nil
local LockEnabled = false
local AimbotSpeed = 100

-- Tạo FOV Circle
local fovCircle = Drawing.new("Circle")
fovCircle.Thickness = 1
fovCircle.NumSides = 100
fovCircle.Radius = FOVSize
fovCircle.Filled = false
fovCircle.Visible = false
fovCircle.ZIndex = 999
fovCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
fovCircle.Color = FOVColor

-- Hàm kiểm tra team
local function IsTeammate(player)
    if not TeamCheck then return false end
    return player.Team == LocalPlayer.Team
end

-- Hàm kiểm tra tầm nhìn
local function IsVisible(targetPart)
    if not WallCheck then return true end
    
    local origin = Camera.CFrame.Position
    local targetPos = targetPart.Position
    local direction = (targetPos - origin)
    local distance = direction.Magnitude
    direction = direction.Unit
    
    local rayParams = RaycastParams.new()
    rayParams.FilterType = Enum.RaycastFilterType.Blacklist
    rayParams.FilterDescendantsInstances = {LocalPlayer.Character, targetPart.Parent}
    
    local result = workspace:Raycast(origin, direction * distance, rayParams)
    return result == nil
end

local function IsInFOV(position)
    local screenPos, onScreen = Camera:WorldToScreenPoint(position)
    if not onScreen then return false end
    
    local screenCenter = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    local screenPosition = Vector2.new(screenPos.X, screenPos.Y)
    return (screenCenter - screenPosition).Magnitude <= FOVSize
end

local function GetClosestPlayerInFOV()
    local closestPlayer = nil
    local shortestDistance = math.huge
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and 
           player.Character and 
           player.Character:FindFirstChild("HumanoidRootPart") and 
           player.Character:FindFirstChild("Humanoid") and 
           player.Character.Humanoid.Health > 0 and
           not IsTeammate(player) then
            
            local targetPart = player.Character:FindFirstChild(AimbotTarget)
            if targetPart then
                local distance = (LocalPlayer.Character.HumanoidRootPart.Position - targetPart.Position).Magnitude
                
                if distance <= AimbotRange and IsVisible(targetPart) and IsInFOV(targetPart.Position) then
                    if distance < shortestDistance then
                        closestPlayer = player
                        shortestDistance = distance
                    end
                end
            end
        end
    end
    
    return closestPlayer
end

local function GetClosestPlayerWhenNoLock()
    if LockEnabled and LockedPlayer and 
       LockedPlayer.Character and 
       LockedPlayer.Character:FindFirstChild("Humanoid") and 
       LockedPlayer.Character.Humanoid.Health > 0 then
        return LockedPlayer
    end
    
    return GetClosestPlayerInFOV()
end

-- Hàm cập nhật
local function UpdateFOVVisibility()
    fovCircle.Visible = AimbotEnabled and not FOVHidden
end

local function UpdateFOVPosition()
    fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
end

local function UpdateFOVColor(color)
    FOVColor = color
    fovCircle.Color = color
end

local function UpdateFOVSize(size)
    FOVSize = size
    fovCircle.Radius = size
end

-- Mobile Aimbot Loop
RunService:BindToRenderStep("MobileAimbot", 1, function()
    UpdateFOVPosition()
    UpdateFOVVisibility()
    
    if not AimbotEnabled then return end
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then return end
    
    local target
    if LockEnabled then
        if LockedPlayer and 
           LockedPlayer.Character and 
           LockedPlayer.Character:FindFirstChild("Humanoid") and 
           LockedPlayer.Character.Humanoid.Health > 0 then
            target = LockedPlayer
        else
            LockedPlayer = GetClosestPlayerInFOV()
            target = LockedPlayer
        end
    else
        target = GetClosestPlayerInFOV()
    end
    
    if target and target.Character and target.Character:FindFirstChild(AimbotTarget) then
        local targetPos = target.Character[AimbotTarget].Position
        local targetCFrame = CFrame.new(Camera.CFrame.Position, targetPos)
        
        -- Tính toán độ mượt dựa trên AimbotSpeed
        local smoothness = math.clamp(1 - (AimbotSpeed / 500), 0, 0.99)
        Camera.CFrame = Camera.CFrame:Lerp(targetCFrame, 1 - smoothness)
    end
end)

-- Events for character
LocalPlayer.CharacterAdded:Connect(function()
    UpdateFOVVisibility()
end)

LocalPlayer.CharacterRemoving:Connect(function()
    UpdateFOVVisibility()
end)

-- Viewport size changed
Camera:GetPropertyChangedSignal("ViewportSize"):Connect(UpdateFOVPosition)

-- Toggle functions
local function ToggleAimbot()
    AimbotEnabled = not AimbotEnabled
    UpdateFOVVisibility()
end

local function ToggleFOV()
    FOVHidden = not FOVHidden
    UpdateFOVVisibility()
end

-- Toggle Aimbot
sections.AimSection1:Toggle({
    Name = "Bật/Tắt Aimbot",
    Default = false,
    Callback = function(value)
        AimbotEnabled = value
        UpdateFOVVisibility()
        Window:Notify({
            Title = "Aimbot đã được " .. (value and "Bật" or "Tắt")
        })
    end
}, "AimbotToggle")

-- Toggle FOV
sections.AimSection1:Toggle({
    Name = "Ẩn Vòng Tròn",
    Default = false,
    Callback = function(value)
        FOVHidden = value
        UpdateFOVVisibility()
        Window:Notify({
            Title = "Vòng tròn đã được " .. (value and "Ẩn" or "Hiện")
        })
    end
}, "FOVToggle")

-- Toggle TeamCheck
sections.AimSection1:Toggle({
    Name = "Kiểm Tra Đồng Đội",
    Default = false,
    Callback = function(value)
        TeamCheck = value
        Window:Notify({
            Title = "Kiểm tra đồng đội đã được " .. (value and "Bật" or "Tắt")
        })
    end
}, "TeamCheckToggle")

sections.AimSection1:Toggle({
    Name = "Kiểm Tra Vật Cản",
    Default = false,
    Callback = function(value)
        WallCheck = value
        Window:Notify({
            Title = "Wall Check đã được " .. (value and "Bật" or "Tắt")
        })
    end
}, "WallCheckToggle")

-- Slider FOV Size
sections.AimSection1:Slider({
    Name = "Size Fov",
    Default = 200,
    Minimum = 50,
    Maximum = 500,
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        UpdateFOVSize(Value)
        Window:Notify({
            Title = "Kích thước vòng tròn: " .. Value
        })
    end
}, "FOVSizeSlider")

-- Dropdown Aimbot Target
sections.Aim2Section1:Dropdown({
    Name = "Vị Trí Nhắm",
    Multi = false,
    Required = true,
    Options = {"Đầu", "Thân"},
    Default = 1,
    Callback = function(Value)
        AimbotTarget = Value == "Đầu" and "Head" or "HumanoidRootPart"
        Window:Notify({
            Title = "Đã chọn nhắm vào: " .. Value
        })
    end
}, "AimbotTargetDropdown")

-- Dropdown Aimbot Range
sections.Aim2Section1:Dropdown({
    Name = "Tầm Xa Aimbot",
    Multi = false,
    Required = true,
    Options = {
        "Mức 1 (50m)",
        "Mức 2 (100m)",
        "Mức 3 (150m)",
        "Mức 4 (300m)",
        "Mức 5 (1000m)"
    },
    Default = 1,
    Callback = function(Value)
        local ranges = {
            ["Mức 1 (50m)"] = 50,
            ["Mức 2 (100m)"] = 100,
            ["Mức 3 (150m)"] = 150,
            ["Mức 4 (300m)"] = 300,
            ["Mức 5 (1000m)"] = 1000
        }
        AimbotRange = ranges[Value]
        Window:Notify({
            Title = "Tầm xa aimbot: " .. Value
        })
    end
}, "AimbotRangeDropdown")

-- Thêm vào phần sections.AimSection1
sections.Aim2Section1:Slider({
    Name = "Khoảng Cách Aim",
    Default = 50,
    Minimum = 0,
    Maximum = 5000,
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        AimbotRange = Value -- Cập nhật giá trị AimbotRange
        print("Aimbot Range set to: " .. Value .. "m")
    end
})

sections.Aim2Section1:Toggle({
    Name = "Khóa Người",
    Default = false,
    Callback = function(value)
        LockEnabled = value
        if not value then
            LockedPlayer = nil -- Reset locked player khi tắt lock mode
        end
        Window:Notify({
            Title = "Lock Mode đã được " .. (value and "Bật" or "Tắt")
        })
    end
}, "LockModeToggle")

-- Thêm Button để Reset Lock Target (nếu muốn)
----sections.AimSection1:Button({
   --- Name = "Cài Lại Khóa",
   --- Callback = function()
        ---LockedPlayer = nil
        ---Window:Notify({
            ---Title = "Đã reset locked target"
       -- })
    --end
--})

sections.Aim2Section1:Slider({
    Name = "Tốc Độ Aimbot",
    Default = 100,
    Minimum = 1,
    Maximum = 500,
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        AimbotSpeed = Value
        Window:Notify({
            Title = "Tốc độ aimbot: " .. Value
        })
    end
}, "AimbotSpeedSlider")


-- Colorpicker FOV Color
sections.Aim2Section1:Colorpicker({
    Name = "Màu Vòng Tròn",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(color)
        UpdateFOVColor(color)
        Window:Notify({
            Title = "Đã cập nhật màu vòng tròn"
        })
    end,
}, "FOVColorpicker")

-- Cleanup
LocalPlayer.CharacterRemoving:Connect(function()
    gui:Destroy()
end)

-------------------------------------
sections.ShaSection1:Header({
	Name = "Lighting"
})

local lighting = game:GetService("Lighting")
local defaultBrightness = lighting.Brightness
local defaultExposure = lighting.ExposureCompensation
local defaultAmbient = lighting.Ambient

sections.ShaSection1:Slider({
    Name = "Độ Sáng Chiếu Sáng",
    Default = 0,
    Minimum = 0,
    Maximum = 100,
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        if Value == 0 then
            lighting.Brightness = defaultBrightness
            lighting.ExposureCompensation = defaultExposure
            lighting.Ambient = defaultAmbient
        else
            lighting.Brightness = Value/25
            lighting.ExposureCompensation = Value/20
            lighting.Ambient = Color3.fromRGB(255, 255, 255):Lerp(Color3.fromRGB(255, 255, 255), Value/100)
        end
        
        Window:Notify({
            Title = Window.Settings.Title
        })
    end
}, "Slider")

local lighting = game:GetService("Lighting")
local defaultBrightness = lighting.Brightness
local defaultClockTime = lighting.ClockTime
local defaultFogEnd = lighting.FogEnd
local defaultGlobalShadows = lighting.GlobalShadows
local defaultOutdoorAmbient = lighting.OutdoorAmbient

sections.ShaSection1:Toggle({
    Name = "Không Tối",
    Default = false,
    Callback = function(Value)
        if Value then
            -- Bật Full Bright
            lighting.Brightness = 2
            lighting.ClockTime = 14
            lighting.FogEnd = 100000
            lighting.GlobalShadows = false
            lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        else
            -- Tắt Full Bright, quay về giá trị mặc định
            lighting.Brightness = defaultBrightness
            lighting.ClockTime = defaultClockTime
            lighting.FogEnd = defaultFogEnd
            lighting.GlobalShadows = defaultGlobalShadows
            lighting.OutdoorAmbient = defaultOutdoorAmbient
        end
        
        Window:Notify({
            Title = Window.Settings.Title
        })
    end
}, "Toggle")
------------------------------------------------------
sections.UniSection1:Header({
	Name = "Universal"
})
-- Phiên bản bypass anti-cheat cơ bản
local speedEnabled = false
local speedValue = 50

-- Bypass basic anti-cheat
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(...)
    local args = {...}
    local method = getnamecallmethod()
    
    if method == "FireServer" or method == "InvokeServer" then
        -- Chặn các remote events liên quan đến speed/movement
        if args[1] == "WalkSpeed" or args[1] == "Speed" or args[1] == "CheckSpeed" then
            return
        end
    end
    return old(...)
end)
setreadonly(mt, true)

-- Sử dụng CFrame movement thay vì WalkSpeed
local function speedBypass()
    if speedEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        local moveDirection = player.Character.Humanoid.MoveDirection
        
        if moveDirection.Magnitude > 0 then
            -- Tăng hệ số chia để giảm độ nhạy của tốc độ cao
            humanoidRootPart.CFrame = humanoidRootPart.CFrame + moveDirection * (speedValue/1000)
        end
    end
end

RunService.Heartbeat:Connect(speedBypass)

sections.UniSection1:Toggle({
    Name = "Speed Hack",
    Default = false,
    Callback = function(value)
        speedEnabled = value
        Window:Notify({
            Title = "Speed Bypass",
            Content = value and "Enabled" or "Disabled"
        })
    end
}, "SpeedToggle")

sections.UniSection1:Slider({
    Name = "Tốc Độ Speed",
    Default = 50,
    Minimum = 16,
    Maximum = 5000, -- Đã tăng maximum lên 5000
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        speedValue = Value
    end
}, "SpeedSlider")

-------------------------
local jumpEnabled = false 
local jumpValue = 50
local defaultJump = 50
local currentMode = "JumpPower"

-- Basic anti-cheat bypass
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(...)
    local args = {...}
    local method = getnamecallmethod()
    if method == "FireServer" or method == "InvokeServer" then
        if args[1] == "JumpPower" or args[1] == "JP" or args[1] == "JumpHeight" then
            return
        end
    end
    return old(...)
end)
setreadonly(mt, true)

-- Function để set jump value dựa theo mode
local function setJumpValue(humanoid, value)
    if currentMode == "JumpPower" then
        humanoid.JumpPower = value
        humanoid.UseJumpPower = true
    else
        humanoid.JumpHeight = value/2.5
    end
end

sections.UniSection1:Dropdown({
    Name = "Mode Nhảy",
    Multi = false,
    Required = true,
    Options = {"JumpPower", "JumpHeight"},
    Default = 1,
    Callback = function(Value)
        currentMode = Value
        if jumpEnabled and player.Character and player.Character:FindFirstChild("Humanoid") then
            setJumpValue(player.Character.Humanoid, jumpValue)
        end
        Window:Notify({
            Title = "Jump Mode",
            Content = "Changed to " .. Value
        })
    end
}, "JumpModeDropdown")

sections.UniSection1:Toggle({
    Name = "Nhảy Cao",
    Default = false,
    Callback = function(value)
        jumpEnabled = value
        if value then
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                setJumpValue(player.Character.Humanoid, jumpValue)
            end
            Window:Notify({
                Title = "Jump Changer",
                Content = "Enabled"
            })
        else
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                setJumpValue(player.Character.Humanoid, defaultJump)
            end
            Window:Notify({
                Title = "Jump Changer",
                Content = "Disabled"
            })
        end
    end
}, "JumpToggle")

sections.UniSection1:Slider({
    Name = "Sức Nhảy",
    Default = 50,
    Minimum = 50,
    Maximum = 5000, -- Tăng maximum lên 5000
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        jumpValue = Value
        if jumpEnabled then
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                setJumpValue(player.Character.Humanoid, Value)
            end
        end
    end
}, "JumpSlider")

-- Handle character respawn
player.CharacterAdded:Connect(function(character)
    local humanoid = character:WaitForChild("Humanoid")
    if jumpEnabled then
        setJumpValue(humanoid, jumpValue)
    end
end)
-----------------------------
sections.Uni2Section1:Paragraph({
	Header = "Ấn Giữ Nút Nhảy Hoặc Bấm Liên Tục Nút Nhảy",
	Body = "Có Thể Dùng Kết Hợp Nhảy Cao"
})

local infiniteJumpEnabled = false

-- Basic anti-cheat bypass
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(...)
    local args = {...}
    local method = getnamecallmethod()
    if method == "FireServer" or method == "InvokeServer" then
        if args[1] == "JumpRequest" or args[1] == "Jump" then
            return
        end
    end
    return old(...)
end)
setreadonly(mt, true)

-- Infinite Jump Logic
UserInputService.JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end
end)

sections.Uni2Section1:Toggle({
    Name = "Nhảy Liên Tục",
    Default = false,
    Callback = function(value)
        infiniteJumpEnabled = value
        Window:Notify({
            Title = "Infinite Jump",
            Content = value and "Enabled" or "Disabled"
        })
    end
}, "InfiniteJumpToggle")
-------------------------------------
local noclipEnabled = false

-- Basic anti-cheat bypass
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(...)
    local args = {...}
    local method = getnamecallmethod()
    if method == "FireServer" or method == "InvokeServer" then
        if args[1] == "Noclip" or args[1] == "Phase" then
            return
        end
    end
    return old(...)
end)
setreadonly(mt, true)

-- Noclip Function
local function noclipRun()
    if noclipEnabled then
        if player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end
end

-- Connect noclip to RunService
RunService.Stepped:Connect(noclipRun)

-- Noclip Toggle
sections.Uni2Section1:Toggle({
    Name = "Xuyên Tường",
    Default = false,
    Callback = function(value)
        noclipEnabled = value
        
        if value then
            -- Khi bật Noclip
            if player.Character then
                for _, part in pairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        else
            -- Khi tắt Noclip
            if player.Character then
                for _, part in pairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                        part.CanCollide = true
                    end
                end
            end
        end

        Window:Notify({
            Title = "Noclip",
            Content = value and "Enabled" or "Disabled"
        })
    end
}, "NoclipToggle")

-- Handle character respawn
player.CharacterAdded:Connect(function(character)
    if noclipEnabled then
        wait(0.5) -- Đợi character load
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)
-------------------------------
local floatEnabled = false
local floatHeight = 100 -- Độ cao mặc định khi bật Float

-- Basic anti-cheat bypass
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(...)
    local args = {...}
    local method = getnamecallmethod()
    if method == "FireServer" or method == "InvokeServer" then
        if args[1] == "Float" or args[1] == "Hover" then
            return
        end
    end
    return old(...)
end)
setreadonly(mt, true)

-- Float function
local function updateFloat()
    if floatEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = player.Character.HumanoidRootPart
        local currentX = humanoidRootPart.Position.X
        local currentZ = humanoidRootPart.Position.Z
        
        humanoidRootPart.Velocity = Vector3.new(humanoidRootPart.Velocity.X, 0, humanoidRootPart.Velocity.Z)
        humanoidRootPart.CFrame = CFrame.new(currentX, floatHeight, currentZ)
    end
end

RunService.Heartbeat:Connect(updateFloat)

-- Float Toggle
sections.UniSection1:Toggle({
    Name = "Lơ Lưng Trên Không",
    Default = false,
    Callback = function(value)
        floatEnabled = value
        if value then
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local humanoidRootPart = player.Character.HumanoidRootPart
                local currentX = humanoidRootPart.Position.X
                local currentZ = humanoidRootPart.Position.Z
                humanoidRootPart.CFrame = CFrame.new(currentX, floatHeight, currentZ)
            end
        end
        Window:Notify({
            Title = "Float",
            Content = value and "Enabled" or "Disabled"
        })
    end
}, "FloatToggle")

-- Slider mới
sections.UniSection1:Slider({
    Name = "Độ Cao",
    Default = 100,
    Minimum = 50,
    Maximum = 500,
    DisplayMethod = "Percent",
    Precision = 0,
    Callback = function(Value)
        floatHeight = Value
        if floatEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local humanoidRootPart = player.Character.HumanoidRootPart
            local currentX = humanoidRootPart.Position.X
            local currentZ = humanoidRootPart.Position.Z
            humanoidRootPart.CFrame = CFrame.new(currentX, floatHeight, currentZ)
        end
    end
})

-- Handle character respawn
player.CharacterAdded:Connect(function(character)
    if floatEnabled then
        wait(0.5)
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        local currentX = humanoidRootPart.Position.X
        local currentZ = humanoidRootPart.Position.Z
        humanoidRootPart.CFrame = CFrame.new(currentX, floatHeight, currentZ)
    end
end)

--------------------
local gravityEnabled = false
local gravityValue = 196.2

-- Basic anti-cheat bypass
local mt = getrawmetatable(game)
setreadonly(mt, false)
local old = mt.__namecall
mt.__namecall = newcclosure(function(...)
    local args = {...}
    local method = getnamecallmethod()
    if method == "FireServer" or method == "InvokeServer" then
        if args[1] == "Gravity" or args[1] == "GravityChange" then
            return
        end
    end
    return old(...)
end)
setreadonly(mt, true)

-- Gravity function
local function updateGravity()
    if gravityEnabled and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        local hrp = player.Character.HumanoidRootPart
        
        if humanoid and hrp then
            -- Chỉ áp dụng gravity khi rơi (velocity.Y < 0)
            local currentVelocity = hrp.Velocity
            if currentVelocity.Y < 0 and humanoid.FloorMaterial == Enum.Material.Air then
                -- Điều chỉnh tốc độ rơi
                local fallSpeed = (currentVelocity.Y * gravityValue) / 196.2
                hrp.Velocity = Vector3.new(
                    currentVelocity.X,
                    fallSpeed,
                    currentVelocity.Z
                )
            end
        end
    end
end

-- Kết nối với RunService
RunService.Heartbeat:Connect(updateGravity)

-- Gravity Toggle
sections.Uni2Section1:Toggle({
    Name = "Trọng Lực",
    Default = false,
    Callback = function(value)
        gravityEnabled = value
        Window:Notify({
            Title = "Trọng Lực",
            Content = value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "GravityToggle")

-- Gravity Slider
sections.Uni2Section1:Slider({
    Name = "Điều Chỉnh Trọng Lực",
    Default = 50,
    Minimum = 0,
    Maximum = 100,
    DisplayMethod = "Percent",
    Precision = 0,
    Callback = function(Value)
        -- Chuyển đổi giá trị phần trăm thành giá trị gravity thực tế
        gravityValue = math.floor((100 - Value) * 1.962)
        print("Đã thay đổi trọng lực thành: " .. gravityValue)
    end
}, "GravitySlider")

-- Preset Gravity
sections.Uni2Section1:Dropdown({
    Name = "Mức Trọng Lực",
    Default = 1,
    Options = {
        "Rơi Chậm (25)",
        "Rơi Vừa (100)",
        "Rơi Nhanh (196)",
    },
    Callback = function(Value)
        local gravityMap = {
            ["Rơi Chậm (25)"] = 25,
            ["Rơi Vừa (100)"] = 100,
            ["Rơi Nhanh (196)"] = 196,
        }
        gravityValue = gravityMap[Value]
    end
}, "GravityPreset")

-- Handle character respawn
player.CharacterAdded:Connect(function()
    if gravityEnabled then
        wait(0.5) -- Đợi character load
    end
end)
-------------------------
local antiAFKEnabled = false

-- Toggle UI với tên tiếng Việt
sections.Uni2Section1:Toggle({
    Name = "Chống AFK",
    Default = false,
    Callback = function(Value)
        antiAFKEnabled = Value
        
        if antiAFKEnabled then
            -- Khi người chơi bị kick vì AFK
            Players.LocalPlayer.Idled:Connect(function()
                if antiAFKEnabled then
                    -- Giả lập click chuột để tránh AFK
                    VirtualUser:Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
                    wait(0.1)
                    VirtualUser:Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
                end
            end)
        end
        
        Window:Notify({
            Title = "Chống AFK",
            Content = Value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "AntiAFKToggle")
-----------------------------
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local camera = workspace.CurrentCamera
local defaultFOV = 70
local fovEnabled = false
local currentFOV = 70

-- FOV Toggle
sections.Uni2Section1:Toggle({
    Name = "FOV Changer",
    Default = false,
    Callback = function(Value)
        fovEnabled = Value
        if Value then
            -- Lưu FOV mặc định
            defaultFOV = camera.FieldOfView
            camera.FieldOfView = currentFOV
        else
            -- Khôi phục FOV mặc định
            camera.FieldOfView = defaultFOV
        end
        
        Window:Notify({
            Title = "Phạm Vi Quan Sát",
            Content = Value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "FOVToggle")

-- FOV Slider
sections.Uni2Section1:Slider({
    Name = "Điều Chỉnh Tầm",
    Default = 70,
    Minimum = 1,
    Maximum = 120,
    DisplayMethod = "Value",
    Precision = 0,
    Callback = function(Value)
        currentFOV = Value
        if fovEnabled then
            camera.FieldOfView = Value
        end
    end
}, "FOVSlider")

-- Để đảm bảo FOV luôn được cập nhật
RunService.RenderStepped:Connect(function()
    if fovEnabled then
        camera.FieldOfView = currentFOV
    end
end)
---------------------
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local clickTpEnabled = false

-- Click Teleport Toggle
sections.Uni2Section1:Toggle({
    Name = "Click Teleport",
    Default = false,
    Callback = function(Value)
        clickTpEnabled = Value
        
        Window:Notify({
            Title = "Click Teleport",
            Content = Value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "ClickTpToggle")

-- Xử lý click teleport
UserInputService.TouchTap:Connect(function(touchPositions)
    if clickTpEnabled and #touchPositions > 0 then
        local ray = workspace.CurrentCamera:ScreenPointToRay(touchPositions[1].X, touchPositions[1].Y)
        local raycastResult = workspace:Raycast(ray.Origin, ray.Direction * 1000)
        
        if raycastResult then
            local character = Players.LocalPlayer.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                character.HumanoidRootPart.CFrame = CFrame.new(raycastResult.Position)
            end
        end
    end
end)
------------------
local Players = game:GetService("Players")
local UserSettings = UserSettings()
local GameSettings = UserSettings.GameSettings

-- Unlock Camera Distance Toggle
sections.Uni2Section1:Toggle({
    Name = "Mở Giới Hạn Camera",
    Default = false,
    Callback = function(Value)
        if Value then
            -- Tắt giới hạn camera của Roblox
            if GameSettings.CameraMode == Enum.CameraMode.LockFirstPerson then
                GameSettings.CameraMode = Enum.CameraMode.Classic
            end
            
            -- Set khoảng cách camera tối đa
            Players.LocalPlayer.CameraMaxZoomDistance = math.huge
            GameSettings.CameraZoomSensitivity = 150
            
            -- Đảm bảo luôn được zoom xa
            spawn(function()
                while Value do
                    wait()
                    Players.LocalPlayer.CameraMaxZoomDistance = math.huge
                    Players.LocalPlayer.CameraMinZoomDistance = 0.5
                end
            end)
        else
            -- Reset về mặc định
            Players.LocalPlayer.CameraMaxZoomDistance = 400
            Players.LocalPlayer.CameraMinZoomDistance = 0.5
            GameSettings.CameraZoomSensitivity = 100
        end
        
        Window:Notify({
            Title = "Mở Giới Hạn Camera",
            Content = Value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "UnlockCamDistToggle")
---------------------
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Invisicam Toggle
sections.UniSection1:Toggle({
    Name = "Camera Xuyên Tường",
    Default = false,
    Callback = function(Value)
        if LocalPlayer.DevEnableMouseLock then        
            LocalPlayer.DevEnableMouseLock = false
        end
        
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.UseJumpPower = false
        end
        
        LocalPlayer.DevCameraOcclusionMode = Value and "Invisicam" or "Zoom"
        
        Window:Notify({
            Title = "Camera Xuyên Tường",
            Content = Value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "InvisicamToggle")
---------------------
sections.Uni2Section1:Paragraph({
	Header = "Máy Yếu Bị Cân Nhắc Bật",
	Body = "X-Ray Dễ Dàng Nhìn Xuyên"
})

local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local xrayEnabled = false

-- X-Ray Toggle
sections.UniSection1:Toggle({
    Name = "Nhìn Xuyên Tường",
    Default = false,
    Callback = function(Value)
        xrayEnabled = Value
        
        if Value then
            -- Thực hiện X-Ray
            RunService:BindToRenderStep("XRay", 2000, function()
                for _, v in pairs(workspace:GetDescendants()) do
                    if v:IsA("BasePart") then
                        -- Kiểm tra xem part có thuộc Player không
                        local isPlayerPart = false
                        for _, player in pairs(Players:GetPlayers()) do
                            if player.Character and v:IsDescendantOf(player.Character) then
                                isPlayerPart = true
                                break
                            end
                        end
                        
                        -- Chỉ áp dụng transparency cho các part không thuộc Player
                        if not isPlayerPart then
                            v.LocalTransparencyModifier = 0.7
                        end
                    end
                end
            end)
        else
            -- Tắt X-Ray
            RunService:UnbindFromRenderStep("XRay")
            for _, v in pairs(workspace:GetDescendants()) do
                if v:IsA("BasePart") then
                    v.LocalTransparencyModifier = 0
                end
            end
        end
        
        Window:Notify({
            Title = "Nhìn Xuyên Tường",
            Content = Value and "Đã Bật" or "Đã Tắt"
        })
    end
}, "XRayToggle")
------------------------
sections.PlaSection1:Paragraph({
	Header = "Miễn Phí Toàn Bộ Animation",
	Body = "Người Khác Vẫn Thấy: Tận Hưởng Vui Vẻ"
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

local animations = {
    ["Default"] = {}, -- Thêm Default animation trống
    ["Confident"] = {
        idle = 1069977950,
        secondIdle = 1069987858,
        walk = 1070017263,
        run = 1070001516,
        swim = 1070009914,
        jump = 1069984524,
        swimIdle = 1070012133,
        climb = 1069946257,
        fall = 1069973677
    },
    ["Popstar"] = {
        idle = 1212900985,
        secondIdle = 1212954651,
        walk = 1212980338,
        run = 1212980348,
        swim = 1212852603,
        jump = 1069984524,
        swimIdle = 1212998578,
        climb = 1213044953,
        fall = 1212900995
    },
    ["Patrol"] = {
        idle = 1149612882,
        secondIdle = 1150842221,
        walk = 1151231493,
        run = 1150967949,
        swim = 1151204998,
        jump = 1150944216,
        swimIdle = 1151221899,
        climb = 1148811837,
        fall = 1148863382
    },
    ["Sneaky"] = {
        idle = 1132473842,
        secondIdle = 1132477671,
        walk = 1132510133,
        run = 1132494274,
        swim = 1132500520,
        jump = 1132489853,
        swimIdle = 1132506407,
        climb = 1132461372,
        fall = 1132469004
    },
    ["Princess"] = {
        idle = 941003647,
        secondIdle = 941013098,
        walk = 941028902,
        run = 941015281,
        swim = 941018893,
        jump = 941008832,
        swimIdle = 941025398,
        climb = 940996062,
        fall = 941000007
    },
    ["Cowboy"] = {
        idle = 1014390418,
        secondIdle = 1014398616,
        walk = 1014421541,
        run = 1014401683,
        swim = 1014406523,
        jump = 1014394726,
        swimIdle = 1014411816,
        climb = 1014380606,
        fall = 1014384571
    },
    ["Stylized Female"] = {
        idle = 4708191566,
        secondIdle = 4708192150,
        walk = 4708193840,
        run = 4708192705,
        swim = 4708189360,
        jump = 4708188025,
        swimIdle = 4708190607,
        climb = 4708184253,
        fall = 4708186162
    }
}

local currentAnimator = nil
local updateConnection = nil
local idleTimer = 0
local isSecondIdle = false
local selectedAnimation = "Default" -- Đặt Default là mặc định

local function cleanupAnimations()
    if updateConnection then
        updateConnection:Disconnect()
        updateConnection = nil
    end
    
    if currentAnimator then
        for _, track in pairs(currentAnimator:GetPlayingAnimationTracks()) do
            track:Stop()
            track:Destroy()
        end
        currentAnimator:Destroy()
        currentAnimator = nil
    end
end

local function changeAnimation(animName)
    if animName == "Default" then
        cleanupAnimations()
        return
    end

    selectedAnimation = animName
    
    local character = LocalPlayer.Character
    local humanoid = character and character:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    cleanupAnimations()
    
    currentAnimator = Instance.new("Animator")
    currentAnimator.Parent = humanoid
    
    local animTracks = {}
    local currentAnim = "idle"
    local animSet = animations[animName]
    
    local function loadAnim(id, name)
        local animation = Instance.new("Animation")
        animation.AnimationId = "rbxassetid://" .. id
        animation.Name = name
        
        local success, animTrack = pcall(function()
            return currentAnimator:LoadAnimation(animation)
        end)
        
        if success then
            animTrack.Looped = (name == "idle" or name == "secondIdle" or 
                              name == "walk" or name == "run" or 
                              name == "swim" or name == "swimIdle" or 
                              name == "climb")
            
            animTrack.Priority = name == "idle" and Enum.AnimationPriority.Idle 
                or (name == "jump" or name == "fall") and Enum.AnimationPriority.Action 
                or Enum.AnimationPriority.Movement
        end
        
        return animTrack
    end
    
    for name, id in pairs(animSet) do
        animTracks[name] = loadAnim(id, name)
    end
    
    local function playAnimation(name)
        if currentAnim == name then return end
        
        if animTracks[currentAnim] and animTracks[currentAnim].IsPlaying then
            animTracks[currentAnim]:Stop(0.1)
        end
        
        if animTracks[name] and not animTracks[name].IsPlaying then
            animTracks[name]:Play(0.1)
        end
        
        currentAnim = name
        
        if name ~= "idle" and name ~= "secondIdle" then
            idleTimer = 0
            isSecondIdle = false
        end
    end
    
    local function updateAnimationState()
        if not character:IsDescendantOf(game.Workspace) then
            cleanupAnimations()
            return
        end
        
        local velocity = humanoid.MoveDirection.Magnitude
        local state = humanoid:GetState()
        
        if state == Enum.HumanoidStateType.Swimming then
            if velocity > 0 then
                playAnimation("swim")
            else
                playAnimation("swimIdle")
            end
        elseif state == Enum.HumanoidStateType.Climbing then
            playAnimation("climb")
        elseif state == Enum.HumanoidStateType.Jumping then
            playAnimation("jump")
        elseif state == Enum.HumanoidStateType.Freefall then
            playAnimation("fall")
        elseif velocity > 0 then
            if humanoid.WalkSpeed > 16 then
                playAnimation("run")
            else
                playAnimation("walk")
            end
            idleTimer = 0
            isSecondIdle = false
        else
            if currentAnim ~= "idle" and currentAnim ~= "secondIdle" then
                playAnimation("idle")
            end
            
            idleTimer = idleTimer + RunService.Heartbeat:Wait()
            
            if idleTimer >= 10 and not isSecondIdle then
                playAnimation("secondIdle")
                isSecondIdle = true
            elseif idleTimer >= 20 then
                playAnimation("idle")
                idleTimer = 0
                isSecondIdle = false
            end
        end
    end
    
    updateConnection = RunService.Heartbeat:Connect(updateAnimationState)
    
    character.AncestryChanged:Connect(function(_, parent)
        if not parent then
            cleanupAnimations()
        end
    end)
    
    playAnimation("idle")
end

LocalPlayer.CharacterAdded:Connect(function(character)
    wait(0.5)
    if selectedAnimation ~= "Default" then
        changeAnimation(selectedAnimation)
    end
end)

sections.PlaSection1:Dropdown({
    Name = "FE Animation Changer",
    Default = "Default",
    Options = {"Default", "Confident", "Popstar", "Patrol", "Sneaky", "Princess", "Cowboy", "Stylized Female"},
    Callback = function(Value)
        changeAnimation(Value)
        if Value ~= "Default" then
            Window:Notify({
                Title = "Animation Changer",
                Content = "Đã đổi sang animation " .. Value
            })
        else
            Window:Notify({
                Title = "Animation Changer", 
                Content = "Đã trở về animation mặc định"
            })
        end
    end
}, "AnimationChanger")

-------------------------
local Player = game:GetService("Players").LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Root = Character:WaitForChild("HumanoidRootPart")
local RunService = game:GetService("RunService")
local SpinConnection = nil
local SpinSpeed = 50 -- Tốc độ mặc định

sections.PlaSection1:Slider({
    Name = "Spin Speed",
    Default = 50,
    Minimum = 0,
    Maximum = 500, -- Tăng maximum lên 500
    DisplayMethod = "Percent",
    Precision = 0,
    Callback = function(Value)
        SpinSpeed = Value
    end
})

sections.PlaSection1:Toggle({
    Name = "Spin",
    Default = false,
    Callback = function(value)
        if value then
            local spinAngle = 0
            SpinConnection = RunService.Heartbeat:Connect(function()
                if Root then
                    spinAngle = spinAngle + math.rad(SpinSpeed/10)
                    Root.CFrame = CFrame.new(Root.Position) * CFrame.Angles(0, spinAngle, 0)
                end
            end)
        else
            if SpinConnection then
                SpinConnection:Disconnect()
                SpinConnection = nil
            end
        end
    end
})

Player.CharacterAdded:Connect(function(newCharacter)
    Character = newCharacter
    Root = Character:WaitForChild("HumanoidRootPart")
    if SpinConnection then
        SpinConnection:Disconnect()
        SpinConnection = nil
    end
end)


----------------------
local Player = game:GetService("Players").LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Root = Character:WaitForChild("HumanoidRootPart")
local Humanoid = Character:WaitForChild("Humanoid")
local RunService = game:GetService("RunService")
local AstronautConnection = nil
local FloatAnimation = nil

sections.PlaSection1:Toggle({
    Name = "Bay Lơ Lửng",
    Default = false,
    Callback = function(value)
        if value then
            -- Tạo animation float
            local FloatTrack = Instance.new("Animation")
            FloatTrack.AnimationId = "rbxassetid://1014384571" -- Animation nằm ngang
            FloatAnimation = Humanoid:LoadAnimation(FloatTrack)
            FloatAnimation:Play()
            
            -- Tạo effect float
            local bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.Name = "AstronautFloat"
            bodyVelocity.MaxForce = Vector3.new(0, math.huge, 0)
            bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            bodyVelocity.Parent = Root
            
            -- Đưa nhân vật lên cao
            Root.CFrame = Root.CFrame + Vector3.new(0, 6, 0)
            
            AstronautConnection = RunService.Heartbeat:Connect(function()
                if Root and bodyVelocity and bodyVelocity.Parent then
                    local offset = math.sin(tick() * 2) * 0.5
                    bodyVelocity.Velocity = Vector3.new(0, offset * 10, 0)
                end
            end)
        else
            if FloatAnimation then
                FloatAnimation:Stop()
                FloatAnimation:Destroy()
                FloatAnimation = nil
            end
            
            if AstronautConnection then
                AstronautConnection:Disconnect()
                AstronautConnection = nil
            end
            
            if Root:FindFirstChild("AstronautFloat") then
                Root:FindFirstChild("AstronautFloat"):Destroy()
            end
        end
    end
})

Player.CharacterAdded:Connect(function(newCharacter)
    Character = newCharacter
    Root = Character:WaitForChild("HumanoidRootPart")
    Humanoid = Character:WaitForChild("Humanoid")
    
    if FloatAnimation then
        FloatAnimation:Stop()
        FloatAnimation:Destroy()
        FloatAnimation = nil
    end
    
    if AstronautConnection then
        AstronautConnection:Disconnect()
        AstronautConnection = nil
    end
    
    if Root:FindFirstChild("AstronautFloat") then
        Root:FindFirstChild("AstronautFloat"):Destroy()
    end
end)
------------------------
local savedPosition = nil
local walkConnection = nil
local humanoidMoving = false
local PathfindingService = game:GetService("PathfindingService")

-- Tạo path visual
local function visualizePath(path)
    -- Xóa path cũ
    local existing = game.Workspace:FindFirstChild("PathVisual")
    if existing then
        existing:Destroy()
    end
    
    -- Tạo folder mới
    local pathVisual = Instance.new("Folder")
    pathVisual.Name = "PathVisual"
    pathVisual.Parent = game.Workspace
    
    -- Tạo beams cho đường đi
    for i = 1, #path - 1 do
        -- Attachment points
        local a0 = Instance.new("Attachment")
        local a1 = Instance.new("Attachment")
        
        -- Create points for beam
        local p0 = Instance.new("Part")
        p0.Name = "BeamPoint" .. i
        p0.Anchored = true
        p0.CanCollide = false
        p0.Transparency = 1
        p0.Position = path[i]
        p0.Size = Vector3.new(0.1, 0.1, 0.1)
        p0.Parent = pathVisual
        a0.Parent = p0
        
        local p1 = Instance.new("Part")
        p1.Name = "BeamPoint" .. (i + 1)
        p1.Anchored = true
        p1.CanCollide = false
        p1.Transparency = 1
        p1.Position = path[i + 1]
        p1.Size = Vector3.new(0.1, 0.1, 0.1)
        p1.Parent = pathVisual
        a1.Parent = p1
        
        -- Tạo beam
        local beam = Instance.new("Beam")
        beam.Name = "PathBeam" .. i
        beam.FaceCamera = true
        beam.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 255, 255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 255, 255))
        })
        beam.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 0.5),
            NumberSequenceKeypoint.new(1, 0.5)
        })
        beam.Width0 = 0.3
        beam.Width1 = 0.3
        beam.Attachment0 = a0
        beam.Attachment1 = a1
        beam.Parent = p0
        
        -- Tạo particle effects tại mỗi điểm
        local emitter = Instance.new("ParticleEmitter")
        emitter.Color = ColorSequence.new(Color3.fromRGB(0, 255, 255))
        emitter.Transparency = NumberSequence.new({
            NumberSequenceKeypoint.new(0, 0),
            NumberSequenceKeypoint.new(1, 1)
        })
        emitter.Size = NumberSequence.new(0.3)
        emitter.Lifetime = NumberRange.new(0.5)
        emitter.Rate = 5
        emitter.Speed = NumberRange.new(0)
        emitter.Parent = p0
    end
    
    -- Tạo điểm đích với particle effect
    local targetPart = Instance.new("Part")
    targetPart.Name = "TargetMarker"
    targetPart.Anchored = true
    targetPart.CanCollide = false
    targetPart.Transparency = 1
    targetPart.Position = path[#path] + Vector3.new(0, 0.1, 0)
    targetPart.Size = Vector3.new(0.1, 0.1, 0.1)
    targetPart.Parent = pathVisual

    local targetAttachment = Instance.new("Attachment")
    targetAttachment.Parent = targetPart

    -- Ring effect ở điểm đích
    local ringEffect = Instance.new("ParticleEmitter")
    ringEffect.Color = ColorSequence.new(Color3.fromRGB(0, 255, 255))
    ringEffect.Transparency = NumberSequence.new({
        NumberSequenceKeypoint.new(0, 0),
        NumberSequenceKeypoint.new(1, 1)
    })
    ringEffect.Size = NumberSequence.new({
        NumberSequenceKeypoint.new(0, 2),
        NumberSequenceKeypoint.new(1, 4)
    })
    ringEffect.Lifetime = NumberRange.new(1)
    ringEffect.Rate = 10
    ringEffect.Speed = NumberRange.new(0)
    ringEffect.SpreadAngle = Vector2.new(0, 360)
    ringEffect.Shape = Enum.ParticleEmitterShape.Disc
    ringEffect.Parent = targetAttachment
end

-- Xóa path visual
local function clearPathVisual()
    local existing = game.Workspace:FindFirstChild("PathVisual")
    if existing then
        existing:Destroy()
    end
end

-- Dropdown để chọn phương thức di chuyển
local moveDropdown = sections.PlaSection1:Dropdown({
    Name = "Loạn Di Chuyển",
    Multi = false,
    Required = false,
    Options = {"Teleport", "Walk", "Tween"},
    Default = "Teleport",
    Callback = function(Value)
        print("Selected: " .. Value)
    end,
})

-- Button lưu vị trí
sections.PlaSection1:Button({
    Name = "Đặt Vị Trí",
    Callback = function()
        local character = game.Players.LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            savedPosition = character.HumanoidRootPart.CFrame
            print("Position saved!")
        end
    end
})

-- Button di chuyển đến vị trí đã lưu
sections.PlaSection1:Button({
    Name = "Bấm Để Đi",
    Callback = function()
        local character = game.Players.LocalPlayer.Character
        if not character or not character:FindFirstChild("HumanoidRootPart") or not savedPosition then
            print("Character or saved position not found!")
            return
        end

        local humanoid = character:FindFirstChild("Humanoid")
        if not humanoid then
            print("Humanoid not found!")
            return
        end

        if walkConnection then
            walkConnection:Disconnect()
            walkConnection = nil
            humanoidMoving = false
        end
        clearPathVisual()

        local moveType = moveDropdown.Value

        if moveType == "Teleport" then
            character.HumanoidRootPart.CFrame = savedPosition
            print("Teleported!")

        elseif moveType == "Walk" then
    local path = PathfindingService:CreatePath({
        AgentRadius = 2,
        AgentHeight = 5,
        AgentCanJump = true
    })

    local success, errorMessage = pcall(function()
        path:ComputeAsync(character.HumanoidRootPart.Position, savedPosition.Position)
    end)

    if success and path.Status == Enum.PathStatus.Success then
        local waypoints = {}
        for _, waypoint in ipairs(path:GetWaypoints()) do
            table.insert(waypoints, waypoint.Position)
        end

        visualizePath(waypoints)

        humanoidMoving = true
        local currentWaypoint = 2
        
        humanoid.WalkSpeed = 16

        local function moveToNextWaypoint()
            if not humanoidMoving then return end
            if currentWaypoint <= #waypoints then
                humanoid:MoveTo(waypoints[currentWaypoint])
            end
        end

        walkConnection = humanoid.MoveToFinished:Connect(function(reached)
            if reached and humanoidMoving then
                currentWaypoint = currentWaypoint + 1
                if currentWaypoint <= #waypoints then
                    moveToNextWaypoint()
                else
                    humanoidMoving = false
                    clearPathVisual()
                    walkConnection:Disconnect()
                    walkConnection = nil
                    print("Reached destination!")
                end
            end
        end)
                
                moveToNextWaypoint()
        print("Walking to position...")
    else
        print("Failed to compute path: " .. tostring(errorMessage))
    end

        elseif moveType == "Tween" then
            local tweenService = game:GetService("TweenService")
            local tweenInfo = TweenInfo.new(
                1,
                Enum.EasingStyle.Linear,
                Enum.EasingDirection.Out
            )
            
            local tween = tweenService:Create(character.HumanoidRootPart, tweenInfo, {
                CFrame = savedPosition
            })
            tween:Play()
            print("Tweening to position...")
        end
    end
})
-----------------
sections.Pla2Section1:Paragraph({
	Header = "Khi Bật Sẽ Ghi Lại Hành Động Khi Tắt Sẽ Tua Ngược Lại Hành Động",
	Body = "Ghi Hành Động Khi Bật Sẽ Ghi Lại Lưư Ý Phải Tắt Đi Mới Lưu. Và Bật Phát Lại Sẽ Lập Đi Lập Lại Hành Động Đã Ghi"
})

local recordedActions = {}
local isRecording = false
local isReplaying = false
local recordingConnection = nil
local reverseConnection = nil

sections.Pla2Section1:Toggle({
    Name = "Tua Ngược Hành Động",
    Default = false,
    Callback = function(value)
        local character = game.Players.LocalPlayer.Character
        local humanoid = character and character:FindFirstChild("Humanoid")
        local rootPart = character and character:FindFirstChild("HumanoidRootPart")
        
        if not character or not humanoid or not rootPart then
            Window:Notify({
                Title = "Error",
                Content = "Character not found!",
                Duration = 3
            })
            return
        end

        -- Cleanup function
        local function cleanupConnections()
            if recordingConnection then
                recordingConnection:Disconnect()
                recordingConnection = nil
            end
            if reverseConnection then
                reverseConnection:Disconnect()
                reverseConnection = nil
            end
        end

        if value then -- Bắt đầu ghi
            cleanupConnections()
            recordedActions = {}
            isRecording = true

            recordingConnection = game:GetService("RunService").Heartbeat:Connect(function()
                if isRecording then
                    table.insert(recordedActions, {
                        position = rootPart.CFrame,
                        jumping = humanoid.Jump,
                        moveDirection = humanoid.MoveDirection
                    })
                end
            end)

            Window:Notify({
                Title = "Recording",
                Content = "Started recording movements",
                Duration = 3
            })

        else -- Kết thúc ghi và phát ngược lại ngay lập tức
            if isRecording then
                isRecording = false
                cleanupConnections()

                if #recordedActions > 0 then
                    -- Phát ngược lại ngay lập tức
                    local actionIndex = #recordedActions
                    reverseConnection = game:GetService("RunService").RenderStepped:Connect(function()
                        if actionIndex < 1 then
                            cleanupConnections()
                            return
                        end

                        local action = recordedActions[actionIndex]
                        rootPart.CFrame = action.position
                        humanoid.Jump = action.jumping
                        humanoid:Move(action.moveDirection)
                        actionIndex = actionIndex - 1
                    end)

                    Window:Notify({
                        Title = "Replaying",
                        Content = "Replaying movements in reverse",
                        Duration = 3
                    })
                end
            end
        end
    end
})
----------------------
local recordedActions = {}
local isRecording = false
local isReplaying = false
local recordingConnection = nil
local replayConnection = nil

-- Toggle để ghi lại hành động
sections.Pla2Section1:Toggle({
    Name = "Ghi Hành Động",
    Default = false,
    Callback = function(value)
        local character = game.Players.LocalPlayer.Character
        local humanoid = character and character:FindFirstChild("Humanoid")
        local rootPart = character and character:FindFirstChild("HumanoidRootPart")
        
        if not character or not humanoid or not rootPart then
            Window:Notify({
                Title = "Error",
                Content = "Character not found!",
                Duration = 3
            })
            return
        end

        if value then -- Bắt đầu ghi
            recordedActions = {}
            isRecording = true

            if recordingConnection then
                recordingConnection:Disconnect()
            end

            recordingConnection = game:GetService("RunService").Heartbeat:Connect(function()
                if isRecording then
                    table.insert(recordedActions, {
                        position = rootPart.CFrame,
                        jumping = humanoid.Jump,
                        moveDirection = humanoid.MoveDirection
                    })
                end
            end)

            Window:Notify({
                Title = "Recording",
                Content = "Started recording movements",
                Duration = 3
            })
        else -- Kết thúc ghi và lưu lại
            isRecording = false
            if recordingConnection then
                recordingConnection:Disconnect()
            end

            Window:Notify({
                Title = "Saved",
                Content = "Recording saved!",
                Duration = 3
            })
        end
    end
})

-- Toggle để phát lại hành động đã ghi
sections.Pla2Section1:Toggle({
    Name = "Phát Lại",
    Default = false,
    Callback = function(value)
        local character = game.Players.LocalPlayer.Character
        local humanoid = character and character:FindFirstChild("Humanoid")
        local rootPart = character and character:FindFirstChild("HumanoidRootPart")
        
        if not character or not humanoid or not rootPart then
            Window:Notify({
                Title = "Error",
                Content = "Character not found!",
                Duration = 3
            })
            return
        end

        if value then -- Bắt đầu phát lại
            if #recordedActions == 0 then
                Window:Notify({
                    Title = "Error",
                    Content = "No recorded movements found!",
                    Duration = 3
                })
                return
            end

            isReplaying = true
            local actionIndex = 1

            if replayConnection then
                replayConnection:Disconnect()
            end

            replayConnection = game:GetService("RunService").RenderStepped:Connect(function()
                if not isReplaying then
                    replayConnection:Disconnect()
                    return
                end

                if actionIndex > #recordedActions then
                    actionIndex = 1 -- Lặp lại từ đầu
                end

                local action = recordedActions[actionIndex]
                rootPart.CFrame = action.position
                humanoid.Jump = action.jumping
                humanoid:Move(action.moveDirection)
                actionIndex = actionIndex + 1
            end)

            Window:Notify({
                Title = "Replaying",
                Content = "Replaying recorded movements",
                Duration = 3
            })
        else -- Dừng phát lại
            isReplaying = false
            if replayConnection then
                replayConnection:Disconnect()
            end

            Window:Notify({
                Title = "Stopped",
                Content = "Stopped replaying movements",
                Duration = 3
            })
        end
    end
})
-------------------
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera
local isShiftLockEnabled = false
local shiftLockConnection

sections.Pla2Section1:Toggle({
    Name = "Shiftlock",
    Default = false,
    Callback = function(value)
        local character = player.Character
        if not character then return end
        
        local humanoid = character:FindFirstChild("Humanoid")
        local rootPart = character:FindFirstChild("HumanoidRootPart")
        if not humanoid or not rootPart then return end

        isShiftLockEnabled = value
        
        if value then
            camera.CameraType = Enum.CameraType.Custom
            
            if shiftLockConnection then
                shiftLockConnection:Disconnect()
            end
            
            shiftLockConnection = game:GetService("RunService").RenderStepped:Connect(function()
                if not isShiftLockEnabled then return end
                
                -- Xoay character theo hướng camera
                local cameraLook = camera.CFrame.LookVector
                rootPart.CFrame = CFrame.new(rootPart.Position, rootPart.Position + Vector3.new(cameraLook.X, 0, cameraLook.Z))
            end)
            
            Window:Notify({
                Title = "Shiftlock",
                Content = "Enabled",
                Duration = 3
            })
        else
            camera.CameraType = Enum.CameraType.Custom
            
            if shiftLockConnection then
                shiftLockConnection:Disconnect()
                shiftLockConnection = nil
            end
            
            Window:Notify({
                Title = "Shiftlock",
                Content = "Disabled",
                Duration = 3
            })
        end
    end
})
---------------------------------------
sections.EspMSection1:Paragraph({
	Header = "Chức Năng Này Hiện Chưa Ổn Định",
	Body = "Sẽ Esp Chams Lên Tất Cả Item Và Model"
})

sections.EspMSection1:Toggle({
    Name = "ESPM Chams",
    Default = false,
    Callback = function(value)
        if value then
            local RunService = game:GetService("RunService")
            local Players = game:GetService("Players")
            local LocalPlayer = Players.LocalPlayer
            
            if not getgenv().espHighlights then
                getgenv().espHighlights = {}
            end
            
            if not getgenv().trackedObjects then
                getgenv().trackedObjects = {}
            end

            local function hasClickDetector(object)
                if object:IsA("BasePart") and object:FindFirstChildOfClass("ClickDetector") then
                    return true
                elseif object:IsA("Model") then
                    for _, part in ipairs(object:GetDescendants()) do
                        if part:FindFirstChildOfClass("ClickDetector") then
                            return true
                        end
                    end
                end
                return false
            end

            local function createHighlight(object, color)
                if object:FindFirstChild("Highlight") then return end
                
                local highlight = Instance.new("Highlight")
                highlight.FillColor = color or Color3.fromRGB(255, 0, 0)
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.FillTransparency = 0.5
                highlight.OutlineTransparency = 0
                highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                highlight.Parent = object
                
                table.insert(getgenv().espHighlights, highlight)
                return highlight
            end

            local function updateHighlight(object)
                if not object or not object.Parent then return end
                
                local highlight = object:FindFirstChild("Highlight")
                if not highlight then return end
                
                -- Cập nhật màu sắc dựa trên loại đối tượng
                if object:IsA("Model") and object:FindFirstChild("Humanoid") then
                    highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Màu đỏ cho NPC
                else
                    highlight.FillColor = Color3.fromRGB(0, 0, 255) -- Màu xanh cho items
                end
            end

            local function handleESP(object)
                -- Xử lý objects có ClickDetector
                if hasClickDetector(object) then
                    if not getgenv().trackedObjects[object] then
                        local pos = object:IsA("Model") and 
                            (object:GetPrimaryPartCFrame() and object:GetPrimaryPartCFrame().Position or object:GetModelCFrame().Position) or 
                            object.Position
                        getgenv().trackedObjects[object] = {
                            isItem = true,
                            lastPosition = pos
                        }
                    end
                    createHighlight(object, Color3.fromRGB(0, 0, 255))
                end
                
                -- Xử lý models có Humanoid (NPC)
                if object:IsA("Model") and object:FindFirstChild("Humanoid") then
                    local isPlayer = false
                    for _, player in ipairs(Players:GetPlayers()) do
                        if player.Character == object then
                            isPlayer = true
                            break
                        end
                    end
                    
                    if not isPlayer then
                        if not getgenv().trackedObjects[object] then
                            getgenv().trackedObjects[object] = {
                                isNPC = true,
                                lastPosition = object:GetPrimaryPartCFrame().Position
                            }
                        end
                        createHighlight(object, Color3.fromRGB(255, 0, 0))
                        
                        -- Theo dõi di chuyển của NPC
                        if not object:GetAttribute("ESP_Tracking") then
                            object:SetAttribute("ESP_Tracking", true)
                            local humanoid = object:FindFirstChild("Humanoid")
                            if humanoid then
                                humanoid.Running:Connect(function()
                                    if getgenv().trackedObjects[object] then
                                        updateHighlight(object)
                                    end
                                end)
                            end
                        end
                    end
                end
            end

            local function cleanupObject(object)
                if getgenv().trackedObjects[object] then
                    getgenv().trackedObjects[object] = nil
                end
                if object:FindFirstChild("Highlight") then
                    local highlight = object.Highlight
                    for i, stored_highlight in ipairs(getgenv().espHighlights) do
                        if stored_highlight == highlight then
                            table.remove(getgenv().espHighlights, i)
                            break
                        end
                    end
                    highlight:Destroy()
                end
            end

            -- Khởi tạo ESP cho objects hiện có
            for _, object in ipairs(workspace:GetDescendants()) do
                handleESP(object)
            end

            local descendantAddedConnection = workspace.DescendantAdded:Connect(function(object)
                task.wait()
                handleESP(object)
            end)

            local descendantRemovingConnection = workspace.DescendantRemoving:Connect(cleanupObject)

            local heartbeatConnection = RunService.Heartbeat:Connect(function()
                for object, data in pairs(getgenv().trackedObjects) do
                    if object and object.Parent then
                        local currentPos
                        if object:IsA("Model") and object:FindFirstChild("HumanoidRootPart") then
                            currentPos = object.HumanoidRootPart.Position
                        else
                            currentPos = object:IsA("Model") and 
                                (object:GetPrimaryPartCFrame() and object:GetPrimaryPartCFrame().Position or object:GetModelCFrame().Position) or 
                                object.Position
                        end
                        
                        -- Cập nhật vị trí cuối cùng
                        if data.lastPosition then
                            local distance = (currentPos - data.lastPosition).Magnitude
                            if distance > 0.1 then
                                -- Object đang di chuyển
                                updateHighlight(object)
                            end
                        end
                        data.lastPosition = currentPos
                    else
                        getgenv().trackedObjects[object] = nil
                    end
                end
            end)

            getgenv().ESPConnections = {
                descendantAdded = descendantAddedConnection,
                descendantRemoving = descendantRemovingConnection,
                heartbeat = heartbeatConnection
            }

        else
            -- Cleanup khi tắt ESP
            if getgenv().ESPConnections then
                for _, connection in pairs(getgenv().ESPConnections) do
                    if connection then
                        connection:Disconnect()
                    end
                end
            end

            if getgenv().espHighlights then
                for _, highlight in ipairs(getgenv().espHighlights) do
                    if highlight and highlight.Parent then
                        highlight:Destroy()
                    end
                end
                table.clear(getgenv().espHighlights)
            end

            if getgenv().trackedObjects then
                for object, _ in pairs(getgenv().trackedObjects) do
                    if object then
                        object:SetAttribute("ESP_Tracking", nil)
                    end
                end
                table.clear(getgenv().trackedObjects)
            end
        end

        Window:Notify({
            Title = Window.Settings.Title,
            Content = value and "ESP Enabled" or "ESP Disabled",
            Duration = 3
        })
    end,
}, "Toggle")
----------------------
local ESP = {
    Enabled = false,
    espLabels = {},
    trackedObjects = {},
    Connections = {}
}

sections.EspMSection1:Toggle({
    Name = "ESP Tên",
    Default = false,
    Callback = function(value)
        ESP.Enabled = value
        
        if value then
            local RunService = game:GetService("RunService")
            local Players = game:GetService("Players")

            local function getObjectColor(object)
                if object:IsA("BasePart") then
                    return object.Color
                elseif object:IsA("Model") then
                    local part = object.PrimaryPart or object:FindFirstChildWhichIsA("BasePart")
                    return part and part.Color or Color3.new(1, 1, 1)
                end
                return Color3.new(1, 1, 1)
            end

            local function createLabel(object)
                local billboard = Instance.new("BillboardGui")
                billboard.Name = "ESPLabel"
                billboard.AlwaysOnTop = true
                billboard.Size = UDim2.new(0, 100, 0, 25)
                billboard.StudsOffset = Vector3.new(0, 1.5, 0)
                billboard.MaxDistance = 100

                local whiteGlowLayers = {
                    {size = 19, transparency = 0.7},
                    {size = 18, transparency = 0.6},
                    {size = 17, transparency = 0.5},
                }

                local coloredLayers = {
                    {size = 16, transparency = 0.2},
                    {size = 15, transparency = 0.1},
                    {size = 14, transparency = 0},
                }

                for _, layer in ipairs(whiteGlowLayers) do
                    local label = Instance.new("TextLabel")
                    label.BackgroundTransparency = 1
                    label.Size = UDim2.new(1, 0, 1, 0)
                    label.Font = Enum.Font.GothamBold
                    label.TextSize = layer.size
                    label.TextTransparency = layer.transparency
                    label.TextStrokeTransparency = 0
                    label.TextStrokeColor3 = Color3.new(0, 0, 0)
                    label.TextColor3 = Color3.new(1, 1, 1)
                    
                    if object:IsA("Model") and object:FindFirstChild("Humanoid") then
                        label.Text = "⚔️ " .. object.Name
                    else
                        label.Text = "📦 " .. object.Name
                    end
                    
                    label.Parent = billboard
                end

                local objectColor = getObjectColor(object)
                for _, layer in ipairs(coloredLayers) do
                    local label = Instance.new("TextLabel")
                    label.BackgroundTransparency = 1
                    label.Size = UDim2.new(1, 0, 1, 0)
                    label.Font = Enum.Font.GothamBold
                    label.TextSize = layer.size
                    label.TextTransparency = layer.transparency
                    label.TextStrokeTransparency = 0
                    label.TextStrokeColor3 = Color3.new(0, 0, 0)
                    label.TextColor3 = objectColor
                    
                    if object:IsA("Model") and object:FindFirstChild("Humanoid") then
                        label.Text = "⚔️ " .. object.Name
                    else
                        label.Text = "📦 " .. object.Name
                    end
                    
                    label.Parent = billboard
                end

                if object:IsA("Model") then
                    local humanoidRootPart = object:FindFirstChild("HumanoidRootPart")
                    if humanoidRootPart then
                        billboard.Parent = humanoidRootPart
                    else
                        billboard.Parent = object.PrimaryPart or object:FindFirstChild("Head") or object:FindFirstChildWhichIsA("BasePart")
                    end
                else
                    billboard.Parent = object
                end

                local glowEffect = true
                spawn(function()
                    while billboard and billboard.Parent and ESP.Enabled do
                        if glowEffect then
                            for i, label in ipairs(billboard:GetChildren()) do
                                local sinVal = math.abs(math.sin(tick() * 2))
                                if i <= #whiteGlowLayers then
                                    label.TextTransparency = whiteGlowLayers[i].transparency * sinVal
                                else
                                    local coloredIndex = i - #whiteGlowLayers
                                    label.TextTransparency = coloredLayers[coloredIndex].transparency * sinVal
                                end
                            end
                        end
                        RunService.RenderStepped:Wait()
                    end
                end)

                table.insert(ESP.Connections, RunService.RenderStepped:Connect(function()
                    if billboard.Enabled and Players.LocalPlayer.Character and ESP.Enabled then
                        local distance = (Players.LocalPlayer.Character:GetPrimaryPartCFrame().Position - billboard.Parent.Position).Magnitude
                        local alpha = math.clamp(1 - (distance/100), 0.3, 1)
                        
                        for i, label in ipairs(billboard:GetChildren()) do
                            if i <= #whiteGlowLayers then
                                local baseTransparency = whiteGlowLayers[i].transparency
                                label.TextTransparency = math.min(1, baseTransparency + (1 - alpha))
                            else
                                local coloredIndex = i - #whiteGlowLayers
                                local baseTransparency = coloredLayers[coloredIndex].transparency
                                label.TextTransparency = math.min(1, baseTransparency + (1 - alpha))
                            end
                        end
                    end
                end))

                table.insert(ESP.espLabels, billboard)
                return billboard
            end

            local function hasClickDetector(object)
                if object:IsA("BasePart") then
                    return object:FindFirstChildOfClass("ClickDetector") ~= nil
                elseif object:IsA("Model") then
                    for _, part in ipairs(object:GetDescendants()) do
                        if part:FindFirstChildOfClass("ClickDetector") then
                            return true
                        end
                    end
                end
                return false
            end

            local function updateLabelColor(object, newColor)
                local label = object:FindFirstChild("ESPLabel") or object:FindFirstDescendant("ESPLabel")
                if label then
                    for i = (#whiteGlowLayers + 1), #label:GetChildren() do
                        local textLabel = label:GetChildren()[i]
                        textLabel.TextColor3 = newColor
                    end
                end
            end

            local function handleESP(object)
                if not ESP.Enabled then return end
                
                local isPlayer = false
                if object:IsA("Model") then
                    for _, player in ipairs(Players:GetPlayers()) do
                        if player.Character == object then
                            isPlayer = true
                            break
                        end
                    end
                end
                
                if isPlayer then return end
                
                if hasClickDetector(object) or (object:IsA("Model") and object:FindFirstChild("Humanoid")) then
                    createLabel(object)
                    ESP.trackedObjects[object] = true
                    
                    if object:IsA("BasePart") then
                        local connection = object.Changed:Connect(function(property)
                            if property == "Color" and ESP.trackedObjects[object] and ESP.Enabled then
                                updateLabelColor(object, object.Color)
                            end
                        end)
                        table.insert(ESP.Connections, connection)
                    end
                    
                    if object:IsA("Model") and object:FindFirstChild("Humanoid") then
                        local humanoid = object:FindFirstChild("Humanoid")
                        if humanoid and not object:GetAttribute("ESP_Connected") then
                            object:SetAttribute("ESP_Connected", true)
                            local connection = humanoid.Running:Connect(function()
                                if object and object.Parent and ESP.trackedObjects[object] and ESP.Enabled then
                                    local label = object:FindFirstDescendant("ESPLabel")
                                    if not label then
                                        createLabel(object)
                                    end
                                end
                            end)
                            table.insert(ESP.Connections, connection)
                        end
                    end
                end
            end

            local function cleanupESP(object)
                if ESP.trackedObjects[object] then
                    ESP.trackedObjects[object] = nil
                    for i, label in ipairs(ESP.espLabels) do
                        if label.Parent and (label.Parent == object or label.Parent:IsDescendantOf(object)) then
                            table.remove(ESP.espLabels, i)
                            label:Destroy()
                            break
                        end
                    end
                end
            end

            for _, object in ipairs(workspace:GetDescendants()) do
                handleESP(object)
            end

            table.insert(ESP.Connections, workspace.DescendantAdded:Connect(function(object)
                task.wait()
                if ESP.Enabled then
                    handleESP(object)
                end
            end))

            table.insert(ESP.Connections, workspace.DescendantRemoving:Connect(function(object)
                if ESP.Enabled then
                    cleanupESP(object)
                end
            end))

            table.insert(ESP.Connections, RunService.Heartbeat:Connect(function()
                if not ESP.Enabled then return end
                for object, _ in pairs(ESP.trackedObjects) do
                    if object and object.Parent then
                        local hasLabel = false
                        if object:IsA("Model") then
                            hasLabel = object:FindFirstDescendant("ESPLabel") ~= nil
                        else
                            hasLabel = object:FindFirstChild("ESPLabel") ~= nil
                        end
                        
                        if not hasLabel then
                            createLabel(object)
                        end
                    else
                        cleanupESP(object)
                    end
                end
            end))

        else
            -- Cleanup khi tắt ESP
            for _, label in ipairs(ESP.espLabels) do
                if label and label.Parent then
                    label:Destroy()
                end
            end
            
            for _, connection in ipairs(ESP.Connections) do
                connection:Disconnect()
            end
            
            ESP.espLabels = {}
            ESP.trackedObjects = {}
            ESP.Connections = {}
        end
    end
})
-------------------

local lighting = game:GetService("Lighting")

local Dropdown = sections.ShaSection1:Dropdown({
    Name = "Technology",
    Multi = false,
    Required = true,
    Options = {
        "Future",
        "Compatibility", 
        "Legacy",
        "ShadowMap",
        "Unified",
        "Voxel"
    },
    Default = "Future",
    Callback = function(Value)
        lighting.Technology = Enum.Technology[Value]
    end,
}, "Technology")
------------------------
local lighting = game:GetService("Lighting")
local isTimeEnabled = false

sections.ShaSection1:Toggle({
    Name = "Real Time Flow",
    Default = false,
    Callback = function(value)
        isTimeEnabled = value
        if value then
            -- Bật thời gian
            spawn(function()
                while isTimeEnabled do
                    lighting.ClockTime = lighting.ClockTime + (1/60)  -- 1 phút game = 1 phút thực
                    wait(1)
                end
            end)
            Window:Notify({
                Title = "Time Control",
                Content = "Time is flowing"
            })
        else
            -- Tắt thời gian
            Window:Notify({
                Title = "Time Control", 
                Content = "Time is frozen"
            })
        end
    end,
}, "TimeControl")
----------------------
sections.EspSection1:Toggle({
    Name = "Cảnh Báo Người Nhìn",
    Default = false,
    Callback = function(value)
        if value then
            loadstring(game:HttpGet("https://raw.githubusercontent.com/MouriPre/Canhbao/refs/heads/main/Hienbao"))()
            Window:Notify({
                Title = "Đã Bật Cảnh Báo",
                Content = "Cảnh báo khi có người nhìn bạn đã được bật",
                Time = 3
            })
        else
            if game:GetService("CoreGui"):FindFirstChild("CompactWarning") then
                game:GetService("CoreGui").CompactWarning:Destroy()
            end
            _G.WatchingESPSettings.Enabled = false
            Window:Notify({
                Title = "Đã Tắt Cảnh Báo",
                Content = "Cảnh báo khi có người nhìn bạn đã được tắt",
                Time = 3
            })
        end
    end,
}, "WatchingAlert")

------------------------------------------------------------------------------------------------------------

-- Hàm lưu vị trí
local function savePosition(position)
    local data = {
        X = position.X.Scale,
        XOffset = position.X.Offset,
        Y = position.Y.Scale,
        YOffset = position.Y.Offset
    }
    writefile("ui_position.json", HttpService:JSONEncode(data))
end

-- Hàm đọc vị trí đã lưu
local function loadPosition()
    if isfile("ui_position.json") then
        local data = HttpService:JSONDecode(readfile("ui_position.json"))
        return UDim2.new(data.X, data.XOffset, data.Y, data.YOffset)
    end
    return UDim2.new(0.1, 0, 0.7, 0)
end

-- Xóa UI cũ nếu tồn tại
local function clearUI()
    local playerGui = Players.LocalPlayer:WaitForChild("PlayerGui")
    local oldUI = playerGui:FindFirstChild("MainUI")
    if oldUI then
        oldUI:Destroy()
    end
end

clearUI()

-- Tạo giao diện chính
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MainUI"
ScreenGui.Parent = Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Tạo nút chính
local MainButton = Instance.new("ImageButton")
MainButton.Name = "MainButton"
MainButton.Size = UDim2.new(0, 50, 0, 50)
MainButton.Position = loadPosition() -- Đọc vị trí đã lưu
MainButton.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainButton.Image = "rbxassetid://128828160886762"
MainButton.BackgroundTransparency = 0
MainButton.Parent = ScreenGui

-- Bo tròn góc
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0.2, 0)
UICorner.Parent = MainButton

-- Thêm viền
local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(255, 255, 255)
UIStroke.Thickness = 1.5
UIStroke.Transparency = 0.8
UIStroke.Parent = MainButton

local Enabled = false

-- Hiệu ứng nhấn nút
local function buttonEffect()
    local pressInfo = TweenInfo.new(0.1, Enum.EasingStyle.Quad)
    local pressAnim = TweenService:Create(MainButton, pressInfo, {
        Size = UDim2.new(0, 45, 0, 45)
    })
    local releaseAnim = TweenService:Create(MainButton, pressInfo, {
        Size = UDim2.new(0, 50, 0, 50)
    })
    
    pressAnim:Play()
    wait(0.1)
    releaseAnim:Play()
end

-- Kích hoạt RightControl
local function activateRightControl()
    local vim = game:GetService("VirtualInputManager")
    vim:SendKeyEvent(true, Enum.KeyCode.RightControl, false, game)
    wait()
    vim:SendKeyEvent(false, Enum.KeyCode.RightControl, false, game)
end

-- Hiệu ứng chuyển đổi
local function updateToggle()
    if Enabled then
        TweenService:Create(MainButton, TweenInfo.new(0.3), {
            BackgroundColor3 = Color3.fromRGB(0, 170, 255)
        }):Play()
    else
        TweenService:Create(MainButton, TweenInfo.new(0.3), {
            BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        }):Play()
    end
end

MainButton.MouseButton1Click:Connect(function()
    buttonEffect()
    Enabled = not Enabled
    updateToggle()
    activateRightControl()
end)

-- Kéo thả UI với lưu vị trí
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    local newPosition = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
    MainButton.Position = newPosition
    savePosition(newPosition) -- Lưu vị trí mới
end

MainButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = MainButton.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

MainButton.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)
----------------------------------------------------------



------------- thông------------
sections.MainSection1:Divider()

sections.Main2Section1:Header({
	Text = "•1Ngày - 3.000 •3Ngày - 9.000 •7Ngày - 18.000 •1Tháng - 69.000!"
})

sections.MainSection1:Header({
	Text = "Lưu ý Nút Ẩn UI Sẽ Lưu Vị Trí Nên Bạn Hãy Nó Đến Một Nên Thích Hợp Nhé!"
})

sections.MainSection1:Paragraph({
	Header = "Cập Nhật Chính",
	Body = "Update Sửa Lỗi Và Thêm ▷Aimbot ▷Đồ Họa ▷Universal"
})

sections.Main2Section1:Paragraph({
	Header = "Cập Nhật Phụ",
	Body = "✔Update• +Visual 【•SpeedHack •Nhảy Cao •Nhảy liên tục •Xuyên tường •Lơ lửng trên không •Trọng lực •Chống Afk •Phạm vi quan sát •Click dịch chuyển •Mở giới hạn camera •Camera xuyên •Nhìn xuyên】 ✔Update 【+Player •Free +7 Animation •Spin •Bay lơ lửng】"
})

sections.MainSection1:Label({
	Text = "VIP Ver•1.2"
})

sections.MainSection1:SubLabel({
	Text = "Ui Mac share và được thêm những chức năng tự code độc quyền."
})

MacLib:SetFolder("Maclib")
tabs.Settings:InsertConfigSection("Left")

Window.onUnloaded(function()
	print("Unloaded!")
end)

tabs.Main:Select()
MacLib:LoadAutoLoadConfig()
