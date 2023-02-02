# MIPS Programming 101

## Introduction

MIPS is a pretty basic assembler programming language.

In Stationeers you have the following available to you:

- Six connectors (d0 through d5) on IC Housings, or two (d0 and d1) on selected devices, plus a connector for the chip-socket (db)
- Sixteen registers (r0 through r15)
- A special address register (ra)
- A stack (sp) that you can push 512 values into, and pop values out of. The stack is last-in-first-out.
- A maximum of 128 lines of code, each line has a maximum of 57 characters

The address register (ra) is not meant to be written to by the user, it's only used for branching to code that needs to be accessed from multiple areas in your code (functions).

## Registers? What are they and how do I use them?

Registers are what most programming languages call variables. But where most programming languages let you create as many variables as you want, MIPS has 18 registers, but only 16 of them are intended for regular use.

| **Register(s)** | **Alias** | **Purpose**    |
|-----------------|-----------|----------------|
| r0-r15          | (None)    | User register  |
| r16             | ra        | Return address |
| r17             | sp        | Stack pointer  |

### So what do you use a register for?

Registers are for storing temporary data. They can be overwritten any time you want.

Think of a register as a shoe-box. You write something on a piece of paper and store it in the box, you can open the box and look at what's written on the note, and you can write a new note and put it in there, but then you have to take out the old note. The shoe-box can only ever hold one note.

In game-terms, registers are where you store things like pressure from a Gas Sensor, the horizontal angle of the Sun, etc.

### How do you use them?

If you want to read the pressure from a Gas Sensor, you'd use the following bit of code:

```
l r0 GasSensor Pressure
```

This ***l***oads ***Pressure*** from ***GasSensor***, and stores it into ***r0***.

## Load? Set? What?

In MIPS, two of the functions you'll use the most are ***l*** and ***s***. ***s*** also has a lot of logical comparators you can use to do boolean operations on the input.

In short, ***l*** loads data from devices to registers, ***s*** sets (or saves) data from registers to registers or devices.

### Examples

#### Loading
```
l r0 GasSensor Pressure
```
This ***l***oads ***Pressure*** from ***GasSensor*** into ***r0***.

#### Setting
```
s CeilingLight On r0
```
This ***s***ets ***CeilingLight***'s ***On*** end-point to the value stored in ***r0***.

This may sound a bit cryptic, but bear with me, I'll explain it all a bit further down.

#### Setting with boolean comparisons
Boolean comparisons are comparisons made to be either true (1) or false (0). These are great at controlling the on/off state of devices like pumps and lights etc.

Boolean comparisons are of the type "equal", "equal to zero", "less/great than", "less/greater than zero" and so on.

If you use one of the operators that compare to zero, you do not need to supply two values for comparison, since the instruction already says one of the values will be zero.

```
sgt r2 CurrentPressure MaxPressure
```
This ***s***ets a 1 (true) in ***r2*** if ***CurrentPressure*** is ***g***reater ***t***han ***MaxPressure***, or a 0 (false) in r2 if it's not.

```
sgtz r2 CurrentTemperature
```
This ***s***ets a 1 (true) in ***r2*** if ***CurrentTemperature*** is ***g***reater ***t***han ***z***ero, or a 0 (false) in r2 if it's not.

### Data end-points, and what can be loaded/set?
When you want to automate something, the Stationpedia is your best friend! It contains a catalogue of all the things you can make, and lists all the data you can *load from* and/or *set to* it. Here's an example:

![Stationpedia page for a Kit(Wall Light)](/images/data-end-points.png)

The section we're interested in here is the ***Logic*** section. As you saw in the Setting-example above, I set the ***On*** end-point to the value stored in a register. As we can see in the picture above, ***On*** can be read (***l***oaded) or written (***s***et). ***On*** can take a 1 to turn on, or a 0 to turn off.

Not all end-points can be written to, this makes sense when you think about it.

- You can read ***On*** to see if the light is turned on or not, or you can write to ***On*** to turn it on or off.
- A light will always use the power it *needs* to work, so you can read ***Power*** to see how much it consumes, but you can't set the power it uses.

