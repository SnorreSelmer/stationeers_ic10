# Stationeers ic10 code
This repo contains ic10 code for the space survival sandbox Stationeers (https://store.steampowered.com/app/544550/Stationeers/).

All code should be used with caution. ~~It should work~~ With all the changes that have happened in Stationeers (and my lack of playing it for a while), this code is a good place to start, but I can't guarantee anything will work as intended. For example, most of the code here was written long before phase-change was a thing, so you might (probably will) break some pipes if you use code that deals with gasses. Beware!

Items needed are described in the comments at the top of each file.

## "I found a bug!"
Awesome! My code is probably not perfect, so please file an issue (https://github.com/SnorreSelmer/stationeers_ic10/issues) and I'll fix it! That's the best way to do it, it benefits everyone.

## The scripts:
- **auto_adv_furnace_library.ic10** - A fork of Elmotrix' furnace-library, removed Prev/Next buttons, added external On/Off-switch.
- **auto_arc_furnace.ic10** - As long as there's ore in the input, the Arc Furnace will smelt.
- **auto_class_sorter.ic10** - Can control multiple sorters to sort out a single itemClass. Useful for looping ingots between printers, and then splitting out the printed items.
- **auto_item_sorter.ic10** - Can control multiple sorters to sort out a single PrefabHash per Sorter. Useful for separating ore output from centrifuges.
- **auto_lights.ic10** - Turns on the lights if you're in a room. ***(Useless now, just use a Sensor (Room Occupancy) and a Logic (Reader) and Logic (Batch Writer) instead.)***
- **automated_canister_filling.ic10** - Safely fills any canister, and then vacuums out the Canister Holder to prevent exploding canisters. ***(Mostly useless now, just use the Suit Storage instead. Can be used to fill welder-cannisters, but why would you use a welder? :D )***
- **battery_monitor.ic10** - Displays average charge of a bank of station batteries on a Diode Slide, and lighting it if you're down to your last battery. It can also take an optional Cable Analyzer and LED light to show if the system is charging (consumption less than production) or discharging.
- **filtration.ic10** - Shows if filters need to be replaced, halts filtering if temperature is too high or filterable gas is too low. Based on CowsAreEvil's setup, minus the gas-cooling.
- **gas_mixer.ic10** - Controls a static Gas Mixer. Prevents mixing gasses that are too far apart in temperature, prevents over-pressure, and stops mixing if input pressure gets too low.
- **heating_cooling.ic10** - Temperature regulator with optional temperature display. ***(This uses the least efficient forms of cooling: Wall Heaters and Coolers. I advice you to make either Atmospherics Air-conditioners or phase-change cooling!)***
- **printer_countdown.ic10** - Uses a counter (Stacker or Dial) to order a given number of items. Stops printing when order is filled.
- **solar_tracking.ic10** - Accurate two-axis solar-panel tracking for up to two types of panels.
- **storm_warning.ic10** - Monitors a Weather Station, announces incoming storms, sounds an alarm when the storm is 3 minutes from hitting, stops alarm when the storm is less than 1 minute from hitting. (Can be adjusted.)
