{{Version|1.8}}{{See also|Country modding}}
This guide covers how to add a new country into the game so that it exists at the start of the game in 1836.

Important starting notes:

* It is recommended to do text editing with a better editor than plain Notepad. My recommendations are Notepad++ (which is what this guide will use), Visual Studio Code, and Sublime.
* All .txt files in the game must have the encoding UTF8-BOM. They may not work properly if they don't have that. If you don't know how to set the encoding, just copy an existing file and change its contents.
* Everything after a <code>#</code> in a text file is "commented out", meaning the game ignores what's written after the #.
* This guide is done in the context of a mod that is already set up and working properly, so make sure you've got that important step done first.

== Defining the country ==
You can find the country definitions in {{path|common/country_definitions}}.

By "defining" we mean two things:
* A human meaning of "deciding where the country will be and what it will look like"
* A code meaning of telling the game information about our country so that it knows what the country is

So first of all, think about what country you want, and especially where it will be located. The country in this guide will be called Cyrenaica and will be located here in Tripolitania's land. It will own the whole of the Libyan Desert state.
<ul><li style="display: inline-block;">[[File:Country creation1.png|thumb|none|The new example country will be here]]</li>
<li style="display: inline-block;">[[File:Country creation2.png|thumb|none|The Libyan Desert state]]</li></ul>

Now it is time to decide on a tag (a three-character short script name for the country). CYR makes sense as a tag for Cyrenaica, but anything could be used. Make sure that CYR isn't already used in the base game by using Notepad++'s ''Find in Files'' function to see whether CYR shows up in the base game country definition files anywhere. And if it doesn't, great! Otherwise, just pick something else. Now go to the mod's country definition folder and add a definition for it.
<ul><li style="display: inline-block;">[[File:Country creation4.png|thumb|upright=1.5|none|Make sure the path points at the Vic3 country definition folder and put your desired tag, then hit Find All]]</li>
<li style="display: inline-block;">[[File:Country creation3.png|thumb|upright=1.5|none|This is the path that the country definition file must be in]][[File:Country creation5.png|thumb|upright=1.5|none|Country definition code]]</li></ul>

