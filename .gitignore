--          [[ Champion ]]
if GetObjectName(GetMyHero()) ~= "Teemo" then return end
--          [[ Updater ]]
local ver = "0.02"

function AutoUpdate(data)
    if tonumber(data) > tonumber(ver) then
        print("New version found! " .. data)
        print("Downloading update, please wait...")
        DownloadFileAsync("https://raw.githubusercontent.com/janilssonn/GoS/master/Teemo.lua", SCRIPT_PATH .. "Teemo.lua", function() print("Update Complete, please 2x F6!") return end)
    end
end

GetWebResultAsync("https://raw.githubusercontent.com/janilssonn/GoS/master/Version/Teemo.version", AutoUpdate)
--          [[ Lib ]]
require ("OpenPredict")
require ("DamageLib")
--          [[ Menu ]]
local TeemoMenu = Menu("Teemo", "Teemo")
--          [[ Combo ]]
TeemoMenu:SubMenu("Combo", "Combo Settings")
TeemoMenu.Combo:Boolean("Q", "Use Q", true)
TeemoMenu.Combo:Boolean("W", "Use W", true)
TeemoMenu.Combo:Boolean("WA", "Use W on Ally", false)
TeemoMenu.Combo:Slider("WM", "Use W on HP", 50, 1, 100, 1)
TeemoMenu.Combo:Slider("WMA", "No Options here", 1, 1, 5, 1)
TeemoMenu.Combo:Boolean("E", "Use E", true)
--          [[ Harass ]]
TeemoMenu:SubMenu("Harass", "Harass Settings")
TeemoMenu.Harass:Boolean("Q", "Use Q", true)
TeemoMenu.Harass:Boolean("E", "Use E", true)
TeemoMenu.Harass:Slider("Mana", "Min. Mana", 40, 0, 100, 1)
--          [[ LaneClear ]]
TeemoMenu:SubMenu("Farm", "Farm Settings")
TeemoMenu.Farm:Boolean("Q", "Use Q", false)
TeemoMenu.Farm:Boolean("E", "Use E", true)
TeemoMenu.Farm:Slider("Mana", "Min. Mana", 40, 0, 100, 1)
--          [[ Jungle ]]
TeemoMenu:SubMenu("JG", "Jungle Settings")
TeemoMenu.JG:Boolean("Q", "Use Q", true)
TeemoMenu.JG:Boolean("E", "Use E", true)
--          [[ KillSteal ]]
TeemoMenu:SubMenu("KS", "KillSteal Settings")
TeemoMenu.KS:Boolean("Q", "Use Q", true)
TeemoMenu.KS:Boolean("E", "Use E", true)
TeemoMenu.KS:Boolean("R", "Use R", true)
--          [[ Draw ]]
TeemoMenu:SubMenu("Draw", "Drawing Settings")
TeemoMenu.Draw:Boolean("Q", "Draw Q", false)
TeemoMenu.Draw:Boolean("W", "Draw W", false)
TeemoMenu.Draw:Boolean("E", "Draw E", false)
--          [[ Spell ]]
local Spells = {
 Q = {range = 1175, delay = 0.25, speed = 1200, width = 70},
 W = {range = 1075, delay = 0.25, speed = 1300, width = 95},
 E = {range = 1100, delay = 0.25, speed = 1300, radius = 330},
 R = {range = 3340, delay = 1.0, speed = math.huge, width = 190}
}
--          [[ Orbwalker ]]
function Mode()
	if _G.IOW_Loaded and IOW:Mode() then
		return IOW:Mode()
	elseif _G.PW_Loaded and PW:Mode() then
		return PW:Mode()
	elseif _G.DAC_Loaded and DAC:Mode() then
		return DAC:Mode()
	elseif _G.AutoCarry_Loaded and DACR:Mode() then
		return DACR:Mode()
	elseif _G.SLW_Loaded and SLW:Mode() then
		return SLW:Mode()
	end
end
--          [[ Tick ]]
OnTick(function()
	KS()
	target = GetCurrentTarget()
	         Combo()
	         Harass()
	         Farm()
	    end)  
--          [[ TeemoQ ]]
function TeemoQ()	
	local QPred = GetPrediction(target, Spells.Q)
	if QPred.hitChance > 0.3 then
		CastSkillShot(_Q, QPred.castPos)
	end	
end   
--          [[ TeemoW ]]
function TeemoW()
		CastSkillShot(_W, myHero.pos)
	end	
--          [[ TeemoE ]]
function TeemoE()
	local EPred = GetCircularAOEPrediction(target, Spells.E)
	if EPred.hitChance > 0.3 then
		CastSkillShot(_E, EPred.castPos)
	end	
