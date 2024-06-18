local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()

local Mobs_Table = {}
for _, v in ipairs(workspace.Mob:GetDescendants()) do
    if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and not table.find(Mobs_Table, v.Name) then
        table.insert(Mobs_Table, v.Name)
        table.sort(Mobs_Table)
    end
end

local X = Material.Load({
    Title = "Rock Fruit 2",
    Style = 1,
    SizeX = 350,
    SizeY = 350,
    Theme = "Dark",
    ColorOverrides = {
        MainFrame = Color3.fromRGB(0,0,0),
        Toggle = Color3.fromRGB(124,37,255),
        ToggleAccent = Color3.fromRGB(255,255,255), 
        Dropdown = Color3.fromRGB(124,37,255),
		DropdownAccent = Color3.fromRGB(255,255,255),
        Slider = Color3.fromRGB(124,37,255),
		SliderAccent = Color3.fromRGB(255,255,255),
        NavBarAccent = Color3.fromRGB(0,0,0),
        Content = Color3.fromRGB(0,0,0),
    }
})

local Y = X.New({
    Title = "Main"
})

local ii = Y.Dropdown({
    Text = "Select Mobs!",
    Callback = function(Value)
        getgenv().mobname = Value
end,
    Options = Mobs_Table
})

local function getNPC()
    local dist, thing = math.huge
    for i, v in pairs(workspace.Mob:GetDescendants()) do
        if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") and v.Name == mobname then
            local mag = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - v.Parent.Position).magnitude
            if mag < dist then
                dist = mag
                thing = v
            end
        end
    end
    return thing
end
Y.Toggle({
    Text = "Auto [Mobs]",
    Callback = function(Value)
        b = Value
        while b do task.wait()
                    pcall(function()
                    
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = getNPC().UpperTorso.CFrame
            end)
        end
	end,
    Enabled = false
})
Y.Toggle({
    Text = "Auto [Attack]",
    Callback = function(Value)
        a = Value
        while a do task.wait()
            local toolName = "Combat" -- Replace "YourToolNameHere" with the name of your tool
    
    local LocalPlayer = game:GetService("Players").LocalPlayer
for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
    if v:IsA('Tool') and v.Name == toolName then
        v.Parent = LocalPlayer.Character
        break -- Stop the loop after picking up the tool
    end
end
LocalPlayer.Character:FindFirstChild(toolName):Activate()
        end
	end,
    Enabled = false
})
