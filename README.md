local PathFindingService = game:GetService("PathfindingService")
local Players = game:GetService("Players")

local debounce = false

local Mobs = workspace.Mobs

while true do wait()
	for _, player in pairs(Players:GetChildren()) do
		for i,Mob in pairs(Mobs:GetChildren()) do
			if Mob:FindFirstChild(tostring(Mob).."_Module") then
				local hum = Mob.Humanoid
				local hrp = Mob:WaitForChild("HumanoidRootPart")
				local EnemyModule = require(Mob:FindFirstChild(tostring(Mob).."_Module"))

				local Damage = EnemyModule.damage

				if player.Character then
					local target = player.Character
					local path = PathFindingService:CreatePath()

					if target then
						path:ComputeAsync(hrp.Position,target.HumanoidRootPart.Position)

						local waypoints = path:GetWaypoints()

						for i,waypoint in pairs(waypoints) do
							local distance = (hrp.Position - target.HumanoidRootPart.Position).Magnitude

							if target and target.Humanoid.Health > 0 then
								if distance > 6 then

									hum:MoveTo(waypoint.Position)

									hum.MoveToFinished:Wait()

									local waypointDistance = (hrp.Position - waypoints[#waypoints].Position).Magnitude
									local playerDistance = (hrp.Position - target.HumanoidRootPart.Position).Magnitude


									if waypointDistance - 1 >= playerDistance then
										break
									end
								else
									if debounce == false then
										debounce = true

										target.Humanoid.Health -= Damage

										task.wait(1.5)
										debounce = false
									end
								end
							end
						end
					end
				end
			end
		end
	end
end
