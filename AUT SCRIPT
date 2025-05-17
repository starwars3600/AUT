-- Load Rayfield UI Library
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Store NPC names and current selection
local npcNames = {}
local selectedNPC = nil
local Dropdown = nil
local toggleActive = false
local autoChestsActive = false

-- Function to scan for NPCs
local function scanNPCs()
    npcNames = {}

    local livingFolder = workspace:FindFirstChild("Living")
    if not livingFolder then
        warn("workspace.Living not found!")
        return
    end

    for _, obj in ipairs(livingFolder:GetChildren()) do
        if obj:IsA("Model") then
            table.insert(npcNames, obj.Name)
        end
    end

    if Dropdown then
        Dropdown:Refresh(npcNames, true)
    end
end

-- Initial scan
scanNPCs()

-- Create Rayfield Window
local Window = Rayfield:CreateWindow({
    Name = "NPC Scanner GUI",
    Icon = 0,
    LoadingTitle = "Rayfield Interface Suite",
    LoadingSubtitle = "by Sirius",
    Theme = "Default",
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "NPC Scanner Config"
    },
    Discord = {
        Enabled = false,
        Invite = "noinvitelink",
        RememberJoins = true
    },
    KeySystem = false
})

-- Create Tabs
local Tab = Window:CreateTab("Scanner", 4483362458)
local TeleportTab = Window:CreateTab("Teleports", 4483362458)
local ReadmeTab = Window:CreateTab("README", 4483362458)

-- Dropdown to select an NPC
Dropdown = Tab:CreateDropdown({
    Name = "NPCs in workspace.Living",
    Options = npcNames,
    CurrentOption = npcNames[1] and {npcNames[1]} or {"None"},
    MultipleOptions = false,
    Flag = "NPCDropdown",
    Callback = function(Options)
        selectedNPC = Options[1]
    end,
})

-- Button to rescan NPCs
Tab:CreateButton({
    Name = "Rescan NPCs",
    Callback = function()
        scanNPCs()
        Rayfield:Notify({
            Title = "Scan Complete",
            Content = "Updated NPC list.",
            Duration = 3
        })
    end,
})

-- Button to teleport to selected NPC
Tab:CreateButton({
    Name = "Teleport to NPC",
    Callback = function()
        if not selectedNPC then
            Rayfield:Notify({
                Title = "Teleport Failed",
                Content = "No NPC selected.",
                Duration = 3
            })
            return
        end

        local npc = workspace:FindFirstChild("Living"):FindFirstChild(selectedNPC)
        if npc then
            local player = game.Players.LocalPlayer
            if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = npc:GetPivot() * CFrame.new(0, 5, 0)
                Rayfield:Notify({
                    Title = "Teleported",
                    Content = "Teleported to " .. selectedNPC,
                    Duration = 3
                })
            end
        else
            Rayfield:Notify({
                Title = "Teleport Failed",
                Content = "NPC not found.",
                Duration = 3
            })
        end
    end,
})

-- Toggle to auto teleport & attack
Tab:CreateToggle({
    Name = "Auto Teleport & Click",
    CurrentValue = false,
    Flag = "AutoTeleportClick",
    Callback = function(Value)
        toggleActive = Value

        if toggleActive then
            spawn(function()
                local player = game.Players.LocalPlayer
                local mouse = player:GetMouse()

                while toggleActive do
                    if selectedNPC then
                        local npc = workspace:FindFirstChild("Living"):FindFirstChild(selectedNPC)
                        if npc and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            player.Character.HumanoidRootPart.CFrame = npc:GetPivot() * CFrame.new(0, 5, 0)
                            mouse1click()
                        end
                    end
                    wait(0.1)
                end
            end)
        end
    end,
})

-- Teleports dropdown and button
local teleportLocations = {
    ["Shibuya"] = workspace:WaitForChild("MappedRegions"):WaitForChild("Shibuya"),
    ["Center City"] = workspace:WaitForChild("MappedRegions"):WaitForChild("Center City"),
    ["Center Wilds"] = workspace:WaitForChild("MappedRegions"):WaitForChild("Center Wilds"),
}
local selectedLocation = "Shibuya"

