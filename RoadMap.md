# Introduction #

This is just a rough road map of what I plan to code up for the Dark Sun campaign.  Some will not go out with the initial release of the framework because I want to get some playtesting in first to determine if I should waste the time for certain features or not.

# Tokenmaker Plans #

## Main list ##

  1. Get the source building on home machine with either Eclipse or NetBeans Java IDEs.  (partially done)
  1. Integrate source code into this SVN repository.
  1. Integrate source code from this SVN with chosen Java IDE if possible, otherwise can use manual checkin/checkout from windows explorer.
  1. Add a dropdown to filter Compendium searches based on source book/magazine.
    * Hopefully pre-populate this list programmatically from the Compendium itself but can hardcode it otherwise and update periodically as books are released that we want to parse.
    * Be sure that once the filter is working that you can search for 

&lt;blank&gt;

 name in a book to return ALL creatures from that book.
  1. Implement a "spider" command to perform some action on each creature card/page returned from a search.
    * The ultimate goal being to grab an entire book and be able to parse every creature from that to make grabbing new content easier.
    * Should be trivial to allow this to work on any sort of filter set up, for example: levels 1-6 creatures from MM3 or something.
  1. Add an option to choose either to dump to the original author's token type, or to dump to our token type.
    * This also entails figuring out how to format stuff to fit our campaign properties, power macro format, default macros and so on.
    * We may have to make some sacrifices for the sake of being able to parse powers better.  The concept of WeaponDice abstracted over all of your melee weapon powers doesn't really work with NPCs from the compendium.  However, if I can get the XML input parser to work we could to this: spider compendium for monster Bob, export Bob to XML in Monster Builder format, manually edit Bob's XML to change from a "1d8" string for longsword damage to instead be "getProperty("WeaponDice1")" or something similar, import that editted XML, export as our token.
  1. Add XML export to file(s), or to text area.
    * The text area export should not work if you are spidering, and when spidering it should probably create an XML file for each creature in a destination folder.
  1. Allow multiple types of export to happen at the same time to save on the amount of spidering required.
    * In theory could dump any/all combination of: XML, our token, his token, HTML.

## Future list ##

  1. Implement a plain text parser for creature cards using either copied text from the Compendium, a PDF, or manually typed text.
    * This will future-proof the Tokenmaker in case Wizards drops the compendium, we stop subscribing to the compendium, or if they break the webcode so badly that you can't spider it anymore.
  1. XML parser that can either read 1 file, a folder of files, or XML pasted into a text area.
    * Monster Builder format is probably the one we should adopt so that Masterplan can read it easily.
  1. Add an option to the PlayerToken maker to export to our player token, or his.
  1. HTML export using whatever goofy format Masterplan likes for its Statblocks.
    * Formats will differ between NPCs, Magic Items, Traps and so on.
  1. Add a raw Compendium HTML file dump to allow the spider to run once and always have all of the various pages related to your query.
  1. Allow import of locally saved Compendium HTML file(s) from a folder or a single pick.

# Masterplan #

  1. Code a quick add-in to import all XML creature files from a folder into X-named library.
  1. Extend this add-in to mass import HTML files if we ever get to the point that we can parse other stuff such as: Templates, Traps, Skill Challenges, or Magic Items.
  1. Add NPCs for Dark Sun Campaign Guide and Creature manual, and MM3.
  1. Add NPCs for Gamma World.
  1. Add other content as wanted.

# Maptools #

