# MathfinderBot

Mathfinder is a Discord bot built as a stat-tracker for Pathfinder 1e. Its features include:

- A math engine you can use to create useful expressions for your character, used together with a
- Statblock that keeps track of base stats and expressions.
- Character sheet imports from other software (PCGen, Pathbuilder, HeroLabs).
- Preset expressions for things like weapons and creature shapes, some of which can be customized before calling.
- Ability to turn formulae into buttons which you can call in rows and grids for easy access.
- Basic inventory management.
- Tools for DMs, including the ability to evaluate character sheets, secrets rolls, initiative tracking.
- Making your pathfinder experience more convoluted than ever!


There are many help options built into the bot using the `/mf-help` command. These include examples on how to use particular commands. I will do my best to update both the documents located here and on the bot.

## W.I.P.
This bot is a work in progress! Please be patient while I fix any ongoing issues and improve tools.


## Stats & Expressions
Statblocks contain two primary values: Stats and Expressions. 

Each `Stat` contains a base value, as well as a list of bonuses. Each `Bonus` contains a value, a name, and a bonus-type. Together, these are used to accurately calculate the total of any Stat when one or many bonuses are applied. (Stacking rules are based on PF1e).

`Expressions` are formulae. These can represent anything from a contant number to an expression of expressions including any number of stats. 

For usability, stats and expressions share a pool of variable names

*NOTE* — The Pathfinder default character sheet contains a number of both stats and expressions that represent the values you would find on any standard character sheet. Other than class-specific features, you should find the majority of what you need; the goal is to have a feature-complete statblock, while offering the freedom for anyone to customize it how they choose.


## Rows & Grids
`Rows` are sets of buttons that represent expressions. These can be created from scratch or saved from presets. Use `/row` to call.

`Grids` are sets of Rows. Up to 5 rows can be called in this manner per command, creating an (up to) 5x5 grid of buttons. Use `/grid` to call.


## Character Sheet Imports
While you can setup a character by manually setting each value, this is not ideal. Mathfinder currently supports three different options for character imports, so that you can update your character at each level.

### PCGen
 - Using the export option `csheet_fantasy_rpgwebprofiler.xml`. Tested with v6.09.05.

### HeroLabs
 - Using the XML export option

### Pathbuilder
 - Using the exported PDF


These files can be uploaded to update any created character. There are known limitation when parsing specifical sheets. For instance, not all bonus-types may be known for a given stat, and cannot be applied accurately, but the totals should remain correct. This can affect the proper calculation of stacking bonuses. I do my best!


## Command Reference
There are other commands than the ones listed below. I've only documented the ones I feel are worth using/testing at the moment. There are also `dm` versions of some commands, which add an additional `target` option for selecting other player's active sheets. These require the `DM` role.




### **-CHARACTER-**

usage: /char `mode` `char-name` `game`

-:-