end  
--          [[ TeemoR ]]
function TeemoR()
	local RPred = GetPrediction(target, Spells.R)
	if RPred.hitChance > 0.8 then
		CastSkillShot(_R, RPred.castPos)
	end	
end  
--          [[ Combo ]]
function Combo()
	if Mode() == "Combo" then
--		[[ Use Q ]]
		if TeemoMenu.Combo.Q:Value() and Ready(_Q) and ValidTarget(target, Spells.Q.range) then
			TeemoQ()
		end	
--		[[ Use W ]]
		if Ready(_W) and GetPercentHP(myHero) <= TeemoMenu.Combo.WM:Value() and TeemoMenu.Combo.W:Value() and EnemiesAround(myHero, Spells.W.range) >= TeemoMenu.Combo.WMA:Value() then
			TeemoW()
			end
-- 		[[ Use E ]]
		if TeemoMenu.Combo.E:Value() and Ready(_E) and ValidTarget(target, Spells.E.range) then
			TeemoE()
		end
-- 		[[ Use R ]]
		--[[if TeemoMenu.Combo.R:Value() and Ready(_R) and ValidTarget(target, Spells.R.range) then
			TeemoR()
		end]]
	end
end
--          [[ Harass ]]
function Harass()
	if Mode() == "Harass" then
		if (myHero.mana/myHero.maxMana >= TeemoMenu.Harass.Mana:Value() /100) then
-- 			[[ Use Q ]]
			if TeemoMenu.Harass.Q:Value() and Ready(_Q) and ValidTarget(target, Spells.Q.range) then
				TeemoQ()
			end
-- 			[[ Use E ]]
			if TeemoMenu.Harass.E:Value() and Ready(_E) and ValidTarget(target, Spells.E.range) then
				TeemoE()
			end
		end
	end
end
--          [[ LaneClear ]]
function Farm()
	if Mode() == "LaneClear" then
		if (myHero.mana/myHero.maxMana >= TeemoMenu.Farm.Mana:Value() /100) then
-- 			[[ Lane ]]
			for _, minion in pairs(minionManager.objects) do
				if GetTeam(minion) == MINION_ENEMY then
-- 					[[ Use Q ]]
					if TeemoMenu.Farm.Q:Value() and Ready(_Q) and ValidTarget(minion, Spells.Q.range) then
							CastSkillShot(_Q, minion)
					    end
-- 					[[ Use E ]]
					if TeemoMenu.Farm.E:Value() and Ready(_E) and ValidTarget(minion, Spells.E.range) then
							CastSkillShot(_E, minion)
						end	
					end
				end	
-- 			[[ Jungle ]]
			for _, mob in pairs(minionManager.objects) do
				if GetTeam(mob) == MINION_JUNGLE then
-- 					[[ Use Q ]]
					if TeemoMenu.JG.Q:Value() and Ready(_Q) and ValidTarget(mob, Spells.Q.range) then
							CastSkillShot(_Q, mob)
						end
-- 					[[ Use E ]]
					if TeemoMenu.JG.E:Value() and Ready(_E) and ValidTarget(mob, Spells.E.range) then
							CastSkillShot(_E, mob)
						end	
					end
				end
			end
		end
	end
--          [[ KillSteal ]]
function KS()
	for _, enemy in pairs(GetEnemyHeroes()) do
-- 		[[ Use Q ]]
		if TeemoMenu.KS.Q:Value() and Ready(_Q) and ValidTarget(enemy, Spells.Q.range) then
			if GetCurrentHP(enemy) < getdmg("Q", enemy, myHero) then
				TeemoQ()
				end
			end

-- 		[[ Use E ]]
		if TeemoMenu.KS.E:Value() and Ready(_E) and ValidTarget(enemy, Spells.E.range) then
			if GetCurrentHP(enemy) < getdmg("E", enemy, myHero) then
				TeemoE()
				end
			end

--		[[ Use R ]]
		if TeemoMenu.KS.R:Value() and Ready(_R) and ValidTarget(enemy, Spells.R.range) then
			if GetCurrentHP(enemy) < getdmg("R", enemy, myHero) then
					TeemoR()
				end
			end
		end
	end
--          [[ Drawings ]]
OnDraw(function(myHero)
	local pos = GetOrigin(myHero)
--  [[ Draw Q ]]
	if TeemoMenu.Draw.Q:Value() then DrawCircle(pos, Spells.Q.range, 0, 25, GoS.Red) end
--  [[ Draw W ]]
	if TeemoMenu.Draw.W:Value() then DrawCircle(pos, Spells.W.range, 0, 25, GoS.Blue) end
--  [[ Draw E ]]
	if TeemoMenu.Draw.E:Value() then DrawCircle(pos, Spells.E.range, 0, 25, GoS.Green) end
end)		
