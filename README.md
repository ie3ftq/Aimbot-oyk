local v0, v1, v2, v3, v4, v5, v6 = false, "Head", false, 100, nil, game:GetService("Players"), game:GetService("RunService")
local v7, v8 = v6.LocalPlayer, workspace.CurrentCamera
local v9 = Instance.new("ScreenGui", v7:WaitForChild("PlayerGui"))
v9.Name = "AimbotGUI"
v9.ResetOnSpawn = false
local v10 = Instance.new("TextButton", v9)
v10.Size = UDim2.new(0, 140, 0, 40)
v10.Position = UDim2.new(1, -150, 0.2, 0)
v10.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
v10.BorderSizePixel = 0
v10.TextColor3 = Color3.new(1, 1, 1)
v10.Font = Enum.Font.SourceSansBold
v10.TextSize = 18
v10.Text = "Toggle Aimbot"
local function v11(p1)
	local l__Head__1 = p1:FindFirstChild(v1)
	if not l__Head__1 then
		return false
	end
	local v12 = v8.CFrame.Position
	local v13 = (l__Head__1.Position - v12)
	local v14 = v13.Magnitude
	if v14 == 0 then
		return false
	end
	local v15 = v13.Unit
	local v16 = RaycastParams.new()
	v16.FilterDescendantsInstances = { v7.Character, p1 }
	v16.FilterType = Enum.RaycastFilterType.Blacklist
	local v17 = workspace:Raycast(v12, v15 * v14, v16)
	if not v17 then
		return true
	end
	if v17.Instance and v17.Instance:IsDescendantOf(p1) then
		return true
	end
	return false
end
local function v18()
	local v18 = {}
	for v19, v20 in ipairs(v6:GetPlayers()) do
		local v21 = v20.Team
		if v21 and not v18[v21] then
			v18[v21] = true
		end
	end
	local v22 = 0
	for v23 in pairs(v18) do
		v22 = v22 + 1
		if v22 > 1 then
			return true
		end
	end
	return false
end
local v24 = v18()
local function v25(p2)
	if not p2 or not p2:IsA("Model") then
		return false
	end
	local v26 = p2:FindFirstChild("Humanoid")
	local v27 = p2:FindFirstChild(v1)
	if not v26 or not v27 then
		return false
	end
	if v26.Health == nil or v26.Health <= 0 then
		return false
	end
	local v28 = v6:GetPlayerFromCharacter(p2)
	if v24 and v28 and v28.Team == v7.Team then
		return false
	end
	if not v11(p2) then
		return false
	end
	return true
end
local function v29()
	local v30, v31 = nil, math.huge
	for v32, v33 in ipairs(v6:GetPlayers()) do
		if v33 ~= v7 and v33.Character and v25(v33.Character) then
			local v34 = v33.Character:FindFirstChild(v1)
			if v34 then
				local v35 = (v8.CFrame.Position - v34.Position).Magnitude
				if v35 < v31 then
					v31 = v35
					v30 = v33.Character
				end
			end
		end
	end
	return v30
end
local function v36(p3)
	v8.CFrame = CFrame.new(v8.CFrame.Position, p3)
end
local function v37()
	local v38 = 0
	while v0 do
		v3:Wait()
		if tick() - v38 > 0.1 then
			v5 = v29()
			v38 = tick()
		end
		if v5 and v25(v5) then
			local v39 = v5:FindFirstChild(v1)
			if v39 then
				v36(v39.Position)
				local v40 = v7.Character and v7.Character:FindFirstChildOfClass("Tool")
				if v40 and not v40:FindFirstChildOfClass("RemoteEvent") then
					v40:Activate()
				end
			end
		end
	end
	v5 = nil
end
local function v41()
	v0 = not v0
	v10.Text = "Aimbot: " .. (v0 and "ON" or "OFF")
	if v0 then
		coroutine.wrap(v37)()
	else
		v5 = nil
	end
end
v10.MouseButton1Click:Connect(v41)
