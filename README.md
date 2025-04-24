local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
 
 
 local Window = Rayfield:CreateWindow({
     Name = "Horrifc housing",
     Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
     LoadingTitle = "Loading...",
     LoadingSubtitle = "by isssacque1234",
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
        Key = {"Key_1010", "Key_9990"} -- List of keys that will be accepted by the system, can be RAW file links (pastebin, github etc) or simple strings ("hello","key22")
     }
  }) 
 
 
 
  local mainTab = Window:CreateTab("Main", "crosshair")
 
  local Section = mainTab:CreateSection("Main Settings")
 
 
 local Toggle = mainTab:CreateToggle({
    Name = "Auto Kill all; use Rocket item",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().autoRocket = Value

        if Value then
            task.spawn(function()
                function getNil(name, class)
                    for _, v in next, getnilinstances() do
                        if v.ClassName == class and v.Name == name then
                            return v
                        end
                    end
                end

                while getgenv().autoRocket do
                    for _, targetPlayer in pairs(game:GetService("Players"):GetPlayers()) do
                        if not getgenv().autoRocket then return end
                        if targetPlayer ~= game.Players.LocalPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                            local localHRP = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                            if localHRP then
                                local direction = (targetPlayer.Character.HumanoidRootPart.Position - localHRP.Position).Unit

                                local args = {
                                    [1] = direction
                                }

                                local rocket = getNil("RocketLauncher", "Tool")
                                if rocket and rocket:FindFirstChild("fire") then
                                    rocket.fire:FireServer(unpack(args))
                                end
                            end
                            task.wait(0.1)
                        end
                    end
                end
            end)
        else
            getgenv().autoRocket = false
        end
    end
})

local Toggle = mainTab:CreateToggle({
    Name = "Auto Freeze all; use Freeze ray item",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().autoFreezeEnabled = Value

        if Value then
            task.spawn(function()
                local Players = game:GetService("Players")
                local LocalPlayer = Players.LocalPlayer

                while getgenv().autoFreezeEnabled do
                    for _, targetPlayer in pairs(Players:GetPlayers()) do
                        if not getgenv().autoFreezeEnabled then return end
                        if targetPlayer ~= LocalPlayer then
                            local targetChar = targetPlayer.Character
                            local localChar = LocalPlayer.Character
                            local targetHRP = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
                            local localHRP = localChar and localChar:FindFirstChild("HumanoidRootPart")

                            if targetHRP and localHRP then
                                local direction = (targetHRP.Position - localHRP.Position).Unit
                                local args = {
                                    [1] = direction
                                }
                                local freezeRay = localChar:FindFirstChild("Freeze Ray")
                                if freezeRay and freezeRay:FindFirstChild("fire") then
                                    freezeRay.fire:FireServer(unpack(args))
                                end
                            end
                        end
                    end
                    task.wait(0.1)
                end
            end)
        else
            getgenv().autoFreezeEnabled = false
        end
    end
})

local Button = mainTab:CreateButton({
    Name = "Use a random potion",
    Callback = function()
        local args = {
            [1] = true
        }

        game:GetService("ReplicatedStorage"):WaitForChild("EventRemotes"):WaitForChild("Potion"):FireServer(unpack(args))
    end
})


local Button = mainTab:CreateButton({
    Name = "bypass Cooldown of Flute; equip flute item",
    Callback = function()
        function getNil(name, class)
            for _, v in next, getnilinstances() do
                if v.ClassName == class and v.Name == name then
                    return v
                end
            end
        end

        local args = {
            [1] = 1,
            [2] = getNil("Handle", "Part")
        }

        getNil("Flute", "Tool"):WaitForChild("Remote"):FireServer(unpack(args))
    end
})


local Toggle = mainTab:CreateToggle({
    Name = "Auto Kill all, version Gears; use Rocket item",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        getgenv().autoRocket = Value

        if Value then
            task.spawn(function()
                function getNil(name, class)
                    for _, v in next, getnilinstances() do
                        if v.ClassName == class and v.Name == name then
                            return v
                        end
                    end
                end

                local rocket = getNil("RocketLauncher", "Tool")
                if not rocket then return end
                local fire = rocket:WaitForChild("fire")

                while getgenv().autoRocket do
                    for _, target in pairs(game.Players:GetPlayers()) do
                        if target ~= game.Players.LocalPlayer then
                            local function isAlive(plr)
                                return plr.Character and plr.Character:FindFirstChild("HumanoidRootPart")
                            end

                            while getgenv().autoRocket and isAlive(target) do
                                local myChar = game.Players.LocalPlayer.Character
                                local myRoot = myChar and myChar:FindFirstChild("HumanoidRootPart")
                                local theirRoot = target.Character and target.Character:FindFirstChild("HumanoidRootPart")

                                if myRoot and theirRoot then
                                    local direction = (theirRoot.Position - myRoot.Position).Unit
                                    local args = { [1] = direction }
                                    fire:FireServer(unpack(args))
                                end
                                task.wait(0.1)
                            end
                        end
                    end
                end
            end)
        else
            getgenv().autoRocket = false
        end
    end
})

 
  Rayfield:Notify({
     Title = "Script by isssacque1234",
     Content = "loaded",
     Duration = 6.5,
     Image = 4483362458,
  })
  
