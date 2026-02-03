# Victoria 3 Modding - Localization Guide

This guide documents the proper format for localization files in Victoria 3 mods, based on template files and best practices.

## File Structure

### Location
All localization files should be placed in:
```
localization/english/
```

### File Naming
- Use descriptive names with `_l_english.yml` suffix
- Example: `uzb_journal_entries_l_english.yml`, `uzb_events_l_english.yml`

## Basic Format

### Header
Every localization file must start with:
```yaml
l_english:
```

### Entry Format
Each localization entry follows this format:
```yaml
 key:0 "Localized text"
```

**Important:**
- Single space before the key
- Colon (`:`) followed by `0` (revision number)
- Space after the colon
- Text in double quotes (`"`)
- No trailing spaces

### Comments
Comments use `#` and can be on their own line or after an entry:
```yaml
 # This is a comment
 key:0 "Text" # Inline comment
```

## Journal Entry Localization

### Standard Keys
Journal entries use these localization keys:

1. **Title**: `<journal_entry_key>:0 "Title"`
   ```yaml
   je_uzb_silicon_and_silk_road:0 "Revitalizing the Silk Road"
   ```

2. **Reason/Description**: `<journal_entry_key>_reason:0 "Description"`
   ```yaml
   je_uzb_silicon_and_silk_road_reason:0 "Revive the ancient Silk Road through silk production..."
   ```

3. **Progress Text**: `<journal_entry_key>_progress:0 "Progress text"`
   ```yaml
   je_uzb_silicon_and_silk_road_progress:0 "GDP progress: [JournalEntry.CalcCurrentGoalValue|*]/[JournalEntry.GetGoalAddValue|*]"
   ```

4. **Tooltips**: Custom tooltip keys for completion requirements
   ```yaml
   uzb_je_silicon_and_silk_road_tt_silk:0 "Build at least 8 levels of silk plantations in Samarqand"
   ```

### Important Notes
- Use `_reason` NOT `_desc` for journal entry descriptions
- `_lobby` and `_goal` are optional and may not be needed
- Avoid using `GetName` tags - use plain text or `[concept_*]` instead

## Event Localization

### Standard Keys
Events use these localization keys:

1. **Title**: `<event_id>.t:0 "Title"`
   ```yaml
   uzb_silk_road_complete_title:0 "The Silk Road Reborn"
   ```

2. **Description**: `<event_id>.d:0 "Description"` or `<event_id>_desc:0 "Description"`
   ```yaml
   uzb_silk_road_complete_desc:0 "Uzbekistan has successfully revitalized the Silk Road..."
   ```

3. **Options**: `<event_id>.a:0`, `<event_id>.b:0`, etc. (up to 5 options: a, b, c, d, e)
   ```yaml
   uzb_silk_road_complete_option_commercial:0 "Focus on Trade and Commerce"
   uzb_silk_road_complete_option_commercial_desc:0 "Emphasize our role as the commercial hub..."
   ```

4. **Flavor Text** (optional): `<event_id>.f:0 "Flavor text"`
   ```yaml
   uzb_silk_road_complete.f:0 "Quote or flavor text here"
   ```

## Modifier Localization

### Standard Keys
Modifiers use these keys:

1. **Name**: `<modifier_key>:0 "Name"`
   ```yaml
   uzb_modifier_pride_of_the_east:0 "Pride of the East"
   ```

2. **Description**: `<modifier_key>_desc:0 "Description"`
   ```yaml
   uzb_modifier_pride_of_the_east_desc:0 "Industrial self-sufficiency and rising prosperity..."
   ```

## State Trait Localization

### Standard Keys
State traits use these keys:

1. **Name**: `<state_trait_key>:0 "Name"`
   ```yaml
   state_trait_silk_road_hub:0 "Silk Road Hub"
   ```

2. **Description**: `<state_trait_key>_desc:0 "Description"`
   ```yaml
   state_trait_silk_road_hub_desc:0 "A historic crossroads of caravan trade and crafts..."
   ```

## State Name Localization

### Standard Keys
State names use these keys:

1. **Name**: `<STATE_KEY>:0 "Name"`
   ```yaml
   STATE_SAMARQAND:0 "Samarqand"
   ```

2. **Description** (optional): `<STATE_KEY>_desc:0 "Description"`
   ```yaml
   STATE_SAMARQAND_desc:0 "Samarqand is one of the oldest cities in Central Asia..."
   ```