(strikethrough means it is finished)

  1. (PHASE 2) make a power creation macro that will set up a power and comment it.  this will ONLY create powers, not edit them so people will need to refer to comments to edit powers later if needed
  1. create a template PC and NPC token for my campaign
    * give PCs 10 attack mod props, and 10 damage mod props
    * npcs probably only need 5 of each
    * use derivied properties to do statmods, maxhp, and healing surge value and whatnot
    * healing surge value property
    * magic item uses property
    * number of AP property
    * make AP and magic item uses part of the mouse over stat sheet, and just use the Used AP state to tell yourself if you've popped an AP yet for a given encounter
  1. finish targetting form
    * it will actually be 2 forms, first will be target picking from dropdowns (for multi/single target powers), and have various modifiers like CA/cover
    * with radio buttons to say affect ALL or affect NONE or affect SOME
    * if anything is selected to say affect SOME then pop second form with tabs in it.  each tab will have all targets listed and allow user to pick
    * which targets get CA or cover against them and so on
    * each target dropdown in the first form will also have a None choice so they can opt to not hit something, and each list will include ALL valid
    * targets so a user could double up on the same guy if they wanted
    * in the 2nd form each target dropdown will not include a None choice because you need to specify bonuses against each dude (defaults will be set to
    * DON'T apply, so they could just click through it and nobody will get modifiers)
  1. ~~figure out how to make heal powers into a macro that does all of the work including update hp, decrement healing surges etc.  might need to make a 7th attack type beyond a utility power even~~ **([r59](https://code.google.com/p/darksun-maptools-framework/source/detail?r=59))**
  1. (PHASE 2) implement striker damage, figure out a clean way to make it only apply to one target per round (in the case of multi-target attacks)
    * if have a drop down appear on the 2nd stage target form (where you apply mods to opponents) can list all available targets for attack and allow
    * player to choose one or none.  make 1st stage form just have a check box to say YES i am applying striker damage or something.
    * otherwise in the case of multi-class guys who get striker damage as an encounter power i can look into the "damage only" typed attack and have
    * them use that to automatically apply it to a single target
  1. ~~give a checkbox to make attack rolls reroll d20s, such as with avengers and their Oath or whatever~~ **([r34](https://code.google.com/p/darksun-maptools-framework/source/detail?r=34))**
  1. ~~figure out a system to implement Vulns/Resists automatically when applying various damages~~
    * ~~the system would check if you have vuln/resist state, and then a vuln/resist amount property to determine how much to reduce damage taken~~
    * ~~damage reduction/increase would not be readily apparent to players and easily hidden with this system by simply putting innate resistances onto monsters in the amount properties~~
    * ~~resist all would need a slightly special consideration since no keywords could counter it~~
    * ~~this means will need to get a real Keyword system going for each attack used to determine what damage type it is and so on~~
    * ~~perhaps would need to look into making Keyword stables similar to the attack/damage-mod stables i'm planning so that a player could toggle on a cold weapon or something for all of their Weapon powers (which use stable1 or whatever), or toggle it off if against a resist monster~~ **([r52](https://code.google.com/p/darksun-maptools-framework/source/detail?r=52))**
    * this system should also check the attacker for certain statuses (Weakened or whatever), and the defender for statuses (Insubstantial or something) and modify damage/attack accordingly
    * would need to consider improving the marking system a bit by giving a marked\_red status, and then a mark\_red\_source status so that you can say as an attacker if i am marked\_color and my defender target is not marked\_color\_source then i get a -2 attack
    * would probably add another 5 mark colors and just try to be smart about the usage of them.....
    * unless i got clever and just did a run down of all tokens on the board filtering based on each mark color in turn until i find an empty one, then use the first target/source pairing that hasn't been used yet (this would be done in the status application coding, just put a "Mark" in under
    * inflicted statuses for a player and the code behind the scenes would automatically assign you an open color)
    * "piercing" type magic items/feats that bypass resistances would be dealt with manually at the start or else can implement a pierce property that indicates how much your say cold powers bypass resistances or something
  1. (PHASE 2) Add a States text box to the Targetting Screen that lets you enter states to be inflicted on that individual power.  Do something as simple as an array like this:
    * 0,Prone,1,Stunned,etc
    * An alternating array where the number is what attack result you want (0 = miss, 1 = hit, 2 = crit, 3 = always), and then the name of a state
  1. change State application code to not need a color choice when putting it in the list.  Instead have code search the map for all monsters with say Quarry\_firstColor and continue down the list until it finds a matching color.  Can then store the fact that you are a "QuarrySource" or whatever locally to your token and make those properties show up on mouseover.
  1. ~~Quick macro to clear MarkedBy prop and all Mark states on current token.~~ **([r33](https://code.google.com/p/darksun-maptools-framework/source/detail?r=33))**
  1. Revamp the Mod system concerning defenses (AC/Will/Fort/Ref/All 5)
    * Add a static number of properties for each defense (and some for All) that hold a single mod.
    * Add a couple Override mod properties per defense that are special in that they don't need any explanation
    * Create a mod list property that shows on mouseover that is just a list made up of ALL the mods on a defense.  Could even just make it a single property using some clever {} code stuff so it would look like: "All: -2, Will: 2, 3, Fort: 7".  This will let players and the GM know that a token has modifiers applied to its defenses.
    * Add a couple Hidden mod properties that don't show up in the mod list?  Would be useful for monster powers that buff/debuff themselves that a player shouldn't know about but should still apply anyway.
    * Each defense will have a property that holds an Object Json that describes each Mod field.  So Json key entry of "0" in the AC mod description property would explain what ACMod0 is.  Probably store Name of the source of the mod, the attack used to apply it.  This will allow us to add defensive mod applications to powers on miss/hit/crit and be sure that they aren't being applied multiple times by the same power/source.  Other applications from powers (Vulns/Resists/States) don't have the same problems that applying mods do because they don't just directly stack.
  1. ~~Add a boolean to attacks to require a Milestone having been passed before allowing the attack, but user can override if needed.  Will need a property on player tokens for Milestone, and a quick GM macro to indicate that a Milestone has come that will increment AP/Magic Item use and set Milestone to 1 for all PCs.~~ **([r55](https://code.google.com/p/darksun-maptools-framework/source/detail?r=55))**
  1. ~~Mess with properties to get Vuln/Resist to appear on mouseover when they exist for PCs or NPCs.~~ **([r52](https://code.google.com/p/darksun-maptools-framework/source/detail?r=52))**
  1. ~~Second Wind power macro so players know when they have used it, and have it do all of the various state setting and healing in 1 click.~~ **([r60](https://code.google.com/p/darksun-maptools-framework/source/detail?r=60))**
  1. Implement latest version of macroIO (v1.2) from aliasmask.
    * Test to see how it handles comments and if it cleanly removes them from the actual macro you import to it may be worth it to start saving all of our macro code using his format so we can insert flipping comments into it.
  1. Integrate group movement macro stuff from here: http://forums.rptools.net/viewtopic.php?f=46&t=13280

DONE:

<b>
1) convert damage tooltip functionality from t: to span like hit rolls<br>
<br>
2) make the entire top part of power card into a tooltip using span with just the power name as text<br>
<br>
4) finish up states for campaign: Grabbed, Grabbing, Vulnerabilities to all damage types<br>
<br>
18) make a milestone macro that will increment magic item usage property, increment AP property, clear the Used AP state<br>
<br>
21) spruce up tooltips for the various things including: attack rolls, damage rolls, extra rolls, and power cards<br>
<br>
27) implement Optional parameter type of behavior in the CallAttack macro so that I can append new parameters to the end of the macro later and not<br>
have older versions on older tokens immediately break<br>
<br>
7) update bonus parser such that if a value for a pairing = 999, then the user actually wants to refer to a property from their token using the<br>
key from the current pairing (exact spelling needed).  This way easy stuff like Level and statmods can just reference macro properties and not<br>
need updating as you level - didn't need to do this, can instead get clever in properties by using a {strformat} to make it emulate a normal macro<br>
operation and pull in other variables.  will need to make variables like HalfLevel/StrMod and so on because I can't parse Properties this way, but<br>
this will work.<br>
<br>
22) update mod parser to allow for die rolls instead of static bonus/penalty - this isn't really feasible without a lot of pain in the ass work.  instead a player could get clever using strformat in their AtkMod# or DamMod# properties to set a placeholder %d to something like eval(1d8) and just<br>
make the name of the variable include the die roll like this: PenorMod->1d8(%d),eval("1d8") which would give something like: PenorMod->1d8(6) when<br>
you actually rolled it.  puts more burden on the player but it works out cleaner this way in code<br>
<br>
3) add Extra Roll functionality for misc. rolls on some powers<br>
<br>
26) when doing the apply damage code if a monster is killed set it to dead and clear all other states<br>
<br>
27) Fix up skill roller to associate stat values with each skill so a player only has to enter their training + racial/class/item bonuses for each<br>
skill to be stored, and the roller will apply Level and stat mod as needed<br>
<br>
5) add to attack macro: states to target on hit/miss/always, states to self on hit/miss/always, halo to self on hit/miss/always<br>
use a single nested Json in each power to store it, similar to:<br>
"{"1":{"marked_yellow","sleeping"},"2":{"marked_blue","prone"}}"<br>
the halo states will have their numeric key and a single color rather than a list<br>
each numeric key will determine which of the 9 possible outcomes is, keyed as:<br>
1 = states to target on hit<br>
2 = states to target on miss<br>
3 = states to target always<br>
4 = states to self on hit<br>
5 = states to self on miss<br>
6 = states to self always<br>
7 = halo to self on hit<br>
8 = halo to self on miss<br>
9 = halo to self always<br>
the second value in each json pair is just a json array containing state names (need exact spelling on them)<br>
this will be a bit ugly to set up by hand, but easily done through a power creation macro<br>
<br>
9) finish creating reticle token<br>
flag it as npc, and create a reticle state to set on it to use for target filtering<br>
add a section of custom light sources of "aura" type with shape square in various sizes<br>
probably do a burst 1, burst 2, burst 3, burst 4, burst 5, and burst 10 as macros that toggle the aura needed to display that burst template<br>
7.5 range with square shape = burst 1, 12.5 = burst 2, and so on<br>
<br>
11) update attack macro to take in a distinct number of targets per power rather than making it a user entered choice<br>
<br>
12) update attack/damage table output to include target on each line (once have targetting form finished)<br>
<br>
13) get the whole package deal working within the attack macro to roll attacks, check defenses of each target, and apply appropriate damage/states<br>
to affected units and so on <a href='this.md'>will be a lot of work</a>

