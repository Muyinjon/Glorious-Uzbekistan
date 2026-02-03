{{version|1.8}}
[[Journal]] entries represent ongoing situations or goals for the player to navigate. Journal entries are defined in {{path|common/journal_entries}}.

==Example entry==
Journal entries can become active either through a scripted [[trigger]] block or by an [[effect]]. Once active, an entry can conclude by ''completing'', ''failing'', ''timing out'', or becoming ''invalid''. When a journal entry activates or concludes, it can fire effects, and while it is active, it can apply a modifier to the country and effects can be fired on weekly, monthly, or yearly pulses. The order of scripted elements in a journal entry does not matter.

{| class="mw-collapsible mw-collapsed"<!-- Table is the easiest way to keep the expand button on the left -->
|+ style="text-align:left; white-space:nowrap;" |Example:
|
<pre>
journal_entry_key = {
	# root = the owner of the Journal Entry (country scope)
	# scope:journal_entry = this Journal Entry (journalentry scope)
	# scope:target = target value, with which this Journal Entry was added using `add_journal_entry` effect

	# optional journal entry group as specified in common/journal_entry_groups
	group = journalentrygroup_mygroup

	# optional image that shows in the journal entry widget near the description, default = NDefines::GUI::JOURNAL_ENTRY_ICON_DEFAULT (set in /defines/00_interfaces.txt)
	icon = "gfx/interface/icons/event_icons/event_industry.dds"

	# optional trigger which determines if a journal entry can be shown, default = no
	# is ignored when JE is added through `add_journal_entry` effect
	is_shown_when_inactive = {
		c:GBR = root
	}

	# one or more scripted buttons. See common/scripted_buttons/_scripted_buttons.info for more info
	scripted_button = scripted_button_key

	# optional trigger - when both this and is_shown_when_inactive is true, the JE is Activated, default = yes
	# is ignored when JE is added through `add_journal_entry` effect
	possible = {
		c:USA = {
			owns_entire_state_region = scope:target
		}
	}

	# effect which happens when a journal entry is activated by having its `is_shown_when_inactive` and `possible` triggers become true or when JE is added through `add_journal_entry` effect
	immediate = {
		c:USA = {
			# saved scopes can be used in any events triggered from the Journal Entry, as well as in the loc for the Journal Entry itself
			# To use saved scopes in loc: JournalEntry.GetTopScope.sCountry('saved_scope_name') or SCOPE.sCountry('saved_scope_name')
			save_scope_as = god_bless_america
			set_variable = {
				name = freedom
				value = 1
			}
		}

		set_variable = test_variable

		# all events in these on_actions can refer to scope:god_bless_america
		trigger_event = {
			id = test_event.1
			days = 0
			popup = yes
		}
	}

	# completion trigger, use is_goal_complete = yes in here if you're testing a tracked goal metric; if left blank, cannot be completed
	complete = {
		scope:god_bless_america = {
			NOT = { owns_entire_state_region = STATE_ALASKA }
		}

		custom_tooltip = {
			# Following loc functions can be used:
			# [JournalEntry.GetTotalGoalValue] - goal absolute value (fixed point)
			# [JournalEntry.CalcCurrentGoalValue] - current progress absolute value (fixed point)
			# [JournalEntry.GetGoalAddValue] - goal relative value (fixed point)
			# [JournalEntry.GetGoalProgressValue] - current progress relative value (fixed point)
			# [JournalEntry.GetGoalProgressPercent] = GetGoalProgressValue / GetGoalAddValue (fixed point)
			text = journal_entry_key_goal_text
			scope:journal_entry = { is_goal_complete = yes }
		}
	}

	# effect which is executed when 'complete' trigger becomes true
	on_complete = {
		trigger_event = {
			id = test_event.10
			days = 0
		}
	}

	# failure trigger, should spawn event explaining what happens when triggered, framed as a failure; if left blank, cannot fail
	fail = {
		NOT = { c:GBR = root }
	}

	# effect which is executed when 'fail' trigger becomes true
	on_fail = {
		remove_variable = test_variable
		scope:god_bless_america = {
			remove_variable = freedom
		}
	}

	# optional invalidation trigger, should not notify player when it triggers, just clean up and silently disappear due to journal entry no longer being valid; if left blank, cannot be invalidated
	invalid = {

	}

	# effect which is executed when 'invalid' trigger becomes true
	on_invalid = {

	}

	# dynamically updated text, which describes the current status of the Journal Entry
	# To use in loc or UI: [JournalEntry.GetStatusDesc]
	# If this is not specified, GetStatusDesc will instead return loc from key <journal_entry_key>_status
	status_desc = {
		first_valid = {
			triggered_desc = {
				desc = journal_entry_key_status_1
				trigger = {
					has_variable = mining_strike
				}
			}
			triggered_desc = {
				desc = journal_entry_key_status_2
				trigger = {
					has_variable = industrial_strike
				}
			}
			triggered_desc = {
				desc = journal_entry_key_status_2
				trigger = {
					has_variable = railway_strike
				}
			}
		}
	}

	# the number of days before this journal entry forcibly transitions, can be used to transition silently or into another event, framed either as success, failure, or neutral; if left blank or set to zero, will not time out
	timeout = 720

	# effect which is executed when journal entry is timed out
	on_timeout = {

	}
	
	# zero or more static modifiers that will be applied to the Journal Entry when it activates, where they will propagate to the country
	modifiers_while_active = {
	
	}


	# on_action which is triggered every first day of the week
	on_weekly_pulse = {
		random_events = {
			100 = 0
			10 = test_event.2
			10 = test_event.3
			10 = test_event.4
		}
	}

	# on_action which is triggered every first day of the month
	on_monthly_pulse = {
		events = {
			test_event.5
		}
	}

	# on_action which is triggered every first day of the year
	on_yearly_pulse = {
		effect = {
			scope:god_bless_america = {
				change_variable = {
					name = freedom
					add = 1
				}
			}
		}
	}

	# a script value computing the goal completion metric
	current_value = {
		value = gdp
	}

	# when the journal entry is activated current_value and goal_add_value are evaluated and added together to determine the goal value
	goal_add_value = {
		value = gdp
		multiply = 0.25
	}

	# the highest weighted active journal entry appears in the goal tracker on the main screen
	weight = 200

	# yes/no, determines if this journal entry should be transfered if the player switches country through a revolution or by releasing a subject. Note that external dependencies such as country variables etc are not automatically inherited
	transferable = no
	
	# yes/no, determines if this journal entry is allowed to be inherited by a victorious revolution. Revolutions also get all variables from the defeated parent country, so most JEs *should* be inherited in this way
	# NOTE: transferable = yes will always mean that revolution inheritance is blocked as these JEs should stay with the player at all times
	can_revolution_inherit = yes

	# optional trigger, progress text is shown if this is defined and true
	is_progressing = {
		exists = scope:target
		scope:target = { is_under_construction = yes }
	}

	# yes/no, if yes, a progress bar is shown
	progressbar = yes

	# yes/no, if yes, the Journal Entry can return to an inactive state if its possible trigger reverts to false
	# if no or unspecified, an activated Journal Entry cannot return to being inactive even if it is no longer considered possible
	can_deactivate = yes
	
	# dynamically updated text, which is shown over the progress bar of the Journal Entry
	# value can be a localization key or first_valid + triggered_desc script
	# To use in loc or UI: [JournalEntry.GetProgressDesc]
	# If this is not specified, GetProgressDesc will instead return loc from key <journal_entry_key>_progress
	progress_desc = journal_entry_goal_progress_integer
	
	# tutorial lesson explaining HOW to complete the Journal Entry
	how_tutorial = how_tutorial_lesson_key
	
	# tutorial lesson explaining the WHY around the Journal Entry
	why_tutorial = why_tutorial_lesson_key

	# whether a Journal Entry should be pinned in its outliner by default. Defaults to 'no'
	should_be_pinned_by_default = yes
}
</pre>
|}
==Entry group==
Journal entries can be visually grouped by scripted categories. These are defined in {{path|common/journal_entry_groups}}. The script name of the group is also used as a [[localization]] key.

