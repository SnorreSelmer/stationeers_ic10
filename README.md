# Stationeers ic10 code

This repo contains ic10 code for the space survival sandbox [Stationeers](https://store.steampowered.com/app/544550/Stationeers/).

All code should be used with caution. ~~It should work~~ With all the changes that have happened in Stationeers (and my lack of playing it for a while), this code is a good place to start, but I can't guarantee anything will work as intended. For example, most of the code here was written long before phase-change was a thing, so you might (probably will) break some pipes if you use code that deals with gasses. Beware!

Items needed are described in the comments at the top of each file.

## "I found a bug!"

Awesome! My code is probably not perfect, so please file an [issue](https://github.com/SnorreSelmer/stationeers_ic10/issues) and I'll fix it! That's the best way to do it, it benefits everyone.

## The scripts

!! All the old scripts have been moved to the [/old-scripts/](/old-scripts/) folder in preparation for an overhaul of all my scripts. !!

Not all the old scripts had any purpose anymore due to changes in the game, so I'll be going over them all to fix things that no longer work, and retiring scripts that are no longer needed.  
Once done, the [/old-scripts/](/old-scripts/) folder will be removed forever. External linking to this folder is not recommended!

- [phase-change-heat-dump.ic10](/scripts/phase-change-heat-dump.ic10) : Used to control a Digital Valve to manage heat-dumping from a phase-change cooling setup