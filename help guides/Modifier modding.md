{{Version|1.10}}{{see also|Modifier types}}
'''Modifiers''' are containers of values that affect gameplay. Modifiers use [[modifier types]] to define their effects. Modifiers can be defined as ''static modifiers'', sometimes called ''event modifiers'', or in various scripted types, such as [[technology modding|technologies]], [[law modding|laws]], and more.

== Static modifiers ==
Static modifiers are defined in {{path|custom=1|/common/static_modifiers}}, for example:
<pre>
splendid_coastal_state = { # This is the static modifier
    icon = gfx/interface/icons/timed_modifier_icons/modifier_gear_positive.dds # This is an icon, and it is optional
    state_infrastructure_add = 20 # This is a modifier type
    character_popularity_add = 100 # You can add whatever modifier types you want, but be mindful of the scope
}
</pre>
Any number of [[modifier types]] can be used in a static modifier, but note the ''flow'' of each modifier type, as they may have no effect if the static modifier is applied to the wrong scope.

=== Static modifier effects and triggers ===
To apply a static modifier, use the [[effect]] <code>add_modifier</code>:
<pre>
add_modifier = { # Applies a modifier to the current scope
    name = splendid_coastal_state # the static modifier you want
    multiplier = -3 # Optional, multiply modifier type values by a number, script value, or math block
    is_decaying = no # Optional, if yes, it will decay over its lifetime, default is no
    months = 3 # Optional, if it is not set or negative, the modifier is permanent. Can also use days, weeks, or years instead.
}
</pre>
The multiplier value adjusts the base values of the static modifier. This allows inverting or scaling a static modifier's effects.

A short form, <code>add_modifier = static_modifier_name</code>, can be used as well, which is the equivalent of 
<pre>
add_modifier = {
    name = static_modifier_name
    multiplier = 1 #i.e., no multiplier
    is_decaying = no
    months = -1 #i.e., permanent
}
</pre>
The effect <code>remove_modifier = static_modifier_name</code> removes the specified static modifier from the current scope.
{{#invoke:Script docs|commonTable|caption=modifier|tType=Effects|modifier}}

The [[trigger]] <code>has_modifier = static_modifier_name</code> checks for the presence of the specified static modifier in the current scope.

The scripted values that the game uses are defined in {{Path|common/script_values/event_values.txt}}

== Modifier blocks ==
Many scripted types may contain one or more modifier blocks. These are typically defined as a block <code>modifier = { }</code> or similar. These work identically to static modifiers, except that they apply automatically to a certain scope, rather than being added by effect. 

== References ==
<references />
{{Modding navbox}}
[[Category:Modding]]