Example:
<pre>
journalentrygroup_mygroup = {

}
</pre>
Note that while it is defined as a block, nothing goes inside the block.

A journal entry is added to a group with <code>group = group_script_name</code>.

==Activation==
For a journal entry to become active, it must either fulfill its two [[trigger]] blocks or be added by the [[effect]] <code>add_journal_entry</code>.

===By effect===
The effect <code>add_journal_entry</code> can activate any journal entry, whether it has scripted trigger blocks or not and whether those are fulfilled or not. Additionally, it can specify a ''target'' for the journal entry which can be used in triggers or effects for the entry.

Example:
<pre>
add_journal_entry = { 
	type = je_ig_suppression_1
	target = scope:suppressed_ig
}
</pre>

===By trigger===
Journal entries can also activate by fulfilling scripted trigger blocks, there are two different trigger blocks <code>is_shown_when_inactive</code> and <code>possible</code>. The first determines whether a journal entry appears in the list of potential entries for a player and the second whether that entry becomes active. Both must be true for an entry to become active without using the <code>add_journal_entry</code> effect. By default, <code>is_shown_when_inactive</code> returns false and <code>possible</code> returns true.

Example:
<pre>
is_shown_when_inactive = {
	is_player = yes
	NOT = { has_variable = mechanized_farming_je_done }
	has_technology_researched = threshing_machine
}