## Character Name Localization

### Format
If you create custom characters, add their first and last names:

```yaml
 Muyinjon:0 "Muyinjon"
 Turobov:0 "Turobov"
```

**Note:** Use the exact name as defined in the character file. If the name has underscores (e.g., `Jean_Francois`), keep the underscore in the key.

## Country Localization

### Standard Keys
Countries use these keys:

1. **Name**: `<TAG>:0 "Country Name"`
   ```yaml
   UZB:0 "Uzbekistan"
   ```

2. **Adjective**: `<TAG>_ADJ:0 "Adjective"`
   ```yaml
   UZB_ADJ:0 "Uzbek"
   ```

3. **Definite Article** (optional): `<TAG>_DEF:0 "Definite Name"`
   ```yaml
   UZB_DEF:0 "Uzbekistan"
   ```

## Special Formatting

### Concept References
Use `[concept_*]` for game concepts:
```yaml
 "Reach 50% [concept_literacy]"
 "Strengthen [concept_colony] administration"
```

### Journal Entry Progress
Use special functions for progress bars:
```yaml
 "GDP progress: [JournalEntry.CalcCurrentGoalValue|*]/[JournalEntry.GetGoalAddValue|*]"
 "Literacy progress: [JournalEntry.CalcCurrentGoalValue|1]/[JournalEntry.GetGoalAddValue|1]%"
```

### Localization Reuse
You can reference other localization keys using `$key$`:
```yaml
 key1:0 "example"
 key2:0 "This is an $key1$"
```

## Common Mistakes to Avoid

### ❌ Don't Use GetName Tags
**Wrong:**
```yaml
 "Build [building_silk_plantation.GetName] in [STATE_SAMARQAND.GetName]"
```

**Correct:**
```yaml
 "Build silk plantations in Samarqand"
```

### ❌ Don't Use _desc for Journal Entries
**Wrong:**
```yaml
 je_uzb_silicon_and_silk_road_desc:0 "Description"
```

**Correct:**
```yaml
 je_uzb_silicon_and_silk_road_reason:0 "Description"
```

### ❌ Don't Forget the Revision Number
**Wrong:**
```yaml
 key: "Text"
```

**Correct:**
```yaml
 key:0 "Text"
```

### ❌ Don't Use Single Quotes
**Wrong:**
```yaml
 key:0 'Text'
```

**Correct:**
```yaml
 key:0 "Text"
```

## File Organization

### Recommended Structure
Organize localization files by category:

1. **Main file**: `Glorious Uzbekistan_cultures_l_english.yml`
   - Cultures
   - Journal entries (main entries)
   - Modifiers
   - State traits
   - State names
   - General content

2. **Journal entries**: `uzb_journal_entries_l_english.yml`
   - Specific journal entry localizations
   - Tooltips

3. **Events**: `uzb_events_l_english.yml`
   - All event localizations

4. **Modifiers**: `uzb_modifiers_l_english.yml`
   - Modifier-specific localizations

## Example Complete File

```yaml
l_english:
 # Journal Entry
 je_uzb_silicon_and_silk_road:0 "Revitalizing the Silk Road"
 je_uzb_silicon_and_silk_road_reason:0 "Revive the ancient Silk Road through silk production, textile manufacturing, and [concept_trade]."
 je_uzb_silicon_and_silk_road_progress:0 "GDP progress: [JournalEntry.CalcCurrentGoalValue|*]/[JournalEntry.GetGoalAddValue|*]"
 
 # Event
 uzb_silk_road_complete_title:0 "The Silk Road Reborn"
 uzb_silk_road_complete_desc:0 "Uzbekistan has successfully revitalized the Silk Road!"
 uzb_silk_road_complete_option_commercial:0 "Focus on Trade and Commerce"
 
 # Modifier
 uzb_modifier_pride_of_the_east:0 "Pride of the East"
 uzb_modifier_pride_of_the_east_desc:0 "Industrial self-sufficiency and rising prosperity..."
```

## Testing

After creating localization files:
1. Check for syntax errors (missing colons, quotes, etc.)
2. Verify all keys referenced in code have corresponding localization
3. Test in-game to ensure text displays correctly
4. Check for missing `:0` revision numbers

## References

- Template files in `k0vpky/Countries creation (Template)/`
- Victoria 3 modding documentation
- Existing vanilla localization files for examples