![char](https://user-images.githubusercontent.com/10622391/192044180-e25c1784-b76b-40a7-be83-5a4ec68fc2e4.jpg)


`mode` *required*

 - —`Set` Set an active character.
   
   - `char-name` The name of the character to set.

 - —`New` Create a new character. This is a required step before importing any character sheet from any third-party app.
   
   - `char-name` The name of the character to create.
   - `game` *optional & not recommended.* The selected game to use. The default is Pathfinder. Any other option is purely experimental.

 - —`List` This will list any created characters you have.

 - —`Export` (EXPERIMENTAL) This will export any active character into JSON format.

 - —`Delete` Any character name listed in char-name will be deleted. It will prompt you to confirm this deletion.
   - `char-name` The name of the character to delete. This will pop-up a window to confirm your selection.




### **-CHARACTER-UPDATE-**

usage: /char-update `sheet-type` `file`

-:-

`sheet-type` *required*

 - —`Pathbuilder` Pathbuilder PDF export.

 - —`HeroLabs` HeroLabs XML export.

 - —`PCGen` Export using the `csheet_fantasy_rpgwebprofiler.xml` option.


`file` *required*

 - The file to use.

![update](https://user-images.githubusercontent.com/10622391/192043907-a72d879d-9fed-42ce-a0df-050e60af9862.jpg)


#### Remarks
This feature is meant to update your character at each level (or whenever you feel like it) by importing a character sheet from a program more suited for character advancement. It should not remove any custom made stats or expressions, but it *will* override any default variables with ones from the sheet.



### **-EVALUATE-**

usage: /eval `expr`

-:-

`expr` *required*

 - The expression to evaluate.

![eval0](https://user-images.githubusercontent.com/10622391/192032624-f0e56353-731d-4b15-9827-606ee703b1a8.jpg)

![eval1](https://user-images.githubusercontent.com/10622391/192047953-34581fa4-bf82-4986-93d7-20138f92f437.jpg)

![eval2](https://user-images.githubusercontent.com/10622391/192032806-774d28bd-acf2-4908-b413-3dcfa75fe8ec.jpg)




#### Remarks
`/eval` supports many different math operators: `+` `-` `*` `/` `>` `<` `==` `!=` `<=` `>=` `%` `()` `=` `+=` `-=` `*=` `/=` `&&` `||` `?:`. Use `/mf-help` on the bot for specific examples.

Assignment operators (`=`, `+=`, etc) can be used to change the current **base** value of any Stat. To view the base value (separated from any bonuses), you can evaluate a stat's name prefixed with an `@` operator. 

To modify bonus values instead, you can use the special `$` operator. Adding a specific bonus to any stat can be done as follows: `STATNAME +$ NAME:TYPE:VALUE`. A name and type are required for determining which bonuses stack. Types can be represented by numbers or names (7 and ENHANCEMENT are identical). To remove a bonus from a stat, you can do: `STATNAME -$ NAME`. This will effectively remove all bonuses with that name from the stat. You can view a specific bonus by doing: `STATNAME $ TYPE`. You can also use /var List-Bonuses to see all bonuses applied to all stats.

*Note* — /eval returns integer values only. This comes with some limitations in how evaluations are performed. For example, true and false are represented by 1 and 0 respectively. `TRUE`/`FALSE` are hardcoded variables that can be used for readability.



### **-INVENTORY-**

usage: /inv `action` `item` `qty` `attachment`

-:-

`action` *required*

- —`Add` Add one or many items to your active character's inventory. Leave all other fields blank to bring up a window where you can input a list of items.
  - `item` *optional* The syntax for adding an item is `NAME:WEIGHT:VALUE`. For example `Sword:5:10` would add a Sword of 5 weight and a value of 10. Only name is required.
  - `qty` *optional.* How many to add. Default is 1.

- —`Import` Import a list of items. CAUTION—This will **REPLACE** any existing list. If you want to add many items, use the `Add` action. You can copy/paste your text file into the subsequent modal.
  - `attachment` A text file containing one item per line.

- —`Export` Export the current list to a text file.

- —`Remove` Remove an item from your list.
  - `item` The name or index number of the item. If a name is given, it will remove the first occurence any matched value.
  - `qty` The number of the specified items to remove.

- —`List` List your current inventory.



### **-MODIFIER-**

usage: /mod `action` `mod-name`

-:-

`action` *required*

 - —`Add` Add the following mod-name.
 - —`Remove` Remove the following mod-name.

`mod-name` Name of the modifier (use /var List-Mods for a coomprehensive list).

![mod0](https://user-images.githubusercontent.com/10622391/192043634-72ca55d6-5a9d-4936-8356-7eedff008113.jpg)

![mod1](https://user-images.githubusercontent.com/10622391/192043649-46aa7dcc-0f48-4c53-b1a5-5d4198febdc4.jpg)


#### Remarks
This command will add or remove a modifier to your current character. Some modifiers contain sub-options, such as `BEAST_SHAPE` or any spell that scales with caster level. These options generally appear as buttons.



### **-VAR-**

usage: /var `action` `var-name` `value`

-:-

`action` *required*

 - —`Set-Expression` 
   - `var-name` Name of the expression.
   - `value` Expression to create. 

 - —`Set-Row` 
   - `var-name` Row name. This will bring up a modal window, where you can make up to 5 expressions. The syntax is `LABEL:EXPRESSION`. The label is optional—if omitted it will use the expression as the label.

 - —`Set-Grid`
   - `var-name` Grid name. The same as Set-Row except you can specify a set of rows.

 - —`Set-Craft` (EXPERIMENTAL) This lets you set a craft a mundane item. 
   - `var-name` Item name. 
   - `value` DC to craft.

 - —`List-Stats` Lists all stats for an active character.

 - —`List-Expressions` Lists all expressions for an active character.

 - —`List-Bonus` Lists all bonuses applied to stats.

 - —`List-Row` Lists all saved Rows. 
   - `var-name` *optional.* List a single Row's expressions.

 - —`List-Presets` Lists all weapon presets.

 - —`List-Shapes` Lists all available creature shapes.

 - —`List-Grids` Lists all saved Grids.

 - —`List-Crafts` List all active crafts.

 - —`Remove-Variable` Removes a Stat, Expression, Row, or Grid. 
   - `var-name` The variable name to remove.




### **-PRESET-WEAPON-**

usage: /preset-weapon `number-or-name` `hit-mod` `damage-mod` `hit-bonus` `dmg-bonus` `size`

-:-

`number-or-name` *required*
 - The name or index number associated with it (use /var List-Presets to see a comprehensive list).

`hit-mod` *required*
 - The modifier used for hitting.

`damage-mod`
 - The modifier used for damage (if any)

`hit-bonus`
 - The bonus to hit (if any)

`dmg-bonus`
 - The bonus damage (if any)

`size`
 - The size of the character. If left blank, it will check the character's Stackblock for size. If none is found, it will default to medium.


![preset](https://user-images.githubusercontent.com/10622391/192030455-2149b615-ea3f-4663-b55d-5d8a56846b8f.jpg)

![preset-1](https://user-images.githubusercontent.com/10622391/192030478-d6cef765-c511-413e-834a-0df05259fcc3.jpg)

![preset-2](https://user-images.githubusercontent.com/10622391/192030805-9971520e-2886-4b47-b7f0-38eb652e402e.jpg)


#### Remarks
You can use `/preset-save` with a `name` to save the last generated preset to your active character sheet. You can then call it with /row.




### **-SHAPE-**

usage: /shape `number-or-name` `hit-mod`

-:-

`numer-or-name` *required*
 - The name or index number associated with it (use /var List-Shapes to see a comprehensive list).
 
`hit-mod` *required*
 - The modifier used for hitting.
 
#### Remarks
`/shape` is meant to be used in-tandem with a polymorph `/mod`, such as `BEAST_SHAPE`. This will generate the attacks (primary and/or secondary) and natural weapons associated with a particular creature's shape. In addition, it will list any speeds, senses, or special abilities you may receive from taking the creature's shape. Be sure to follow the particular spell's allowances, as many creatures are listed with their maximum possible features.
