local args = {
    [1] = "Add Stats",
    [2] = "Strength",
    [3] = 1,
}

local StatusIncreaseAmount = math.min(args[3], 150)

game:GetService("ReplicatedStorage").Remote.Event.CommE_:FireServer(unpack(args))

for i = 1, StatusIncreaseAmount do
    wait(0.5)
    game:GetService("ReplicatedStorage").Remote.Event.CommE_:FireServer("Add Stats", "Strength", 1)
end