possible = {
	has_technology_researched = mechanized_farming
}
</pre>

===Immediate effect===
Once a journal becomes active, it can fire an effect through an <code>immediate</code> block. 

Example:
<pre>
immediate = {
	trigger_event = { id = suffragist_events.9 popup = yes }
}
</pre>

==Conclusion==
Journal entries conclude and are removed by one of three scripted trigger blocks or timing out. While there is no technical distinction between the triggered conclusions, they are presented differently to the player. The trigger blocks return false by default. Each type of conclusion can trigger effects.

===By completion===
Completion is flavored as positive, accomplishing the goal of the entry, such a owning certain states or achieving a high standard of living.

Example:
<pre>
complete = {
	pop_type_percent_country = {
		pop_type = peasants
		percent < 0.35
	}
	NOT = {
		has_law = law_type:law_serfdom 
		has_law = law_type:law_land_based_taxation
	}
	ig:ig_industrialists = {
		is_marginal = no
	}
}

on_complete = {
	trigger_event = { id = urbanization_events.1 }
}
</pre>

===By failure===
Failure is flavored as negative, not avoiding certain conditions, such as losing states or having a low standard of living.

Example:
<pre>
fail = {
	NOT = { has_state_in_state_region = STATE_ORAN }
	NOT = { has_state_in_state_region = STATE_ALGIERS }
	NOT = { has_state_in_state_region = STATE_CONSTANTINE }
}

on_fail = {
	trigger_event = algeria_events.3
}
</pre>

===By invalidation===
Invalidation is flavored as neutral, such as the entry no longer being relevant.

Example:
<pre>
invalid = {
	NOT = {
		exists = c:BRZ
	}
}

on_invalid = {
	if = {
		limit = {
			has_modifier = brazilian_slave_trade_modifier
		}
		hidden_effect = {
			remove_modifier = brazilian_slave_trade_modifier
		}
	}
}
</pre>

===By timeout===
Timeout is a number of days before the journal concludes. This is often paired with either a <code>complete</code> or <code>fail</code> trigger block to represent either a time limit to complete the entry or a 'hold out' time to avoid the failure condition. A journal entry might also have only a timeout with no <code>complete</code> or <code>fail</code> to represent a situation that has some effect then disappears after some time.

