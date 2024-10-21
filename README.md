# Shinobi-Storm-Script-Druum-

# Shinobi-Storm-Script-by-Druum

local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "Shinobi Storm", HidePremium = false, SaveConfig = true, ConfigFolder = "by Druum"})
local Player = game.Players.LocalPlayer

local function GetAllWinsChar(value)
    if value then
        local Stats = Player:FindFirstChild("ShopValues")
        if Stats then
            for _, BoolValues in ipairs(Stats:GetChildren()) do
                if BoolValues:IsA("BoolValue") then
                    BoolValues.Value = true
                end
            end
        else
            warn("ShopValues not found")
        end
    end
end

local function Page3(value)
    if value then
        local GuiPlayer = Player:WaitForChild("PlayerGui")
        local CharacterSelectGui = GuiPlayer:FindFirstChild("CharacterSelect")
        if CharacterSelectGui then
            CharacterSelectGui.Main.Visible = false
            CharacterSelectGui.MainNotAdded.Visible = false
            CharacterSelectGui.MainNotAdded2.Visible = false
            CharacterSelectGui.MainShippudenNotAdded3.Visible = false
            CharacterSelectGui.MainShippudenNotAdded.Visible = true
            CharacterSelectGui.Main2.Visible = false
            CharacterSelectGui.MainShippuden2.Visible = false
            CharacterSelectGui.MainShippuden3.Visible = false
            CharacterSelectGui.MainShippuden.Visible = true
        else
            warn("CharacterSelectGui not found")
        end
    end
end

local function Page4(value)
    if value then
        local GuiPlayer = Player:WaitForChild("PlayerGui")
        local CharacterSelectGui = GuiPlayer:FindFirstChild("CharacterSelect")
        if CharacterSelectGui then
            CharacterSelectGui.Main.Visible = false
            CharacterSelectGui.MainNotAdded.Visible = false
            CharacterSelectGui.MainNotAdded2.Visible = true
            CharacterSelectGui.MainShippudenNotAdded3.Visible = false
            CharacterSelectGui.MainShippudenNotAdded.Visible = false
            CharacterSelectGui.Main2.Visible = false
            CharacterSelectGui.MainShippuden2.Visible = true
            CharacterSelectGui.MainShippuden3.Visible = false
            CharacterSelectGui.MainShippuden.Visible = false
        else
            warn("CharacterSelectGui not found")
        end
    end
end

local function Page5(value)
    if value then
        local GuiPlayer = Player:WaitForChild("PlayerGui")
        local CharacterSelectGui = GuiPlayer:FindFirstChild("CharacterSelect")
        if CharacterSelectGui then
            CharacterSelectGui.Main.Visible = false
            CharacterSelectGui.MainNotAdded.Visible = false
            CharacterSelectGui.MainNotAdded2.Visible = false
            CharacterSelectGui.MainShippudenNotAdded3.Visible = true
            CharacterSelectGui.MainShippudenNotAdded.Visible = false
            CharacterSelectGui.Main2.Visible = false
            CharacterSelectGui.MainShippuden2.Visible = false
            CharacterSelectGui.MainShippuden3.Visible = true
            CharacterSelectGui.MainShippuden.Visible = false
        else
            warn("CharacterSelectGui not found")
        end
    end
end

local function HitBox(Number)
    local HeadSize = Number
    local IsDisabled = false
    local IsTeamCheckEnabled = false 

    for _, player in ipairs(game:GetService('Players'):GetPlayers()) do
        if player ~= Player and (not IsTeamCheckEnabled or player.Team ~= Player.Team) then
            local humanoidRootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                humanoidRootPart.Size = Vector3.new(HeadSize, HeadSize, HeadSize)
                humanoidRootPart.Transparency = 0.7
                humanoidRootPart.BrickColor = BrickColor.new("Really blue")
                humanoidRootPart.Material = Enum.Material.Neon
                humanoidRootPart.CanCollide = false
            end
        end
    end
end

local autoAttackActive = false
local currentTarget = nil

local function AutoAttack()
    while autoAttackActive do
        local AllPlayers = game.Players:GetPlayers()
        if #AllPlayers > 1 then
            if not currentTarget or (currentTarget and currentTarget:FindFirstChild("Humanoid") and currentTarget.Humanoid.Health <= 0) then
                local GetRandomPlayerIndex = math.random(1, #AllPlayers)
                local SelectedPlayer = AllPlayers[GetRandomPlayerIndex]
                if SelectedPlayer and SelectedPlayer ~= Player and SelectedPlayer.Character then
                    currentTarget = SelectedPlayer.Character
                    print("Attacking: " .. SelectedPlayer.Name)
                end
            end
            
            if currentTarget and currentTarget:FindFirstChild("Humanoid") and currentTarget.Humanoid.Health > 0 then
                local MyCharacter = Player.Character
                if MyCharacter and MyCharacter:FindFirstChild("HumanoidRootPart") then
                    MyCharacter.HumanoidRootPart.CFrame = CFrame.new(currentTarget.HumanoidRootPart.Position)

                    task.spawn(function()
                        local tools = Player.Backpack:GetChildren()
                        if #tools > 0 then
                            local randomTool = tools[math.random(1, #tools)]
                            if randomTool:IsA("Tool") then
                                randomTool.Parent = MyCharacter
                                randomTool:Equip()
                                print("Equipped tool: " .. randomTool.Name)
                                wait(0.2)
                            end
                        else
                            warn("No tools found in backpack")
                        end
                    end)

                    for _, Tool in ipairs(MyCharacter:GetChildren()) do
                        if Tool:IsA("Tool") then
                            local AttackEvent = Tool:FindFirstChild("RemoteEvent")
                            if AttackEvent then
                                AttackEvent:FireServer()
                                wait(0.1)
                            end
                        end
                    end
                end
            else
                currentTarget = nil
            end
        end
        wait(0.1)
    end
end

local Tab = Window:MakeTab({
    Name = "Shop",
    Icon = "rbxassetid://2599236521",
    PremiumOnly = false
})

local Tab2 = Window:MakeTab({
    Name = "Farm",
    Icon = "rbxassetid://2599236521",
    PremiumOnly = false
})

local Section = Tab:AddSection({
    Name = "Shop"
})

local Section2 = Tab2:AddSection({
    Name = "Farm"
})

Tab:AddButton({
    Name = "Get all Wins characters (Click multiple times)",
    Callback = function()
        GetAllWinsChar(true)
    end    
})

Tab:AddButton({
    Name = "Open page 3",
    Callback = function()
        Page3(true)
    end    
})

Tab:AddButton({
    Name = "Open page 4",
    Callback = function()
        Page4(true)
    end    
})

Tab:AddButton({
    Name = "Open page 5",
    Callback = function()
        Page5(true)
    end    
})

local SizeValue = nil

Tab2:AddSlider({
    Name = "Hit Box Size",
    Min = 0,
    Max = 300,
    Default = 10,
    Color = Color3.fromRGB(255,255,255),
    Increment = 1,
    ValueName = "Size",
    Callback = function(Value)
        SizeValue = Value
    end    
})

Tab2:AddButton({
    Name = "Set HitBox",
    Callback = function()
        HitBox(SizeValue)
    end    
})

Tab2:AddToggle({
    Name = "Auto Farm",
    Default = false,
    Save = true,
    Flag = "Value",
    Callback = function(Value)
        autoAttackActive = Value
        if autoAttackActive then
            currentTarget = nil
            AutoAttack()
        end
    end
})
