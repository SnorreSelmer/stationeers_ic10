# MIPS Programming 101

# Introduction

MIPS is a pretty basic assembler programming language.

In Stationeers you have the following available to you:

- <div class="li">Six connectors (d0 through d5) on IC Housings, or two (d0 and d1) on selected devices, plus a connector for the chip-socket (db)</div>
- <div class="li">Sixteen registers (r0 through r15)</div>
- <div class="li">A special address register (ra)</div>
- <div class="li">A stack (sp) that you can push 512 values into, and pop values out of. The stack is last-in-first-out.</div>
- <div class="li">A maximum of 128 lines of code, each line has a maximum of 57 characters</div>

The address register (ra) is not meant to be written to by the user, it's only used for branching to code that needs to be accessed from multiple areas in your code (functions).

<div class="level2" id="bkmrk-suffix-prefix-b--b-a"><div class="table sectionedit3"></div></div>

# Registers? What are they and how do I use them?

Registers are what most programming languages call variables. But where most programming languages let you create as many variables as you want, MIPS has 18 registers, but only 16 of them are intended for regular use.

<table border="1" id="bkmrk-register%28s%29-alias-pu" style="border-collapse: collapse; width: 100%;"><colgroup><col style="width: 33.3745%;"></col><col style="width: 33.3745%;"></col><col style="width: 33.3745%;"></col></colgroup><thead><tr><td>**Register(s)**</td><td>**Alias**</td><td>**Purpose**</td></tr></thead><tbody><tr><td>r0-r15</td><td>(None)</td><td>User register</td></tr><tr><td>r16</td><td>ra</td><td>Return address</td></tr><tr><td>r17</td><td>sp</td><td>Stack pointer</td></tr></tbody></table>

## So what do you use a register for?

Registers are for storing temporary data. They can be overwritten any time you want.

Think of a register as a shoe-box. You write something on a piece of paper and store it in the box, you can open the box and look at what's written on the note, and you can write a new note and put it in there, but then you have to take out the old note. The shoe-box can only ever hold one note.

In game-terms, registers are where you store things like pressure from a Gas Sensor, the horizontal angle of the Sun, etc.

## How do you use them?

If you want to read the pressure from a Gas Sensor, you'd use the following bit of code:

```
l r0 GasSensor Pressure
```

This <span style="color: rgb(224, 62, 45);">***l***</span>oads <span style="color: rgb(224, 62, 45);">***Pressure*** </span>from <span style="color: rgb(224, 62, 45);">***GasSensor***</span>, and stores it into <span style="color: rgb(224, 62, 45);">***r0***</span>.

# Load? Set? What?

In MIPS, two of the functions you'll use the most are <span style="color: rgb(224, 62, 45);">***l***</span> and <span style="color: rgb(224, 62, 45);">***s***</span>. <span style="color: rgb(224, 62, 45);">***s***</span> also has a lot of logical comparators you can use to do boolean operations on the input.

In short, <span style="color: rgb(224, 62, 45);">***l***</span> loads data from devices to registers, <span style="color: rgb(224, 62, 45);">***s***</span> sets (or saves) data from registers to registers or devices.

## Examples

### Loading

```
l r0 GasSensor Pressure
```

This <span style="color: rgb(224, 62, 45);">***l***</span>oads ***<span style="color: rgb(224, 62, 45);">Pressure </span>***from ***<span style="color: rgb(224, 62, 45);">GasSensor </span>***into <span style="color: rgb(224, 62, 45);">***r0***</span>.

### Setting

```
s CeilingLight On r0
```

This <span style="color: rgb(224, 62, 45);">***s***</span>ets <span style="color: rgb(224, 62, 45);">***CeilingLight***</span>'s <span style="color: rgb(224, 62, 45);">***On*** </span>end-point to the value stored in <span style="color: rgb(224, 62, 45);">***r0***</span>.

This may sound a bit cryptic, but bear with me, I'll explain it all a bit further down.

### Setting with boolean comparisons

Boolean comparisons are comparisons made to be either true (1) or false (0). These are great at controlling the on/off state of devices like pumps and lights etc.

Boolean comparisons are of the type "equal", "equal to zero", "less/great than", "less/greater than zero" and so on.

If you use one of the operators that compare to zero, you do not need to supply two values for comparison, since the instruction already says one of the values will be zero.

```
sgt r2 CurrentPressure MaxPressure
```

This <span style="color: rgb(224, 62, 45);">***s***</span>ets a 1 (true) in <span style="color: rgb(224, 62, 45);">***r2***</span> if ***<span style="color: rgb(224, 62, 45);">CurrentPressure </span>***is ***<span style="color: rgb(224, 62, 45);">g</span>***reater <span style="color: rgb(224, 62, 45);">***t***</span>han <span style="color: rgb(224, 62, 45);">***MaxPressure***</span>, or a 0 (false) in r2 if it's not.

```
sgtz r2 CurrentTemperature
```

