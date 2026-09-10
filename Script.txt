local Rayfield = loadstring(game:HttpGet("https://sirius.menu/gen2"))()
local window = Rayfield:CreateWindow({
    name = "Immie Hub",
    subtitle = "Climb Worm Tower For Brainrots",
    sidebarLayout = true,
})

local tab = window:CreateTab({ name = "Home", icon = 93364949241311 })

tab:CreateButton({
    name = "Teleport to secret",
    callback = function()
        window:Notify({ title = "Teleport", content = "Teleported to Secret!" })
        -- Ensure the environment is ready
        local Players = game:GetService("Players")
local Player = Players.LocalPlayer

-- Safe path finding to avoid execution timeout
local world = workspace:WaitForChild("World", 5)
if not world then return warn("Test Failed: 'World' model not found in Workspace") end

local platform = world:WaitForChild("Platform", 5)
local content = platform and platform:WaitForChild("Content", 5)
local secretFolder = content and content:WaitForChild("Secret", 5)

if not secretFolder then 
    return warn("Test Failed: Complete path to 'Secret' folder could not be resolved.") 
end

-- Immediate Execution Sequence
local function executeImmediateTeleport()
    local children = secretFolder:GetChildren()
    local target = children[81] -- Target the specific 81st element

    if target and Player.Character then
        local rootPart = Player.Character:FindFirstChild("HumanoidRootPart")
        if rootPart then
            local targetCFrame
            if target:IsA("BasePart") then
                targetCFrame = target.CFrame
            elseif target:IsA("Model") then
                targetCFrame = target:GetPivot()
            end

            if targetCFrame then
                -- Apply the CFrame shift immediately
                rootPart.CFrame = targetCFrame + Vector3.new(0, 3, 0)

                print("Test Action: Teleporting player immediately to index 81 location.")
            else
                warn("Test Failed: Target object has no valid CFrame or Pivot.")
            end
        else
            warn("Test Failed: Character's HumanoidRootPart is missing.")
        end
    else
        warn("Test Failed: The 81st item in the Secret folder does not exist.")
    end
end

-- Fire the logic right away on script run
executeImmediateTeleport()
    end,
})

tab:CreateButton({
    name = "Noclip",
    callback = function()
        window:Notify({ title = "Noclip turned on!", content = "Collisions disabled." })
        
        local RunService = game:GetService("RunService")
        local Players = game:GetService("Players")
        local Player = Players.LocalPlayer

        -- Connect to the frame step to constantly disable collisions
        RunService.Stepped:Connect(function()
            if Player.Character then
                for _, part in ipairs(Player.Character:GetDescendants()) do
                    if part:IsA("BasePart") and part.CanCollide then
                        part.CanCollide = false
                    end
                end
            end
        end)
    end -- This closes the function
}) -- This properly closes the table and the CreateButton call

tab:CreateButton({
    name = "Remove 67 worm",
    callback = function()
        window:Notify({ title = "worm deleted", content = "67 worm removed!" })
        
        local Players = game:GetService("Players")
        local Player = Players.LocalPlayer

        -- Trace down the directory path safely
        local playerScripts = Player:WaitForChild("PlayerScripts", 5)
        local tsFolder = playerScripts and playerScripts:WaitForChild("TS", 5)
        local controllersFolder = tsFolder and tsFolder:WaitForChild("controllers", 5)
        local snakeFolder = controllersFolder and controllersFolder:FindFirstChild("snake")

        if snakeFolder then
            snakeFolder:Destroy()
            print("Runtime Diagnostics: Successfully deleted the 'snake' folder and all children.")
        else
            warn("Runtime Diagnostics: Target folder 'snake' could not be found or was already removed.")
        end
    end
})


tab:CreateSlider({
    name = "Walkspeed",
    range = { 20, 100 },
    increment = 1,
    value = 90,
    suffix = " studs",
    callback = function(value)
        -- Safe check to ensure player character and humanoid exist before setting value
        local character = game:GetService("Players").LocalPlayer.Character
        if character then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = value
            end
        end
    end, -- Make sure this comma exists to separate the callback from the closing bracket
})