Example:
<pre>
timeout = 3650

on_timeout = {
	trigger_event = { id = red_scare.20 }
}
</pre>

==Pulse effects==
There are three different pulses: <code>on_weekly_pulse</code>, <code>on_monthly_pulse</code>, and <code>on_yearly_pulse</code>. Each fires its contained effects on the first day of each period. Pulses are used for random events or repeating effects related to the journal entry. Both uses can be used in the same pulse if desired. Pulses are effectively an [[on actions|on action]] for the journal entry.

Example:
<pre>
on_monthly_pulse = {
	effect = {
		if = {
			limit = {
				has_law = law_type:law_slavery_banned
			}
			change_variable = {
				name = acw_progress_var
				add = 1
			}
		}
	}
	random_events = {
		# American Civil War buildup
		200 = 0
		10 = acw_events.1 # The Abolitionist's Martyrdom
		10 = acw_events.3 # The Wilmot Proviso
	}
}
on_yearly_pulse = {
	events = {
    	example_event.1 # an event which always triggers on pulse
    }
	random_events = {
		40 = 0
		20 = acw_events.2 # The Fugitive Slave
		20 = acw_events.5 # Bleeding Kansas
		20 = acw_events.6 # The Caning of Charles Sumner
	}
}
</pre>

==Scripted buttons==
Scripted buttons are a way of tying [[decision modding|decisions]] to a journal entry. A journal entry can add any number of scripted buttons using <code>scripted_button = scripted_button_key</code> for each one. Scripted buttons are defined in {{path|common/scripted_buttons}}.

A scripted button's script is very similar to a decision, with a trigger block for showing the button <code>visible</code>, another for being able to use it <code>possible</code>, an <code>ai_chance</code> block, and an <code>effect</code> block. Each scripted button key must be unique, but the name and localization keys do not.

Example:
<pre>
scripted_button_key = {
	name = "LOC_KEY"
	desc = "LOC_KEY_DESC"

	visible = { always = yes }
	
	#Country scope
	ai_chance = {
		value = 0
		if = {
			limit = { bureaucracy > 0 }
			add = 5
		}
	}

	possible = {
		not = {
			has_variable = my_cool_var
		}
	}

	effect = {
		set_variable = my_cool_var
	}
}
</pre>

==Other elements==
Journal entries have a number of other optional elements.

===Goal===
Journals can use a 'goal' which is a [[script value]] computed when the journal activates and can be anything from a scripted variable to a game value such as GDP or standard of living.

The goal's base value is set through the script value <code>current_value</code> which is added to <code>goal_add_value</code>. Together these can be used with the special boolean journal trigger <code>is_goal_complete</code>, which can be used in any of the conclusion trigger blocks. This goal amount cannot be changed after the entry activates, and is often used in conjunction with the progress bar to check a value, such as number of months, towards a goal.

Example:
<pre>
current_value = {
	value = scope:coffee_scope.market_goods_buy_orders
}

goal_add_value = {
	value = scope:coffee_scope.market_goods_buy_orders
	multiply = 2.5
}
</pre>
Note that in the example above, because <code>current_value</code> and <code>goal_add_value</code> are added together, the final goal amount is 3.5 times the buy orders of coffee compared to when the entry activates.

===Modifier===
Journals can use the block <code>modifiers_while_active</code> to define a list of [[modifier modding|scripted modifiers]].

Example:
<pre>
modifiers_while_active = {
	modifier_assimilation_nnc
}
</pre>

===Progress bar===
<code>progressbar = yes</code> adds a dynamic progress bar, which is tied to the goal value if defined.

===Scripted progress bar===
These types of progress bars are defined in {{Path|common/scripted_progress_bars}}. A journal entry can add any number of scripted progress bars using <code>scripted_progress_bar = scripted_progress_bar_key</code> for each one. Scripted progress bars do not interact with the goal value and instead have a defined maximum, minimum, and start value.