Aha! But setting Power could be like making a dimmer for the light, right? That would have been nice, but that would more likely be done with an end-point called ***Setting***, which this variant of light doesn't have. To my knowledge, there are no dimmable lights in Stationeers (yet).

## Making code more readable!

> **Code is read much more often than it is written.**
> 
> *- Guido Van Rossum (the guy that created the Python programming language)*

While the MIPS implementation in Stationeers has some limitations (128 lines, with a maximum of 57 characters per line), I've rarely run into problems like running out of room. This is mostly because I try not to write "multi-mega-scripts" that do a ton of things. I'm a fan of the UNIX mantra of "do one thing, and do it well". This also means that I use ***aliases*** and ***defines*** a lot. They take more space than just writing things straight up, but they also make the code much easier to read!

Compare these two lines of code:
```
slt r2 r0 r1
```
and
```
slt HeaterOn CurrentTemperature MinimumTemperature
```

They could do the exact same thing, but the bottom one actually explains what's going on. And it's still shorter than 57 characters.

If you're juggling 10+ registers in your code without naming them, the chances of getting them mixed up increases greatly. And if you do make a mistake somewhere, trying to follow the code can be more complicated than if you used named variables.

This is not to say that you *have* to name variables. It's up to you.

### Alias

Making an ***alias*** is the act of giving a register or a device a more human-readable name. For devices, this has the added benefit of naming the pins on the IC housing, making it easier to remember what devices goes on what pin when you set up the housing.

```
alias Furnace d0
alias FurnaceTemperature r0
```

This gives the device on pin ***d0*** the ***alias*** (name) ***Furnace***, and the register ***r0*** the ***alias*** (name) ***FurnaceTemperature***. In the code you can now refer to Furnace instead of d0, and FurnaceTemperature instead of r0. Like this:

```
l FurnaceTemperature Furnace Temperature
```

This loads the ***Temperature*** in the ***Furnace (d0)*** into ***FurnaceTemperature (r0)***.

### Define
A define is what we in other programming languages would call a constant. As the name implies, a constant is ... constant. It never changes. This is great for threshold-values and item-hashes (a unique number given to each type of item in the game, used for comparing items in sorting, and for reading from or writing to a lot of identical items, called batch-reading/-writing).

```
define MinimumTemperature 20
define KelvinConvert 273.15
```

This ***define***s ***MinimumTemperature*** as ***20*** (maybe for use as 20 Celsius in a temperature control circuit), and ***define***s ***KelvinConvert*** (the conversion-number from Kelvin to Celsius) as ***273.15***.

## Branch/set matrix

There is also logic for relative jumping in code, and it works by adding an 'r' after the 'b' when branching (so 'b' becomes 'br'). Instead of giving the branch a label to go to, you tell it the number of lines to jump (positive number jumps forwards, negative number jumps backwards).