TeleportTab:CreateDropdown({
    Name = "Select Location",
    Options = {"Shibuya", "Center City", "Center Wilds"},
    CurrentOption = {"Shibuya"},
    MultipleOptions = false,
    Flag = "TeleportDropdown",
    Callback = function(Options)
        selectedLocation = Options[1]
    end,
})

TeleportTab:CreateButton({
    Name = "Teleport",
    Callback = function()
        local loc = teleportLocations[selectedLocation]
        local player = game.Players.LocalPlayer
        if loc and player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = loc:GetPivot() + Vector3.new(0, 5, 0)
            Rayfield:Notify({
                Title = "Teleported",
                Content = "Teleported to " .. selectedLocation,
                Duration = 3
            })
        else
            Rayfield:Notify({
                Title = "Teleport Failed",
                Content = "Location not found or character missing.",
                Duration = 3
            })
        end
    end,
})

-- Auto Chest Toggle
Tab:CreateToggle({
    Name = "Auto Chests",
    CurrentValue = false,
    Flag = "AutoChests",
    Callback = function(Value)
        autoChestsActive = Value

        if autoChestsActive then
            spawn(function()
                local player = game.Players.LocalPlayer
                local virtualInput = game:GetService("VirtualInputManager")
                local chestNames = { "Common_Chest", "Rare_Chest", "Epic_Chest" }

                while autoChestsActive do
                    for _, chestName in ipairs(chestNames) do
                        local chest = workspace:FindFirstChild(chestName)
                        if chest and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            player.Character.HumanoidRootPart.CFrame = chest:GetPivot() + Vector3.new(0, 3, 0)

                            -- Press E to interact
                            virtualInput:SendKeyEvent(true, Enum.KeyCode.E, false, game)
                            virtualInput:SendKeyEvent(false, Enum.KeyCode.E, false, game)

                            -- Click repeatedly for 4 seconds
                            local screenSize = workspace.CurrentCamera.ViewportSize
                            local x, y = screenSize.X / 2, screenSize.Y - 5

                            local endTime = tick() + 4
                            while tick() < endTime do
                                virtualInput:SendMouseButtonEvent(x, y, 0, true, game, 0)
                                virtualInput:SendMouseButtonEvent(x, y, 0, false, game, 0)
                                wait(0.1)
                            end

                            task.wait(1.5)
                        end
                    end
                    task.wait(0.5)
                end
            end)
        end
    end,
})

-- README info tab
ReadmeTab:CreateParagraph({
    Title = "SUPER IMPORTANT MUST READ",
    Content = [[
AUTOFARM USE: The ~ button next to 1 on your keyboard makes you autofarm thugs automatically. DO NOT turn on the autofarm toggle at the same time as you have pressed ~. Press ~ again to turn off the thug autofarm.

Use Auto Chests while autofarming and put your mouse at the bottom of the screen (where the SelectAll and Close buttons are).

DO NOT USE UI while autofarming — your mouse will click everything. Move your cursor away FAST when turning it on.

Mobile compatibility: probably NOT.

<THIS IS WIP SO DONT EXPECT IT TO BE THAT GOOD>
    ]]
})

-- Black Market Teleport
Tab:CreateButton({
    Name = "Teleport to Blackmarket Dealer",
    Callback = function()
        local dealer = workspace:FindFirstChild("NPCS") and workspace.NPCS:FindFirstChild("Black Market")
        local player = game.Players.LocalPlayer

        if dealer and player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = dealer:GetPivot() + Vector3.new(0, 3, 0)
            Rayfield:Notify({
                Title = "Teleported",
                Content = "Teleported to Black Market dealer.",
                Duration = 3
            })
        else
            Rayfield:Notify({
                Title = "Black Market Not Found",
                Content = "Could not find NPCS.Black Market in workspace.",
                Duration = 3
            })
        end
    end,
})