As can be seen, there are some important parts to a country definition. For a full list of country definition parameters, see [[Country modding#Country definition|Country modding § Country definition]].

It starts with the tag, then add a colour (this can be in HSV360, HSV decimal, or RGB255 format). Use an online RGB color picking tool, or image editing software to find the desired colour.

After that, choose what type of country it will be:

* recognized - Mostly used for "western" countries
* unrecognized - Used for "non-western" countries, akin to Vic2's "uncivilized"
* decentralized - For countries without a central system of government, usually used for indigenous people
* colonial - For countries that are colonies or former colonies of recognized countries

Then state its tier, which is essentially the size of the country, as well as controlling in part whether it can form another country

* city_state
* principality
* grand_principality
* kingdom
* empire
* hegemony

After that, input the primary cultures. The in-game spellings are usually the same as the code spellings, but not always. Double check in the base game's {{path|common/cultures}} folder if unsure.

Finally, define the default capital state. As above, this is normally the same as what's written in game, except that spaces are always underscores, which is why CYR's state capital is STATE_LIBYAN_DESERT instead of Libyan Desert. A state will always be something like STATE_(NAME). If unsure, check in {{path|map_data/state_regions}} or {{path|common/history/states}}.

Make sure there is a closing bracket, too. Most fancier word processors like Notepad++, Visual Studio Code and Sublime have some way of doing this. Proper bracketing is very important!

== The essential history files ==
History files location:  {{path|common/history}}

History files required for editing: states, pops, buildings, countries

Now that the country is defined, it is time to make it show up in the game. Right now, the game only knows that CYR is a possible country, but it hasn't been told that it should exist. To do this, the history folder is needed, and specifically these three folders, as well as the countries folder.
<ul><li style="display: inline-block;">[[File:Country creation6.png|thumb|none|upright=1.5|These three history folders are the most important ones]]</li>
<li style="display: inline-block;">[[File:Country creation7.png|thumb|none|upright=1.5|Here's the countries history folder]]</li></ul>
[[File:Country creation10.png|thumb|upright|Remember to change TRI to CYR in the POPS history]][[File:Country creation11.png|thumb|And in BUILDINGS, too]]

The order of editing doesn't matter, but this guide begins with states. Files in this folder tell the game who owns what states (or what provinces in the state) at the start of the game, as well as information about what cultures consider the state to be their homeland. Note that the state history files must use a <code>STATES = { }</code> block to enclose all state data. Navigate to STATE_LIBYAN_DESERT:

As seen from the highlighted section, TRI (Tripolitania) currently controls the state. Let's change that:
<ul><li style="display: inline-block;">[[File:Country creation8.png|thumb|none|State history with TRI as the owner]]</li>
<li style="display: inline-block;">[[File:Country creation9.png|thumb|none|Now changed to CYR being the owner]]</li></ul>

If you launch the game now, CYR controls the state, but it's missing information about population and buildings, so it is not yet ready to play. So next is the pops folder, and once again find STATE_LIBYAN_DESERT, and there, too, change TRI to CYR. And do the same in the buildings folder. There are no defined buildings in the state. That makes sense, considering it's mostly desert and has a very small population, but remember that subsistence buildings are added automatically.

Copy another country's file in {{path|common/history/countries}} and paste in your mod's countries folder rename it <code>cyr - cyrenaica.txt</code>. You can change the content later.

And with that, the history folder entries are done for now.

== Localization ==
{{see also|Localization}}
The country is now playable, but in game it is still called CYR instead of a proper country name. That's where localization comes in.

[[File:Country creation12.png|thumb|upright=1.5|Here's where the localization file is. The path is more important than the name of the file.]]
Localization is the way that code is translated into human languages like English. Time to go to our mod's localization folder.

New localization files can have any name as long as it fits a certain pattern. The file must:

* be a <code>.yml</code> file
* have <code>_l_english</code> at the end of the file name (if your localization is English, otherwise use the language's script name, typically its English name)
* start with <code>l_english:</code> in the contents of the file

It is suggested just to copy a localization file from another mod or the base game and empty it (except for the <code>l_english:</code> part).

[[File:Country creation13.png|thumb|Example localization]]
The localization is pretty simple, as shown here:

<pre>TAG: "country name"
TAG_ADJ: "country adjective"</pre>

<code>TAG</code> stands in here for the country's tag, i.e. CYR in this case.

Now the country works properly and the name looks acceptable.
[[File:Country creation14.png|thumb|none|upright=1.5|The country works, but maybe a custom flag and ruler would be better]]

== Population history ==
[[File:Country creation16.png|thumb|upright=1.5|Example population history entry]]
An quite optional file to do: Population history. Not to be confused with the Pops history, the population folder in the history folder gives information about the people who live in the country, specifically their wealth and their literacy. It typically uses two scripted effects, but can use any set of relevant effects. These modify the starting characteristics of a country's population, but the main effect depends on other factors, such as the buildings and production methods present at start for wealth, and technology and education institution for literacy. That is, even a country with <code>effect_starting_pop_literacy_very_high = yes</code> will be mostly illiterate if no education institution is present.

Wealth: (defined in {{path|common/scripted_effects/00_starting_pop_wealth.txt}})

* effect_starting_pop_wealth_low
* effect_starting_pop_wealth_medium
* effect_starting_pop_wealth_high
* effect_starting_pop_wealth_very_high

Literacy (defined in {{path|common/scripted_effects/00_starting_pop_literacy.txt}})

* effect_starting_pop_literacy_baseline
* effect_starting_pop_literacy_very_low
* effect_starting_pop_literacy_low
* effect_starting_pop_literacy_middling
* effect_starting_pop_literacy_high
* effect_starting_pop_literacy_very_high

This is a good idea for helping with the initial balance of the country, and will have important effects on the country over time.

== Characters ==
{{See also|Character modding}}
[[File:Country creation17.png|thumb|upright=1.5|Plenty to talk about for this!]]
If one prefers a ruler and flag which are not automatically generated, those, too, can be defined.

To have a specific character to be the country's ruler – or any other role, the country needs an entry in a file in the character history folder ({{path|common/history/characters}}). Again, it is recommended to go through the base game and/or a mod and copying information and files from there. Possible settings for characters can be found in the base game's common folder, look for folders mentioning character.

Cyrenaica's script is here. There is a first name and a family name. '''Important note:''' If your names are not already localized because they exist as character or culture names, you must add separate localization for them. In Cyrenaica's case, both names were already localized by something, great! If it is necessary to localize them, the localization can be put into any file (generally speaking, localization is only organized so that people can find where things are more easily).

If it is necessary to localize the name, it's pretty much the same as above with the country localization:

<pre>Name: "Localized name"</pre>

So if Cyrenaica's leader's family name needed new localization, it would have looked like this:

<pre>al-Idrisi: "al-Idrisi"</pre>

On the left is the script key of "al-Idrisi" and on the right is the English localization of "al-Idrisi." This may seem like extra steps, but it is important. If the game doesn't detect localization for a name, it won't use the name supplied and will instead use a random name.

Next is the birthdate, so age can be calculated.

Yahya is supposed to be the country's ruler, so the script for that <code>ruler = yes</code>. There are other roles, such as <code>heir</code>. See other character files in the base game or a mod to find out more. The UK has many of characters, for example. It is possible to set the ruler to be a general at the game start, but in Cyrenaica's case that's unnecessary. It has been commented it out so that the game ignores it, but the curious can see what it looks like.

Then one must define what Interest Group the character belongs to or represents. This character is a member of the Sunni Ulema, the Muslim devout interest group. Next is an ideology. These are found in {{path|common/ideologies/00_leader_ideologies}}. Since Cyrenaica is not very urbanized and primarily survives on subsistence, I think the Traditionalist ideology fits best. Finally, a couple of traits, which are in the {{path|common/character_traits}} folder.

Here's what I've got!

[[File:Country creation20.png|thumb|none|There Yahya is, working like a charm!]]

== Flag ==
{{See also|Flag modding}}
[[File:Country creation18.png|thumb|This will look great]]
The new flag will take inspiration from the flag of the Ottoman eyalet of Tripolitania and have some small changes.

To make a basic flag in Vic3, all that is needed is an entry for it in a file in {{path|common/coat_of_arms/coat_of_arms}}. Flags can put in as plain images, but it takes up a bit more space than writing them in script and isn't as flexible. Here is the script for Cyrenaica:

A brief explanation: All flags need a basic pattern to be based on. These are located in {{path|gfx/coat_of_arms/patterns}}. It's important that all flag components be put in quotes and include the file extension (.tga, .dds) for them to work.

What follows is <code>color1</code>, which refers to the first colour that can be used in the flag item. In Cyrenaica's case, <code>pattern_solid.tga</code> only has one colour, so that's all that is need. More colours can be added if the user would like to refer to them later with upcoming items (so instead of having <code>color1 = "yellow"</code> in all three of the crescents, the code could have been written as <code>color1 = color2</code> with <code>color2 = "yellow"</code> underneath my pattern's <code>color1</code>. This would be a quick way to change the colours of all three crescents without needing to change "yellow" to a different colour three times.

There are the three crescents which came from the files in {{path|gfx/coat_of_arms/colored_emblems}}, and they are all the same emblem. A coloured emblem's colour needs to be specified, as mentioned above, but also you don't want the default size and in the center of the flag, it is necessary to tell the game that "this instance should be of this size and the center of the emblem should be located here", which is what the ''instances'' in Cyrenaica's flag are doing. The scale refers to the x and y axis distortion, so if distortion is not desired, just put the same number for both. This flag has shrunk both axes by half, so these crescents are a quarter of their default size. The first crescent has been placed at <code>{ 0.25 0.3 }</code>, which means that 0.25 is a quarter of the way from the left side to the right side (0 is at the left edge of the flag), and that 0.3 is 30% of the way from the top of the flag to the bottom of the flag (0 is at the top). A rotation value has been set for the two later crescents to make sure they don't face the same direction as the first one.

'''Another important note:''' To make your life easier when making flags, it is suggested that the user open the game's command line (right click the game in the Steam library and look at Preferences, and there on the bottom) they should write ''-filewatcher -debug_mode''. The first will update some files in real time as they're saved, so it will not be necessary to restart the game to see flag adjustments, and the second makes it easier to hunt down bugs and find information in the game.

The flag is now done, as is the country as a whole. 
[[File:Country creation19.png|thumb|none|There's the flag]]

== Dynamic country name and colors ==
Many countries have alternative names and colors for certain circumstances. For example, Nejd gets dynamic name "Saudi Arabia" when Nejd fully owns Hedjaz and Germany turns green if it has the "council republic" law. Use of dynamic country names and colors may get a better experience in your mod.

=== Dynamic country name ===
You can define a dynamic name in {{path|common/dynamic_country_names}}, just add a text file in the encoding of <code>UTF-8 with BOM</code>.

For example<pre>
CYR = {
	dynamic_country_name = {
		name = dyn_CYR_empire #This can be just the TAG to reuse the default name
		adjective = dyn_CYR_empire_adj # This can be TAG_ADJ to reuse the default adjective

		is_main_tag_only = yes # default no, if yes then only the primary country for a definition can use this name (i.e. Poland, not a civil war revolt Poland)
		priority = 0 # default 0, if multiple names have valid triggers, higher priority is used. If same priority, first valid one in file is used
		
		trigger = { # Scripted trigger, country-scope
			coa_def_monarchy_flag_trigger = yes
			scope:actor = {
				owns_entire_state_region = STATE_TUNISIA
			}
		}
	}	
}
</pre>
Then, localize the dynamic name and dynamic adjective:<pre>
dyn_CYR_empire:0 "Cyrenacia Empire"
dyn_CYR_empire_adj:0 "Cyrenacian"
</pre>

=== Dynamic country color ===
You can define the color in {{path|common/named_colors}}, and apply the name color in {{path|common/dynamic_country_map_colors}}. Dynamic colors apply the first color that have a valid trigger.

In <code>named_colors</code><pre>
colors = {	
	cyrenacia_empire_red = rgb { 1.0 0.15 0.15 } #This is "decimal" RGB, this value is similar to RBG 255 38 38 or hexcode ff2626
}</pre>
In <code>dynamic_country_map_colors</code>:<pre>
cyrenacia_empire_red = {
	color = "cyrenacia_empire_red"

	possible = {
		OR = {
			AND = {
				exists = c:CYR
				THIS = c:CYR
			}
			AND = {
				exists = c:TRI # dynamic color can be applied in other country
				THIS = c:TRI
			}
		}
		owns_entire_state_region = STATE_LIBYAN_DESERT
		owns_entire_state_region = STATE_TUNISIA
	}
}</pre>

== References ==
{{Modding navbox}}
[[Category:Modding]]