<code>start_value =</code> sets the initial value, with <code>max_value =</code> the maximum value and with <code>min_value =</code> the minimum value. The value can be updated weekly, monthly, or yearly, or some combination of those. Each is set as a block scripted values setting the amounts

There are several skins for the progress_bars, these would be:

* double_sided_gold
* default_green
* default_bad
* default
* double_sided_bad

The double sided skins are for bars in the style of The Great Game, with opposing outcomes.

The term <code>is_inverted = yes</code> inverts descriptions so that negative progress is good and positive progress is bad, as opposed to the default descriptions.

Example:<pre>
divided_monarchists_bonapartist_progress_bar = {
	name = "divided_monarchists_bonapartist_progress_bar"
	desc = "divided_monarchists_bonapartist_progress_bar_desc"

	default = yes #This is the skin

	monthly_progress = {
		if = {
			limit = {
				NOT = {
					has_variable = fra_divided_monarchists_freeze #This variable stops iteration, and is granted by reaching 360 with one dynasty. It's reset when this JE is completed, or when je_cement_the_rightful_dynasty fails.
				}
			}
			add = {
				desc = "bonapartist_progress_from_characters"
				value = owner.var:bonapartist_progress_from_characters
			}
			add = {
				desc = "bonapartist_progress_from_igs"
				value = owner.var:bonapartist_progress_from_igs
			}
			add = {
				desc = "bonapartist_progress_from_military"
				value = owner.var:bonapartist_progress_from_military
			}
			add = {
				desc = "bonapartist_progress_from_legitimacy"
				value = owner.var:bonapartist_progress_from_legitimacy
			}
			add = {
				desc = "bonapartist_progress_from_effects"
				value = owner.var:bonapartist_progress_from_effects
			}
		}
	}

	start_value = 10
	min_value = 0
	max_value = 360

}
</pre>

===Localization and status===
Journal entries have the following default localization keys:
*<journal_entry_script_name>: the localized name/title of the entry
*<journal_entry_script_name>_reason: the description/flavor of the entry
*<journal_entry_script_name>_status: text that describes the current status of the entry, can be overridden with a <code>status_desc</code> block
*<journal_entry_script_name>_progress: text above the progress bar, can be overridden with a <code>progress_desc</code> key or block

The progress text is only shown if the trigger block <code>is_progressing</code> returns true.

Example:
<pre>
is_progressing = {
	has_journal_entry = je_pedro_brazil
	scope:target ?= {
		is_heir = yes
	}
	NOT = {
		has_law = law_type:law_no_womens_rights
	}
}
</pre>

===Icon===
An icon can be defined by <code>icon = "<file path>"</code>, for example <code>icon = "gfx/interface/icons/event_icons/event_industry.dds"</code>.

If no icon is defined, the default icon <code>event_default.dds</code> [[File:Event default.png|36px]] is used.

===Misc===

*<code>can_deactivate = yes/no</code>: default no, if yes, the entry deactivates if the possible trigger block returns false at any time.
*<code>weight = <int></code>: orders entries in the outliner by highest to lowest weight
*<code>transferable = yes/no</code>: default no, if yes, the entry is transferred with the player through country changes due to revolution or releasing and playing countries.
*<code>can_revolution_inherit = yes/no</code>: default yes, if no a victorious revolution does not inherit this entry; note that <code>transferable = yes</code> blocks this from applying
*<code>should_be_pinned_by_default = yes/no</code>: default no, if yes the journal starts pinned to the outliner when activated
*<code>how_tutorial = scripted_tutorial</code>: adds a clickable tutorial to the journal entry, flavored as ''how'' to complete the entry
*<code>why_tutorial = scripted_tutorial</code>: adds a clickable tutorial to the journal entry, flavored as ''why'' to complete the entry
*<code>display_progressbar_as_months = yes/no</code>: default no, if yes, the progress bar will display the remaining progress in years and months

==References==
<references />
{{Modding navbox}}
[[Category:Modding]]
