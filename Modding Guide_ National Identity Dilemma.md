# **Victoria 3 Modding Guide: The "Sart" National Identity Dilemma**

This guide is optimized for a custom "Uzbekistan" tag that starts with both **Uzbek** and **Tajik** as primary cultures. The central conflict simulates the historical "Sart" dilemma: the struggle to define a singular national identity for a sedentary population that was traditionally defined by lifestyle and religion rather than rigid ethnicity. The goal is to resolve this identity within a strict 20-year window.

## **1\. Core Mechanics: The Journal Entry**

**Title:** The Question of the Sown

**Timer:** 20 Years (7300 days) tracked via a persistent variable identity\_timer.

### **The "Chaos" Trigger**

If the identity\_timer reaches 0 without the player successfully completing one of the three resolution paths (Uzbek Primacy, Tajik Primacy, or the Middle Path), the "Default Chaos" event, **A House Divided**, triggers. This represents a total collapse of state authority over the ethnic narrative.

* **Political Implications:** The country gains the "Identity Crisis" modifier for 10 years, providing \-25% Authority and \-20% Tax Capacity as local administrators ignore the central government.  
* **Ethnic Strife:** All Uzbek and Tajik pops gain \+30% Radicalism. If the "Nationalism" tech is researched, a Tajik secession movement will immediately spawn in Samarkand with a high chance of starting an immediate civil war.  
* **Administrative Failure:** The state loses all progress toward the special bonuses of the three paths, essentially leaving the player with a "failed state" that must spend decades recovering.

## **2\. Expanded Event List (The "Pulse" Events)**

These events should fire roughly every 3-4 years. They serve as the primary way for the player to "lean" the country toward a specific identity.

### **Event 1: The Language of the Bazaar**

* **Description:** A heated dispute in the Samarkand bazaar between a Turkic-speaking merchant and a Persian-speaking clerk escalates into a city-wide riot. The local governor demands a decree on which language holds legal precedence in commercial contracts.  
* **Option A (Favor Turks):** \+10 Uzbek Path. Tajiks gain \+20% Radicalism. *Implication: Shifts the Urban Intelligentsia toward Turkic nationalism.*  
* **Option B (Favor Iranics):** \+10 Tajik Path. Uzbeks gain \+20% Radicalism. *Implication: Empowers the traditional Persian-speaking bureaucracy.*  
* **Option C (Mediate):** \+5 Middle Path. Both groups gain \+5% Loyalty. Costs 50 Bureaucracy for 3 years. *Implication: Demonstrates the state's role as a neutral arbiter, though at the cost of administrative efficiency.*

### **Event 2: The Jadid Reformers (Script & Education)**

* **Description:** The Jadid movement—modernizing intellectuals—proposes a standardized curriculum to increase literacy. The choice of script will define the nation's cultural orientation for a century.  
* **Option A (Turkic-First Curriculum):** Focuses on "Chagatai" Turkic.  
  * **Benefits:** \+10% Education Access, \+5% Tech Spread.  
  * **Identity:** Pushes heavily toward the Uzbek path.  
* **Option B (Persian Literary Standard):** Focuses on the classical Persian of the Timurid era.  
  * **Benefits:** \+15% Prestige, \+10% Intelligentsia Attraction.  
  * **Identity:** Pushes heavily toward the Tajik path.  
* **Option C (Bilingual Modernism):** Teaches both languages as equal partners in a modern state.  
  * **Benefits:** \+5% Education Access, \-10% State Turmoil.  
  * **Identity:** A mandatory building block for the "Middle Path."

### **Event 3: The Tax on the Sedentary**

* **Description:** Historically, "Sarts" were defined by being taxed differently than nomadic tribes. To modernize the treasury, your tax collectors need a rigid definition of who is "Sedentary" (taxable) and who is "Tribal" (exempt/military service).  
* **Option A (Tax by Language):** Assigns status based on Persian vs. Turkic usage. *Consequence: Forces bilingual families to pick a side to avoid double-taxation, fueling ethnic resentment.*  
* **Option B (Tax by Lifestyle):** Ignore ethnicity; tax everyone residing in urban centers. *Consequence: Strengthens the "Middle Path" but angers the Landowners IG, who see their traditional "tribal" exemptions threatened by urban expansion.*

### **Event 4: The Muhakamat al-Lughatayn (The Judgment of Two Languages)**

* **Description:** Inspired by Ali-Shir Nava'i's famous work, a grand debate is held at the palace. Is Turkic the language of the warrior and king, or is Persian the incomparable language of the soul and the divine?  
* **Option A (Turkic is the language of Power):** \+10% Armed Forces Approval, \+15 Uzbek Path.  
* **Option B (Persian is the language of the Soul):** \+10% Devout Approval, \+15 Tajik Path.

### **Event 5: The Call of Turan**

* **Description:** Agitators from the West bring news of Pan-Turkic movements. They claim that "Tajik" is merely a label for Turkic people who have forgotten their mother tongue.  
* **Option A (Embrace Pan-Turkism):** Massive Uzbek Loyalty, but Tajiks gain \+30% Radicalism.  
* **Option B (Denounce the Foreigners):** Boosts Tajik Loyalty and helps the Middle Path, but alienates the Intelligentsia and Armed Forces.

## **3\. The Three Resolution Paths**

When a path reaches 100 "Progress" or the timer expires with a clear leader, the following resolutions occur. These are not just flavor—they fundamentally change how the country plays.

| Path | Primary Cultures | Major Benefit | Penalty |
| :---- | :---- | :---- | :---- |
| **Uzbek Primacy** | Uzbek | **The Lion of Turan:** \+15% Offense/Defense. Grants claims on neighboring Turkic lands. | Tajik becomes "Unaccepted." Large-scale Tajiks migration out of the country; turmoil in cities. |
| **Tajik Primacy** | Tajik | **The Persian Renaissance:** \+20% Prestige, \+10% Innovation. Significant boost to diplomatic relations with Iran. | Uzbek becomes "Unaccepted." High risk of a Military Coup (as the army is often Uzbek-dominated). |
| **Middle Path** | Uzbek & Tajik | **Central Asian Harmony:** \-20% Radicalism from SOL decreases. \+10% Migration Attraction. | **Hardest to achieve.** Requires "Multiculturalism" or "Cultural Exclusion" laws and consistent mediation in events. |

## **4\. AI Prompting Instructions**

"Generate Victoria 3 script for:

1. **Journal Entry:** je\_uzbekistan\_identity with a 20-year countdown. Include a failure effect that triggers a start\_secession for the tajik culture if the timer hits zero.  
2. **On-Action:** Create an on\_three\_year\_pulse or similar to trigger the 5 events above.  
3. **Middle Path Check:** The final resolution event for the Middle Path must check has\_law \= law\_multiculturalism or has\_law \= law\_cultural\_exclusion. If the law is not met, the "Chaos" event should trigger even if the player attempted the Middle Path.  
4. **Culture Manipulation:** Use add\_primary\_culture and remove\_primary\_culture commands in the final resolution events."