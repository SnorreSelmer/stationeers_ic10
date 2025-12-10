# IC10 Programming 101

## Introduction

IC10 is a pretty basic assembler programming language, inpired by MIPS.

In Stationeers you have the following available to you:

- Six connectors (d0 through d5) on IC Housings, or two (d0 and d1) on selected devices, plus a connector for the chip-socket (db)
- Sixteen registers (r0 through r15)
- A special address register (ra)
- A stack (sp) that you can push 512 values into, and pop values out of. The stack is last-in-first-out.
- A maximum of 128 lines of code, each line has a maximum of 90 characters

The address register (ra) is not meant to be written to by the user, it's only used for branching to code that needs to be accessed from multiple areas in your code (functions).

## Registers? What are they and how do I use them?

Registers are what most programming languages call variables. But where most programming languages let you create as many variables as you want, IC10 has only 18 registers, and only 16 of them are intended for regular use.

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

```mips
l r0 GasSensor Pressure
```

This ***l***oads ***Pressure*** from ***GasSensor***, and stores it into ***r0***.

## Load? Set? What?

In IC10, two of the functions you'll use the most are ***l*** and ***s***. ***s*** also has a lot of logical comparators you can use to do boolean operations on the input.

In short, ***l*** loads data from devices to registers, ***s*** sets (or saves) data from registers to registers or devices.

### Examples

#### Loading

```mips
l r0 GasSensor Pressure
```

This ***l***oads ***Pressure*** from ***GasSensor*** into ***r0***.

#### Setting

```mips
s CeilingLight On r0
```

This ***s***ets ***CeilingLight***'s ***On*** end-point to the value stored in ***r0***.

This may sound a bit cryptic, but bear with me, I'll explain it all a bit further down.

#### Setting with boolean comparisons

Boolean comparisons are comparisons made to be either true (1) or false (0). These are great for controlling the on/off state of devices like pumps and lights etc.

Boolean comparisons are of the type "equal", "equal to zero", "less/greater than", "less/greater than zero" and so on.

If you use one of the operators that compare to zero, you do not need to supply two values for comparison, since the instruction already says one of the values will be zero.

```mips
sgt r2 CurrentPressure MaxPressure
```

This ***s***ets a 1 (true) in ***r2*** if ***CurrentPressure*** is ***g***reater ***t***han ***MaxPressure***, or a 0 (false) in r2 if it's not.