This <span style="color: rgb(224, 62, 45);">***s***</span>ets a 1 (true) in <span style="color: rgb(224, 62, 45);">***r2***</span> if ***<span style="color: rgb(224, 62, 45);">CurrentTemperature </span>***is ***<span style="color: rgb(224, 62, 45);">g</span>***reater <span style="color: rgb(224, 62, 45);">***t***</span>han <span style="color: rgb(224, 62, 45);">***z***</span>ero, or a 0 (false) in r2 if it's not.

## Data end-points, and what can be loaded/set?

When you want to automate something, the Stationpedia is your best friend! It contains a catalogue of all the things you can make, and lists all the data you can *load from* and/or *set to* it. Here's an example:

[![2022-10-21 20_40_00-Stationeers v0.2.3657.17736.png](https://bookstack.snorreselmer.net/uploads/images/gallery/2022-10/scaled-1680-/2022-10-21-20-40-00-stationeers-v0-2-3657-17736.png)](https://bookstack.snorreselmer.net/uploads/images/gallery/2022-10/2022-10-21-20-40-00-stationeers-v0-2-3657-17736.png)

The section we're interested in here is the <span style="color: rgb(224, 62, 45);">***Logic*** </span>section. As you saw in the Setting-example above, I set the ***<span style="color: rgb(224, 62, 45);">On </span>***end-point to the value stored in a register. As we can see in the picture above, ***<span style="color: rgb(224, 62, 45);">On </span>***can be read (<span style="color: rgb(224, 62, 45);">***l***</span>oaded) or written (<span style="color: rgb(224, 62, 45);">***s***</span>et). <span style="color: rgb(224, 62, 45);">***On***</span> can take a 1 to turn on, or a 0 to turn off.

Not all end-points can be written to, this makes sense when you think about it.

- You can read ***<span style="color: rgb(224, 62, 45);">On</span>*** to see if the light is turned on or not, or you can write to ***<span style="color: rgb(224, 62, 45);">On</span>*** to turn it on or off.
- A light will always use the power it *needs* to work, so you can read ***<span style="color: rgb(224, 62, 45);">Power</span>*** to see how much it consumes, but you can't set the power it uses.

Aha! But setting Power could be like making a dimmer for the light, right? That would have been nice, but that would more likely be done with an end-point called ***<span style="color: rgb(224, 62, 45);">Setting</span>***, which this variant of light doesn't have. To my knowledge, there are no dimmable lights in Stationeers (yet).

# Making code more readable!

> **Code is read much more often than it is written.**
> 
> *- Guido Van Rossum (the guy that created the Python programming language)*

While the MIPS implementation in Stationeers has some limitations (128 lines, with a maximum of 57 characters per line), I've rarely run into problems like running out of room. This is mostly because I try not to write "multi-mega-scripts" that do a ton of things. I'm a fan of the UNIX mantra of "do one thing, and do it well". This also means that I use <span style="color: rgb(224, 62, 45);">***aliases*** </span>and <span style="color: rgb(224, 62, 45);">***defines*** </span>a lot. They take more space than just writing things straight up, but they also make the code much easier to read!

Compare these two lines of code:

```
slt r2 r0 r1
```

and

```
slt HeaterOn CurrentTemperature MinimumTemperature
```

They could do the exact same thing, but the bottom one actually explains what's going on. And it's still shorter that 57 characters.

If you're juggling 10+ registers in your code without naming them, the chances of getting them mixed up increases greatly. And if you do make a mistake somewhere, trying to follow the code can be more complicated than if you used named variables.

This is not to say that you *have* to name variables. It's up to you.

## Alias

Making an <span style="color: rgb(224, 62, 45);">***alias*** </span>is the act of giving a register or a device a more human-readable name. For devices, this has the added benefit of naming the pins on the IC housing, making it easier to remember what devices goes on what pin when you set up the housing.

```
alias Furnace d0
alias FurnaceTemperature r0
```

This gives the device on pin <span style="color: rgb(224, 62, 45);">***d0*** </span>the <span style="color: rgb(224, 62, 45);">***alias*** </span>(name) <span style="color: rgb(224, 62, 45);">***Furnace***</span>, and the register <span style="color: rgb(224, 62, 45);">***r0*** </span>the <span style="color: rgb(224, 62, 45);">***alias*** </span>(name) <span style="color: rgb(224, 62, 45);">***FurnaceTemperature***</span>. In the code you can now refer to Furnace instead of d0, and FurnaceTemperature instead of r0. Like this:

```
l FurnaceTemperature Furnace Temperature
```

This loads the <span style="color: rgb(224, 62, 45);">***Temperature*** </span>in the <span style="color: rgb(224, 62, 45);">***Furnace (d0)***</span> into <span style="color: rgb(224, 62, 45);">***FurnaceTemperature*** **(r0)**</span>.

## Define

A define is what we in other programming languages would call a constant. As the name implies, a constant is ... constant. It never changes. This is great for threshold-values and item-hashes (a unique number given to each type of item in the game, used for comparing items in sorting, and for reading from or writing to a lot of identical items, called batch-reading/-writing).

```
define MinimumTemperature 20
define KelvinConvert 273.15
```

This <span style="color: rgb(224, 62, 45);">***define***</span>s ***<span style="color: rgb(224, 62, 45);">MinimumTemperature </span>***as <span style="color: rgb(224, 62, 45);">***20*** </span>(maybe for use as 20 Celsius in a temperature control circuit), and ***<span style="color: rgb(224, 62, 45);">define</span>***s ***<span style="color: rgb(224, 62, 45);">KelvinConvert </span>***(the conversion-number from Kelvin to Celsius) as <span style="color: rgb(224, 62, 45);">***273.15***</span>.

# Branch/set matrix

There is logic for relative jumping in code. I have yet to find a use for it that can't be done better with any of the other functions, so I won't cover it here.

<div class="level2" id="bkmrk-suffix-prefix-b--b-a"><div class="table sectionedit3"><table class="inline"><thead><tr class="row0"><th class="col0">Suffix</th><th class="col1" colspan="4">Prefix</th></tr></thead><tbody><tr class="row1"><th class="col0" rowspan="2">  
</th><td class="col1">  
</td><td class="col2">b-</td><td class="col3">b-al</td><td class="col4">s-</td></tr><tr class="row2"><th class="col0">Description</th><th class="col1">Branch to line</th><th class="col2">Branch to line and store return address</th><th class="col3">Set register</th></tr><tr class="row3"><th class="col0">&lt;none&gt;</th><td class="col1">unconditional</td><td class="col2">j</td><td class="col3">jal</td><td class="col4">s</td></tr><tr class="row4"><th class="col0">-eq</th><td class="col1">if a == b</td><td class="col2">beq</td><td class="col3">beqal</td><td class="col4">seq</td></tr><tr class="row5"><th class="col0">-eqz</th><td class="col1">if a == 0</td><td class="col2">beqz</td><td class="col3">beqzal</td><td class="col4">seqz</td></tr><tr class="row6"><th class="col0">-ge</th><td class="col1">if a &gt;= b</td><td class="col2">bge</td><td class="col3">bgeal</td><td class="col4">sge</td></tr><tr class="row7"><th class="col0">-gez</th><td class="col1">if a &gt;= 0</td><td class="col2">bgez</td><td class="col3">bgezal</td><td class="col4">sgez</td></tr><tr class="row8"><th class="col0">-gt</th><td class="col1">if a &gt; b</td><td class="col2">bgt</td><td class="col3">bgtal</td><td class="col4">sgt</td></tr><tr class="row9"><th class="col0">-gtz</th><td class="col1">if a &gt; 0</td><td class="col2">bgtz</td><td class="col3">bgtzal</td><td class="col4">sgtz</td></tr><tr class="row10"><th class="col0">-le</th><td class="col1">if a ⇐ b</td><td class="col2">ble</td><td class="col3">bleal</td><td class="col4">sle</td></tr><tr class="row11"><th class="col0">-lez</th><td class="col1">if a ⇐ 0</td><td class="col2">blez</td><td class="col3">blezal</td><td class="col4">slez</td></tr><tr class="row12"><th class="col0">-lt</th><td class="col1">if a &lt; b</td><td class="col2">blt</td><td class="col3">bltal</td><td class="col4">slt</td></tr><tr class="row13"><th class="col0">-ltz</th><td class="col1">if a &lt; 0</td><td class="col2">bltz</td><td class="col3">bltzal</td><td class="col4">sltz</td></tr><tr class="row14"><th class="col0">-ne</th><td class="col1">if a != b</td><td class="col2">bne</td><td class="col3">bneal</td><td class="col4">sne</td></tr><tr class="row15"><th class="col0">-nez</th><td class="col1">if a != 0</td><td class="col2">bnez</td><td class="col3">bnezal</td><td class="col4">snez</td></tr><tr class="row16"><th class="col0">-dns</th><td class="col1">if d? is not set</td><td class="col2">bdns</td><td class="col3">bdnsal</td><td class="col4">sdns</td></tr><tr class="row17"><th class="col0">-dse</th><td class="col1">if d? is set</td><td class="col2">bdse</td><td class="col3">bdseal</td><td class="col4">sdse</td></tr><tr class="row18"><th class="col0">-ap</th><td class="col1">if a ~ b</td><td class="col2">bap</td><td class="col3">bapal</td><td class="col4">sap</td></tr><tr class="row19"><th class="col0">-apz</th><td class="col1">if a ~ 0</td><td class="col2">bapz</td><td class="col3">bapzal</td><td class="col4">sapz</td></tr><tr class="row20"><th class="col0">-na</th><td class="col1">if a !~ b</td><td class="col2">bna</td><td class="col3">bnaal</td><td class="col4">sna</td></tr><tr class="row21"><th class="col0">-naz</th><td class="col1">if a !~ 0</td><td class="col2">bnaz</td><td class="col3">bnazal</td><td class="col4">snaz</td></tr></tbody></table>

</div></div>

# Indirect referencing

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