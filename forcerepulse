--------------------------------------------------- [ DECLARATIONS ] ---------------------------------------------------
local ply = LocalPlayer()

local wep = ply:GetActiveWeapon()

local bToggle = false

local bToggleDelay = false

local bCounterDelay = false

local bResetDelay = false

local iCharges = 0

--------------------------------------------------- [ MAIN LOGIC ] --------------------------------------------------- check if load script with other gun and then use lightsaber
function isValidWep()

    if (IsValid(ply) && ply:Alive()) then 
        if (ply:GetActiveWeapon():GetPrintName() == "Lightsaber") then 
            wep = ply:GetActiveWeapon()
            return true
        end
    end

    iCharges = 0

    return false
end

hook.Add("Think", "UpdateValues", function() 

    if ( !IsValid(ply) ) then 
        ply = LocalPlayer()
    end

    if (isValidWep()) then 
        wep = ply:GetActiveWeapon()
    end

    if (input.IsKeyDown(KEY_PAD_5) && !bToggleDelay ) then 

        bToggleDelay = !bToggleDelay

        bToggle = !bToggle

        timer.Simple( 0.5, function() bToggleDelay = !bToggleDelay end)
    end

    if (input.IsKeyDown(KEY_PAD_6) && !bResetDelay ) then 

        bResetDelay = !bResetDelay

        iCharges = 0

        timer.Simple( 0.5, function() bResetDelay = !bResetDelay end)
    end

    if (!ply:Alive()) then 
        iCharges = 0
    end

end)

function GetForce()
    if (isValidWep()) then 
        return wep:GetForce()
    end 

    return 0
end

function IncreaseCounter()
    if (!bCounterDelay) then
        bCounterDelay = !bCounterDelay
        iCharges = iCharges + 1
        timer.Simple( 4, function() bCounterDelay = !bCounterDelay end)
    end
end

hook.Add("Think", "LoadRepulse", function()

local force = GetForce()

if (bToggle) then 
    if (isValidWep()) then
        if ( force >= 100 ) then 
            RunConsoleCommand("rb655_select_force", "3") 
            RunConsoleCommand("+attack2", 1)
        elseif ( force < 3  ) then 
            RunConsoleCommand("rb655_select_force", "2")
            RunConsoleCommand("-attack2", 1)
            IncreaseCounter()
        end
    end
else 
    RunConsoleCommand("-attack2", 1)
end

end)

--------------------------------------------------- [ HUD ] ---------------------------------------------------

local szStatusText = ""
local cColor = Color(255, 255, 255, 255)

surface.CreateFont( "ScriptRepulseFont", {
	font = "Arial",
	extended = false,
	size = 13,
	weight = 500,
	antialias = false,
	outline = true,
} )

hook.Add("HUDPaint", "HelloThere", function() 
    if ( bToggle == true ) then 
        szStatusText = "Currently activated!"
        cColor = Color(0, 255, 0, 255)
    else
        szStatusText = "Currently deactivated"
        cColor = Color(255, 0, 0, 255)
    end

    draw.DrawText("Press [NUMPAD5] to toggle the script!", "ScriptRepulseFont", ScrW() * 0.5, ScrH() * 0.85, Color(255, 255, 255, 255), TEXT_ALIGN_CENTER)
    draw.DrawText("Press [NUMPAD6] to reset. Amount of Charges: " .. iCharges, "ScriptRepulseFont", ScrW() * 0.5, ScrH() * 0.87, Color(255, 255, 255, 255), TEXT_ALIGN_CENTER)
	draw.DrawText(szStatusText, "ScriptRepulseFont", ScrW() * 0.5, ScrH() * 0.89, cColor, TEXT_ALIGN_CENTER)
end)