<div class="level2" id="bkmrk-suffix-prefix-b--b-a"><div class="table sectionedit3"><table class="inline"><thead><tr class="row0"><th class="col0">Suffix</th><th class="col1" colspan="4">Prefix</th></tr></thead><tbody><tr class="row1"><th class="col0" rowspan="2">  
</th><td class="col1">  
</td><td class="col2">b-</td><td class="col3">b-al</td><td class="col4">s-</td></tr><tr class="row2"><th class="col0">Description</th><th class="col1">Branch to line</th><th class="col2">Branch to line and store return address</th><th class="col3">Set register</th></tr><tr class="row3"><th class="col0">&lt;none&gt;</th><td class="col1">unconditional</td><td class="col2">j</td><td class="col3">jal</td><td class="col4">s</td></tr><tr class="row4"><th class="col0">-eq</th><td class="col1">if a == b</td><td class="col2">beq</td><td class="col3">beqal</td><td class="col4">seq</td></tr><tr class="row5"><th class="col0">-eqz</th><td class="col1">if a == 0</td><td class="col2">beqz</td><td class="col3">beqzal</td><td class="col4">seqz</td></tr><tr class="row6"><th class="col0">-ge</th><td class="col1">if a &gt;= b</td><td class="col2">bge</td><td class="col3">bgeal</td><td class="col4">sge</td></tr><tr class="row7"><th class="col0">-gez</th><td class="col1">if a &gt;= 0</td><td class="col2">bgez</td><td class="col3">bgezal</td><td class="col4">sgez</td></tr><tr class="row8"><th class="col0">-gt</th><td class="col1">if a &gt; b</td><td class="col2">bgt</td><td class="col3">bgtal</td><td class="col4">sgt</td></tr><tr class="row9"><th class="col0">-gtz</th><td class="col1">if a &gt; 0</td><td class="col2">bgtz</td><td class="col3">bgtzal</td><td class="col4">sgtz</td></tr><tr class="row10"><th class="col0">-le</th><td class="col1">if a ⇐ b</td><td class="col2">ble</td><td class="col3">bleal</td><td class="col4">sle</td></tr><tr class="row11"><th class="col0">-lez</th><td class="col1">if a ⇐ 0</td><td class="col2">blez</td><td class="col3">blezal</td><td class="col4">slez</td></tr><tr class="row12"><th class="col0">-lt</th><td class="col1">if a &lt; b</td><td class="col2">blt</td><td class="col3">bltal</td><td class="col4">slt</td></tr><tr class="row13"><th class="col0">-ltz</th><td class="col1">if a &lt; 0</td><td class="col2">bltz</td><td class="col3">bltzal</td><td class="col4">sltz</td></tr><tr class="row14"><th class="col0">-ne</th><td class="col1">if a != b</td><td class="col2">bne</td><td class="col3">bneal</td><td class="col4">sne</td></tr><tr class="row15"><th class="col0">-nez</th><td class="col1">if a != 0</td><td class="col2">bnez</td><td class="col3">bnezal</td><td class="col4">snez</td></tr><tr class="row16"><th class="col0">-dns</th><td class="col1">if d? is not set</td><td class="col2">bdns</td><td class="col3">bdnsal</td><td class="col4">sdns</td></tr><tr class="row17"><th class="col0">-dse</th><td class="col1">if d? is set</td><td class="col2">bdse</td><td class="col3">bdseal</td><td class="col4">sdse</td></tr><tr class="row18"><th class="col0">-ap</th><td class="col1">if a ~ b</td><td class="col2">bap</td><td class="col3">bapal</td><td class="col4">sap</td></tr><tr class="row19"><th class="col0">-apz</th><td class="col1">if a ~ 0</td><td class="col2">bapz</td><td class="col3">bapzal</td><td class="col4">sapz</td></tr><tr class="row20"><th class="col0">-na</th><td class="col1">if a !~ b</td><td class="col2">bna</td><td class="col3">bnaal</td><td class="col4">sna</td></tr><tr class="row21"><th class="col0">-naz</th><td class="col1">if a !~ 0</td><td class="col2">bnaz</td><td class="col3">bnazal</td><td class="col4">snaz</td></tr></tbody></table>

</div></div>

## Indirect referencing

Indirect referencing is when you use the value in one register to determine what other register or device to interact with.

It looks like this:
```
move r0 2
l r1 dr0 Setting
```

The indirect referencing is the `dr0` part. You can read it as `d(r0)`, which means it reads from device `d2` since `r0` is `2`.

If you want, you can reference multiple times in the same reference, but that gets unreadable *fast!*

```
move r0 7
move r1 2
move r2 0
l r15 drrr1 Setting
```

So what device are we addressing here? Let's break it down:

- `drrr1` can be read as `d(r(r(r1)))`
- `r1` is `2`, so now we have `d(r(r2))`
- `r2` is `0`, so now we have `d(r0)`
- `r0` is `7`, so now we have `d7`

Since the maximum number of devices an IC Housing can connect is 6 (d0-5), this would break the script. This is why multi-level referencing should be used extremely sparingly.
