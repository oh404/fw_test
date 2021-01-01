#include <amxmodx>
#include <amxmisc>
#include <hamsandwich>

#define is_valid_player(%1) (1 <= %1 <= 32)

public plugin_init() {
	register_plugin(PLUGINNAME, VERSION, AUTHOR) 
	RegisterHam(Ham_TakeDamage, "func_breakable", "fw_TakeDamage") // damage to breakable entity registrerd
}

// disabling team-damage if entity set up by the player from the same team for sentry/sentrybase

public fw_TakeDamage (ent, inflictor, attacker, Float:damage, damagebits)
{
    if (!pev_valid (ent))
        return HAM_IGNORED
   
    new sClassname[11]
    pev (ent, pev_classname, sClassname, charsmax ( sClassname ))
   
    if (equal (sClassname, "sentry") || equal ( sClassname, "sentrybase" ))
    {
        new owner = GetSentryPeople(ent, OWNER)
		   
		if (!is_user_connected (owner) || !is_valid_player (owner) || !is_user_connected (attacker) || !is_valid_player (attacker)) {
			return HAM_SUPERCEDE
		} else if (attacker == owner || cs_get_user_team (owner) == cs_get_user_team (attacker)) {
			return HAM_SUPERCEDE
		}
    }
    return HAM_IGNORED   
} 