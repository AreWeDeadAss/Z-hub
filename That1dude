local Rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local lp, cg, plrs = game.Players.LocalPlayer, game.CoreGui, game.Players

local Window = Rayfield:CreateWindow({
    Name = "z hub by that1dude",
    LoadingTitle = "Z hub",
    LoadingSubtitle = "( loading z hub.. )",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "MyHub",
        FileName = "Settings"
    }
})

-- 🔍 ESP Tab
local espTab = Window:CreateTab("ESP", 4483362458)
local espFolder = Instance.new("Folder", cg) espFolder.Name = "ESP"
local function drawESP(p)
    if p ~= lp and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
        local b = Instance.new("BoxHandleAdornment", espFolder)
        b.Adornee, b.Size, b.Transparency, b.AlwaysOnTop, b.ZIndex, b.Color3 =
            p.Character.HumanoidRootPart, Vector3.new(4, 6, 4), 0.5, true, 5, Color3.fromRGB(255, 0, 0)
    end
end
local function clearESP() for _, v in ipairs(espFolder:GetChildren()) do v:Destroy() end end
espTab:CreateToggle({
    Name = "Enable ESP",
    CurrentValue = false,
    Callback = function(on)
        clearESP()
        if on then
            for _, p in ipairs(plrs:GetPlayers()) do drawESP(p) end
            plrs.PlayerAdded:Connect(function(p)
                p.CharacterAdded:Connect(function() task.wait(1) drawESP(p) end)
            end)
        end
    end
})

-- 🚀 Movement Tab
local moveTab = Window:CreateTab("Movement", 4483362458)
moveTab:CreateSlider({
    Name = "WalkSpeed",
    Range = {16, 150},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(v)
        local function setSpeed()
            if lp.Character and lp.Character:FindFirstChildOfClass("Humanoid") then
                lp.Character:FindFirstChildOfClass("Humanoid").WalkSpeed = v
            end
        end
        setSpeed()
        lp.CharacterAdded:Connect(function(c)
            c:WaitForChild("Humanoid")
            setSpeed()
        end)
    end
})

-- 📜 Script Loader Tab
local scriptTab = Window:CreateTab("Script Loader", 4483362458)
local userScript = ""
scriptTab:CreateInput({
    Name = "Paste Script",
    PlaceholderText = "-- print('Hello')",
    RemoveTextAfterFocusLost = false,
    Callback = function(txt) userScript = txt end
})
scriptTab:CreateButton({
    Name = "Execute Script",
    Callback = function()
        if userScript ~= "" then
            local ok, err = pcall(function()
                local f = loadstring(userScript)
                if f then f() end
            end)
            Rayfield:Notify({
                Title = ok and "Executed!" or "Error",
                Content = ok and "Script ran." or err,
                Duration = 4
            })
        end
    end
})

-- 🥊 Combat Tab (Hitbox Expander)
local combatTab = Window:CreateTab("Combat", 4483362458)
local size, enabled = Vector3.new(2, 2, 2), false
local function expand()
    for _, p in pairs(plrs:GetPlayers()) do
        if p ~= lp and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local part = p.Character.HumanoidRootPart
            part.Size, part.Transparency, part.Material, part.CanCollide =
                size, 0.5, Enum.Material.ForceField, false
        end
    end
end
combatTab:CreateSlider({
    Name = "Hitbox Size",
    Range = {2, 10},
    Increment = 1,
    CurrentValue = 2,
    Callback = function(v)
        size = Vector3.new(v, v, v)
        if enabled then expand() end
    end
})
combatTab:CreateToggle({
    Name = "Enable Hitbox Expander",
    CurrentValue = false,
    Callback = function(on)
        enabled = on
        if on then
            expand()
            plrs.PlayerAdded:Connect(function(p)
                p.CharacterAdded:Connect(function()
                    task.wait(1)
                    if enabled then expand() end
                end)
            end)
        else
            for _, p in pairs(plrs:GetPlayers()) do
                if p ~= lp and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    local hrp = p.Character.HumanoidRootPart
                    hrp.Size, hrp.Transparency, hrp.Material =
                        Vector3.new(2, 2, 1), 0, Enum.Material.Plastic
                end
            end
        end
    end
})

Rayfield:LoadConfiguration()
