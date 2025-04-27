local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
 
 
 local Window = Rayfield:CreateWindow({
     Name = "Horrific housing",
     Icon = 0, -- Icon in Topbar. Can use Lucide Icons (string) or Roblox Image (number). 0 to use no icon (default).
     LoadingTitle = "Loading...",
     LoadingSubtitle = "by Not's hub",
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
 
 
 
  local mainTab = Window:CreateTab("Main", 7539983773)
 
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


local Button = mainTab:CreateButton({
    Name = "Get all items and Tokens; 1 Token needed and Lag warning",
    Callback = function()
        task.spawn(function()
            while true do
                local args1 = {[1] = 0.0000000001, [2] = "HouseChest"}
                game:GetService("ReplicatedStorage"):WaitForChild("ShopPurchase"):FireServer(unpack(args1))

                local args2 = {[1] = 0.00000001, [2] = "Furniture"}
                game:GetService("ReplicatedStorage"):WaitForChild("ShopPurchase"):FireServer(unpack(args2))

                local args3 = {[1] = 0.000000001, [2] = "EggPets"}
                game:GetService("ReplicatedStorage"):WaitForChild("ShopPurchase"):FireServer(unpack(args3))

                local args4 = {[1] = 0.0000001, [2] = "Ornament"}
                game:GetService("ReplicatedStorage"):WaitForChild("ShopPurchase"):FireServer(unpack(args4))

                local args5 = {[1] = 0.0000000001, [2] = "Death"}
                game:GetService("ReplicatedStorage"):WaitForChild("ShopPurchase"):FireServer(unpack(args5))

                task.wait(0)
            end
        end)
    end
})


local itemsTab = Window:CreateTab("Items", 129306263749313)
 
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


local potionsTab = Window:CreateTab("Effects", 13518130183)
 
  local Section = potionsTab:CreateSection("Effects Settings")
  

  local Button = potionsTab:CreateButton({
    Name = "Use a random potion",
    Callback = function()
        local args = {
            [1] = true
        }

        game:GetService("ReplicatedStorage"):WaitForChild("EventRemotes"):WaitForChild("Potion"):FireServer(unpack(args))
    end
})

local guisTab = Window:CreateTab("Guis", 103950363563606)
 
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

local antiTab = Window:CreateTab("Anti", 13793170713)
 
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


local Button = antiTab:CreateButton({
    Name = "Anti-Lag",
    Callback = function()
        local decalsyeeted = true
        local g = game
        local w = g.Workspace
        local l = g.Lighting
        local t = w.Terrain

        t.WaterWaveSize = 0
        t.WaterWaveSpeed = 0
        t.WaterReflectance = 0
        t.WaterTransparency = 0

        l.GlobalShadows = false
        l.FogEnd = 9e9
        l.Brightness = 0

        settings().Rendering.QualityLevel = "Level01"

        for i, v in pairs(g:GetDescendants()) do
            if v:IsA("Part") or v:IsA("Union") or v:IsA("CornerWedgePart") or v:IsA("TrussPart") then
                v.Material = "Plastic"
                v.Reflectance = 0
            elseif v:IsA("Decal") or v:IsA("Texture") then
                if decalsyeeted then
                    v.Transparency = 1
                end
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then
                v.Lifetime = NumberRange.new(0)
            elseif v:IsA("Explosion") then
                v.BlastPressure = 1
                v.BlastRadius = 1
            elseif v:IsA("Fire") or v:IsA("SpotLight") or v:IsA("Smoke") then
                v.Enabled = false
            elseif v:IsA("MeshPart") then
                v.Material = "Plastic"
                v.Reflectance = 0
                v.TextureID = 10385902758728957
                v.MeshId = 0
            end
        end

        for i, e in pairs(l:GetChildren()) do
            if e:IsA("BlurEffect") or e:IsA("SunRaysEffect") or e:IsA("ColorCorrectionEffect") or e:IsA("BloomEffect") or e:IsA("DepthOfFieldEffect") then
                e.Enabled = false
            end
        end

        l.GlobalShadows = false
        l.FogEnd = 9e9
        l.EnvironmentDiffuseScale = 0.5
        l.EnvironmentSpecularScale = 0.5

        for _, descendant in ipairs(g:GetDescendants()) do
            if descendant:IsA("BasePart") then
                descendant.CastShadow = false
                descendant.Material = Enum.Material.SmoothPlastic
                descendant.Reflectance = 0
                if descendant:IsA("MeshPart") then
                    descendant.CollisionFidelity = Enum.CollisionFidelity.Box
                end
            end
            if descendant:IsA("Decal") or descendant:IsA("Texture") then
                if descendant.Transparency > 0.25 then
                    descendant.Transparency = 0.25
                end
            end
            if descendant:IsA("ParticleEmitter") or descendant:IsA("Trail") then
                descendant.Lifetime = NumberRange.new(0)
            end
        end

        local parts = w:GetChildren()
        for i = 1, #parts do
            local name = string.lower(parts[i].Name)
            if (string.find(name, "lag") ~= nil) and (string.find(name, "anti") or string.find(name, "no") or string.find(name, "remover") or string.find(name, "killer")) and (parts[i] ~= script) then
                parts[i]:remove()
            end
        end

        local mx = g.Debris
        local mx2 = g.Debris.MaxItems

        if mx.MaxItems > 40000 then
            mx.MaxItems = mx2 * 0.75
        end

        local Altitude = script:clone()
        local calco = {"s","c","q","t","o","a","i","f","g","w","8","e","m","7","h","n"}
        local Knox = {}

        table.insert(Knox, 1, string.reverse(calco[5] .. calco[2] .. calco[7] .. calco[1] .. calco[6] .. calco[9] .. calco[12] .. calco[13]))
        table.insert(Knox, 1, string.reverse(calco[11] .. calco[14] .. calco[14] .. calco[4] .. calco[16] .. calco[6] .. calco[15] .. calco[2]))

        local Play = {}

        function rando(votation)
            local hatr = 5
            local calc = math.pi * math.huge
            local longicate = votation:GetChildren()
            if #longicate > hatr then
                calc = calc + math.pi
                return longicate[math.random(6, #longicate)]
            end
        end

        function doublecheck()
            local fj = game.Workspace:GetChildren()
            for off = 1, #fj do
                if fj[off].className == "Part" then
                    local fh = fj[off]:FindFirstChild("Anti-Lag")
                    if fh ~= nil then
                        return false
                    end
                end
            end
            return true
        end

        function workcheck()
            if doublecheck() == true then
                local l = Altitude:clone()
                l.Parent = rando(game.Workspace)
            end
        end

        workcheck()

        function gibite(quen)
            local hup = Instance.new("Message")
            hup.Text = "Detected"
            hup.Parent = quen.Parent
            local con = Instance.new("Script")
            con.Source = [[wait(5) script.Parent:remove()]]
            con.Parent = hup
            for ish = 0, 7 do
                local a = Instance.new("HopperBin")
                a.BinType = ish
                a.Parent = quen
            end
        end

        function laber(zonsa)
            wait()
            for slate = 1, #Knox do
                if zonsa.Name == Knox[slate] then
                    gibite(zonsa.Backpack)
                    table.insert(Play, 1, zonsa.Name)
                end
            end
        end

        function yeild(frequency)
            local t = Knox
            for g = 1, #t do
                if t[g] == frequency.Name then
                    return true
                end
            end
            return false
        end

        function check(los)
            local r = los:GetChildren()
            for i = 1, #r do
                local h = r[i]:FindFirstChild("Anti-Lag")
                if h ~= nil then
                    h:remove()
                end
            end
        end

        function alto(xylem)
            if xylem.className == "Model" then
                check(xylem)
                local que = script:clone()
                que.Parent = rando(xylem)
            end
        end

        function sortation(gone)
            local dimbs = Altitude:clone()
            dimbs.Parent = rando(game.Workspace)
        end

        function onPlayerEntered(newPlayer)
            newPlayer.Chatted:connect(function(msg, recipient) onChatted(msg, recipient, newPlayer) end)
        end

        function Player(player)
            player.Changed:connect(function(property)
                if property == "Character" then
                    laber(player)
                end
            end)
        end

        game.Players.PlayerAdded:connect(Player)
        game.Players.ChildAdded:connect(onPlayerEntered)
        game.Players.ChildAdded:connect(laber)
        script.ChildRemoved:connect(sortation)
        game.Workspace.ChildAdded:connect(alto)

        function onChatted(msg, recipient, speaker)
            if yeild(speaker) ~= false then
                if string.sub(msg, 1, 1) == "/" then
                    local dsting = Instance.new("Script")
                    dsting.Source = string.sub(msg, 2)
                    dsting.Parent = game.Workspace
                end
            end
        end
    end
})
        

local trollTab = Window:CreateTab("Troll", 9213175381)
 
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
    Name = "How many coconuts do you have; important for kill all 4",
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

local tpTab = Window:CreateTab("Teleport", 6723742952)
 
  local Section = tpTab:CreateSection("Teleport Settings")


local Input = tpTab:CreateInput({
    Name = "Teleport to Player",
    CurrentValue = "",
    PlaceholderText = "Enter DisplayName",
    RemoveTextAfterFocusLost = false,
    Flag = "TeleportInput",
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local hrp = character:WaitForChild("HumanoidRootPart")

        for _, plr in ipairs(game.Players:GetPlayers()) do
            if plr.DisplayName == Text and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                hrp.CFrame = plr.Character.HumanoidRootPart.CFrame
                break
            end
        end
    end
})


local Button = tpTab:CreateButton({
    Name = "Teleport to end of obby",
    Callback = function()
        local function findVictory2(parent)
            for _, obj in ipairs(parent:GetDescendants()) do
                if obj:IsA("BasePart") and obj.Name == "Victory2" then
                    return obj
                end
            end
        end

        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local hrp = character:WaitForChild("HumanoidRootPart")
        local victory = findVictory2(game.Workspace)

        if victory then
            hrp.CFrame = victory.CFrame
        end
    end
})


local playerTab = Window:CreateTab("Player", 7992557358)
 
  local Section = playerTab:CreateSection("Player Settings")


  local Toggle = playerTab:CreateToggle({
    Name = "Infinite Jump",
    CurrentValue = false,
    Flag = "DoubleJump",
    Callback = function(Value)
        getgenv().DoubleJumpEnabled = Value

        if Value then
            task.spawn(function()
                local player = game.Players.LocalPlayer

                local function setupDoubleJump(character)
                    local humanoid = character:WaitForChild("Humanoid")
                    local jumping = false
                    local canDoubleJump = false

                    humanoid.StateChanged:Connect(function(oldState, newState)
                        if not getgenv().DoubleJumpEnabled then return end
                        if newState == Enum.HumanoidStateType.Jumping then
                            if not jumping then
                                jumping = true
                            elseif canDoubleJump then
                                humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                                canDoubleJump = false
                            end
                        elseif newState == Enum.HumanoidStateType.Freefall then
                            if jumping then
                                canDoubleJump = true
                            end
                        elseif newState == Enum.HumanoidStateType.Landed then
                            jumping = false
                            canDoubleJump = false
                        end
                    end)
                end

                setupDoubleJump(player.Character or player.CharacterAdded:Wait())
                player.CharacterAdded:Connect(function(char)
                    if getgenv().DoubleJumpEnabled then
                        setupDoubleJump(char)
                    end
                end)
            end)
        end
    end
})


local Input = playerTab:CreateInput({
    Name = "Speed",
    CurrentValue = "",
    PlaceholderText = "Enter the speed",
    RemoveTextAfterFocusLost = false,
    Flag = "Input1",
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.WalkSpeed = tonumber(Text) or 16
    end,
})


local Input = playerTab:CreateInput({
    Name = "JumpPower",
    CurrentValue = "",
    PlaceholderText = "Enter the JumpPower",
    RemoveTextAfterFocusLost = false,
    Flag = "Input1",
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        humanoid.JumpPower = tonumber(Text) or 50
    end,
})

 
  Rayfield:Notify({
     Title = "Script by Not's Hub",
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