```mips
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

## "Moar executions" is not "moar betterer"!

In Stationeers, everything happens in "ticks", and a single tick is 0.5 seconds. But the code we write in ICs can execute much (***much!***) faster than once per tick. But looping over the code many times in a single tick doesn't make it work better, it just "burns resources".

What happens is, each IC10 script is allowed 128 lines of exection per tick. If your script executes one loop totalling 70 lines, you will go through almost two loops in one tick, then the execution is halted until the next tick. If you execute 70 lines in a loop, then you execute the remaining 58 lines this tick, and the script will be at line 59 of the loop when it resumes next tick. This can have unintended consequences. Plus, if you read the pressure from a Gas Sensor 30 times in a tick, the pressure will be the same every time since pressure is only updated by the game *once per tick*.

The solution is to begin or end the loop with `yield`! It doesn't really matter if you put the `yield` as the first or last line in your loop, as long as there is at least one `yield` executed every single tick. This ensures that you start execution at a controlled location next tick.

Good:

```mips
start:
yield
l GasPressure GasSensor Pressure
s GasDisplay Setting GasPressure
j start
```

or

```mips
start:
l GasPressure GasSensor Pressure
s GasDisplay Setting GasPressure
yield
j start
```

Bad:

```mips
start:
l GasPressure GasSensor Pressure
s GasDisplay Setting GasPressure
j start
```

## Making code more readable!

> **Code is read much more often than it is written.**
>
> *- Guido Van Rossum (the guy that created the Python programming language)*

While the IC10 implementation in Stationeers has some limitations (128 lines, with a maximum of 90 characters per line), I've rarely run into problems like running out of room. This is mostly because I try not to write "multi-mega-scripts" that do a ton of things. I'm a fan of the UNIX mantra of "do one thing, and do it well". This also means that I use ***aliases*** and ***defines*** a lot. They take more space than just writing things straight up, but they also make the code much easier to read!

Compare these two lines of code:

```mips
slt r2 r0 r1
```

and

```mips
slt HeaterOn CurrentTemperature MinimumTemperature
```

They could do the exact same thing, but the bottom one actually explains what's going on. And it's still shorter than 90 characters.

If you're juggling 10+ registers in your code without naming them, the chances of getting them mixed up increases greatly. And if you do make a mistake somewhere, trying to follow the code can be more complicated than if you used named variables.

This is not to say that you *have* to name variables. It's up to you.

### Alias

Making an ***alias*** is the act of giving a register or a device a more human-readable name. For devices, this has the added benefit of naming the pins on the IC housing, making it easier to remember what devices goes on what pin when you set up the housing.

```mips
alias Furnace d0
alias FurnaceTemperature r0
```

This gives the device on pin ***d0*** the ***alias*** (name) ***Furnace***, and the register ***r0*** the ***alias*** (name) ***FurnaceTemperature***. In the code you can now refer to Furnace instead of d0, and FurnaceTemperature instead of r0. Like this:

```mips
l FurnaceTemperature Furnace Temperature
```

This loads the ***Temperature*** in the ***Furnace (d0)*** into ***FurnaceTemperature (r0)***.

### Define

A define is what we in other programming languages would call a constant. As the name implies, a constant is ... constant. It never changes. This is great for threshold-values and item-hashes (a unique number given to each type of item in the game, used for comparing items in sorting, and for reading from or writing to a lot of identical items, called batch-reading/-writing).

```mips
define MinimumTemperature 20
define KelvinConvert 273.15
```

This ***define***s ***MinimumTemperature*** as ***20*** (for use as 20 Celsius in a temperature control circuit), and ***define***s ***KelvinConvert*** (the conversion-number from Kelvin to Celsius) as ***273.15***.

## Branch/set matrix

There is also logic for relative jumping in code, and it works by adding an `r` after the `b` when branching (so `b` becomes `br`). Instead of giving the branch a label to go to, you tell it the number of lines to jump (positive number jumps forwards, negative number jumps backwards).

<div class="level2" id="bkmrk-suffix-prefix-b--b-a"><div class="table sectionedit3"><table class="inline" style="width: 100.247%;"><thead><tr class="row0"><th class="col0" style="width: 13.2426%;">Stem</th><th style="width: 16.5842%;">Description</th><th class="col1" colspan="2" style="width: 26.8564%;">Prefix</th><th style="width: 43.3168%;">Suffix</th></tr></thead><tbody><tr class="row1"><th class="col0" rowspan="2" style="width: 13.2426%;">  
</th><td class="col1" rowspan="2" style="width: 16.5842%;">  
</td><td class="col2" style="width: 9.90099%;">b-</td><td class="col4" style="width: 16.9554%;">s-</td><td class="col3" style="width: 43.3168%;">-al</td></tr><tr class="row2"><th class="col1" style="width: 9.90099%;">Branch to line</th><th class="col3" style="width: 16.9554%;">Set register</th><th class="col2" style="width: 43.3168%;">Branch to line and store return address</th></tr><tr class="row3"><th class="col0" style="width: 13.2426%;">&lt;none&gt;</th><td class="col1" style="width: 16.5842%;">unconditional</td><td class="col2" style="width: 9.90099%;">j</td><td class="col4" style="width: 16.9554%;">s</td><td class="col3" style="width: 43.3168%;">jal</td></tr><tr class="row4"><th class="col0" style="width: 13.2426%;">eq</th><td class="col1" style="width: 16.5842%;">if a == b</td><td class="col2" style="width: 9.90099%;">beq</td><td class="col4" style="width: 16.9554%;">seq</td><td class="col3" style="width: 43.3168%;">beqal</td></tr><tr class="row5"><th class="col0" style="width: 13.2426%;">eqz</th><td class="col1" style="width: 16.5842%;">if a == 0</td><td class="col2" style="width: 9.90099%;">beqz</td><td class="col4" style="width: 16.9554%;">seqz</td><td class="col3" style="width: 43.3168%;">beqzal</td></tr><tr class="row6"><th class="col0" style="width: 13.2426%;">ge</th><td class="col1" style="width: 16.5842%;">if a &gt;= b</td><td class="col2" style="width: 9.90099%;">bge</td><td class="col4" style="width: 16.9554%;">sge</td><td class="col3" style="width: 43.3168%;">bgeal</td></tr><tr class="row7"><th class="col0" style="width: 13.2426%;">gez</th><td class="col1" style="width: 16.5842%;">if a &gt;= 0</td><td class="col2" style="width: 9.90099%;">bgez</td><td class="col4" style="width: 16.9554%;">sgez</td><td class="col3" style="width: 43.3168%;">bgezal</td></tr><tr class="row8"><th class="col0" style="width: 13.2426%;">gt</th><td class="col1" style="width: 16.5842%;">if a &gt; b</td><td class="col2" style="width: 9.90099%;">bgt</td><td class="col4" style="width: 16.9554%;">sgt</td><td class="col3" style="width: 43.3168%;">bgtal</td></tr><tr class="row9"><th class="col0" style="width: 13.2426%;">gtz</th><td class="col1" style="width: 16.5842%;">if a &gt; 0</td><td class="col2" style="width: 9.90099%;">bgtz</td><td class="col4" style="width: 16.9554%;">sgtz</td><td class="col3" style="width: 43.3168%;">bgtzal</td></tr><tr class="row10"><th class="col0" style="width: 13.2426%;">le</th><td class="col1" style="width: 16.5842%;">if a ⇐ b</td><td class="col2" style="width: 9.90099%;">ble</td><td class="col4" style="width: 16.9554%;">sle</td><td class="col3" style="width: 43.3168%;">bleal</td></tr><tr class="row11"><th class="col0" style="width: 13.2426%;">lez</th><td class="col1" style="width: 16.5842%;">if a ⇐ 0</td><td class="col2" style="width: 9.90099%;">blez</td><td class="col4" style="width: 16.9554%;">slez</td><td class="col3" style="width: 43.3168%;">blezal</td></tr><tr class="row12"><th class="col0" style="width: 13.2426%;">lt</th><td class="col1" style="width: 16.5842%;">if a &lt; b</td><td class="col2" style="width: 9.90099%;">blt</td><td class="col4" style="width: 16.9554%;">slt</td><td class="col3" style="width: 43.3168%;">bltal</td></tr><tr class="row13"><th class="col0" style="width: 13.2426%;">ltz</th><td class="col1" style="width: 16.5842%;">if a &lt; 0</td><td class="col2" style="width: 9.90099%;">bltz</td><td class="col4" style="width: 16.9554%;">sltz</td><td class="col3" style="width: 43.3168%;">bltzal</td></tr><tr class="row14"><th class="col0" style="width: 13.2426%;">ne</th><td class="col1" style="width: 16.5842%;">if a != b</td><td class="col2" style="width: 9.90099%;">bne</td><td class="col4" style="width: 16.9554%;">sne</td><td class="col3" style="width: 43.3168%;">bneal</td></tr><tr class="row15"><th class="col0" style="width: 13.2426%;">nez</th><td class="col1" style="width: 16.5842%;">if a != 0</td><td class="col2" style="width: 9.90099%;">bnez</td><td class="col4" style="width: 16.9554%;">snez</td><td class="col3" style="width: 43.3168%;">bnezal</td></tr><tr class="row16"><th class="col0" style="width: 13.2426%;">dns</th><td class="col1" style="width: 16.5842%;">if d? is not set</td><td class="col2" style="width: 9.90099%;">bdns</td><td class="col4" style="width: 16.9554%;">sdns</td><td class="col3" style="width: 43.3168%;">bdnsal</td></tr><tr class="row17"><th class="col0" style="width: 13.2426%;">dse</th><td class="col1" style="width: 16.5842%;">if d? is set</td><td class="col2" style="width: 9.90099%;">bdse</td><td class="col4" style="width: 16.9554%;">sdse</td><td class="col3" style="width: 43.3168%;">bdseal</td></tr><tr class="row18"><th class="col0" style="width: 13.2426%;">ap</th><td class="col1" style="width: 16.5842%;">if a ~ b</td><td class="col2" style="width: 9.90099%;">bap</td><td class="col4" style="width: 16.9554%;">sap</td><td class="col3" style="width: 43.3168%;">bapal</td></tr><tr class="row19"><th class="col0" style="width: 13.2426%;">apz</th><td class="col1" style="width: 16.5842%;">if a ~ 0</td><td class="col2" style="width: 9.90099%;">bapz</td><td class="col4" style="width: 16.9554%;">sapz</td><td class="col3" style="width: 43.3168%;">bapzal</td></tr><tr class="row20"><th class="col0" style="width: 13.2426%;">na</th><td class="col1" style="width: 16.5842%;">if a !~ b</td><td class="col2" style="width: 9.90099%;">bna</td><td class="col4" style="width: 16.9554%;">sna</td><td class="col3" style="width: 43.3168%;">bnaal</td></tr><tr class="row21"><th class="col0" style="width: 13.2426%;">naz</th><td class="col1" style="width: 16.5842%;">if a !~ 0</td><td class="col2" style="width: 9.90099%;">bnaz</td><td class="col4" style="width: 16.9554%;">snaz</td><td class="col3" style="width: 43.3168%;">bnazal</td></tr></tbody></table>

</div></div>

## Indirect referencing

Indirect referencing is when you use the value in one register to determine what other register or device to interact with.

It looks like this:

```mips
move r0 2
l r1 dr0 Setting
```

The indirect referencing is the `dr0` part. You can read it as `d(r0)`, which means it reads from device `d2` since `r0` is `2`.

If you want, you can reference multiple times in the same reference, but that gets unreadable *fast!*

```mips
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
