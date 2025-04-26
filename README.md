local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
 
 
 local Window = Rayfield:CreateWindow({
     Name = "Horrifc housing",
     Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
     LoadingTitle = "Loading...",
     LoadingSubtitle = "by Not's server",
     Theme = "Default", -- Check https://docs.sirius.menu/rayfield/configuration/themes
  
     DisableRayfieldPrompts = false,
     DisableBuildWarnings = false, -- Prevents Rayfield from warning when the script has a version mismatch with the interface
  
     ConfigurationSaving = {
        Enabled = true,
        FolderName = skibidi, -- Create a custom folder for your hub/game
        FileName = "skibidi script"
     },
 
  
     KeySystem = true, -- Set this to true to use our key system
     KeySettings = {
        Title = "Key",
        Subtitle = "Key System",
        Note = "https://discord.gg/c4Ctp35FHm", -- Use this to tell the user how to get a key
        FileName = "skibidi key", -- It is recommended to use something unique as other scripts using Rayfield may overwrite your key file
        SaveKey = true, -- The user's key will be saved, but if you change the key, they will be unable to use your script
        GrabKeyFromSite = false, -- If this is true, set Key below to the RAW site you would like Rayfield to get the key from
        Key = {"Key_4810", "Key_9990"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
     }
  }) 
 
 
 
  local mainTab = Window:CreateTab("Main", "crosshair")
 
  local Section = mainTab:CreateSection("Main Settings")

 
local Toggle = mainTab:CreateToggle({
    Name = "Anti-void",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().giantPartToggle = Value

        if Value then
            local part = Instance.new("Part")
            part.Size = Vector3.new(2000, 1, 2000)
            part.Position = Vector3.new(1.2150108814239502, -0.8852427005767822, 65.64254760742188)
            part.Anchored = true
            part.CanCollide = true
            part.Transparency = 0.5
            part.Name = "GiantPart"
            part.Parent = workspace
        else
            local existing = workspace:FindFirstChild("GiantPart")
            if existing then
                existing:Destroy()
            end
        end
    end
})


local itemsTab = Window:CreateTab("Items", "crosshair")
 
  local Section = itemsTab:CreateSection("Items Settings")



local Toggle = itemsTab:CreateToggle({
    Name = "NoCooldown Blade (dueling sword)",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().bladeLoop = Value

        if Value then
            task.spawn(function()
                while getgenv().bladeLoop do
                    game:GetService("Players").LocalPlayer.Character.Blade.Event:FireServer()
                    task.wait(0)
                end
            end)
        else
            getgenv().bladeLoop = false
        end
    end
})

local Toggle = itemsTab:CreateToggle({
    Name = "NoCooldown Sword",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().swordLoop = Value

        if Value then
            task.spawn(function()
                while getgenv().swordLoop do
                    game:GetService("Players").LocalPlayer.Character.Sword.Event:FireServer()
                    task.wait(0)
                end
            end)
        else
            getgenv().swordLoop = false
        end
    end
})


local Button = itemsTab:CreateButton({
    Name = "NoCooldown Prompts",
    Callback = function()
        local services = game:GetChildren()
        for _, service in ipairs(services) do
            for _, obj in ipairs(service:GetDescendants()) do
                if obj:IsA("ProximityPrompt") then
                    obj.HoldDuration = 0.1
                end
            end
        end
    end
})


local Button = itemsTab:CreateButton({
    Name = "Auto get map",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        local savedCFrame = humanoidRootPart.CFrame

        local function findFirstPrompt(descendants)
            for _, descendant in ipairs(descendants) do
                if descendant:IsA("ProximityPrompt") then
                    return descendant
                end
            end
            return nil
        end

        local function teleportAndClickPrompt(modelName)
            local model = nil
            for _, descendant in ipairs(workspace:GetDescendants()) do
                if descendant.Name == modelName then
                    model = descendant
                    break
                end
            end

            if model then
                local prompt = findFirstPrompt(model:GetDescendants())
                if prompt then
                    humanoidRootPart.CFrame = model.CFrame
                    task.wait(0.25)
                    prompt.HoldDuration = 0
                    fireproximityprompt(prompt)
                end
            end
        end

        teleportAndClickPrompt("MapModel")

        task.wait(0.25)

        teleportAndClickPrompt("Point")

        task.wait(0.25)

        humanoidRootPart.CFrame = savedCFrame
    end
})


local Button = itemsTab:CreateButton({
    Name = "Auto touch Illumina",
    Callback = function()
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        local savedCFrame = humanoidRootPart.CFrame

        local illumina
        for _, descendant in ipairs(workspace:GetDescendants()) do
            if descendant.Name == "Illumina" and (descendant:IsA("BasePart") or descendant:IsA("Model")) then
                illumina = descendant
                break
            end
        end

        if illumina then
            local targetCFrame
            if illumina:IsA("Model") then
                if illumina.PrimaryPart then
                    targetCFrame = illumina.PrimaryPart.CFrame
                else
                    targetCFrame = illumina:GetPivot()
                end
            elseif illumina:IsA("BasePart") then
                targetCFrame = illumina.CFrame
            end

            if targetCFrame then
                humanoidRootPart.CFrame = targetCFrame
                task.wait(0.25)
                humanoidRootPart.CFrame = savedCFrame
            end
        end
    end
})


local potionsTab = Window:CreateTab("Potions", "crosshair")
 
  local Section = potionsTab:CreateSection("Potions Settings")
  

  local Button = potionsTab:CreateButton({
    Name = "Use a random potion",
    Callback = function()
        local args = {
            [1] = true
        }

        game:GetService("ReplicatedStorage"):WaitForChild("EventRemotes"):WaitForChild("Potion"):FireServer(unpack(args))
    end
})

local guisTab = Window:CreateTab("Guis", "crosshair")
 
  local Section = guisTab:CreateSection("Guis Settings")

  
  local Toggle = guisTab:CreateToggle({
    Name = "Hide effects status",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().statusToggle = Value

        local player = game.Players.LocalPlayer
        local gui = player:FindFirstChild("PlayerGui")
        if not gui then return end

        if Value then
            getgenv().statusToggleConnection = game:GetService("RunService").RenderStepped:Connect(function()
                local status = gui:FindFirstChild("statusAffects")
                if status then
                    status.Enabled = false
                end
            end)
        else
            if getgenv().statusToggleConnection then
                getgenv().statusToggleConnection:Disconnect()
                getgenv().statusToggleConnection = nil
            end
            local status = gui:FindFirstChild("statusAffects")
            if status then
                status.Enabled = true
            end
        end
    end
})


local Toggle = guisTab:CreateToggle({
    Name = "Hide Ads",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().removerAds = Value

        if Value then
            task.spawn(function()
                local player = game.Players.LocalPlayer
                while getgenv().removerAds do
                    local gui = player:FindFirstChild("PlayerGui")
                    if gui then
                        local ads = gui:FindFirstChild("Ads")
                        if ads then
                            ads.Enabled = false
                        end
                    end
                    task.wait(1)
                end
            end)
        else
            local player = game.Players.LocalPlayer
            local gui = player:FindFirstChild("PlayerGui")
            if gui then
                local ads = gui:FindFirstChild("Ads")
                if ads then
                    ads.Enabled = true
                end
            end
        end
    end
})

local antiTab = Window:CreateTab("Anti", "crosshair")
 
  local Section = antiTab:CreateSection("Anti Settings")


  local Button = antiTab:CreateButton({
    Name = "Anti-Lava",
    Callback = function()
        for _, v in ipairs(workspace:GetDescendants()) do
            if v:IsA("Part") and v.Name == "LavaPlate" then
                v:Destroy()
            end
        end
    end,
})


local Button = antiTab:CreateButton({
    Name = "Anti-LavaSpinner",
    Callback = function()
        for _, v in ipairs(workspace:GetDescendants()) do
            if v:IsA("Model") and v.Name == "Spinner" then
                v:Destroy()
            end
        end
    end,
})


local trollTab = Window:CreateTab("Troll", "crosshair")
 
  local Section = trollTab:CreateSection("Troll Settings")


  local Toggle = trollTab:CreateToggle({
    Name = "Kill all; use rocket item",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().rocketSpam = Value

        if Value then
            task.spawn(function()
                local player = game.Players.LocalPlayer
                while getgenv().rocketSpam do
                    for _, target in pairs(game.Players:GetPlayers()) do
                        if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                            local direction = (target.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Unit

                            local args = {
                                [1] = direction
                            }

                            player.Character.RocketLauncher.fire:FireServer(unpack(args))
                            task.wait(0)
                        end
                    end
                end
            end)
        else
            getgenv().rocketSpam = false
        end
    end
})


local Toggle = trollTab:CreateToggle({
    Name = "Freeze all; use freeze ray",
    CurrentValue = false,
    Flag = "FreezeRayToggle",
    Callback = function(Value)
        getgenv().freezeRaySpam = Value
        if Value then
            task.spawn(function()
                while getgenv().freezeRaySpam do
                    for _, player in ipairs(game:GetService("Players"):GetPlayers()) do
                        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            local pos = player.Character.HumanoidRootPart.Position
                            local args = {
                                Vector3.new(pos.X, pos.Y, pos.Z)
                            }
                            local freezeRay = game.Players.LocalPlayer.Character:FindFirstChild("Freeze Ray")
                            if freezeRay and freezeRay:FindFirstChild("fire") then
                                freezeRay.fire:FireServer(unpack(args))
                            end
                        end
                    end
                    task.wait(0.1)
                end
            end)
        else
            getgenv().freezeRaySpam = false
        end
    end
})


local Toggle = trollTab:CreateToggle({
    Name = "Kill all 2; equip SnowBall",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().autoKillEnabled = Value

        if Value then
            task.spawn(function()
                while getgenv().autoKillEnabled do
                    for _, targetPlayer in pairs(game.Players:GetPlayers()) do
                        if targetPlayer ~= game.Players.LocalPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local targetPos = targetPlayer.Character.HumanoidRootPart.Position
                            local args = {
                                targetPos
                            }
                            game:GetService("Players").LocalPlayer.Character.Snowball.remote:FireServer(unpack(args))
                            task.wait(0.01)
                        end
                    end
                    task.wait(0)
                end
            end)
        else
            getgenv().autoKillEnabled = false
        end
    end
})


local Toggle = trollTab:CreateToggle({
    Name = "Kill all 3; use paintball item",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().paintballAll = Value

        if Value then
            task.spawn(function()
                while getgenv().paintballAll do
                    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
                        if not getgenv().paintballAll then break end
                        if player ~= game:GetService("Players").LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            local targetPosition = player.Character.HumanoidRootPart.Position
                            local args = {
                                [1] = targetPosition
                            }

                            game:GetService("Players").LocalPlayer.Character.PaintballGun.remote:FireServer(unpack(args))
                            task.wait(0)
                        end
                    end
                    task.wait(0)
                end
            end)
        else
            getgenv().paintballAll = false
        end
    end
})


local Input = trollTab:CreateInput({
    Name = "Coconut Count; important",
    CurrentValue = "",
    PlaceholderText = "Enter number of coconuts",
    RemoveTextAfterFocusLost = false,
    Flag = "Input1",
    Callback = function(Text)
        getgenv().coconutCount = tonumber(Text) or 0
    end,
})

local Toggle = trollTab:CreateToggle({
    Name = "Kill all 4; use coconut",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().coconutLaunch = Value

        if Value then
            task.spawn(function()
                local player = game.Players.LocalPlayer
                local coconutCount = getgenv().coconutCount
                while getgenv().coconutLaunch do
                    for _, targetPlayer in pairs(game.Players:GetPlayers()) do
                        if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local args = {
                                [1] = targetPlayer.Character.HumanoidRootPart.Position,
                                [2] = targetPlayer.Character.UpperTorso,
                                [3] = coconutCount
                            }

                            game:GetService("Players").LocalPlayer.Character.Coconut.throwEvent:FireServer(unpack(args))
                        end
                    end
                    task.wait(0)
                end
            end)
        else
            getgenv().coconutLaunch = false
        end
    end
})

 
  Rayfield:Notify({
     Title = "Script by Not's server",
     Content = "loaded",
     Duration = 6.5,
     Image = 4483362458,
  })


Rayfield:Notify({
     Title = "Important",
     Content = "I recommend leaving videos, comments etc as private or only people with a link can enter, This makes the devs not censor and the script last longer",
     Duration = 15,
     Image = 4483362458,
  })
