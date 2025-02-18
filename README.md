-- Create the GUI
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local UIListLayout = Instance.new("UIListLayout")
local WelcomeText = Instance.new("TextLabel")

-- Buttons
local WalkSpeedButton = Instance.new("TextButton")
local JumpPowerButton = Instance.new("TextButton")
local InfiniteYieldButton = Instance.new("TextButton")
local GhostHubButton = Instance.new("TextButton")
local WallWalkButton = Instance.new("TextButton")
local ChangeCharacterButton = Instance.new("TextButton")
local FlyButton = Instance.new("TextButton")  -- Fly button
local InvisibleButton = Instance.new("TextButton")  -- Invisible button

-- Parent the GUI to the player's PlayerGui
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
Frame.Parent = ScreenGui

-- Set up the Frame (side panel)
Frame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
Frame.Position = UDim2.new(0, 0, 0.3, 0)
Frame.Size = UDim2.new(0, 200, 0, 500)

-- Set up the Welcome Text
WelcomeText.Parent = Frame
WelcomeText.Size = UDim2.new(1, 0, 0, 50)
WelcomeText.BackgroundTransparency = 1
WelcomeText.TextColor3 = Color3.new(1, 1, 1) -- White text
WelcomeText.Font = Enum.Font.SourceSansBold
WelcomeText.TextScaled = true
WelcomeText.Text = "Welcome Austin!" -- Text welcoming the user

-- Set up the layout of buttons
UIListLayout.Parent = Frame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 10)

-- Create Button Template
local function createButton(button, text)
    button.Size = UDim2.new(1, 0, 0, 50)
    button.BackgroundColor3 = Color3.new(1, 0, 0) -- Red color
    button.TextColor3 = Color3.new(1, 1, 1) -- White text
    button.Text = text
    button.Parent = Frame
end

-- Setup the buttons
createButton(WalkSpeedButton, "Set WalkSpeed (60)")
createButton(JumpPowerButton, "Set JumpPower (150)")
createButton(InfiniteYieldButton, "Load Infinite Yield")
createButton(GhostHubButton, "Load Ghost Hub")
createButton(WallWalkButton, "Toggle Wall Walk")
createButton(ChangeCharacterButton, "Become Tuber93")
createButton(FlyButton, "Toggle Fly")  -- New fly button
createButton(InvisibleButton, "Toggle Invisibility")  -- Invisible button

-- Button Functions

-- Set WalkSpeed to 60
WalkSpeedButton.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 60
        print("WalkSpeed set to 60")
    end
end)

-- Set JumpPower to 150
JumpPowerButton.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.JumpPower = 150
        print("JumpPower set to 150")
    end
end)

-- Load Infinite Yield
InfiniteYieldButton.MouseButton1Click:Connect(function()
    local infiniteYieldScript = [[
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    ]]

    -- Execute Infinite Yield
    local success, err = pcall(function()
        loadstring(infiniteYieldScript)()
    end)

    if success then
        print("Infinite Yield Loaded")
    else
        warn("Failed to load Infinite Yield: " .. err)
    end
end)

-- Load Ghost Hub
GhostHubButton.MouseButton1Click:Connect(function()
    local ghostHubScript = [[
        loadstring(game:HttpGet("https://raw.githubusercontent.com/RebounderYT/GhostHub/main/GhostHub.lua"))()
    ]]

    -- Execute Ghost Hub
    local success, err = pcall(function()
        loadstring(ghostHubScript)()
    end)

    if success then
        print("Ghost Hub Loaded")
    else
        warn("Failed to load Ghost Hub: " .. err)
    end
end)

-- Toggle Wall Walking
local wallWalking = false
WallWalkButton.MouseButton1Click:Connect(function()
    wallWalking = not wallWalking
    local player = game.Players.LocalPlayer
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")

    if wallWalking then
        print("Wall Walking enabled")
        -- Make the player able to walk on walls by changing their gravity and surface friction properties
        if humanoid and humanoid:FindFirstChild("HumanoidRootPart") then
            humanoid.HumanoidRootPart.Velocity = Vector3.new(0, 0, 0)
            humanoid.PlatformStand = true -- Allows walking on walls
        end
    else
        print("Wall Walking disabled")
        if humanoid and humanoid:FindFirstChild("HumanoidRootPart") then
            humanoid.PlatformStand = false -- Disables wall walking
        end
    end
end)

-- Change Character to Tuber93
ChangeCharacterButton.MouseButton1Click:Connect(function()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()

    -- Customize the player's appearance to match Tuber93's style
    local success, err = pcall(function()
        -- Set Tuber93 avatar appearance using IDs of the clothing items
        character:FindFirstChild("Shirt").ShirtTemplate = "rbxassetid://607785314" -- Tuber93 shirt
        character:FindFirstChild("Pants").PantsTemplate = "rbxassetid://607785315" -- Tuber93 pants
        character:FindFirstChild("Humanoid"):AddAccessory(Instance.new("Accessory", character)) -- Add accessories
        print("Changed character to Tuber93")
    end)

    if not success then
        warn("Failed to change character: " .. err)
    end
end)

-- Fly Script (Toggle Fly similar to Infinite Yield fly)
local flying = false
local flySpeed = 50
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
local bodyGyro
local bodyVelocity

FlyButton.MouseButton1Click:Connect(function()
    flying = not flying
    if flying then
        local character = player.Character
        if not character then return end
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")

        if humanoidRootPart then
            bodyGyro = Instance.new("BodyGyro")
            bodyGyro.P = 9e4
            bodyGyro.maxTorque = Vector3.new(9e4, 9e4, 9e4)
            bodyGyro.cframe = humanoidRootPart.CFrame
            bodyGyro.Parent = humanoidRootPart

            bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.velocity = Vector3.new(0, 0, 0)
            bodyVelocity.maxForce = Vector3.new(9e4, 9e4, 9e4)
            bodyVelocity.Parent = humanoidRootPart

            print("Fly enabled")

            -- Fly Movement Controls
            game:GetService("RunService").RenderStepped:Connect(function()
                if not flying then return end
                bodyGyro.cframe = humanoidRootPart.CFrame
                bodyVelocity.velocity = (humanoidRootPart.CFrame.lookVector * flySpeed)
            end)

            -- Control the fly speed using keys
            mouse.KeyDown:Connect(function(key)
                if key:lower() == "w" then
                    flySpeed = 100
                elseif key:lower() == "s" then
                    flySpeed = -50
                end
            end)

            mouse.KeyUp:Connect(function(key)
                if key:lower() == "w" or key:lower() == "s" then
                    flySpeed = 50
                end
            end)
        end
    else
        if bodyGyro then bodyGyro:Destroy() end
        if bodyVelocity then bodyVelocity:Destroy() end
        print("Fly disabled")
    end
end)

-- Toggle Invisibility
local invisible = false
InvisibleButton.MouseButton1Click:Connect(function()
    invisible = not invisible
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()

    if invisible then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.Transparency = 1 -- Set transparency to 1 (invisible)
            end
        end
        print("You are now invisible")
    else
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.Transparency = 0 -- Set transparency back to 0

