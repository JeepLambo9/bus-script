local Bus = script.Parent -- Reference to the bus model
local Speed = 20 -- Adjust the speed as necessary
local TurnSpeed = 15 -- Adjust turning speed as necessary
local DriverSeat = Bus:WaitForChild("DriverSeat") -- Assuming there is a part named DriverSeat

-- Function to move the bus forward or backward
local function Move(direction)
    Bus.Velocity = Bus.CFrame.LookVector * Speed * direction
end

-- Function to turn the bus left or right
local function Turn(direction)
    Bus.CFrame = Bus.CFrame * CFrame.Angles(0, math.rad(TurnSpeed * direction), 0)
end

-- Bind controls to the driver seat
DriverSeat.OnSeatOccupied:Connect(function()
    game:GetService("UserInputService").InputBegan:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.W then
            Move(1) -- Forward
        elseif input.KeyCode == Enum.KeyCode.S then
            Move(-1) -- Backward
        elseif input.KeyCode == Enum.KeyCode.A then
            Turn(-1) -- Left
        elseif input.KeyCode == Enum.KeyCode.D then
            Turn(1) -- Right
        end
    end)

    game:GetService("UserInputService").InputEnded:Connect(function(input)
        if input.KeyCode == Enum.KeyCode.W or input.KeyCode == Enum.KeyCode.S then
            Move(0) -- Stop moving forward/backward
        elseif input.KeyCode == Enum.KeyCode.A or input.KeyCode == Enum.KeyCode.D then
            Turn(0) -- Stop turning
        end
    end)
end)

DriverSeat.OnSeatVacated:Connect(function()
    -- Stop all bus movement and turning when the driver leaves the seat
    Move(0)
    Turn(0)
end)