15) revisit recharge powers macro<br>
- changed it to simply run on the token it is called from to make things easier<br>
<br>
17) add another attack type for items beyond utility that will check number of magic item uses on token and spit out a warning if = 0, otherwise<br>
will simply decrement it downwards<br>
<br>
19) update short rest/extended rest macro to only work if the number of things you have selected = 1<br>
<br>
24) implement on-miss damage to be rolled/used for stuff like Reaping Strike<br>
when going to apply damage if target is an NPC and MaxHP=1 then know that it is a minion and do not apply damage and indicate as such in damage<br>
printout<br>
- also implemented 1/2 damage with this parameter<br>
<br>
16) revisit initiative macros for adding all units/tokens to init list<br>
- fixed up AddAllPCToInitiative for stomp's game<br>
- In order to make NPCs work easier just make a same named "Initiative" macro on each token, highlight ALL npcs you want to roll for, and click the<br>
"Initiative" macro in the Common macros area and should be good to go<br>
<br>
30) Similar to #29 but only for the Marked_color states.  Make a MarkSource property that is only filled if you mark a guy, and remember to clear it<br>
if you have no marks applied for your turn.  Update the DoAttack code to check if you have any Marked_color states, and if you do and then attack<br>
a target check that your Marked_color state = their MarkSource, otherwise take a -2 to attack.<br>
<br>
31) If players in game take feats/powers that increase the default -attack penalty for Marking then will have to implement a MarkPenalty property on<br>
tokens, and have any attack that marks include the penalty in an optional parameter.  Default to -2.  Only implement this if needed.<br>
<br>
32) Add a checkbox to TargetScreen called "Do not roll damage" that stops damage from being rolled for a power, but still applies States/Marks.  Use<br>
this for when a person uses something that allows them to reroll a power.<br>
<ul><li>Added a checkbox (Yes roll damage?) that is defaulted to checked.  Should work for both games.</li></ul>

33) Make macros to manually set or remove Attack & Damage bonuses, and Vuln/Resists in a seperate macro.<br>
<ul><li>Done as an individual macro, and as a pair of macros for the GM to use over multiple selected tokens<br>
</b>