# Stationeers ic10 code
This repo contains ic10 code for the space survival sandbox Stationeers (https://store.steampowered.com/app/544550/Stationeers/).

All code should be used with caution. It should work:tm: but I can't guarantee it's not broken. Items needed are described in the comments at the top of each file.

## The scripts:
- **auto_arc_furnace.ic10** - As long as there's ore in the input, the Arc Furnace will smelt.
- **auto_class_sorter.ic10** - Can control multiple sorters to sort out a single itemClass. Useful for looping ingots between printers, and then splitting out the printed items.
- **auto_lights.ic10** - Turns on the lights if you're in a room.
- **automated_canister_filling.ic10** - Safely fills any canister, and then vacuums out the Canister Holder to prevent exploding canisters.
- **battery_monitor.ic10** - Displays average charge of a bank of station batteries on a Diode Slide, and lighting it if you're down to your last battery.
- **cooling_tower_drain.ic10** - Isolates a furnace cooling-tower until the temperature is low enough to safely drain.
- **filtration.ic10** - **!!!OLD!!!** Shows if filters need to be replaced, prevents processing gasses that are too hot or the filtered tank is too full, has a purge-pump function in case the filtered output gets contaminated. **Still works fine, but doesn't use the capabilities of the new filtration units!**
- **gas_mixer.ic10** - Controls a static Gas Mixer. Prevents mixing gasses that are too far apart in temperature, prevents over-pressure, and stops mixing if input pressure gets too low.
- **heating_cooling.ic10** - Temperature regulator with optional temperature display.
- **printer_countdown.ic10** - Uses a counter (Stacker or Dial) to order a given number of items. Stops printing when order is filled.
- **solar_tracking.ic10** - Accurate two-axis solar-panel tracking for up to two types of panels.
- **storm_warning.ic10** - Monitors a Weather Station, announces incoming storms, sounds an alarm when the storm is 3 minutes from hitting, stops alarm when the storm is less than 1 minute from hitting. (Can be adjusted.)
