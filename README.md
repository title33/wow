
local StatusRemote = game:GetService("ReplicatedStorage").Remote.Event.CommE_

local function IncreaseStats(amount)
    local originalPoints = 0

    local originalFireServer = StatusRemote.FireServer
    StatusRemote.FireServer = newcclosure(function(remote, ...)
        local args = {...}
        local method = getnamecallmethod()

        if not checkcaller() and method == "FireServer" and remote == StatusRemote then
            if args[1] == "Add Stats" then
                originalPoints = tonumber(args[3]) or 0
                args[3] = tostring(amount)
            end
        end

        return originalFireServer(remote, unpack(args))
    end)
end

IncreaseStats(0)  -- Set the amount you want to increase

-- Example usage:
-- To increase stats without losing points
StatusRemote:FireServer("Add Stats", "Strength", "1")
