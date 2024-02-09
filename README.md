-- ส่วนสำหรับส่งข้อมูลอัพสถานะ
local args = {
    [1] = "Melee",
    [2] = 1
}

game:GetService("ReplicatedStorage").Remotes.UpStats:FireServer(unpack(args))

-- ส่วนสำหรับทำให้ไม่เสียคะแนน
if not _G.XYLONOPOINTLOSING then
    _G.XYLONOPOINTLOSING = true

    local StatusIncreaseAmount = 0
    local RemoteEvent = game:GetService("ReplicatedStorage").Remotes.UpStats

    local function FireServerWithoutLosingPoints(...)
        local args = { ... }
        
        if args[1] >= 1 and args[1] <= 150 then
            StatusIncreaseAmount = args[1]
        elseif args[1] >= 1 and args[1] > 150 then
            StatusIncreaseAmount = 150
        end
        
        args[1] = 0
        
        for i = 1, StatusIncreaseAmount do
            RemoteEvent:FireServer(unpack(args))
            wait(0.5)
        end
    end

    -- แทนที่ RemoteEvent:FireServer ด้วยฟังก์ชันที่ไม่เสียคะแนน
    local oldFireServer = RemoteEvent.FireServer
    RemoteEvent.FireServer = function(self, ...)
        if self == RemoteEvent then
            FireServerWithoutLosingPoints(...)
        else
            return oldFireServer(self, ...)
        end
    end
end
