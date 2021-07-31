# Scrap-Mechanic-Issue
I need to be able to change the effect of the sledgehammer so that it can actually deal damage to a Farmbot. Currently it does zero damage and i'd like support to help change the script. I'm only experimenting with lua so i'm not doing this to give mobs a weakness, i like to disassemble some code and write new code to see what i can do.

Bellow is the line i believe controls the effects if the sledgehammer.

I need help to figure out my problem. I want the sledgehammer to do damage to the Farmbot (only to get comfortable with lua) but below. if i've read it right, it states the sledgehammer does absolutely nothing to it. How would one change the script to allow the sledgehammer to damage the Farmbot ?


function FarmbotUnit.server_onMelee( self, hitPos, attacker, damage )
    if not sm.exists( self.unit ) or not sm.exists( attacker ) then
        return
    end
    local teamOpponent = false
    if type( attacker ) == "Unit" then
        if not SurvivalGame then
            teamOpponent = not InSameTeam( attacker, self.unit )
        end
    end

    if type( attacker ) == "Player" or teamOpponent then
        local attackingCharacter = attacker:getCharacter()
        if self.eventTarget == nil then
            self.eventTarget = attackingCharacter
        end
        local attackDirection = ( self.unit.character.worldPosition - attackingCharacter.worldPosition ):normalize()
        local effectRotation = sm.vec3.getRotation( sm.vec3.new( 0, 0, 1 ), -attackDirection )
        sm.effect.playEffect( "Sledgehammer - Hit", hitPos, nil, effectRotation, nil, { Material = 9 } )
        sm.effect.playEffect( "Weapon - Impact", hitPos, nil, effectRotation )
        if teamOpponent then
            local impact = attackDirection * 6
            self:sv_takeDamage( damage, impact, hitPos )
        end
    end
end(edited)
[4:20 AM]
*
would i change the 'nil' in the sm.effect lines? Or perhaps change the '6'. I haven't learnt too much about lua though from reading the script, i assume this is the section which controls sledgehammer damage regarding Farmbot
