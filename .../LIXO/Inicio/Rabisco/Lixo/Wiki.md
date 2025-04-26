# Home

Welcome to the **_Fantasy Map Generator_** wiki!
_The Wiki is incomplete, please [contact me](mailto:azgaar.fmg@yandex.com) if you can help with editing_

## Introduction

_**Fantasy Map Generator**_ (FMG) is a free tool that [procedurally generates](https://en.wikipedia.org/wiki/Procedural_generation) highly customizable fantasy maps. You can use auto-generated maps or create your own world from scratch.

The project is under active development. Join our [Discord server](https://discordapp.com/invite/X7E84HU) and [Reddit forum](https://www.reddit.com/r/FantasyMapGenerator) for the latest updates and to get help from the community.

[Q&A](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Q&A) |
[Quick Start Tutorial](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Quick-Start-Tutorial) |
[Current Generator version](https://azgaar.github.io/Fantasy-Map-Generator/) |
[Changelog](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Changelog) |
[Blog](https://azgaar.wordpress.com/) |
[Development board](https://trello.com/b/7x832DG4/fantasy-map-generator)

## How can I use the tool?

- _Exploration_ - click on the _New map!_ button to get a random map. Open the _Layers_ tab (press <kbd>Tab</kbd>) and select a desired layers preset. Zoom in and explore the generated world

- _Tuning_ - go to _Options_, change the default settings like map template and states number and generate a new map to better fit your needs

- _Customization_ - open the _Tools_ tab, select one of the available editors and change the map in any desired way

- _Controlled generation_ - open the _Tools_ tab, then click on _Heightmap_ and select an _Erase_ mode. Click on _Template editor_ and create your own heightmap template. Apply the template to see the result. Don't forget to share good templates with the community

- _Conversion_ - if you already have a map image and want to re-create it in a generator, open the _Image converter_ (the button next to _Template editor_), load the image and fine-tune the conversion into a heightmap. Then edit the map using the _Tools_ provided

- _Drawing_ - use the _Paint brushes_ to draw a map from scratch. It may takes a lot of time, so I would recommend you to follow the _Controlled generation_ approach to get a basic landmass and then use _Paint brushes_ for a fine-tuning

---

# Battle Simalator

**Battle Simulator**, added to the Generator in [version 1.4](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Changelog), allows to simulate battles between two or more regiments. It works based on units power parameter that can be set in the [Military units editor](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Military-Forces#military-units-editor).

To start a battle select a regiment, click on the _Attack foreign regiment_ button and then click on another regiment to attack it. You cannot attack regiments of the same state as the selected regiment or regiments without any forces. It does not matter where attacked regiment is located and whether it's reachable or not - attacker regiment will be moved straight to the selected one.

## Battle process

![](https://cdn.discordapp.com/attachments/587406457725779968/722571310290567209/Battle_Simulator.png)

Battle simulation is iterative. It means that it is processed step by step, with each step being controlled automatically, but there is also an ability of a manual change. To progress to the next iteration click on ▶️ button. To apply the current result and end the battle click on ✅, to abandon and cancel the results - click on ❎ button. To add a regiment click on 🙍‍♂️➕ button, select any regiment the list and click on a button to set the side. There is no restriction for state - you can add regiment of the same state both to attackers and defenders side. You can also define battle name using 🅰️ button.

Once battle is initiated, system automatically selects a _Battle type_. Battle type defines _battle phases_, which control the battle process. There are 6 battle types supported, each having its own specific and logic. Both battle type and specific phase can be changed manually at any time. For each iteration _strength_ of both sides are getting calculated. Strength depends on available units quantity and their power, modified by phase adjuster. See the next section for the details.

One more parameter that is auto-define on battle start is _morale_. Initial morale is based on difference in strength - weaker army gets low morale. _Supply line length_ also affects initial morale: the bigger distance to regiments base is, the bigger penalty to morale is applied. Please note that supply line length affects only initial morale, it has no effect for armies strength.

To add a random factor into battles, a 6-sided die is getting rolled. Click on a die to re-roll. Dice work as a multiplier for army strength, ⚅ (die 6) means full power, while ⚀ (die 1) - half-power.

For each iteration combined strength of both armies is considered to calculate _casualties_. Casualties are displayed as red line with negative numbers, _survivors_ -as green line with positive value. While there is a significant random factor affecting casualties, the generic pattern is defined by strength ratio and battle phase. It means that displayed _power_ actually shows which army excels at the current iteration and what would be ratio of casualties.

If one of the armies has no survivors left, you cannot proceed with iterations. Now you can either apply battle results or cancel them. Please note that you can apply results at any iteration, you don't need to wait while one army is completely destroyed. If results are applied, regiments' note (legend) is getting updated with a short info about the battle and also a battlefield _marker_ is getting added.

## Battle types

#### 🗡️ Field battle

Field battle is a standard type of combat. It starts with 🎯 _skirmish_ phase, where ranged and machinery units prevail. Skirmish usually lasts for a few iterations depending on how many ranged units are in both armies. Then ⚔️ _melee_ battle begins - melee and armored units excel at this phase. Once morale of one army drops, it is entering a 🏳️ _retreat_ phase, where strength of all units is drastically decreased. Another side immediately starts to 🐎 _pursue_ the enemy, getting a good advantage for mounted units.

|             | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| ----------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| 🎯 Skirmish | 0.2   | 2.4    | 0.1     | 3         | 1     | 0.2     | 1.8      | 1.8     |
| ⚔️ Melee    | 2     | 1.2    | 1.5     | 0.5       | 0.2   | 2       | 0.8      | 0.8     |
| 🏳️ Retreat  | 0.1   | 0.01   | 0.5     | 0.01      | 0.2   | 0.1     | 0.8      | 0.05    |
| 🐎 Pursue   | 1     | 1      | 4       | 0.05      | 1     | 1       | 1.5      | 0.6     |

#### 🌊 Naval battle

Naval battle type is auto-selected when both attacker and defender regiments are naval. It starts with 💣 _shelling_ phase, where only ranged units deal damage. In a few iterations ⚔️ _boarding_ can start. Melee units excel in this phase, while other types are not that effective. If boarding is not started and morale of one side is getting low, it is entering a 🏳️ _withdrawal_ phase. The enemy starts ⛵ _chase_, but contrary to field battle the damage at this phase is low.

|               | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| ------------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| 💣 Shelling   | 0     | 0.2    | 0       | 2         | 2     | 0       | 0.1      | 0.5     |
| ⚔️ Boarding   | 1     | 0.5    | 0.5     | 0         | 0.5   | 0.4     | 0        | 0.2     |
| 🏳️ Withdrawal | 0     | 0.15   | 0       | 1         | 1     | 0       | 0.15     | 0.5     |
| ⛵ Chase      | 0     | 0.02   | 0       | 0.5       | 0.1   | 0       | 0.1      | 0.3     |

#### 🏰 Siege

If attacked regiment is located in a walled town, the system automatically selects siege as a battle type. Siege is the most complex type with a number of optional phases and different variants for attackers and defenders.

For attackers siege always starts with a ⏳ _blockade_ phase. This is an inactive phase, where attackers prepare or hold a blockade. No damage dealt until attackers are ready to start a 💣 _bombardment_ or ⚔️ _storm_ the town. Machinery units excel at bombardment phase, while storming is risky and does not provide good results if attackers are not dominating in numbers.

Defenders can start a bombardment from the very first iteration. From time to time, or if they don't have machinery units, defender have to 🔒 _shelter_ in an inactive phase. Once in a few iterations defenders can send a 🚪 _sortie_ to attackers army. Sortie can be pretty successful, so attackers are not safe during the siege.

When attackers initiate storming, besieged army is switching to a 🛡️ _defense_ phase. As siege can be pretty long and defenders are usually ready for a storming and hence they get good modifiers during the defense. Storming is a short phase, so if it is not successful and defenders are not 🏳️ _surrender_, attackers have to get back to a bombardment or blockade phase. If defenders cannot combat anymore and capitulate, attackers start ☠️ _looting_ without significant resistance.

If siege is not successful, which is a pretty common case, attacker may decide to 🏳️ _retreat_. Defenders start 🐎 _pursue_ the enemy, getting a huge advantage for mounted units.

|                 | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| --------------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| ⏳ Blockade     | 0.25  | 0.25   | 0.2     | 0.5       | 0.2   | 0.1     | 0.25     | 0.25    |
| 🔒 Sheltering   | 0.3   | 0.5    | 0.2     | 0.5       | 0.2   | 0.1     | 0.25     | 0.25    |
| 🚪 Sortie       | 2     | 0.5    | 1.2     | 0.2       | 0.1   | 0.5     | 1        | 1       |
| 💣 Bombardment  | 0.2   | 0.5    | 0.2     | 3         | 1     | 0.5     | 1        | 1       |
| ⚔️ Storming     | 1     | 0.6    | 0.5     | 1         | 0.1   | 0.1     | 0.5      | 0.5     |
| 🛡️ Defense      | 2     | 3      | 1       | 1         | 0.1   | 1       | 0.5      | 1       |
| 🏳️ Surrendering | 0.1   | 0.1    | 0.05    | 0.01      | 0.01  | 0.02    | 0.1      | 0.3     |
| ☠️ Looting      | 1.6   | 1.6    | 0.5     | 0.2       | 0.02  | 0.2     | 0.1      | 0.3     |
| 🏳️ Retreat      | 0.1   | 0.01   | 0.5     | 0.01      | 0.2   | 0.1     | 0.8      | 0.05    |
| 🐎 Pursue       | 1     | 1      | 4       | 0.05      | 1     | 1       | 1.5      | 0.6     |

#### 🌳 Ambush

Ambush is getting auto-selected with a 20% chance if defenders are in forest or marshes biomes. It starts with a ⚡ _surprise attack_ of the defenders that causes attackers' 💫 _shock_. Defenders get a huge advantage with the surprise factor, but if attackers army is still stronger, the shock will end quickly. Once shock is over, sides enter a standard ⚔️ _melee_ phase, which usually ends with a 🏳️ _retreat_ of the side with dropped morale. Other side start a 🐎 _pursue_ where mounted units excel.

|             | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| ----------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| ⚡ Surprise | 2     | 2.4    | 1       | 1         | 1     | 1       | 0.8      | 1.2     |
| 💫 Shock    | 0.5   | 0.5    | 0.5     | 0.4       | 0.3   | 0.1     | 0.4      | 0.5     |
| ⚔️ Melee    | 2     | 1.2    | 1.5     | 0.5       | 0.2   | 2       | 0.8      | 0.8     |
| 🏳️ Retreat  | 0.1   | 0.01   | 0.5     | 0.01      | 0.2   | 0.1     | 0.8      | 0.05    |
| 🐎 Pursue   | 1     | 1      | 4       | 0.05      | 1     | 1       | 1.5      | 0.6     |

#### 🔱 Landing

Landing is happening when attackers regiment is naval and has land units, while defending regiment is a land one. The battle starts with a ⚓ _landing_ phase, which is quite risky for attackers. Defenders are either 💫 _shocked_ and get penalties, or ready for a 🛡️ _defense_ and can withstand or even force attackers to ⛵ _flee_. Shock / defense selection is pure random with 50% chance. If attackers are fleeing, defenders can 🐎 _pursue_ the enemy, but usually they need some time to prepare and hence are ⌛ _waiting_ at inactive phase.

After first few iterations sides are entering a standard ⚔️ _melee_ phase. If landing is successful, morale of defenders is dropping and they have to 🏳️ _retreat_. In this case attackers immediately start to 🐎 _pursue_ the enemy.

|            | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| ---------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| ⚓ Landing | 0.8   | 0.6    | 0.6     | 0.5       | 0.5   | 0.5     | 0.5      | 0.6     |
| 💫 Shock   | 0.5   | 0.5    | 0.5     | 0.4       | 0.3   | 0.1     | 0.4      | 0.5     |
| 🛡️ Defense | 2     | 3      | 1       | 1         | 0.1   | 1       | 0.5      | 1       |
| ⚔️ Melee   | 2     | 1.2    | 1.5     | 0.5       | 0.2   | 2       | 0.8      | 0.8     |
| 🏳️ Retreat | 0.1   | 0.01   | 0.5     | 0.01      | 0.2   | 0.1     | 0.8      | 0.05    |
| 🐎 Pursue  | 1     | 1      | 4       | 0.05      | 1     | 1       | 1.5      | 0.6     |
| ⛵ Flee    | 0.1   | 0.01   | 0.5     | 0.01      | 0.5   | 0.1     | 0.2      | 0.05    |
| ⌛ Waiting | 0.05  | 0.5    | 0.05    | 0.5       | 2     | 0.05    | 0.5      | 0.5     |

#### 💨 Air battle

Air battle type is auto-selected in very rare cases, when all units participating in battle have type _aviation_. As all units are supposed to have the same type, type-depending modifiers does not play significant role and can be ignored. Air battle starts with a 🎯 _maneuvering_ phase, where damage is not significant. In a few iterations an active 🐕 _dogfight_ begins. If morale of one side is getting low, it is entering a 🏳️ _retreat_ phase. The enemy starts a 🐎 _pursue_.

|                | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| -------------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| 🎯 Maneuvering | 0     | 0.1    | 0.1     | 0.2       | 0     | 0       | 1        | 0.2     |
| 🐕 Dogfight    | 0     | 0.1    | 0       | 0.1       | 0     | 0       | 2        | 0.1     |
| 🏳️ Retreat     | 0.1   | 0.01   | 0.5     | 0.01      | 0.2   | 0.1     | 0.8      | 0.05    |
| 🐎 Pursue      | 1     | 1      | 4       | 0.05      | 1     | 1       | 1.5      | 0.6     |

---

# Changelog

_The current stable version is available [here](https://azgaar.github.io/Fantasy-Map-Generator)._

Notable changes to the project should be documented in this file. The purpose of a changelog entry is to document the noteworthy difference and communicate them clearly to end users.

All save files (`.gz` or `.map`) created after `v0.61b` are compatible with the current version. Just load them and click OK to get auto-updated. Incompatible version 0.61b is available [here](https://azgaar.github.io/Fantasy-Map-Generator-old). To use an old version click on a link to download an archive. Unzip all file and [run the Tool locally](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Run-FMG-locally).

For older maps use the [old version](https://azgaar.github.io/Fantasy-Map-Generator-old).

Check out [Trello board](https://trello.com/b/7x832DG4/fantasy-map-generator) to see what is planned.

# Current version

Current version of the Fantasy Map Generator is the latest `master` branch. You can download it here: https://github.com/Azgaar/Fantasy-Map-Generator/archive/refs/heads/master.zip. Also see [the wiki](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Q&A#can-i-use-the-generator-offline).

Recent changes:

- Significantly reduce amount of ocean ice generated [1.108.3]
- Ability to set custom image as Marker or Regiment icon (started by _[Issac411](Issac411)_) [1.107.00]
- Submap and Transform tools rework [1.106.00]
- AI Assistant Bot [1.105.00]
- Layers rendering major refactoring [1.104.00]
- Cell info: Geozone by _[Avengium](https://github.com/Avengium)_ [1.103.00]
- Grid overlay: new types by _[Avengium](https://github.com/Avengium)_ [1.102.00]
- Style: ability to set letter spacing by _[oriolowsky](https://github.com/oriolowsky)_ [1.101.00]
- Zones refactoring and data collection [1.100.00]
- AI text generation for notes [1.99.06]
- `slider-input` web component [1.99.05]
- New style preset - Dark Seas by _[deuzeksmckinna](https://github.com/deuzeksmckinna)_ [1.99.03]

---

# Releases

**[1.99](https://github.com/Azgaar/Fantasy-Map-Generator/archive/refs/tags/v1.99.zip) - 2024-08-15**:

- New routes generation algorithm [1.99.00]
- Routes overview tool [1.99.00]
- Configurable longitude [1.98.01]
- Auto-apply changes checkbox for World Configurator [1.98.00]
- Renamed windrose element [1.98.00]
- Village map preview [1.97.00]
- Ocean heightmap rendering [1.96.00]
- Scale bar styling [1.96.00]
- Vignette layer [1.95.00]
- Fit loaded maps to the current canvas size [1.94.00]
- Ability to define custom heightmap color scheme [1.93.12]
- Random encounter markers - integration with [Deorum](https://deorum.vercel.app) [1.93.04]
- Auto-load of the last saved map is back to optional [1.93.00]
- Save files compression (file extension is changed to `.gz`) by _[yldrefruz](https://github.com/yldrefruz)_ [1.93.00]
  <br>-> Rolled back in [1.93.02] due to the time the compression takes on big maps and limited browser support
- State Labels placement 'raycasting' algorithm [1.92.00]
- Emblems data model change and positioning fixes [1.91.00]
- Texture format change from base64 to url [1.90.02]

**[1.9](https://github.com/Azgaar/Fantasy-Map-Generator/archive/refs/tags/v1.9.zip) - 2023-08-06**:

- North and South Poles temperature can be set independently by _[StempunkDev](https://github.com/StempunkDev)_
- More than 70 new heraldic charges by _[Blipz](https://github.com/Blipz)_
- Multi-color heraldic charges support by _[Blipz](https://github.com/Blipz)_
- New 3D scene options and improvements by _[yldrefruz](https://github.com/yldrefruz)_
- Type selection for marker placement by _[elad-haviv](https://github.com/elad-haviv)_
- Autosave [1.89.29]
- Google translation support
- Religions can be edited and redrawn like cultures
- Lock states, provinces, cultures, and religions from regeneration by _[Minivera](https://github.com/Minivera)_ [1.89.00]
- Heightmap brushes: linear edit option [1.88.00]
- Simplify style on 'optimizeSpeed' rendering [1.87.15]
- Levantine names base and culture changes [1.87.14]
- Data charts screen [1.87.00]
- Hierarchy tree UX improvement [1.86.06]
- Multi-parental cultures and religions hierarchy trees [1.86.00]
- Load from cloud: change format and order in dropdown [1.85.00]
- Adjective generation rules improvement [1.84.12]
- Heightmap selection screen [1.84.0]
- Dialogs optimization for mobile [1.84.0]
- New heightmap template - Fractious [1.83.0]
- Template Editor - mask and invert steps [1.83.0]
- Cultures editor - load script dynamically [1.82.2]
- States editor - load script dynamically [1.82.0]
- PWA Installation [1.81.1]
- 14 new fonts [1.81.0]
- Caching [1.81.0]

**[1.8](https://github.com/Azgaar/Fantasy-Map-Generator/archive/refs/tags/v1.8.zip) - 2022-04-26**:

- Submap tool by _Goteguru_ [1.732]
- Resample tool by _Goteguru_ [1.732]
- Pre-defined heightmaps [1.732]
- Advanced notes editor [1.731]
- Zones editor: filter by type by _Avengium_ and _Azgaar_ [1.73]
- Color picker: new hatches by _Avengium_ and _evolvedexperiment_ [1.73]
- New style preset: Atlas by _LordGnome_ [1.722]
- New style preset: Cyberpunk by _Terraria Eyeball with Salt in it_ [1.721]
- Disallow local usage without server [1.721]
- Burg temperature graph by _Kerravitarr_ [1.72]
- 4 new textures [1.71]
- Province capture logic rework [1.71]
- Button to release all provinces [1.71]
- Limit military units by biome, state, culture and religion [1.71]

**[1.7](https://github.com/Azgaar/Fantasy-Map-Generator/archive/refs/tags/v1.7.zip) - 2021-10-06**:

- New marker types
- New markers editor
- Markers overview screen
- Markers regeneration menu
- Burg editor update
- Theme color option
- Add font dialog
- Save to Dropbox via SDK

**[1.66](https://github.com/Azgaar/Fantasy-Map-Generator/archive/fe330ede3a27a2792766e7b53b0f5b72a8211b74.zip) - 2021-09-01**:

- Save and load .map files to Dropbox
- Ability to add control points on river edit
- New heightmap template: Taklamakan
- Option to not scale labels on zoom

**[1.65](https://github.com/Azgaar/Fantasy-Map-Generator/archive/ad1a8cf17edc89ffce45e7e8ff654520140edfb8.zip) - 2021-07-28**:

- Refactoring river rendering
- River data is now stored in object, not svg
- Keep river course on edit
- Ability to add river selecting its cells

**[1.64](https://github.com/Azgaar/Fantasy-Map-Generator/archive/6405b442a545fb47ca343772cf8bf1159781eb3d.zip) - 2021-07-10**:

- 3d labels in 3d view
- Refactoring states rendering
- State halo styling
- New styles: Light and Watercolor

**[1.63](https://github.com/Azgaar/Fantasy-Map-Generator/archive/05700cc338b43d575cdfa03dc368722918c7e89f.zip) - 2021-07-1**:

- Ability to style label shadow
- Save map as tiles

**[1.62](https://github.com/Azgaar/Fantasy-Map-Generator/archive/820cb22463b454882877f26a3b8a2a05eaac1aa5.zip) - 2021-06-09**:

- Rivers generation refactoring
- Lake handling for river generation change
- Creating lakes in deep depression areas
- Options to control lakes generation in Heightmap Edit

**[1.61](https://github.com/Azgaar/Fantasy-Map-Generator/archive/354830d4ec054122e262a13c0c8b5db59f0bffd7.zip) - 2021-03-06**:

- Ability to lock burgs from regeneration
- Polyline ruler
- Route ruler
- New ocean pattern: Kiwiroo

**[1.6](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.6.zip) - 2021-03-02**:

- River overview and River editor rework
- River generation code refactoring
- Rivers flux calculation
- Lake editor rework
- Lake type defined dynamically based on river system
- Lake flux, inlets and outlet tracked properly
- Lake evaporation calculated using simplified Penman formula: evaporation depends on surface, temperature and elevation
- Lake outlet rendered with starting width depending on flux
- Lake name generation

**[1.5](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.5.zip) - 2021-02-23**:

- State, province and burg Emblems generation
- Emblem editor integrated with Armoria
- Burg editor screen update
- Speak name functionality

**[1.4](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.4.zip) - 2020-06-21**:

- Battle simulation
- Ice layer and ice editor
- Route Elevation profile
- Image Converter enhancement
- Name generator improvement
- Fogging style change
- Improved integration with MFCG
- New lake style: Dry lake
- Bug fixes

**[1.3](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.3.zip) - 2020-04-01**:

- Military Forces generation
- Military Forces Overview
- Military Units Editor
- Regiments Editor
- Bug fixes

**[1.2](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.2.zip) - 2019-10-23**:

- 3d scene
- Globe view
- Save as JPEG
- Physical layer preset
- Diplomacy Editor enhancements
- Rivers Overview
- Bug fixes

**[1.1](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.1.zip) - 2019-09-28**:

- Color picker enhancement
- Religions Editor enhancement
- Map loading from URL
- Map quick save and quick load
- Data export in geojson format
- Error handling
- Religions tree
- Provinces, states and burgs graphs
- Culture presets
- Lake Editor
- Coastline Editor
- New Lake groups (types)

**[1.0](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v1.0.zip) - 2019-08-31**:

- Provinces and Provinces Editor
- Religions Layer and Religions Editor
- Full state names (state types)
- Multi-lined labels
- State relations (diplomacy)
- Custom layers (zones)
- Places of interest (auto-added markers)
- New color picker and hatching fill
- Legend boxes
- World Configurator presets
- Improved state labels placement
- Relief icons sets
- Fogging
- Custom layer presets
- Custom biomes
- State, province and burg COAs

**[0.9b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.9b.zip) - 2019-06-09**:

- Relief icons by Arzak Rubin
- Ability to re-generate Burgs
- Ability to re-generate States

**[0.8b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.8b.zip) - 2019-04-21**:

- 0.7a changes moved to production
- Bug fixes
- UI improvement

**[0.7a](https://github.com/Azgaar/Fantasy-Map-Generator-alpha/archive/v0.7-alpha.zip) - 2019-04-14**:

- World size and climate configuration
- Biomes layer and biomes editor
- Temperature and precipitation layers
- Reworked population model (now depends on biome)
- New states and cultures spread algorithm
- 15 new cultures
- Texture layer and ability to link a custom texture
- Seed history (to open previously generated maps)
- Optimization (faster generation and smoother edit experience)
- UI and usability changes
- Reworked hotkeys

**[0.61b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/707913f630a79a37e6f2a0c86cf311e929055bdb.zip) - 2018-11-18**:

- New Relief Icons editor
- Updated Relief Icons
- UI controls for Relief icons density and size
- 2 new cultures
- Remove selected element on "Delete" key press
- Changed cultures editor behavior
- Bug fixes

**[0.60b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.60-beta.zip) - 2018-09-23**:

- Map Markers
- Legend Editor (text notes)
- Curved Labels
- User-friendly overlay size display (e.g. "50 mi", not just "10")
- Bug fixes

**[0.59b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.59-beta.zip) - 2018-08-28**:

- Random seed support
- New heightmap templates
- Integration with Medieval Fantasy City Generator
- Relocate Burg option
- Dialogs style changes, optional transparency
- Ability to toggle Label groups separately
- Bug fixes

**[0.58b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.58-beta.zip) - 2018-07-28**:

- Cultures editor
- Namesbase editor
- Reworked lakes
- Options preservation
- New filters
- Non-island maps (wip)
- Bug fixes

**[0.57b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.57-beta.zip) - 2018-06-17**:

- Unified burg editor
- Relief icons editor
- New Options
- Bug fixes

**[0.56b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.56-beta.zip) - 2018-05-06**:

- Performance improvement
- Route editor
- Countries and River editors fixes
- Map saving and loading fixes
- New loading screen

**[0.55b](https://github.com/Azgaar/Fantasy-Map-Generator/archive/v0.55-beta.zip) - 2018-03-30**:

- Demo version is moved to azgaar.github.io
- Default map resolution is set to full-screen
- 'About' tab in Options with project description and links
- 'Share' buttons in 'About' tab
- 'The version is obsolete' message to display on gist version

**[0.54b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/70ab7efaa966b91670d218cc3659c865ff900bbc) - 2018-03-30**:

- Countries editor
- Burgs editor
- Scale editor
- Scale bar (depends on set scales, auto-rounded and auto-sized)
- Overlays: hex grid, square grid, wind rose
- Change population calculation logic
- Remember editors position
- River rendering method is simplified, now it works much faster

**[0.53b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/f217163cee6b662eddc20b7bab75296587008723) - 2018-03-07**:

- 'Graph size' option is usable now
- 'Add River' button (click to auto-add a new River)
- 'Paint Brushes' window is re-worked, Undo-Redo buttons added
- 'Perspective view' for Heightmap (looks weak, should I try real 3D?)
- Heightmap Templates improvement
- River Editor improvement
- 'Print Map' button
- Active zooming (resclale labels on zooming)
- Options randomization
- New modal windows
- Web-safe fonts added to default fonts list
- Save/load buttons are moved to option footer

**[0.52b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/187961af7c4373567cbb726436281a2bf12393dd) - 2018-02-26**:

- Image to Heightmap Converter
- Roll back to Heightmap option
- Rescale Dialog for Heightmap customization (draft)
- Edges connection bug fix (can still rarely occur)
- Version control for updates and save/load function
- Save/load function support for Firefox (direct Chrome dependency is removed)
- Downloaded .png resolution is increased from 960x540 to 1920x1080

**[0.51b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/77a6076e5a7c6788bc66b5b8f8d0e664a888b782) - 2018-02-10** ([offline version](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Working-offline) is [here](https://yadi.sk/d/utFHCBE33SJCzF)) - Style Editor update:

- Stroke-width, stroke-dasharray, stroke-linecap for routes/borders
- Size for burgs/labels
- Color for cultures
- Color scheme for heightmap
- SVG filters

**[0.50b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/9ceb5b2d3adc820b02831bdb3e35b3231650e0aa) - 2018-02-06**:

- Heightmap templates
- Template Editor
- New CSS filters
- UI improvements
- Polygonal rivers
- River Editor

**[0.49b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/be7a5f7647421256b302444b264ca618cc4c8b00) - 2018-01-10**:

- Ocean layers selection

**[0.43b](http://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/be7a5f7647421256b302444b264ca618cc4c8b00) - 2018-01-03**:

- Save map
- Load map
- Graph size option

**[0.1b](https://bl.ocks.org/Azgaar/b845ce22ea68090d43a4ecfb914f51bd/581c7172035ed41f0bcf2403943e6a1852683c6d) - 2017-03-20**:

- First published version

---

# Culture sets

## Culture Sets

A culture set is a collection of cultures to be displayed in the same map.

- All-world: A group of 32 cultures from around the globe.
- European: A list of 15 cultures from Europe.
- Oriental: A list of 13 cultures from Asia.
- English: A group of 10 cultures all with English culture.
- Antique: A list of 15 cultures from ancient europe.
- High fantasy: A list of 13 fictional and 4 human cultures for fantasy settings.
- Dark fantasy: A list of 6 english, 5 european, 14 ethnic, and 9 fictional cultures for human ruled fantasies.

### All-world

- Shwazen: It represents proto-Germanic and Germanic peoples of different times and places. Namesbase: German. [More info about Germanic culture](https://en.wikipedia.org/wiki/Germanic_culture).
- Angshire: It encompasses cultures of England. Namesbase: English. [More info about Angshire culture](https://en.wikipedia.org/wiki/Culture_of_England).
- Luari: Represents a culture of France. Namesbase: French. [More info about French culture](https://en.wikipedia.org/wiki/Culture_of_France).
- Tallian: A culture in Italy after the fall of the roman empire. It can be imagined as venetian or any other italian culture. Namesbase: Italian. [More info about Italian culture](https://en.wikipedia.org/wiki/Culture_of_Italy).
- Astellian: A culture in Spain after the fall of the roman empire. Can be [House of Trastámara](https://en.wikipedia.org/wiki/House_of_Trastámara), maybe [Habsburg Spain](https://en.wikipedia.org/wiki/Habsburg_Spain) or any other time period of Spain. Namesbase: Spanish. [More info about Spanish culture](https://en.wikipedia.org/wiki/Culture_of_Spain).
- Slovan: A culture that can be any slavic culture. Maybe the [Kievan Rus'](https://en.wikipedia.org/wiki/Kievan_Rus%27), maybe some proto-slavic tribe, maybe the west, east or south slavs in a different time. Namesbase: Ruthenian. [More info about Slavic culture](https://en.wikipedia.org/wiki/List_of_Slavic_cultures).
- Norse: A culture that represents Iceland, Norway, Sweden, and Viking culture. Namesbase: Nordic. [More info about Vikings](https://en.wikipedia.org/wiki/Vikings).
- Elladan: A culture that can represent ancient or modern Greece. Namesbase: Greek. [More info about Culture of Greece](https://en.wikipedia.org/wiki/Culture_of_Greece).
- Romian: Ancient Roman culture. Maybe the monarchy, republic, imperium or any other period. Namesbase: Roman. [More info about Ancient Rome](https://en.wikipedia.org/wiki/Ancient_Rome).
- Soumi: Represents culture of Finland. Namesbase: Finnic. [More info about culture of Finland](https://en.wikipedia.org/wiki/Culture_of_Finland).
- Koryo: Korean ancient culture. Can represent [Gojoseon](https://en.wikipedia.org/wiki/Gojoseon), [Goguryeo](https://en.wikipedia.org/wiki/Goguryeo), [Joseon](https://en.wikipedia.org/wiki/Joseon), [Silla](https://en.wikipedia.org/wiki/Silla) or any other period. Namesbase: Korean. [More info about Korean Culture](https://en.wikipedia.org/wiki/Culture_of_Korea).
- Hantzu: Chinese ancient culture. Inspired in: [Han chinese](https://en.wikipedia.org/wiki/Han_Chinese) but can be any other. Namesbase: Chinese. [More info about East Asian cultural sphere](https://en.wikipedia.org/wiki/East_Asian_cultural_sphere).
- Yamoto: Inspired by [Yamato people](https://en.wikipedia.org/wiki/Yamato_people) but can be any other [people from japan](https://en.wikipedia.org/wiki/Ethnic_groups_of_Japan). Namesbase: Japanese. [More info about Japanese culture](https://en.wikipedia.org/wiki/Culture_of_Japan).
- Portuzian: Inspired in [Portuguese people](https://en.wikipedia.org/wiki/Portuguese_people). Namesbase: Portuguese. [More info about Portuguese culture](https://en.wikipedia.org/wiki/Portuguese_people).
- Nawatli: Inspired by [Nahua ethnicity](https://en.wikipedia.org/wiki/Nahuas) like [Mexicas](https://en.wikipedia.org/wiki/Mexica), [Aztecs](https://en.wikipedia.org/wiki/Aztecs) and [Toltecs](https://en.wikipedia.org/wiki/Toltec). Namesbase: Nahuatl. [More info about Nahuas](https://en.wikipedia.org/wiki/Nahuas).
- Vengrian: Inspired by the [Hungarians](https://en.wikipedia.org/wiki/Hungarians). Namesbase: Hungarian. [More info about culture of Hungary](https://en.wikipedia.org/wiki/Culture_of_Hungary).
- Turchian: Inspired in the several [turkic ethnicities](https://en.wikipedia.org/wiki/Turkic_peoples). Namesbase: Turkish. [More info on Turkic dynasties and countries](https://en.wikipedia.org/wiki/List_of_Turkic_dynasties_and_countries).
- Berberan: inspired in [Berber people](https://en.wikipedia.org/wiki/Berbers). Namesbase: Berber. [More info about Berber culture](https://en.wikipedia.org/wiki/Berbers#Culture).
- Eurabic: It represents the [Arab states](https://en.wikipedia.org/wiki/Arab_world) and different arabic cultures. Namesbase: Arabic. [More info about Arab culture](https://en.wikipedia.org/wiki/Arab_culture).
- Inuk: It represents [Inuit](https://en.wikipedia.org/wiki/Inuit) and related ethnic groups. Namesbase: Inuit. [More info about Inuit culture](https://en.wikipedia.org/wiki/Inuit_culture).
- Euskati: Inspired in [Basque people](https://en.wikipedia.org/wiki/Basques) and related ethnic groups. Namesbase: Basque. [More info about Basque culture](https://en.wikipedia.org/wiki/Basques#Culture).
- Yoruba: Inspired in [Yoruba people](https://en.wikipedia.org/wiki/Yoruba_people) Namesbase: Nigerian. [More info about Yoruba culture](https://en.wikipedia.org/wiki/Yoruba_culture).
- Keltan: Represents the collection of several [Celtic people](https://en.wikipedia.org/wiki/Celts) Namesbase: Celtic. [More info about Celtic culture](https://en.wikipedia.org/wiki/Celtic_culture).
- Efratic: Represents the collection of several cultures that appeared millenia ago in [Mesopotamia] (https://en.wikipedia.org/wiki/Mesopotamia). Can be [Sumerian](https://en.wikipedia.org/wiki/Sumer), [Akkadian](https://en.wikipedia.org/wiki/Akkadian_Empire) or any other related culture. Namesbase: Mesopotamian. [More info about Mesopotamian history](https://en.wikipedia.org/wiki/History_of_Mesopotamia).
- Tehrani: Inspired by [Iranian peoples](https://en.wikipedia.org/wiki/Iranian_peoples) like proto-Iranians, the [Persians](https://en.wikipedia.org/wiki/Persians), the [Scythians](https://en.wikipedia.org/wiki/Scythians) and other groups in that region. Namesbase: Iranian. [More info about Iranian culture](https://en.wikipedia.org/wiki/Culture_of_Iran).
- Maui: This represents the [Polynesians](https://en.wikipedia.org/wiki/Polynesians) such as Samoans, Tongans, Hawaiians and other related groups. Namesbase: Hawaiian. [More info about Polynesian culture](https://en.wikipedia.org/wiki/Polynesian_culture).
- Carnatic: [Dravidian peoples](https://en.wikipedia.org/wiki/Dravidian_peoples) of the South of India. Kannada is a language spoken in Karnataka, India. Namesbase: Karnataka. [More info about South Indian culture](https://en.wikipedia.org/wiki/South_Indian_culture).
- Inqan: Inspired by the [Inca empire](https://en.wikipedia.org/wiki/Inca_Empire) and related groups of [Quechua people](https://en.wikipedia.org/wiki/Quechua_people). Namesbase: Quechua. [More info about Inca society](https://en.wikipedia.org/wiki/Inca_society).
- Kiswaili: [Swahili people's](https://en.wikipedia.org/wiki/Swahili_people) name for themselves is Waungwana, which means "the civilized ones." Namesbase: Swahili. [More info about Swahili culture](https://en.wikipedia.org/wiki/Swahili_culture).
- Vietic: Represents a [group of ethnicities](https://en.wikipedia.org/wiki/Vietic_peoples) from Southeast Asia. Namesbase: Vietnamese. [More info about Vietnamese culture](https://en.wikipedia.org/wiki/Culture_of_Vietnam).
- Guantzu: Inspired by [Cantonese people](https://en.wikipedia.org/wiki/Cantonese_people). Useful as an additional culture in China. Namesbase: Cantonese. [More info about Cantonese culture](https://en.wikipedia.org/wiki/Lingnan_culture).
- Ulus: Inspired by the [Mongolic peoples](https://en.wikipedia.org/wiki/Mongolic_peoples) like the [Mongols](https://en.wikipedia.org/wiki/Mongols). Ulus translates to "state" or "nation" in a political way. Namesbase: Mongolian. [More info about Mongolian culture](https://en.wikipedia.org/wiki/Culture_of_Mongolia).

### European

- Shwazen
- Angshire
- Luari
- Tallian
- Astellian
- Slovan
- Norse
- Elladan
- Romian
- Soumi
- Portuzian
- Vengrian
- Turchian
- Euskati
- Keltan

### Oriental

- Koryo
- Hantzu
- Yamoto
- Turchian
- Berberan
- Eurabic
- Efratic
- Tehrani
- Maui
- Carnatic
- Vietic
- Guantzu
- Ulus

### English

This contains a pool of ten cultures all of english culture. Useful for making maps with the same culture.

### Antique

- Roman
- Roman
- Roman
- Roman
- Hellenic: Namesbase: Greek.
- Hellenic Namesbase: Greek.
- Macedonian Namesbase: Greek.
- Celtic
- Germanic
- Persian: Namesbase: Persian.
- Scythian Namesbase: Persian.
- Cantabrian: Namesbase: Basque.
- Estian: Namesbase: Finnic.
- Carthaginian: Namesbase: Berber.
- Mesopotamian

### High fantasy

- // fantasy races
- Quenian (Elfish): Elves of fantasy.
- Eldar (Elfish): Another elven culture.
- Trow (Dark Elfish): Dark elfs of the underdark.
- Lothian (Dark Elfish): Another dark elf culture.
- Dunirr (Dwarven): Dwarves of the hills.
- Khazadur (Dwarven): Dwarves of the deep.
- Kobold (Goblin): Goblins or [Kobaloi](https://en.wikipedia.org/wiki/Kobalos) of old. This includes spirits such as the Northern English boggart, Scottish bogle, French goblin, Medieval gobelinus, German kobold, and English Puck.
- Uruk (Orkish): Orcish culture.
- Ugluk (Orkish): Another orcish culture.
- Yotunn (Giants): Live in places with -10C annual temperature.
- Rake (Drakonic): Draconic creatures.
- Arago (Arachnid): Big spiders.
- Aj'Snaga (Serpents): Loves rainforest and tropical seasonal forests.
- // fantasy human
- Anor (Human): Generic human culture.
- Dail (Human): Another generic human culture.
- Rohand (Human): Turkish human culture.
- Dulandir (Human): Mongolian human culture.

### Dark fantasy

- // common real-world English
- Angshire
- Enlandic
- Westen
- Nortumbic
- Mercian
- Kentian
- // rare real-world western
- Norse
- Schwarzen: German.
- Luarian: French.
- Hetallian: Italian.
- Astellian: Spanish.
- // rare real-world exotic
- Kiswaili
- Yoruba
- Koryo
- Hantzu
- Yamoto
- Guantzu
- Ulus
- Turan: Turkish.
- Berberan
- Eurabic
- Slovan
- Keltan
- Elladan
- Romian
- // fantasy races
- Eldar
- Trow
- Durinn
- Kobblin
- Uruk
- Yotunn
- Drake
- Rakhnid
- Aj'Snaga

### Random

This culture set picks a random list of cultures from the total.

---

# Culture types

Each culture gets a type assigned on generation. The type define how culture will grow and what territories it will get, but also serves world-building needs and affects multiple systems.

## Culture generation

When a culture is generated, its type is determined by the geographical features it spawned in. A culture is guaranteed to be assigned the first culture type that meets the criteria in the listed order, except the Naval culture type which has a random probability check in addition to its normal requirement.

### Nomadic

Nomadic cultures are generated in deserts or grassland with height less than 70 points.

### Highland

Highland cultures are generated in cells where height is over 50 points.

### Lake

Lake cultures are generated in cells around lakes that are over 5 cells in size.

### Naval

Naval cultures have a chance of being generated in coastal cells. The chance is slightly higher if the cell is next to an ocean (as opposed to a lake), and significantly higher if the cell is an island.

### River

River cultures are generated in cells with a river of over 100 flux points.

### Hunting

Hunting cultures are generated in cells that are more than two cells away from a coast and the biome is either Savannah, Rain forest, Taiga, Tundra, or Wetland.

### Generic

A culture is set to be generic when its spawn location is ineligible for any other culture types, or in the case of the Naval culture type, failed a random probability check.

## Culture spread

Culture spread is a competitive system, where each culture tries to get more cells considering the cost of it. Culture type is crucial in defining cell "cost" and the way culture will spread.

### Culture expansionism

Every culture has a randomly generated expansionism value. The random value is then multiplied according to the culture type:

- Generic: 1
- Lake: 0.8
- Naval: 1.5
- River: 0.9
- Nomadic: 1.5
- Hunting: 0.7
- Highland: 1.2

The more expansionism values is, the less cell cost is for the culture. So Nomadic cultures generally tends to occupy vast territories, while Hunting and Lake cultures are usually pretty limited in area.

### Biome cost

The base biome costs are as follows:

- Hot desert: 200
- Cold desert: 150
- Savanna: 60
- Grassland: 50
- Tropical seasonal forest: 70
- Temperate deciduous forest: 70
- Tropical rainforest: 80
- Temperate rainforest: 90
- Taiga: 200
- Tundra: 1000
- Glacier: 5000
- Wetland: 150

The cost is reduced to 10 if it's the culture's native biome type. The cost is multiplied by 5 for Hunting cultures in all non-native biome types. The cost is multiplied by 10 for Nomadic cultures in the five forest biome types. Otherwise, the cost is multiplied by 2. Additionally, if the cell has a different biome type than the one the culture is spreading from, 20 is added to the final cost.

### Water crossing cost

Lake cultures crossing a lake has a flat cost of 10. Naval cultures crossing water costs 2 times the size of the cell in pixels. Nomadic cultures crossing water costs 50 times the size of the cell in pixels. All other cultures crossing water (and Lake cultures crossing a sea) costs 6 times the size of the cell in pixels.

### River crossing cost

For non-River cultures, river cells costs 20-100 more to cross, depending on the flux of the river. For River cultures, river cells don't cost extra, but non-river cells cost 100 more.

### Distance to coast cost

Coastal cells cost 60 more for Nomadic cultures, and 20 for other non-Naval non-River cultures. Cells next to coastal cells costs 30 more for Naval and Nomadic cultures. Cells further inland costs 100 more for Naval and River cultures. The costs are added together then divided by expansionism of the culture. When the total cost exceeds the budget, expansion stops.

---

# Data model

**FMG data model** is poorly defined, inconsistent and not documented in the codebase. This page is the first and the only attempt to document it. Once everything is documented, it can be used for building a new consistent model. Please note the current document reflect the object model **as is**, so with all its quirks. The model we want to get is [covered separately](<https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Data-model-(in-progress)>).

FMG exposes all its data into the global namespace. The global namespace is getting polluted and it can cause conflicts with 3rd party extensions. Meanwhile it simplifies debugging and allows users to run custom JS code in dev console to alter the tool behavior.

# Basic objects

FMG has two meta-objects storing most of the map data:

- `grid` contains map data before _repacking_
- `pack` contains map data after _repacking_

Repacking is a process of amending an initial [voronoi diagram](https://en.wikipedia.org/wiki/Voronoi_diagram), that is based on a jittered square grid of points, into a voronoi diagram optimized for the current landmass (see [my old blog post](https://azgaar.wordpress.com/2017/10/05/templates) for the details). So the `pack` object is used for most of the data, but data optimized for square grid is available only via the `grid` object.

## Voronoi data

Both `grid` and `pack` objects include data representing voronoi diagrams and their inner connections. Both initial and repacked voronoi can be build from the initial set of points, so this data is stored in memory only. It does not included into the .map file and getting calculated on map load.

### Grid object

- `grid.cellsDesired`: `number` - initial count of cells/points requested for map creation. Used to define `spacing` and place points on a jittered square grid, hence the object name. Actual number of cells is defined by the number points able to place on a square grid. Default `cellsDesired` is 10 000, maximum - 100 000, minimal - 1 000
- `grid.spacing`: `number` - spacing between points before jittering
- `grid.cellsY`: `number` - number of cells in column
- `grid.cellsX`: `number` - number of cells in row
- `grid.points`: `number[][]` - coordinates `[x, y]` based on jittered square grid. Numbers rounded to 2 decimals
- `grid.boundary`: `number[][]` - off-canvas points coordinates used to cut the diagram approximately by canvas edges. Integers
- `grid.cells`: `{}` - cells data object, including voronoi data:
- - `grid.cells.i`: `number[]` - cell indexes `Uint16Array` or `Uint32Array` (depending on cells number)
- - `grid.cells.c`: `number[][]` - indexes of cells adjacent to each cell (neighboring cells)
- - `grid.cells.v`: `number[][]` - indexes of vertices of each cell
- - `grid.cells.b`: `number[]` - indicates if cell borders map edge, 1 if `true`, 0 if `false`. Integers, not Boolean

- `grid.vertices`: `{}` - vertices data object, contains only voronoi data:
- - `grid.vertices.p`: `number[][]` - vertices coordinates `[x, y]`, integers
- - `grid.vertices.c`: `number[][]` - indexes of cells adjacent to each vertex, each vertex has 3 adjacent cells
- - `grid.vertices.v`: `number[][]` - indexes of vertices adjacent to each vertex. Most vertices have 3 neighboring vertices, bordering vertices has only 2, while the third is still added to the data as `-1`

### Pack object

- `pack.cells`: `{}` - cells data object, including voronoi data:
- - `pack.cells.i`: `number[]` - cell indexes `Uint16Array` or `Uint32Array` (depending on cells number)
- - `pack.cells.p`: `number[][]` - cells coordinates `[x, y]` after repacking. Numbers rounded to 2 decimals
- - `pack.cells.c`: `number[][]` - indexes of cells adjacent to each cell (neighboring cells)
- - `pack.cells.v`: `number[][]` - indexes of vertices of each cell
- - `pack.cells.b`: `number[]` - indicator whether the cell borders the map edge, 1 if `true`, 0 if `false`. Integers, not Boolean
- - `pack.cells.g`: `number[]` - indexes of a source cell in `grid`. `Uint16Array` or `Uint32Array`. The only way to find correct `grid` cell parent for `pack` cells

- `pack.vertices`: `{}` - vertices data object, contains only voronoi data:
- - `pack.vertices.p`: `number[][]` - vertices coordinates `[x, y]`, integers
- - `pack.vertices.c`: `number[][]` - indexes of cells adjacent to each vertex, each vertex has 3 adjacent cells
- - `pack.vertices.v`: `number[][]` - indexes of vertices adjacent to each vertex. Most vertices have 3 neighboring vertices, bordering vertices has only 2, while the third is still added to the data as `-1`

## Features data

Features represent separate locked areas like islands, lakes and oceans.

### Grid object

- `grid.features`: `object[]` - array containing objects for all enclosed entities of original graph: islands, lakes and oceans. Feature object structure:
- - `i`: `number` - feature id starting from `1`
- - `land`: `boolean` - `true` if feature is land (height >= `20`)
- - `border`: `boolean` - `true` if feature touches map border (used to separate lakes from oceans)
- - `type`: `string` - feature type, can be `ocean`, `island` or `lake

### Pack object

- `pack.features`: `object[]` - array containing objects for all enclosed entities of repacked graph: islands, lakes and oceans. Note: element 0 has no data. Stored in .map file. Feature object structure:
- - `i`: `number` - feature id starting from `1`
- - `land`: `boolean` - `true` if feature is land (height >= `20`)
- - `border`: `boolean` - `true` if feature touches map border (used to separate lakes from oceans)
- - `type`: `string` - feature type, can be `ocean`, `island` or `lake`
- - `group`: `string`: feature subtype, depends on type. Subtype for ocean is `ocean`; for land it is `continent`, `island`, `isle` or `lake_island`; for lake it is `freshwater`, `salt`, `dry`, `sinkhole` or `lava`
- - `cells`: `number` - number of cells in feature
- - `firstCell`: `number` - index of the first (top left) cell in feature
- - `vertices`: `number[]` - indexes of vertices around the feature (perimetric vertices)
    \*\* `name`: `string` - name, available for `lake` type only

## Specific cells data

World data is mainly stored in typed arrays within `cells` object in both `grid` and `pack`.

### Grid object

- `grid.cells.h`: `number[]` - cells elevation in `[0, 100]` range, where `20` is the minimal land elevation. `Uint8Array`
- `grid.cells.f`: `number[]` - indexes of feature. `Uint16Array` or `Uint32Array` (depending on cells number)
- `grid.cells.t`: `number[]` - [distance field](https://prideout.net/blog/distance_fields/) from water level. `1, 2, ...` - land cells, `-1, -2, ...` - water cells, `0` - unmarked cell. `Uint8Array`
- `grid.cells.temp`: `number[]` - cells temperature in Celsius. `Uint8Array`
- `grid.cells.prec`: `number[]` - cells precipitation in unspecified scale. `Uint8Array`

### Pack object

- `pack.cells.h`: `number[]` - cells elevation in `[0, 100]` range, where `20` is the minimal land elevation. `Uint8Array`
- `pack.cells.f`: `number[]` - indexes of feature. `Uint16Array` or `Uint32Array` (depending on cells number)
- `pack.cells.t`: `number[]` - distance field. `1, 2, ...` - land cells, `-1, -2, ...` - water cells, `0` - unmarked cell. `Uint8Array`
- `pack.cells.s`: `number[]` - cells score. Scoring is used to define best cells to place a burg. `Uint16Array`
- `pack.cells.biome`: `number[]` - cells biome index. `Uint8Array`
- `pack.cells.burg`: `number[]` - cells burg index. `Uint16Array`
- `pack.cells.culture`: `number[]` - cells culture index. `Uint16Array`
- `pack.cells.state`: `number[]` - cells state index. `Uint16Array`
- `pack.cells.province`: `number[]` - cells province index. `Uint16Array`
- `pack.cells.religion`: `number[]` - cells religion index. `Uint16Array`
- `pack.cells.area`: `number[]` - cells area in pixels. `Uint16Array`
- `pack.cells.pop`: `number[]` - cells population in population points (1 point = 1000 people by default). `Float32Array`, not rounded to not lose population of high population rate
- `pack.cells.r`: `number[]` - cells river index. `Uint16Array`
- `pack.cells.fl`: `number[]` - cells flux amount. Defines how much water flow through the cell. Use to get rivers data and score cells. `Uint16Array`
- `pack.cells.conf`: `number[]` - cells flux amount in confluences. Confluences are cells where rivers meet each other. `Uint16Array`
- `pack.cells.harbor`: `number[]` - cells harbor score. Shows how many water cells are adjacent to the cell. Used for scoring. `Uint8Array`
- `pack.cells.haven`: `number[]` - cells haven cells index. Each coastal cell has haven cells defined for correct routes building. `Uint16Array` or `Uint32Array` (depending on cells number)
- `pack.cells.routes`: `object` - cells connections via routes. E.g. `pack.cells.routes[8] = {9: 306, 10: 306}` shows that cell `8` has two route connections - with cell `9` via route `306` and with cell `10` by route `306`
- `pack.cells.q`: `object` - quadtree used for fast closest cell detection

# Secondary data

Secondary data available as a part of the `pack` object.

## Cultures

Cultures (races, language zones) data is stored as an array of objects with strict element order. Element 0 is reserved by the _wildlands_ culture. If culture is removed, the element is not getting removed, but instead a `removed` attribute is added. Object structure:

- `i`: `number` - culture id, always equal to the array index
- `base`: `number` - _nameBase_ id, name base is used for names generation
- `name`: `string` - culture name
- `origins`: `number[]` - ids of origin cultures. Used to render cultures tree to show cultures evolution. The first array member is main link, other - supporting out-of-tree links
- `shield`: `string` - shield type. Used for emblems rendering
- `center`: `number` - cell id of culture center (initial cell)
- `code`: `string` - culture name abbreviation. Used to render cultures tree
- `color`: `string` - culture color in hex (e.g. `#45ff12`) or link to hatching pattern (e.g. `url(#hatch7)`)
- `expansionism`: `number` - culture growth multiplier. Used mainly during cultures generation to spread cultures not uniformly
- `type`: `string` - culture type, see [culture types](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Culture-types)
- `area`: `number` - culture area in pixels
- `cells`: `number` - number of cells assigned to culture
- `rural`: `number` - rural (non-burg) population of cells assigned to culture. In population points
- `urban`: `number` - urban (burg) population of cells assigned to culture. In population points
- `lock`: `boolean` - `true` if culture is locked (not affected by regeneration)
- `removed`: `boolean` - `true` if culture is removed

## Burgs

Burgs (settlements) data is stored as an array of objects with strict element order. Element 0 is an empty object. If burg is removed, the element is not getting removed, but instead a `removed` attribute is added. Object structure:

- `i`: `number` - burg id, always equal to the array index
- `name`: `string` - burg name
- `cell`: `number` - burg cell id. One cell can have only one burg
- `x`: `number` - x axis coordinate, rounded to two decimals
- `y`: `number` - y axis coordinate, rounded to two decimals
- `culture`: `number` - burg culture id
- `state`: `number` - burg state id
- `feature`: `number` - burg feature id (id of a landmass)
- `population`: `number` - burg population in population points
- `type`: `string` - burg type, see [culture types](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Culture_types)
- `coa`: `object` - emblem object, data model is the same as in [Armoria](https://github.com/Azgaar/Armoria) and covered in [API documentation](https://github.com/Azgaar/armoria-api#readme). The only additional fields are optional `size`: `number`, `x`: `number` and `y`: `number` that controls the emblem position on the map (if it's not default). If emblem is loaded by user, then the value is `{ custom: true }` and cannot be displayed in Armoria
- `MFCG`: `number` - burg seed in [Medieval Fantasy City Generator](https://watabou.github.io/city-generator) (MFCG). If not provided, seed is combined from map seed and burg id
- `link`: `string` - custom link to burg in MFCG. `MFCG` seed is not used if link is provided
- `capital`: `number` - `1` if burg is a capital, `0` if not (each state has only 1 capital)
- `port`: `number` - if burg is not a port, then `0`, otherwise feature id of the water body the burg stands on
- `citadel`: `number` - `1` if burg has a castle, `0` if not. Used for MFCG
- `plaza`: `number` - `1` if burg has a marketplace, `0` if not. Used for MFCG
- `shanty`: `number` - `1` if burg has a shanty town, `0` if not. Used for MFCG
- `temple`: `number` - `1` if burg has a temple, `0` if not. Used for MFCG
- `walls`: `number` - `1` if burg has walls, `0` if not. Used for MFCG
- `lock`: `boolean` - `true` if burg is locked (not affected by regeneration)
- `removed`: `boolean` - `true` if burg is removed

## States

States (countries) data is stored as an array of objects with strict element order. Element 0 is reserved for `neutrals`. If state is removed, the element is not getting removed, but instead a `removed` attribute is added. Object structure:

- `i`: `number` - state id, always equal to the array index
- `name`: `string` - short (proper) form of the state name
- `form`: `string` - state form type. Available types are `Monarchy`, `Republic`, `Theocracy`, `Union`, and `Anarchy`
- `formName`: `string` - string form name, used to get state `fullName`
- `fullName`: `string` - full state name. Combination of the proper name and state `formName`
- `color`: `string` - state color in hex (e.g. `#45ff12`) or link to hatching pattern (e.g. `url(#hatch7)`)
- `center`: `number` - cell id of state center (initial cell)
- `pole`: `number[]` - state pole of inaccessibility (visual center) coordinates, see [the concept description](https://blog.mapbox.com/a-new-algorithm-for-finding-a-visual-center-of-a-polygon-7c77e6492fbc?gi=6bd4fcb9ecc1)
- `culture`: `number` - state culture id (equals to initial cell culture)
- `type`: `string` - state type, see [culture types](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Culture types)
- `expansionism`: `number` - state growth multiplier. Used mainly during state generation to spread states not uniformly
- `area`: `number` - state area in pixels
- `burgs`: `number` - number of burgs within the state
- `cells`: `number` - number of cells within the state
- `rural`: `number` - rural (non-burg) population of state cells. In population points
- `urban`: `number` - urban (burg) population of state cells. In population points
- `neighbors`: `number[]` - ids of neighboring (bordering by land) states
- `provinces`: `number[]` - ids of state provinces
- `diplomacy`: `string[]` - diplomatic relations status for all states. 'x' for self and neutrals. Element 0 (neutrals) `diplomacy` is used differently and contains wars story as `string[][]`
- `campaigns`: `object[]` - wars the state participated in. The was is defined as `start`: `number` (year), `end`: `number` (year), `name`: `string`
- `alert`: `number` - state war alert, see [military forces page](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Military-Forces)
- `military`: `Regiment[]` - list of state regiments, see [military forces page](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Military-Forces)
- `coa`: `object` - emblem object, data model is the same as in [Armoria](https://github.com/Azgaar/Armoria) and covered in [API documentation](https://github.com/Azgaar/armoria-api#readme). The only additional fields are optional `size`: `number`, `x`: `number` and `y`: `number` that controls the emblem position on the map (if it's not default). If emblem is loaded by user, then the value is `{ custom: true }` and cannot be displayed in Armoria
- `lock`: `boolean` - `true` if state is locked (not affected by regeneration)
- `removed`: `boolean` - `true` if state is removed

### Regiment

- `i`: `number` - regiment id, equals to the array index of regiment in the `state[x].military` array. Not unique, as unique string `regimentStateId-regimentId` is used
- `x`: `number` - regiment x coordinate
- `y`: `number` - regiment y coordinate
- `bx`: `number` - regiment base x coordinate
- `by`: `number` - regiment base y coordinate
- `angle`: `number` - regiment rotation angle degree
- `icon`: `number` - Unicode character to serve as an icon
- `cell`: `number` - original regiment cell id
- `state`: `number` - regiment state id
- `name`: `string` - regiment name
- `n`: `number` - `1` if regiment is a separate unit (like naval units), `0` is not
- `u`: `Record<unitName, number>` - regiment content object

## Provinces

Provinces data is stored as an array of objects with strict element order. Element 0 is not used. If religion is removed, the element is not getting removed, but instead a `removed` attribute is added. Object structure:

- `i`: `number` - province id, always equal to the array index
- `name`: `string` - short (proper) form of the province name
- `formName`: `string` - string form name, used to get province `fullName`
- `fullName`: `string` - full state name. Combination of the proper name and province `formName`
- `color`: `string` - province color in hex (e.g. `#45ff12`) or link to hatching pattern (e.g. `url(#hatch7)`)
- `center`: `number` - cell id of province center (initial cell)
- `pole`: `number[]` - province pole of inaccessibility (visual center) coordinates, see [the concept description](https://blog.mapbox.com/a-new-algorithm-for-finding-a-visual-center-of-a-polygon-7c77e6492fbc?gi=6bd4fcb9ecc1)
- `area`: `number` - province area in pixels
- `burg`: `number` - id of province capital burg if any
- `burgs`: `number[]` - id of burgs within the province. Optional (added when Province editor is opened)
- `cells`: `number` - number of cells within the province
- `rural`: `number` - rural (non-burg) population of province cells. In population points
- `urban`: `number` - urban (burg) population of state province. In population points
- `coa`: `object` - emblem object, data model is the same as in [Armoria](https://github.com/Azgaar/Armoria) and covered in [API documentation](https://github.com/Azgaar/armoria-api#readme). The only additional fields are optional `size`: `number`, `x`: `number` and `y`: `number` that controls the emblem position on the map (if it's not default). If emblem is loaded by user, then the value is `{ custom: true }` and cannot be displayed in Armoria
- `lock`: `boolean` - `true` if province is locked (not affected by regeneration)
- `removed`: `boolean` - `true` if province is removed

## Religions

Religions data is stored as an array of objects with strict element order. Element 0 is reserved for "No religion". If province is removed, the element is not getting removed, but instead a `removed` attribute is added. Object structure:

- `i`: `number` - religion id, always equal to the array index
- `name`: `string` - religion name
- `type`: `string` - religion type. Available types are `Folk`, `Organized`, `Heresy` and `Cult`
- `form`: `string` - religion form
- `deity`: `string` - religion supreme deity if any
- `color`: `string` - religion color in hex (e.g. `#45ff12`) or link to hatching pattern (e.g. `url(#hatch7)`)
- `code`: `string` - religion name abbreviation. Used to render religions tree
- `origins`: `number[]` - ids of ancestor religions. `[0]` if religion doesn't have an ancestor. Used to render religions tree. The first array member is main link, other - supporting out-of-tree links
- `center`: `number` - cell id of religion center (initial cell)
- `culture`: `number` - religion original culture
- `expansionism`: `number` - religion growth multiplier. Used during religion generation to define competitive size
- `expansion`: `string` - religion expansion type. Can be `culture` so that religion grow only within its culture or `global`
- `area`: `number` - religion area in pixels
- `cells`: `number` - number of cells within the religion
- `rural`: `number` - rural (non-burg) population of religion cells. In population points
- `urban`: `number` - urban (burg) population of state religion. In population points
- `lock`: `boolean` - `true` if religion is locked (not affected by regeneration)
- `removed`: `boolean` - `true` if religion is removed

## Rivers

Rivers data is stored as an unordered array of objects (so element id is _not_ the array index). Object structure:

- `i`: `number` - river id
- `name`: `string` - river name
- `type`: `string` - river type, used to get river full name only
- `source`: `number` - id of cell at river source
- `mouth`: `number` - id of cell at river mouth
- `parent`: `number` - parent river id. If river doesn't have a parent, the value is self id or `0`
- `basin`: `number` - river basin id. Basin id is a river system main stem id. If river doesn't have a parent, the value is self id
- `cells`: `number[]` - if of river points cells. Cells may not be unique. Cell value `-1` means the river flows off-canvas
- `points`: `number[][]` - river points coordinates. Auto-generated rivers don't have points stored and rely on `cells` for rendering
- `discharge`: `number` - river flux in m3/s
- `length`: `number` - river length in km
- `width`: `number` - river mouth width in km
- `sourceWidth`: `number` - additional width added to river source on rendering. Used to make lake outlets start with some width depending on flux. Can be also used to manually create channels

## Markers

Markers data is stored as an unordered array of objects (so element id is _not_ the array index). Object structure:

- `i`: `number` - marker id. `'marker' + i` is used as svg element id and marker reference in `notes` object
- `icon`: `number` - Unicode character (usually an [emoji](https://emojipedia.org/)) to serve as an icon
- `x`: `number` - marker x coordinate
- `y`: `number` - marker y coordinate
- `cell`: `number` - cell id, used to prevent multiple markers generation in the same cell
- `type`: `string` - marker type. If set, style changes will be applied to all markers of the same type. Optional
- `size`: `number` - marker size in pixels. Optional, default value is `30` (30px)
- `fill`: `string` - marker pin fill color. Optional, default is `#fff` (white)
- `stroke`: `string` - marker pin stroke color. Optional, default is `#000` (black)
- `pin`: `string`: pin element type. Optional, default is `bubble`. Pin is not rendered if value is set to `no`
- `pinned`: `boolean`: if any marker is pinned, then only markers with `pinned = true` will be rendered. Optional
- `dx`: `number` - icon x shift percent. Optional, default is `50` (50%, center)
- `dy`: `number` - icon y shift percent. Optional, default s `50` (50%, center)
- `px`: `number` - icon font-size in pixels. Optional, default is `12` (12px)
- `lock`: `boolean` - `true` if marker is locked (not affected by regeneration). Optional

## Routes

Routes data is stored as an unordered array of objects (so element id is _not_ the array index). Object structure:

- `i`: `number` - route id. Please note the element with id `0` is a fully valid route, not a placeholder
- `points`: `number[]` - array of control points in format `[x, y, cellId]`
- `feature`: `number` - feature id of the route. Auto-generated routes cannot be place on multiple features
- `group`: `string` - route group. Default groups are: 'roads', 'trails', 'searoutes'
- `length`: `number` - route length in km. Optional
- `name`: `string` - route name. Optional
- `lock`: `boolean` - `true` if route is locked (not affected by regeneration). Optional

## Zones

Zones data is stored as an array of objects with `i` not necessary equal to the element index, but order of element defines the rendering order and is important. Object structure:

- `i`: `number` - zone id. Please note the element with id `0` is a fully valid zone, not a placeholder
- `name`: `string` - zone description
- `type`: `string` - zone type
- `color`: `string` - link to hatching pattern (e.g. `url(#hatch7)`) or color in hex (e.g. `#45ff12`)
- `cells`: `number[]` - array of zone cells
- `lock`: `boolean` - `true` if zone is locked (not affected by regeneration). Optional
- `hidden`: `boolean` - `true` if zone is hidden (not displayed). Optional

# Secondary global data

Secondary data exposed to global space.

## Biomes

Biomes data object is globally available as `biomesData`. It stores a few arrays, making it different from other data. Object structure:

- `i`: `number[]` - biome id
- `name`: `string[]` - biome names
- `color`: `string[]` - biome colors in hex (e.g. `#45ff12`) or link to hatching pattern (e.g. `url(#hatch7)`)
- `biomesMartix`: `number[][]` - 2d matrix used to define cell biome by temperature and moisture. Columns contain temperature data going from > `19` °C to < `-4` °C. Rows contain data for 5 moisture bands from the drier to the wettest one. Each row is a `Uint8Array`
- `cost`: `number[]` - biome movement cost, must be `0` or positive. Extensively used during cultures, states and religions growth phase. `0` means spread to this biome costs nothing. Max value is not defined, but `5000` is the actual max used by default
- `habitability`: `number[]` - biome habitability, must be `0` or positive. `0` means the biome is uninhabitable, max value is not defined, but `100` is the actual max used by default
- `icons`: `string[][]` - non-weighed array of icons for each biome. Used for _relief icons_ rendering. Not-weighed means that random icons from array is selected, so the same icons can be mentioned multiple times
- `iconsDensity`: `number[]` - defines how packed icons can be for the biome. An integer from `0` to `150`

## Notes

Notes (legends) data is stored in unordered array of objects: `notes`. Object structure is as simple as:

- `i`: `string` - note id
- `name`: `string` - note name, visible in Legend box
- `legend`: `string` - note text in html

## Name bases

Name generator consumes training sets of real-world town names (with the exception of fantasy name bases) stored in `nameBases` array, that is available globally. Each array element represent a separate base. Base structure is:

- `i`: `number` - base id, always equal to the array index
- `name`: `string` - names base proper name
- `b`: `string` - long string containing comma-separated list of names
- `min`: `number` - recommended minimal length of generated names. Generator will adding new syllables until min length is reached
- `max`: `number` - recommended maximal length of generated names. If max length is reached, generator will stop adding new syllables
- `d`: `string` - letters that are allowed to be duplicated in generated names
- `m`: `number` - if multi-word name is generated, how many of this cases should be transformed into a single word. `0` means multi-word names are not allowed, `1` - all generated multi-word names will stay as they are

---

# Data model (in progress)

In this document I would like to outline the expected data structure.

Relevant links:

- [Current data model](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Data-model)
- [Trello ticket](https://trello.com/c/IWltiMC2/902-rework-save-data-format)
- [Refactor branch contract changes](https://github.com/Azgaar/Fantasy-Map-Generator/pull/842)

Structure:

`.map` file is a valid JSON capturing all data required to render and operate the map, including UI and style settings.

```json
{
  "meta": {
    "copyright": "Azgaar's Fantasy Map Generator",
    "license": "MIT",
    "source": "http://azgaar.github.io/Fantasy-Map-Generator",
    "initial": {
      "timestamp": "2023-09-11T23:36:17.227Z",
      "version": "2.1.12",
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)"
    },
    "current": {
      "timestamp": "2025-02-15T14:42:31.748Z",
      "version": "2.126.3",
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)",
      "revision": 124
    }
  },
  "settings": {
    "seed": "342342342323",
    "graph": {
      "width": 1280,
      "heigh": 740,
      "points": 50000
    },
    "heightmap": {
      "template": "Volcano",
      "isRandom": false,
      "isPrecreated": false,
      "isCustom": false
    },
    "cultures": {
      "set": "Oriental",
      "limit": 11,
      "sizeVariety": 2,
      "growthRate": 1.3
    },
    "states": {
      "limit": 14,
      "sizeVariety": 2,
      "growthRate": 3,
      "labels": {
        "mode": "auto"
      }
    },
    "provinces": {
      "ratio": 30
    },
    "burgs": {
      "limit": null,
      "showMfcgMap": true
    },
    "religions": {
      "limit": 7
    },
    "labels": {
      "autoHide": true,
      "rescaleOnZoom": true
    },
    "notes": {
      "pinned": false
    },
    "scaleBar": {
      "label": "",
      "position": {
        "x": 99,
        "y": 99
      }
    },
    "military": {
      "units": {
        "0": {
          "name": "infantry"
        }
      }
    },
    "world": {
      "name": "Narnia",
      "calendar": {
        "year": 2024,
        "era": "Test Era",
        "eraShort": "TE"
      },
      "climate": {
        "temperature": {
          "equator": 30,
          "northPole": -30,
          "southPole": -25
        },
        "winds": [225, 45, 225, 315, 135, 315],
        "precipitation": 100
      },
      "geography": {
        "mapSize": 11,
        "latitudeShift": 50,
        "coordinates": {
          "latN": 34
        }
      },
      "units": {
        "distance": {
          "unit": "m",
          "scale": 3
        },
        "area": {
          "unit": "square",
          "scale": 1
        },
        "height": {
          "unit": "ft",
          "exponent": 2
        },
        "temperature": {
          "unit": "°C",
          "scale": 1
        },
        "population": {
          "scale": 1000,
          "urbanization": {
            "rate": 1,
            "density": 10
          }
        }
      }
    },
    "layers": {
      "heightmap": false,
      "states": true
    }
  },
  "style": {
    "scaleBar": {
      "size": 2,
      "backOpacity": 0.2,
      "backColor": "#ffffff"
    }
  },
  "data": {
    "grid": {
      "cells": {
        "i": [],
        "temp": []
      },
      "vertices": {
        "c": [[]]
      }
    },
    "pack": {
      "cells": {
        "i": [],
        "g": [],
        "state": [],
        "culture": []
      },
      "vertices": {
        "c": [[]]
      }
    },
    "biomes": {
      "0": {
        "name": "Marine",
        "isCustom": false,
        "cells": 354
      },
      "1": {}
    },
    "states": {
      "0": {},
      "1": {}
    },
    "cultures": {},
    "notes": {
      "0": {}
    },
    "rulers": {
      "0": {
        "i": 0,
        "type": "ruler",
        "points": [
          [0, 0],
          [642, 17]
        ]
      }
    }
  }
}
```

---

# Dependencies

The unsorted list of used libraries. Many thanks to authors:

- [D3.js v.5](https://d3js.org) by Mike Bostock and contributors
- [Delaunator](https://github.com/mapbox/delaunator) by Vladimir Agafonkin
- [Polylabel](https://github.com/mapbox/polylabel) by Vladimir Agafonkin
- [Lineclip](https://github.com/mapbox/lineclip) by Vladimir Agafonkin
- [Priority-queue](https://github.com/adamhooper/js-priority-queue) by Adam Hooper
- [Quantize](https://gist.github.com/thinkxl/4262930) by Nick Rabinowitz
- [jQuery](https://code.jquery.com/jquery-3.1.1.min.js) and [jQuery-ui](https://jqueryui.com) by jQuery team
- [Three.js](https://github.com/mrdoob/three.js) by mrdoob and Three.js contributors
- [OrbitControls](https://github.com/mrdoob/three.js/blob/master/examples/js/controls/OrbitControls.js) by qiao, mrdoob, alteredq, WestLangley, erich666 and ScieCode
- [Pell](https://github.com/jaredreich/pell) by Jared Reich
- [JSZip](https://github.com/Stuk/jszip) by Stuart Knightley, David Duponchel, Franz Buchinger and António Afonso
- [aleaPRNG](https://github.com/macmcmeans/aleaPRNG) by Johannes Baagøe

---

# GIS data export

# What is GIS?

A geographic information system (GIS) is a framework for gathering, managing, and analyzing data. Rooted in the science of geography, GIS integrates many types of data. It analyzes spatial location and organizes layers of information into visualizations using maps and 3D scenes.

# Why GIS with a Fantasy Map?

GIS tools are custom-built to handle maps, with large data sets, integrate with other tools such as databases and create interactive maps such as Google Maps.

Being able to import a fantasy map into a GIS system puts all the power of those systems into your hands. Among other things, a GIS map can have many more layers, for example to have a view of the world at different times in history. It can link to a database to bring in as much information about the world as you have. It can also dynamically update itself from that database. For example, online game [Might & Fealty](http://mightandfealty.com) allows to build roads, and roads can deteriorate over time. These roads and their condition are part of the game map.

Also, at this time, it is the only way to zoom from world map to street view without going to a different tool.

What GIS does not do is creating a map, it's made to deal with the real world, it doesn't generate random maps or invent cultures and religions. This is where Fantasy Map Generator comes in order to merge generated data with whatever other data you have in your game and to create an interactive read-only map view.

Here is a fantasy world made with the Fantasy Map Generator and exported to GIS tools: https://lemuria.org/dragoneye/atlas/Auseka

# Quantum GIS

QGIS is a free multi-platform tool for GIS work. It can load, edit and print maps in high quality as well as export the results for interactive online maps. In essence, if you ever thought that having Google Maps for your fantasy world would be incredible cool, this is how you do it.

Download and installation instructions can be found [here](https://qgis.org). There are also many videos and online instructions on QGIS:

- https://freegistutorial.com/qgis-tutorial-beginners/
- https://www.qgistutorials.com/en/
- https://www.youtube.com/watch?v=kCnNWyl9qSE
- https://www.youtube.com/watch?v=ltsKFhYQy_4
- https://www.youtube.com/watch?v=kCnNWyl9qSE

You are not, of course, limited to only QGIS. Data exported can also be used in other GIS tools, such as [GRASS](https://grass.osgeo.org/), [ArcGIS](https://www.arcgis.com) and others.

# Loading into QGIS

Fantasy Map Generator allows to export some data in GIS-compatible format.

## Cell Data

In the Save... menu of the Generator there is an option _.json_ to save the cell data into as a GeoJSON file. These can be imported into QGIS by choosing _Layer_ -> _Add Layer..._ -> _Add Vector Layer..._

![Steps](https://azgaar.files.wordpress.com/2019/09/add_vectorlayer.png)

Choose the saved .geojson file. It should be set up correctly as well, but doesn't show much. For the biomes, a prepared style can be found [here](https://raw.githubusercontent.com/evolvedexperiment/FMGImages/master/styles/Style_Biomes.qml). Load it for the new layer you just created and the biomes should show up (click on the properties of the initial "cells" layer and then in the symbology tab of the options, you click style then load.)

![Steps](https://azgaar.files.wordpress.com/2019/09/add_vectorlayer2.png)

There is additional cell information such as population, height (for a heightmap), but also states, provinces, culture, etc. exported, all of which can be used in QGIS to render this information.

Unfortunately, there are sometimes gaps or overlaps in the cell export data. You can find them in QGIS with _Vector -> Geometry Tools... -> Check Validity_. They are easy to fix by hand, if you need to (only needed if you want to do further processing with the data).

## Burg Data

Burg data can be downloaded as _.csv_ file using the button in the _Burgs Editor_ . The downloaded file contains position information (longitude, latitude and height) for burgs. These can be imported into QGIS by choosing _Layer_ -> _Add Layer..._ -> _Add Delimited Text Layer..._

![Steps](https://azgaar.files.wordpress.com/2019/09/add_csv.png)

Choose the exported .csv file. It should all be set up correctly automatically, so just check that x and y are correctly set to the longitude and latitude fields.

![Steps](https://azgaar.files.wordpress.com/2019/09/add_csv2.png)

## Marker Data

Points-of-Interest (markers) also contain location information and can be exported and imported in the same way as Burgs data.

# Processing in QGIS

To create a political/religious/provinces/culture/etc. map use _Vector_ -> _Geoprocessing Tools_ -> _Dissolve..._ and select the .geojson file as input and the attribute you want to merge by (e.g. "state"). QGIS will merge all the polygons that share the same attribute so that you end up with one multipolygon per state/culture/religion/etc.

![Steps](https://azgaar.files.wordpress.com/2019/09/merge-by-attribute.png)

![Steps](https://azgaar.files.wordpress.com/2019/09/dissolve.png)

Note that QGIs automatically turns a polygon layer into a multipolygon layer for this.

Simply repeat this step for all the layers that you want to use (once for states, once for provinces, etc.). This will give you one layer in QGIS per attribute, and you can then style and hide/show the layers as needed.

# Editing in QGIS

When working with cells, you need to make sure that neighbor cells are changed as well, to prevent overlaps or gaps. QGIS supports [topological editing](https://gis.stackexchange.com/questions/302965/how-can-i-enable-topological-editing-in-qgis-3).

# Rendering

To get that painted look, you want to use textures to fill, especially for biome data. Simply set up the symbology to use a Raster Fill Image, pick your texture and make sure to set the Coord mode to "Viewport":

![steps](https://azgaar.files.wordpress.com/2019/09/symbology.png)

![steps](https://azgaar.files.wordpress.com/2019/09/texturing.png)

# See also

- [How to create an interactive, detailed world atlas of your fantasy game world? Part One](https://www.youtube.com/watch?v=WIqd_WK2cvM)

- [How to create an interactive, detailed world atlas of your fantasy game world? Part Two](https://www.youtube.com/watch?v=C8mZKV9vVp4)

- [How to create an interactive, detailed world atlas of your fantasy game world? Part Three](https://www.youtube.com/watch?v=3Ut4hoiprC0)

- [A script to add random points](https://cdn.discordapp.com/attachments/587406457725779968/620223033205850133/add_random_points.php)

---

# Heightmap customization

The **heightmap editor** allows you to create and customize the heightmap manually, giving you more control over the world's terrain, unlike random generation.

To access the Heightmap Editor, you can either:

1. Navigate to Tools > Heightmap in the menu. (Look at the image below)
2. Use the hotkey _shift + h_.

<details>
<summary>Show Image</summary>

![Heightmap Editor Menu](https://github.com/ZZWILLIAMXXTrue/FMG-Console-Codes/blob/main/Image/Heightmap%20Selection.png)

</details>

### Modes

The heightmap editor offers three different modes to choose from. Its recommended to save your map beforehand.

1. **Erase**:

   - This mode regenerates all data on your map, including cultures, states, biomes, and more.
   - It offers the most customization features, such as the image converter and template editor.

2. **Keep**:

   - This mode allows you to retain most of the map's existing data.
   - However, it does not allow changes to the coastline.

3. **Risk**:
   - This mode allows you to keep most of the map's data while also allowing changes to the coastline.
   - Note that using this mode can potentially cause some errors.

<details>
<summary>Show Image</summary>

![Modes](https://github.com/ZZWILLIAMXXTrue/FMG-Console-Codes/blob/main/Image/Heightmap%20Selection%202.png)

</details>

### Heightmap Editor Features

The Heightmap Editor includes tools to help you create and customize your map. Below is a detailed list of all available features:

- **Paint Brushes**:

  - Automatically opens and allows you to edit the map.
  - Use different brushes to draw and modify the height.

- **Template Editor**:

  - Edit, load, or create templates for your heightmap.

- **Image Converter**:

  - Convert images into heightmaps.

- **Preview**:

  - Displays a monochromatic image of the map in the bottom left corner.
  - You can click the image to download it.

- **3D Scene**:

  - Opens a 3D version of the map in a new window.
  - Allows for a more immersive view of the map's shape and features.

- **Enable Ocean Cells**:

  - Enable or disable ocean cells on your map.
  - Useful for defining water bodies and coastlines.

- **Allow Water Erosion**:
  - Simulate water erosion effects on the terrain.
  - Adds realism to the map by shaping the landscape according to water flow.

Once you are satisfied with the Heightmap, click on **Exit Customization** in the bottom right.

<details>
<summary>Show Image</summary>

![Features](https://github.com/ZZWILLIAMXXTrue/FMG-Console-Codes/blob/main/Image/Heightmap%20Features.png)

</details>

## Paint brushes

- **Radius (top slider)**: Controls the radius for brushes.
- **Power (bottom slider)**: Controls the intensity of brushes.
  - Note: This has no effect on align tool.
- **Raise**: Increases cell height by power value; drag for continuous usage.
- **Elevate**: Drag to gradually increase cell height by power value.
- **Lower**: Decreases cell height by power value; drag for continuous usage.
- **Depress**: Drag to gradually decrease cell height.
- **Align**: Fits cell height to the first tagged cell; drag for continuous usage.
- **Smooth**: Smooths cell height; drag the map for continuous usage.
- **Disrupt**: Randomizes heights slightly; drag for continuous usage.
- **Checkbox “change only land cells”**: Prevents changes to the coastline.
- **Line Tool**: Creates mountains or trenches in lines.

### Footer Buttons

- **Un-do**: Steps back, works only for Heightmap customization.
- **Re-do**: Steps forward, works only for Heightmap customization.
- **Rescaler**: Slider to rescale all cells.
- **Condition**: Conditional height rescaler.
- **Smooth**: Smooths all heights a bit.
- **Disrupt**: Randomizes all heights a bit.
- **Clear**: Sets all heights to 0.

<details>
<summary>Show Image</summary>

![Brushes](https://github.com/BroCrows/FMG-Console-Codes/blob/main/Image/paintbrushes.png)

</details>

## Template Editor

Please see [Template Editor](../wiki/Heightmap-template-editor).

## Image Converter

Once opened, you'll be asked to select an image from your files.

- **Upload Image**:

  - Here you can upload a different image you’d like to convert into a heightmap.
  - Recommended are monochromatic images with black representing the ocean.

- **Auto-Assign Colors Based on Luminosity**:

  - This option assigns the colors to heights based on their luminosity.
  - Good for monochromatic images.

- **Auto-Assign Colors Based on Hue**:

  - This option assigns the colors to heights based on their hue.
  - Suitable for maps with differing colors.

- **Auto-Assign Colors Based on Generated Scheme**:

  - This option assigns the colors to heights based on Azgaar's bright color scheme.
  - Ideal for maps made with the generator.

- **Set Maximum Amount of Colors**:

  - Allows you to set the number of colors you can assign.
  - Can be any number from 3 to 255.

- **Cancel the Conversion**:

  - Cancel the image conversion and revert back to the previous map.

- **Complete the Conversion**:
  - Fully loads the map into the Fantasy Map Generator.
  - All unassigned colors will default to ocean.

To assign a color by hand, click the color you want to edit and then click the height you want on the set height slider.

<details>
<summary>Show Image</summary>

![Converter](https://github.com/BroCrows/FMG-Console-Codes/blob/main/Image/paintbrushes.png)

</details>

---

# Heightmap image overlay

## Overview

The "Texture" layer can be used as an image overlay - an image that appears over the existing map.

This can be enabled during heightmap editing to allow you to see both the heightmap and the image. Possible uses are for tracing landmasses or adding features more accurately.

## Caveats

For the texture to work, the image must be available on the web.
Due to browser security restrictions, only websites that have a relatively open Cross-Origin Resource Sharing (CORS) policy will work. You can possibly use a CORS proxy for other sites. [Imgur](https://www.imgur.com) is one site that works for this feature.

## Loading an image as an overlay

Go to Style, then click the "+" button next to Image.

![Style texture dialog](https://evolvedexperiment.github.io/FMGImages/images/style_texture1.png)

Enter the URL of the image you want to use - when it is correct, the image will appear here - this does not mean that it will work - just that the URL is correct. Click Apply.

![Image URL](https://evolvedexperiment.github.io/FMGImages/images/style_texture2.png)

Still on the Texture style, change Clipping to "No Clipping".

Go to Layers, choose the Heightmap preset.
Enable the Texture layer, then drag the Texture layer before the Heightmap layer.

![Texture and Heightmap layers both enabled](https://evolvedexperiment.github.io/FMGImages/images/style_texture3.png)

Lower the opacity of the texture under Style.  
![Map showing both heightmap and image overlay](https://evolvedexperiment.github.io/FMGImages/images/style_texture4.png)
Alternatively you can lower the opacity of the heightmap - it depends on what looks best to you.  
At this point you should be able to see both your map and the image.
You can play with it till everything looks clear.

Go to Tools, Heightmap, Erase.
Initially it will hide the Texture layer - go to Layers, then enable the Texture layer again, and it should show
both the heightmap and your image.

---

# Heightmap template editor

Template is a set of actions to be applied to get a heightmap. Template can be pretty prescriptive and provide similar-looking heightmaps on each execution, but there is always a significant level of randomness that allow templates to produce different maps of the same type.

Possible actions are:

- Add a **hill** (raises surrounding land)
- Add a **pit** (lowers surrounding land)
- Add a **range** (a thin raised section)
- Add a **trough** (a thin lowered section)
- Add a **strait** (a vertical or horizontal lowered section)
- **Mask** heightmap (lower all cells along map edge or in map center)
- **Invert** heightmap (mirror by X, Y or both axes)
- **Add** or subtract from all heights
- **Multiply** all heights
- **Smooth** all heights

Once you have given all the instructions, you can then run the template (process all instructions in sequence).
There are also options to download and upload templates.

When running a template, the map is cleared first. Undo / redo buttons can be used to check the heightmap on each step. In seed is locked, it will be used for heightmap generation, so the map will be the same on each execution.

Step can be skipped by clicking on the checkbox on the left. To execute the template click on the Execute button or just press <kbd>Enter</kbd>.

Step can have the following values:

- `n` - the number of times it must occur, can be a decimal value or range for random selection
- `h` - the height range, from 1-100 where 1 is deep ocean, and 100 is maximum height. 20 is land at sea level.
- `x` and `y` - percentage values (from 0-100), indicating where the change must take place, provide as a range
- - `x` is the horizontal axis, from 0 at the left, to 100 on the right.
- - `y` is the vertical axis, from 0 at the top, to 100 at the bottom.

## Hill

When you open the Template Editor, the instruction is like this:

- `Hill    n:1    h:90-100   x:65-75   y:47-53`

This says to add 1 hill, of a random height between 90-100, somewhere in the center-right of the map.

To test it, change the Hill entry to:

- `Hill    n:1    h:20   x:50-50   y:50-50`

If you click Execute template, it will show a map with just a few land cells right in the map center.

![Heightmap showing a single hill of height 20](https://evolvedexperiment.github.io/FMGImages/images/template1.png)

I suggest you enable "Render ocean cells" option to see the effect it has on the ocean depths.

Now change n:1 to n:2

You will see massive change — this is because the land has been raised by 40 height in total.

![Heightmap showing two hills of height 20 at the same place](https://evolvedexperiment.github.io/FMGImages/images/template2.png)

Hills are main 'bricks' that construct a heightmap.

## Pit

Pit is the opposite of Hill, it creates a "hole".

To test it, add a Pit so your instructions look like this:

- `Hill    n:2    h:20   x:50-50   y:50-50`
- `Pit     n:1    h:20   x:50-50   y:50-50`

Run it and you will see a hole — the exact look will vary a bit.

![Heightmap showing two hills and pit at the same place](https://evolvedexperiment.github.io/FMGImages/images/template3.png)

## Range

Add a range and disable the Hill and Pit entries.

You will see it makes an elongated raised area.

- `Range   n:1    h:40-50   x:15-85   y:20-80`

![Heightmap showing a single range](https://evolvedexperiment.github.io/FMGImages/images/template4.png)

If you change the Range and enable the Hill and Pit, you will see that it combines:

- `Hill    n:2    h:20   x:50-50   y:50-50`
- `Pit     n:1    h:20   x:50-50   y:50-50`
- `Range   n:1    h:40-50   x:50-60   y:50-50`

![Heightmap showing two hills, a pit, and a range.](https://evolvedexperiment.github.io/FMGImages/images/template5.png)

## Trough

Trough works exactly like Range, except that it lowers height.

### Add and Straits

Delete all the instructions and click on the + button to get the "Add" instruction.
The Add instruction adds height - it can have a negative value to lower height.

Change the V to 20 so your whole line looks like this:

- `Add     V:20    to all cells`

Run it and it will change to whole map to land at sea-level - remember that 20 is just above sea-level.
Add a Strait - it only has 2 values, width and direction:

- w - width
- d - vertical or horizontal

Run it and you will see land divided by a river somewhere - note that you cannot control the location.

![Heightmap showing a strait](https://evolvedexperiment.github.io/FMGImages/images/template6.png)

## Multiply

Multiply works similar to add, except you have slightly better control to adjust small things, so multiplying by 1.1
will make raise land slightly - smaller changes to low values, and larger changes to high values. You can multiply
by decimals as well, so multiply by 0.8 will lower everything a bit.

## Smooth

Smooth averages cell heights by their neighbors heights. This means land next to a pit will lower, and land next to a hill will rise. Smooth removes any spiky bits near land and makes FMG performance better.

![Heightmap showing a strait between two hilly areas.](https://evolvedexperiment.github.io/FMGImages/images/template7.png)

## Mask

Mask is a newer step so it was not included into the original guide. To see what is does lets assign maximum height (100) to all cells and then add Mask with default `1` fraction. It will gradually mask height of the cells along map edge to 0.

![image](https://user-images.githubusercontent.com/26469650/169668247-36d8d414-15bd-4feb-97ab-aa850faab972.png)

Fraction `2` will blend masked values with the original ones, so map edges will get `50` height.

![image](https://user-images.githubusercontent.com/26469650/169668347-215fcf81-0626-4ad2-af2d-4eb2f0c786e4.png)

Negative fraction inverts the mask, so that cells in the map center are lowered.

![image](https://user-images.githubusercontent.com/26469650/169668348-a5474fee-92fc-47fa-b0f1-61eb0cb4bb82.png)

Fraction can even be a decimal value.

![image](https://user-images.githubusercontent.com/26469650/169668364-82dad6eb-2665-4cc6-aa80-fd4f66f72273.png)

## Invert

Invert is also a newer action. It mirrors the heightmap by X, Y or both axes. It is useful for maps that lean towards one side. In this case we can add invert with some probability (in 0-1 range) to mirror the map.

![image](https://user-images.githubusercontent.com/26469650/169668430-d64589c1-4e8f-44f6-bd92-69dcb2fc83c2.png)

---

# Hotkeys

This page covers Hotkeys available in _Fantasy Map Generator_. Please note hotkeys work only when map screen is active (click on map to activate)

## Hotkeys:

General:

- <kbd>F1</kbd> - show info
- <kbd>F2</kbd> - generate new map
- <kbd>F6</kbd> - quick save
- <kbd>F9</kbd> - quick load
- <kbd>Tab</kbd> - toggle options pane
- <kbd>Esc</kbd> - close all edit screens
- <kbd>Delete</kbd> - remove selected element
- <kbd>Shift</kbd> - hold to continue adding elements on click (e.g. burgs and labels)
- <kbd>+</kbd> or <kbd>=</kbd> - increase brush size
- <kbd>-</kbd> - decrease brush size
- <kbd>Ctrl</kbd> + <kbd>S</kbd> - download .map file
- <kbd>Ctrl</kbd> + <kbd>C</kbd> - save .map file to Dropbox

Map scrolling and zooming:

- <kbd>↑</kbd> - scroll up
- <kbd>→</kbd> - scroll right
- <kbd>↓</kbd> - scroll down
- <kbd>←</kbd> - scroll left
- <kbd>+</kbd> - zoom in
- <kbd>-</kbd> - zoom out
- Double click - zoom into clicked area
- <kbd>Shift</kbd> + double click - zoom out
- <kbd>0</kbd> - reset zoom
- <kbd>1</kbd> - <kbd>9</kbd> - set zoom

Toggle map layers:

- <kbd>X</kbd> - toggle Texture layer
- <kbd>H</kbd> - toggle Heightmap layer
- <kbd>B</kbd> - toggle Biomes layer
- <kbd>E</kbd> - toggle Cells layer
- <kbd>G</kbd> - toggle Grid layer
- <kbd>O</kbd> - toggle Coordinates layer
- <kbd>W</kbd> - toggle Compass (Wind) Rose layer
- <kbd>V</kbd> - toggle Rivers layer
- <kbd>F</kbd> - toggle Relief icons layer
- <kbd>C</kbd> - toggle Cultures layer
- <kbd>S</kbd> - toggle States layer
- <kbd>P</kbd> - toggle Provinces layer
- <kbd>Z</kbd> - toggle Zones layer
- <kbd>D</kbd> - toggle Borders layer
- <kbd>R</kbd> - toggle Religions layer
- <kbd>U</kbd> - toggle Routes layer
- <kbd>T</kbd> - toggle Temperature layer
- <kbd>N</kbd> - toggle Population layer
- <kbd>J</kbd> - toggle Ice layer
- <kbd>A</kbd> - toggle Precipitation layer
- <kbd>Y</kbd> - toggle Emblems layer
- <kbd>L</kbd> - toggle Labels layer
- <kbd>I</kbd> - toggle Icons layer
- <kbd>M</kbd> - toggle Military layer
- <kbd>K</kbd> - toggle Markers layer
- <kbd>=</kbd> - toggle Rulers
- <kbd>/</kbd> - toggle Scale bar

Tools:

- <kbd>Shift</kbd> + <kbd>H</kbd> - edit Heightmap
- <kbd>Shift</kbd> + <kbd>B</kbd> - edit Biomes
- <kbd>Shift</kbd> + <kbd>S</kbd> - edit States
- <kbd>Shift</kbd> + <kbd>P</kbd> - edit Provinces
- <kbd>Shift</kbd> + <kbd>D</kbd> - edit Diplomacy
- <kbd>Shift</kbd> + <kbd>C</kbd> - edit Cultures
- <kbd>Shift</kbd> + <kbd>N</kbd> - edit Namesbase
- <kbd>Shift</kbd> + <kbd>Z</kbd> - edit Zones
- <kbd>Shift</kbd> + <kbd>R</kbd> - edit Religions
- <kbd>Shift</kbd> + <kbd>Q</kbd> - edit Units
- <kbd>Shift</kbd> + <kbd>O</kbd> - edit Notes

- <kbd>Shift</kbd> + <kbd>A</kbd> - open Data Charts
- <kbd>Shift</kbd> + <kbd>T</kbd> - open Burgs Overview
- <kbd>Shift</kbd> + <kbd>V</kbd> - open Rivers Overview
- <kbd>Shift</kbd> + <kbd>M</kbd> - open Military Overview
- <kbd>Shift</kbd> + <kbd>K</kbd> - open Markers Overview
- <kbd>Shift</kbd> + <kbd>E</kbd> - open Cells Details view

- <kbd>Shift</kbd> + <kbd>1</kbd> - click to add Burg
- <kbd>Shift</kbd> + <kbd>2</kbd> - click to add Label
- <kbd>Shift</kbd> + <kbd>3</kbd> - click to add River
- <kbd>Shift</kbd> + <kbd>4</kbd> - click to add Route
- <kbd>Shift</kbd> + <kbd>5</kbd> - click to add Marker

Heightmap editor:

- <kbd>Ctrl</kbd> + <kbd>Z</kbd> - undo an action
- <kbd>Ctrl</kbd> + <kbd>Y</kbd> - redo an action

---

# Editing Markers

## Marker overview

Markers are icons on the map representing a point of interest, e.g. a battlefield, a bridge, an inn, etc. You can toggle the markers layer on or off using the button on the Layers tab.

## Marker generation and editing

After map generation, the markers are randomly generated according to a set of criteria (e.g. bridges can only appear on rivers), and you can add your own markers anywhere you want. Each randomly generated marker has some randomly generated relevant notes attached. Notes on both randomly generated markers and manually added markers can be edited. Notes can have different fonts which you can edit in the notes editor. You can also edit properties for markers like the icon, icon size, and position. Icons have to be from a list of icons, or you can add a new icon which must be a unicode character. Custom icons are not possible at the moment. To edit a marker, click the marker icon. A small window with marker properties will appear. You can edit notes using the bottom-left button.

## Special markers

There are special markers for dungeons, which link to Watabou's one-page dungeon generator (https://watabou.itch.io/one-page-dungeon). These special markers show a preview in the notes. In the notes editor, click on the <> button to show the HTML code used for the preview. You can use the same text to generate your own previews to dungeons. The text is below:

`<div>Undiscovered dungeon. See <a href="https://watabou.github.io/one-page-dungeon/?seed=573621350629" target="_blank" rel="noopener">One page dungeon</a></div>`
`<p><iframe src="https://watabou.github.io/one-page-dungeon/?seed=573621350629" sandbox="allow-scripts allow-same-origin"></iframe></p>`

You can create an image preview using the <> button and text similar to this:

`<p>Ruins of an ancient city. Untold riches may lie within.</p>`
`<p>&nbsp;</p>`
`<img src="https://upload.wikimedia.org/wikipedia/commons/4/43/Peru_Machu_Picchu_Sunrise.jpg" alt="Machu Picchu at sunrise" width="150" height="200" />`

You can also make a clickable link with an image preview:

`<p>Ruins of an ancient city. Untold riches may lie within.</p>`
`<p>&nbsp;</p>`
`<a href="https://yourwebsitehere.com">`
`<img src="https://upload.wikimedia.org/wikipedia/commons/4/43/Peru_Machu_Picchu_Sunrise.jpg" alt="Machu Picchu at sunrise" width="150" height="200" />`
`</a>`

## Marker criteria

Criteria and types of randomly added markers are below:

- Battlefields: These are areas within a state that have a noticeable population, found in mid-elevation regions.
- Bridges: These are found in cities located near rivers, especially where the river is not close to the ocean and the water flow is significant.
- Brigands: Roads with high activity, usually where there’s a strong cultural presence, may attract outlaws.
- Canoes: These appear in regions with rivers, representing local watercraft.
- Circuses: Found in cultural areas with developed roads, particularly near or around sea level.
- Dances: These events take place in towns with a moderately sized population.
- Dungeons: Isolated and sparsely populated regions often feature these hidden or abandoned places.
- Hill Monsters: These creatures are found in highland areas where there is some population present.
- Hot Springs: Located in elevated regions, these natural thermal baths are found in hilly or mountainous areas.
- Inns: These rest stops appear along busy roads in well-populated areas.
- Jousts: Held in larger towns, especially in places with a robust population.
- Lake Monsters: Found in freshwater lakes, representing mythical creatures.
- Lighthouses: These are positioned along coastlines with good access to the sea and a large number of water routes.
- Migrations: These occur in sparsely populated midland regions where people or animals move.
- Mines: Found in towns located in higher elevations where valuable minerals may be extracted.
- Mirage: Seen in hot desert regions, these are illusions caused by extreme heat and light conditions.
- Pirates: Found on the open sea along major shipping routes, representing maritime outlaws.
- Portals: These mystical gateways appear in the oldest towns.
- Rifts: Occur in sparsely populated biomes that are still somewhat habitable, representing splits in the earth.
- Ruins: Found in mid-elevation cultural areas, these represent remnants of past civilizations.
- Sacred Forests: These appear in cultural regions with temperate forests, believed to be places of natural reverence.
- Sacred Mountains: Found in high mountain regions, close to cultural areas, these are considered spiritually important.
- Sacred Palm Groves: Found in desert regions with cultural significance and some population, connected by roads.
- Sacred Pineries: These are located in cultural areas within boreal forests.
- Sea Monsters: Mythical creatures found in the ocean along busy sea routes.
- Statues: These monuments appear in low to mid-elevation regions.
- Volcanoes: Found in very high mountain regions, representing active or dormant volcanic activity.
- Waterfalls: Located in hilly or mountainous areas with rivers, especially where there’s a sharp drop in terrain nearby.

---

# Military Forces

**Military forces**, added to the Generator in [version 1.3](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Changelog), allows to view auto-generated regiments and setup custom units. Generation is based on population, land possession and cultural specific, climate, diplomacy and other factors. The process is cell-based, but aggregated to Regiments level for usability purposes.

Generation rules, unit types and regiments themselves are editable and customizable. You can add fantasy, modern or ancient units, change parameters and re-generate military to let the system automatically recreate regiments. Changes specific to state or culture can be done manually on regiments level.

Regiments are displayed as state-colored boxes in a separate layer called _Military_ (<kbd>M</kbd> to toggle). To edit specific Regiment click on its box. To open Military overview screen go to _Tools_ and click on _Military_ (hotkey: <kbd>Shift</kbd> + <kbd>M</kbd>).

## Military overview

![](https://cdn.discordapp.com/attachments/587406457725779968/719203701180596779/military_overview.png)

Military overview shows the army that states have. _Total_ value shows sum of all military units, considering unit crew (see the next section for the details). _Rate_ shows a percentage of military personnel to state population (militarization index), by default is about 1-2%. _War Alert_ defines how much state is willing to wage a war and works as a multiplier to the units number. The calculation method is described in details in the Generation logic section below. The button on the right shows a list of all state's regiments and alow to add new regiments.

The bottom menu allows you to open Units Editor (see below), show units numbers as a percentage, download data for reference and trigger military forces recalculation.

## Military units editor

![](https://cdn.discordapp.com/attachments/587406457725779968/720368676360028252/military_options.png)

Military units that are used for generation are customizable. To open units editor click on the _Cog_ icon in the Military overview screen. Here you can add new units and specify their features. The maximum number of units is not defined, but having a lot will make overview screens too complex to observe. The idea of unit is defined by its _type_. Type variants are hard-coded and define specific rules to be applied on generation, you can see all of these rules described in the next section.

Customizable parameters are:

- **Icon** - unit symbol. We are using Unicode emojis for simplicity. While there is a list of pre-selected ones, you can use any Unicode character. Please note that Unicode emojis look different in different systems and browsers. Here is [the full list](https://unicode.org/emoji/charts/full-emoji-list.html)
- **Name** - a unique unit name. Can be pretty much any
- **Rural** - percentage of rural population to be conscripted to the unit. It defines how many troops of this unit will be generated for a cell population point. Then this number is getting adjusted based on specific rules described in the next section. Set to 0 if you want unit to be generated in burgs only
- **Urban** - percentage of urban population to be conscripted to the unit. Works the same as _rural_, but for burg population
- **Crew** - average number of people in one unit. Like tank crew is usually 4. This number is used for total people calculation and does not affect unit _power_ at all
- **Power** - damage dealt by unit. Used in battle simulation only
- **Type** - a set of rules to be applied for the unit, see the details in the next section
- **Sep.** - check if unit is _separate_ and not forming a regiment with platoons of other types. It means that e.g. naval units are not getting mixed with non-naval units in one regiment by default.

For any change you have to click on _Apply_ and it will trigger generation. It's done to avoid possible issues and as for now there is no way to change units without regeneration.

## Generation logic

Fantasy Map Generator regiments creation logic is pretty advanced and considers different aspects such as state diplomacy, type, culture and religion, cell biome and elevation, as well as military unit specific.

For each state _War Alert_ is getting calculated. It shows how much state is willing to wage a war. War Alert acts as a modifier to all military forces of the state. For example if state has 1000 infantry generated and War Alert is 2, total infantry number will be 2000. War Alert rate is a combination of _Expansion Fulfillment_ (State Expansionism / State Area) and _Diplomatic alert_ (rate of diplomatic relations). It means that expansionist states with relatively small area get higher War Alert rate than big states with moderate expansionism. Diplomatic alert is a sum of relations rates, where bad relations increase the value, while good decrease it. Diplomatic alert is calculated separately for neighboring and all states, so relations with neighbors have more impact on War Alert. Diplomatic relations modifiers are:

`Ally: -0.2, Friendly: -0.1, Neutral: 0, Suspicion: 0.1, Enemy: 1, Unknown: 0, Rival: 0.5, Vassal: 0.5, Suzerain: -0.5`

War Alert is not the only state-specific modifier. The other one depends on _State Type_ and getting applied based on hard-coded matrix:

|          | Melee | Ranged | Mounted | Machinery | Naval | Armored | Aviation | Magical |
| -------- | ----- | ------ | ------- | --------- | ----- | ------- | -------- | ------- |
| Generic  | 1     | 1      | 1       | 1         | 1     | 1       | 1        | 1       |
| Nomadic  | 0.5   | 0.9    | 2.3     | 0.8       | 0.5   | 1       | 0.5      | 1       |
| Highland | 1.2   | 1.3    | 0.6     | 1.4       | 0.5   | 0.5     | 0.5      | 2       |
| Lake     | 1     | 1      | 0.7     | 1.1       | 1.2   | 1       | 1.2      | 1       |
| Naval    | 0.7   | 0.8    | 0.3     | 1.4       | 1.8   | 1       | 1.2      | 1       |
| Hunting  | 1.2   | 2      | 0.7     | 0.4       | 0.7   | 0.7     | 0.6      | 1       |
| River    | 1.1   | 0.8    | 0.8     | 1.1       | 1.2   | 1.1     | 1.2      | 1       |

Some state forms have an additional modifier. _Hordes_ get x2 mounted units, while _Republics_ get x1.2 naval units.

The next step is to calculate troops number for each cell and burg. Calculation is done separately for each unit and considers possession-specific divider, unit percentage set in military options, state modifier calculated above and hard-coded unit type matrix. For example mounted units have x3 modifier in cells with nomadic biomes, while their number is reduced in highlands. The formula is:

`Troops = Population_points / 100 * Possession_divider * Unit_percentage * State_mod * Unit_mod * Population_rate`

Possession-specific divider is applied in 3 cases only:

- Cell culture is not state's dominant culture: divider is 1.2 for _Unions_ and 2 for other states
- Cell religion is not state's dominant religion: divider is 2.2 for _Theocracies_ and 1.4 for other states
- Cell is on a different island than state's center: divider is 1.2 for _Naval_ states and 1.8 for others.

Unit-specific modifiers are applied for cells with _Nomadic_ and _Wetland_ biomes, and also for _Highland_ cells (_height_ > 70):

|               | Melee | Ranged | Mounted | Machinery | Armored | Magical | Naval | Aviation |
| ------------- | ----- | ------ | ------- | --------- | ------- | ------- | ----- | -------- |
| Nomadic cell  | 0.2   | 0.5    | 3       | 0.4       | 1.6     | 0.5     | 0.3   | 1        |
| Wetland cell  | 0.8   | 2      | 0.3     | 1.2       | 0.2     | 0.5     | 1.2   | 0.5      |
| Highland cell | 1.2   | 1.6    | 0.3     | 3         | 0.8     | 2       | 0.3   | 0.3      |
| Nomadic burg  | 0.3   | 0.8    | 3       | 0.4       | 1.6     | 0.5     | 0.3   | 1        |
| Wetland burg  | 1     | 1.6    | 0.2     | 1.2       | 0.2     | 0.5     | 1.2   | 0.5      |
| Highland burg | 1.2   | 2      | 0.3     | 3         | 0.8     | 2       | 0.3   | 0.3      |

Here are generic troops number for 10000 population if state modifier is 1 and standard units options and population rate are applied:

|               | Melee | Ranged | Mounted | Machinery | Naval |
| ------------- | ----- | ------ | ------- | --------- | ----- |
| Generic cell  | 25    | 12     | 12      | 0         | 0     |
| Nomadic cell  | 5     | 6      | 36      | 0         | 0     |
| Wetland cell  | 20    | 24     | 4       | 0         | 0     |
| Highland cell | 30    | 19     | 4       | 0         | 0     |
| Generic burg  | 20    | 20     | 3       | 3         | 1     |
| Nomadic burg  | 6     | 16     | 9       | 1         | 0     |
| Wetland burg  | 20    | 32     | 1       | 4         | 1     |
| Highland burg | 24    | 40     | 1       | 9         | 0     |

Please also note that _Naval_ units can be generated only in port burgs.

Troops for each cell and burg form a _platoon_. As number of platoons is too high, they are getting sorted by troops number and aggregated to _regiments_ based on distance between platoons and expected size. Regiment expected size is calculated by a simple formula of `3 * Population_rate`. If the current regiment is big enough, new one is getting created. Then system generates names and a few details for regiments and place them on a map.

## Regiment Editor

![](https://cdn.discordapp.com/attachments/587406457725779968/720385818694254602/regiment_editor.png)

If you click on a regiment box, a Regiment Editor will pop up at the top left corner. Once editor is displayed, you can drag regiment boxes to move them around. A circle shows regiment's base and can be dragged as well.

From the Regiment Editor screen you can rename the regiment or restore its original name, change regiment emblem and composition. You can also define regiment's type: _land_ or _naval_. Naval regiments look almost the same as land, the only visual difference is box size.

Buttons at the bottom allows to attack foreign regiment (see [Battle Simulator](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Battle-Simulator) for the details), add a new regiment, split the regiment into 2 ones, attach the regiment to another one, regenerate or edit regiment's note (legend) and also remove the regiment.

---

# Q&A

Here I want to answer the most common questions _Fantasy Map Generator_ (FMG) users may have. Please feel free to raise a new [issue](https://github.com/Azgaar/Fantasy-Map-Generator/issues) in order to request additional answers.

### I have issues with the Generator, what should I do?

Please try to reproduce the issue on your own. If it's reproducible, please log [an issue](https://github.com/Azgaar/Fantasy-Map-Generator/issues). A lot of issues are caused by browsers, please also try to use incognito mode and/or another browser. I recommend Chrome as the fastest browser in terms of svg rendering.

### The map performance is poor, how can I improve it?

The performance mainly depends on the number of visible elements and visible map area. The optimization strategies are:

- Toggle off unnecessary layers. Be mindful of the _Relief Icons_ layer in particular – it’s the most resource-demanding one.
- Open the Generator in a separate browser window, make it _much_ smaller (about 900 x 560 pixels) and re-generate the map. Then, zoom in to see the map in detail. It will reduce the rending area and _drastically_ improve the performance.
- When generating maps, set _Points number_ to 10K. Points (cells) number highly affects performance.
- Toggle off map and element filters.
- Close all irrelevant browser tabs and applications.
- Use a leading edge browser (fresh versions on Chrome or Edge). Firefox is reported to be slower.

### Who owns the maps created?

You. The Generator is licensed under [MIT license](https://github.com/Azgaar/Fantasy-Map-Generator/blob/master/LICENSE) and derivative works such as maps are free of charge. You can sell them or make them available for free.

### My saved map is not working properly. What should I do?

If there is no version conflict, please [raise a defect](https://github.com/Azgaar/Fantasy-Map-Generator/issues/new). If your map is _obsolete_, and it's clearly stated on load, you may either use an [appropriate version](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Changelog) of the Generator or re-create the map in a current version. There is no way to update it. The tool is under development and version conflicts are inevitable.

### Can I export a created map?

Sure, there are a number of available options:

- Save to machine: save file can be directly loaded to the Generator.
- Save to Dropbox: save file can be directly loaded to the Generator.
- Save to storage: save map data to the browser's internal database. File will be loaded automatically on page refresh. Bear in mind that saving to desktop is safer since browser storage can be accidentally cleared

- Export .svg: save a full map as a scalable vector image. You can open the file in a browser or edit it using a vector graphics editor.
- Export .png: save the currently displayed map fragment as a raster image. You can edit the file in any raster graphics editor.
- Export to tiles as .zip: split map on .png chunks and save them all as a single .zip file. it allows to save giant raster images once chunks are combined.
- Export to .json: save the map data to be used in GIS software.

### How can I open a saved .gz or .map file?

Open the generator, click on _Load_ and select the file. Or just drag and drop the file onto the Generator window.

### Can I manually edit save file?

Yes, you can do it using any text editor. However, if you break the formatting the file won't be loading. The common error is that most text editors automatically split embedded svg into separate lines.

### Can I use the Generator offline?

Yes, you can with some limitations. The easiest way is to [install the PWA](Q&A#is-there-a-desktop-version). Make sure you open the App when you have a connection so that all required files can be cached on your machine. After that PWA should work offline.

Another option is to serve the tool locally. Download the [source code](https://github.com/Azgaar/Fantasy-Map-Generator/archive/refs/heads/master.zip) and unzip _all files_ from the downloaded archive. Then you need to run a local web-server, follow [the instructions](Run-FMG-locally).

Please note that some functions like alternative Fonts or Styles are not available offline.

### Is there a desktop version?

Modern technologies allow any web-application to be turned into a desktop application that supports all features of the web app. There are 3 options supported:

- Chrome App. A shortcut app can be created in Chrome in just three clicks without installation: `Settings => More Tools => Create Shortcut...`.
- PWA. Chromium-based browsers (Chrome, Edge, etc.) in Windows will prompt you with a big button 'Install'. Just click on it and confirm the installation. The installed tool will be added to your desktop and opened automatically. You can also create a shortcut (app) manually, see [how to do it in Chrome](https://support.google.com/chrome_webstore/answer/3060053).

All versions require internet connection for the first run. Once app data is loaded to the memory, it may work offline with some limitations.

### Which browsers are supported?

The Generator should work in any modern browser except for Safari. The tool is developed using Chrome, so it has the best support. Firefox is reported to have worse performance. Edge 79 and later will also work. Latest Opera and Yandex Browser should work fine as well. Outdated browsers like Internet Explorer are not supported. Weird browsers like Brave may not work at all.

### Can I use the Generator on mobile?

You can, but I doubt you will enjoy the experience. The Generator GUI is not suitable for mobile devices and performance is subpar. In general, I would say mobile devices are not supported.

### What about non-English localization?

Localization is planned, but not ready from the coding side. Once my part with code support is done, I will inform the community to help me with actual translations. Preparation can take a lot of time as it's not a current priority.

### Can I embed the map to my website?

Yes, you can and it's easy to do. Please follow the guide [here](https://sites.google.com/view/fantasy-map-generator-embedded/home).

### What does _Azgaar_ mean?

It's my nickname, it has no meaning. The name of the tool is _Azgaar's Fantasy Map generator_, pointing to me as creator. The short form is _FMG_.

### You've mentioned a Medieval Dynasty simulator. What is it?

It's my meta-project. A CK2-style genealogical game focused on genetics. Generally a wedding/dynasty breeding simulator (see the [screenshot](https://i2.wp.com/azgaar.files.wordpress.com/2018/02/screenshot-2018-2-9-dynasty-v0-11.png)). It's in pre-alpha and currently on hold, so no demo is available.

### How can I help to improve the Generator?

Just use it, log defects and suggest enhancements (please use the [issues](https://github.com/Azgaar/Fantasy-Map-Generator/issues) page for both cases). Share the Generator link within your community! Post on FB, Twitter etc.

We need a good video-tutorial. Please contact me if you have a video-blog and want to help.

Professional help from coders / UI designers is highly appreciated.

We also accept donations on [Patreon](https://www.patreon.com/azgaar).

### What is the team behind the project?

There is no team, but there is a Community. The tool is created by me, Azgaar. The community is based on our Reddit and Discord servers. Thanks for [contributors](https://github.com/Azgaar/Fantasy-Map-Generator/graphs/contributors), Community moderators and all our community members.

### Do you accept donations?

Yes, you can support the project on [Patreon](https://www.patreon.com/azgaar). If you don't want to pay monthly, you are able to decline the donation at any moment in time. Other donation platforms are not supported.

### Is there a place where I can discuss the Generator, share created maps and so on?

Yes, we have a dedicated [Discord server](https://discordapp.com/invite/X7E84HU) and [Reddit community](https://www.reddit.com/r/FantasyMapGenerator/). Both have a very active and supportive community.

### How can I contact you directly?

Please PM me on Discord or [send me an email](mailto:azgaar.fmg@yandex.com).

---

# Quick Start Tutorial

_The tutorial is for version 1.98_

![loading_screen](https://github.com/user-attachments/assets/154617dd-a690-4da5-a36c-187cd51a5c04)

Also watch the [video tutorial](https://www.youtube.com/playlist?list=PLtgiuDC8iVR2gIG8zMTRn7T_L0arl9h1C).

## Introduction

_Fantasy Map Generator_ is a tool that creates highly customizable fantasy worlds for you. It runs in a browser and does not require any software installation. It’s free and you can use created maps for any purposes including commercial.

The tool generates a new fantasy map on opening. The map is auto-generated, but it doesn’t mean that you cannot control the generation. To open the controls click on the arrow button at the top left corner of the screen or press <kbd>Tab</kbd>. You can either change the generation parameters and generate a new map, or edit the current map. You can also create a new map from scratch using paint brushes.

## Moving around

The map is generated fully zoomed out. Double click on the map to zoom into the clicked area. Double click again to zoom in even more. The maximum zoom level is 20x, but it can be increased in the options. To zoom out hold <kbd>Shift</kbd> and double click. Click and drag to move around the enlarged area.

The same operations can be performed using keyboard. Press <kbd>+</kbd> to zoom in, <kbd>-</kbd> to zoom out. Use <kbd>1</kbd>-<kbd>9</kbd> number keys to set an exact zoom level. Press <kbd>0</kbd> to reset zoom to default. Use arrows keys to move around.

## Map layers

By default the map shows the world’s political situation, but it’s not the only available preset. Open the first tab of the menu – _Layers_ – to change the preset. _Preset_ is a set of layers to be toggled on. There are number of presets available by default: political, cultural, religions, biomes, provinces, heightmap, places of interest, and other. You can either select one of the presets or use buttons below to display or hide a particular layer.

![menu](https://github.com/user-attachments/assets/70171c15-a44e-4460-9e33-4f4eed7f1b2a)

To make it simple each layer has a shortcut assigned. Hover the mouse over the layer button to see a tooltip showing what the button does and which key is assigned to it. Check out [the Hotkeys](https://github.com/Azgaar/Fantasy-Map-Generator/wiki/Hotkeys) wiki-page to get a full list of shortcuts. Click on the plus button next to the presets list to save the current layers as a custom preset. On Generator reload the map preset will be defaulted to the the previously selected one.

Some layers may not be visible if an upper (top) layer is toggled on. You can drag the layer button to change the layer's order.

## Styling and filters

There are cases when you may want to see several full map-covering layers simultaneously. The only way you can do it is to make the upper layer partially transparent. Open the _Style_ and choose the required layer from the _Select element_ drop-down list and change the opacity slider. Note that this may not work if clipping is enabled.

The same approach is used for styling all map elements. For example if you want to change the color of state borders, select _Borders_ element, then border group (type) and click on the color box to set a new color. Here you can also change border stroke width, its style and apply a visual filter. Different map elements have different styling controls. To restore the default style to all elements click on the counter-clockwise arrow next to the elements list.

![style_borders](https://github.com/user-attachments/assets/7e47b11a-259d-400f-bd83-a5a919bffc41)

The _Toggle global filters_ section below allows to apply a color filter to the whole map.

## Performance tips

Some users report performance issues on map dragging and zooming. These are number of tips that will allow you to avoid problems with performance.

- Toggle off layers you are not working on. Especially take care of the _Relief Icons_ layer – it’s the most resource-demanding one.
- If you have a wide screen, open the Generator in a separate browser window, make it _much_ smaller (about 900 x 560 pixels) and re-generate the map. Then zoom in to see the map in detail. It will reduce the rending area and drastically improve the performance.
- Generate maps with _Points number_ = 10K. Points (cells) number highly affects performance.
- Toggle off map and element filters.
- Close all irrelevant browser tabs and applications.
- Use the top-edge browser. As for now the best performance is observed in _Chrome_. _Firefox_ is also pretty fast, but there are some minor UI issues specific to this browser. Other browsers may not be supported.

## Saving the map

If you like the current map you probably want to **Save** it for a later use. There are 3 saving options:

- _Save to machine_. Download `.map` file that stores all the map data including your manual changes to your machine. The file then can be directly loaded into the Generator. It's highly recommend to save the map you are working on at least every 30 minutes.

- _Save to dropbox_. Log into your Dropbox account and save the `.map` file there.

- _Save to browser_. Save the `.map` file to the browser storage. Then it can be loaded as a fully-functional map. Press <kbd>F6</kbd> for a quick save and <kbd>F9</kbd> for a quick load. Please use it only for a quick safe, browser storage can be purged from time to time and you can lose your progress. Always keep `.map` file saved on your machine.

The only reliable method is to have a `.map` file saved on your machine, preferably with multiple copies on Cloud or other machines in case of your machine collapse. FMG doesn't store any of your data and cannot restore the map if you lost the file!

![save_options](https://github.com/user-attachments/assets/b04fb365-7169-4179-88f2-98c292a7f58d)

There are also a few ways how you can get the map _image_ or map _data_. Click on **Export** and use one of the options there. Please note that image (png, svg, or jpeg) is not a fully-functional `.map` file and cannot be loaded back to the tool. Consider it as a screenshot.

## Generation and UI settings

Let’s move to the settings overview. Open the Menu and click on the _Options_ tab. There are multiple options split into 2 categories. Map generation options require a new map to be applied. Generator settings are getting applied immediately on change.

![options](https://github.com/user-attachments/assets/65fefee6-cdc7-439f-83e2-374c21f9a455)

- _Canvas size_: the size of the map in pixels. There is no way to change the map size after the map generation and it's always advised to use the default value. The button on the left resets the size back to the default.
- _Map seed_: a number defining random values generation. If options and map size are the same, two maps generated with the same seed will be exactly the same. The seed cannot store user’s changes, so please don’t use it as a save function. The small button on the left allows to browse through generated seeds and restore previous maps if map options where not locked.
- _Points number_: the number of generated points and cells. The more points are generated, the more detailed is map. Points number highly affects performance, so the only recommended value is 10K.
- _Map name_: the name of the map or the world. Click on button with arrows to re-generate the name.
- _Year and era_: current year of the world and the era name. Used for some text generation.
- _Heightmap_: template to be applied on heightmap generation. Basically a type of the landmass: high island, continents, archipelago etc. You can create your own template or change the existing in the _Template Editor_ (part of the _Heightmap Editor_ tools).
- _Cultures number_: the number of cultures to be generated. Cultures can be edited via the _Cultures Editor_.
- _Cultures set_: a set of cultures to be used for map generation. Can be a list of real-world-like or fantasy cultures.
- _States number_: the number of countries (_states_) to be generated. States can be edited via the _States Editor_.
- _Provinces ratio_: the percentage of burgs that will have their own province. Provinces are sub-units of states and can be edited via the _Provinces Editor_.
- _Size variety_: defines how much states area should be different. The lower value, the more uniform state areas are.
- _Growth rate_: defines how far states and cultures will expand into neutral lands on generation. The lower value you have, the more lands will stay politically neutral.
- _Towns number_: the number of towns to be generated. Towns are non-capital settled areas (_burgs_). If there is not enough suitable space to put a requested number of towns, the Generator will place only a limited number of towns.
- _Religions number_: the number of religions to be generated. Controls only the number of organized religions and cults. Religions can be edited via the _Religions Editor_.
- _State labels_: defines whether the generator should render full state names or can use short variants where there is not enought space.

Generator settings:

- _Interface size_: size of the control panes. If the GUI size is too small, please also check out browser's zoom level (<kbd>Ctrl +</kbd>, <kbd>Ctrl -</kbd>).
- _Tooltip size_: size of the tooltips displayed at the bottom of the screen.
- _Theme color_: main color of the control panes.
- _Transparency_: opacity of the control panes.
- _Autosave interval_: number of minutes the map should be auto-saved to browser memory. Set to `0` to disable the autosave.
- _Onload behavior_: define what should be done when Generator is opened: a new map generated or a previously saved map auto-opened.
- _Speaker voice_: select voice to speak burg and other names. Voice generation functionality if provided by browser.
- _Emblem shape_: defines shield shape used during emblems generation.
- _Zoom extent_: minimal and maximal zoom levels. Click on the button on the right to restore the default values.
- _Rendering_: set map rendering quality. Best quality can reduce the map performance.
- _Language_: load Google Translator and select a language to translate the tool. Translations can cause issues with saving.

There is also the _Reset to defaults_ button. The button cancels all user changes and refreshes the page.

## Climate configuration

Click on _Configure World_ to set up map position on a globe and climate. Toggle biomes, precipitation or temperature layers on to see how configuration changes affect the map.

![configure_world](https://github.com/user-attachments/assets/f36932b0-db18-4633-ad5b-0fe475afc44e)

The Globe on the right shows relative position of the map as a brighter area. The map position is calculated based on two inputs: _Map size_ and _Latitudes_. The North is always on the top, the West is on the left and so on. Map size defines a relative size of the map to the whole world. With 100% size the generated map will cover all the world; with 50% - half of the world and so on. If the size is not 100%, the map can be shifted towards North or South. The shift is controlled by the _Latitudes_ input. With Latitudes equal to 0 the map North edge will be at the North pole, with 100 - South edge will be on the South pole, with 50 - the world equator will lie through the center of the map.

The map latitudes affects the temperature gradient and winds applied to the map. You can change the temperature on Equator and Poles and it will be re-calculated based on map position on a globe. To change winds use the arrows on the right. Please note that wind is also latitude-dependent.

The _precipitation_ value defines how much vapor clouds can bring and spread across the map. The bigger the precipitation value, the greater number of rivers and fewer deserts you get. With zero precipitation the entire world will be an unlivable desert.

## Heightmap editing

If you don’t like the heightmap generated automatically, you may edit it or even create a new one from the scratch. Open the _Tools_ tab in the menu and click on _Heightmap_. Now you need to select an edit mode. All secondary data (rivers, burgs, states, markers and so on) depend on the heightmap, so changing the height manually will break the default generation logic. The best practice here is to use the _Erase_ mode, create the heightmap you want and let the system to re-generate the secondary data. It will remove all the changes made for burgs rivers and so on, but it will guarantee the smooth generation.

![edit_heightmap](https://github.com/user-attachments/assets/4f1f890a-31ab-4c5d-a4b1-38824ee2f4c0)

If you want to perform just a minor change, select the _Keep_ mode. You won’t be able to change the coastline, but all the data, including manually placed relief icons and rivers, will be kept as it is. So it’s more like a visual change without changing the underlying data.

If you have done some changes and still need to amend the coastline, you may use the _Risk_ mode. As the title says, it’s not safe and can cause some issues. So please use this mode only if you really need to.

Heightmap editing process has multiple build-in tools. These tools won’t be covered in this tutorial, just note that they allow you to use brushes to "paint" the map, to edit and apply heightmap template and to convert any image into a heightmap.

![paint_brushes](https://github.com/user-attachments/assets/dab037a3-797e-447a-ab96-6c99af1342d9)

## Customization tools

The Heightmap Editor tools are not the only available ones. From the same _Tools_ tab you can also open Biomes, States, Provinces, Diplomacy, Cultures, Namesbase, Zones, Religions, Burgs, Units and Notes editors. All these Editors work in a different way and won’t be covered in this tutorial.

The same tab contains tools to re-generate map elements. If, for example, you have added and moved some burgs, the burgs won’t be connected with roads anymore. You can click on initiate routes regeneration to get them connected. You can also re-calculate State Labels positions, relief icons, population, rivers, burgs and states.

The next _Tools_ section allows adding elements like burgs, labels, rivers, routes and markers. Select the tool and then click on the map to add an object. Hold <kbd>Shift</kbd> and click multiple times to add several objects.

The last section, which is collapsed by default, provides some info about the map cell your cursor is over.

## Editing map elements (labels, rivers, roads etc.)

Individual map elements such as rivers, routes, relief icons, labels, markers, burgs and labels can be edited on mouse click. The editor screen that is opened is different for each element and it’s also out of this tutorial's coverage.

![label_editor](https://github.com/user-attachments/assets/37e706d1-7674-4c81-abc4-0be166c85d5b)

## Points of contact

That was a brief overview of the main Generator elements and approaches. It’s not full by any meaning and just serves as a starting point. You can always find more details and help on our supportive [Discord server](https://discordapp.com/invite/X7E84HU) and [Reddit community](https://www.reddit.com/r/FantasyMapGenerator).

---

# Resources: spread functions

# 🚧 Under construction

Resource **spread models** are applied to check whether resource can or cannot be placed in a cell. Fantasy Map Generator allows to create custom spread models, but it requires some understanding on how models work. Resources are generated before states and cultures, so models are built on top of geographical data. Once model is changed, you need to regenerate all resources.

# Technical info

Technically spread models are expressions evaluated for each cell to return `true` or `false`. If `true` is returned, the resource can be placed in the cell. The expressions are valid JS functions, you can use logical operators (`!` for not, `||` for OR, `&&` for and, etc.) and other features.

The options are limited to a number of build-in functions. These functions make models syntax easier to read and write:

- `random(number)`: percentage of true, e.g. `50` will return true in 50% of cases
- `nth(number)`: true for each only n-th cell: 1st, 2nd, 3rd and so on. For example `nth(2)` skips 1/2 (50%) of cells and `nth(5)` skips 4/5 (80%) of cells.
- `habitable()`: true if biome habitability is greater than 0
- `habitability()`: check against biome habitability. Always true for habitability `>=100`, false for `0`, skips 50% of cells if habitability is `50` and so on
- `elevation()`: check against cell's height - the higher cell is, the greater chance is. If you need resource to be more frequent in lower elevation areas, than just negate the function: `!elevation()`
- `biome(biomeId, biomeId, ...)`: check against list of biome ids, see below to get biome id reference
- `minHeight(number)`: true if cell height >= number. Number is in range `[0-100]`, where `0` is deep ocean and `20` is minimal land elevation
- `maxHeight(number)`: true if cell height <= number
- `minTemp(number)`: true if cell temperature (in Celsius) >= number
- `maxTemp(number)`: true if cell temperature <= number
- `shore(ringId, ringId, ...)`: check against distance to the closest shoreline. `1` is land cells next to water (coastline), `2` - next land ring, `-1` - water cells next to land (shallow water), `-2, -3, ...` - deeper water cells
- `type(string, string, ...)`: check against cell type. Types of all water cells connected to map border is `ocean`, lake types are `freshwater`, `salt`, `sinkhole`, `frozen`, `lava` and `dry`. Land types are defined not so clear, so I don't recommend to use them. In any case land types are `continent`, `island`, `isle` and `lake_island`
- `river()`: true if there is a river in the cell

### Biomes ids

0: Marine;
1: Hot desert;
2: Cold desert;
3: Savanna;
4: Grassland;
5: Tropical seasonal forest;
6: Temperate deciduous forest;
7: Tropical rainforest;
8: Temperate rainforest;
9: Taiga;
10: Tundra;
11: Glacier;
12: Wetland.

These are the default biomes. To get actual ids run `biomesData.name.map((n,i) => i+". "+n)` in FMG console (F12).

# Examples

Let's say we want a resource to be generated in hot and highly elevated areas. If altitude is medium, let it also be allowed, but pretty rarely. The model will be something like `minTemp(15) && (minHeight(70) || minHeight(40) && nth(5))`.

Another usual case is when we want resource frequency vary based on biomes. In this case you need to join multiple `biome()` functions like `biome(1) && (biome(2) && nth(2)) && (biome(3) && nth(3))`.

Check the build-in models below to get the gist.

### Built-in models

- Deciduous_forests: `biome(6, 7, 8)`
- Any_forest: `biome(5, 6, 7, 8, 9)`
- Temperate_and_boreal_forests: `biome(6, 8, 9)`
- Hills: `minHeight(40) || (minHeight(30) && nth(10))`
- Mountains: `minHeight(60) || (minHeight(40) && nth(10))`
- Mountains_and_wetlands: `minHeight(60) || (biome(12) && nth(8))`
- Headwaters: `river() && minHeight(40)`
- Biome_habitability: `habitability()`
- Marine_and_rivers: `type("ocean", "freshwater", "salt") || (river() && shore(1, 2))`
- Pastures_and_temperate_forest: `(biome(3, 4) && !elevation()) || (biome(6) && random(70)) || (biome(5) && nth(5))`
- Tropical_forests: `biome(5, 7)`
- Arid_land_and_salt_lakes: `type("salt", "dry") || (biome(1, 2) && random(70)) || (biome(12) && nth(10))`
- Hot_desert: `biome(1)`
- Deserts: `biome(1, 2)`
- Grassland_and_cold_desert: `biome(3) || (biome(2) && nth(4))`
- Hot_biomes: `biome(1, 3, 5, 7)`
- Hot_desert_and_tropical_forest: `biome(1, 7)`
- Tropical_rainforest: `biome(7)`
- Tropical_waters: `shore(-1) && minTemp(18)`
- Hilly_tropical_rainforest: `minHeight(40) && biome(7)`
- Subtropical_waters: `shore(-1) && minTemp(14)`
- Habitable_biome_or_marine: `shore(-1) || habitable()`
- Foresty_seashore: `shore(1) && biome(6, 7, 8, 9)`
- Boreal_forests: `biome(9) || (biome(10) && nth(2)) || (biome(6, 8) && nth(5)) || (biome(12) && nth(10))`
- Less_habitable_seashore: `shore(1) && habitable() && !habitability()`
- Less_habitable_biomes: `habitable() && !habitability()`
- Arctic_waters: `biome(0) && maxTemp(7)`

---

# River Editor

_River Editor_ is aimed to individually edit existing river or create a new river. To open the Editor click on any river.

To change a river course drag red points (river key points). Close the Editor when customization is done.

---

# Editing Run FMG locally

To run the Generator locally you need to download its files and start a local web-server.

## Download

Open [Releases](https://github.com/Azgaar/Fantasy-Map-Generator/releases) page, select the latest available version and click on _Source code (zip)_. Unzip _all files_ from the downloaded archive.

## Live Server

If you are a developer you probably don't want to restart the server manually each time you make a change to the code. So you need a web-server with live reload feature. If you are VS Code user, I recommend to install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension. It runs a local server in one click and automatically restarts it on file save.

### Python server

The easiest way to run a local web-server is to use Python's `http.server` module.

1. Install Python. If you are using Linux or macOS, it should be available on your system already. If you are a Windows user, you can get an installer from the [Python homepage](https://www.python.org/downloads). On the first installer page, make sure you check the "Add Python 3.xxx to PATH" checkbox.

2. If you are Windows user, run the `run_python_server.bat` file in the Fantasy Map Generator folder. It will start the web-server and automatically open the Application in Chrome. You can edit the `.bat` file in any text editor if you want to use a different browser or host.

3. For other systems open the terminal and print a `python -m http.server 8000` command. Then open `http://localhost:8000/` in browser.

### PHP server

1. Install PHP (version 5.4 and higher) from package repository or build from sources. If you are a Windows user, you can get an zip from the [PHP website](https://windows.php.net/download/). Don't forget add path to `php` executable to `PATH` variable.

2. If you are Windows user, run the `run_php_server.bat` file in the Fantasy Map Generator folder. It will start the web-server and automatically open the Application in Chrome. You can edit the `.bat` file in any text editor if you want to use a different browser, host, port or set path to PHP (by default is globally defined in Windows `PATH` variable).

3. For other systems open the terminal and print a `php -S localhost:3000` command. Then open `http://localhost:3000/` in browser.

---

# Scale and distance

The scale of a map refers to the ratio between a distance on the map in pixels and the corresponding distance in the world. In Azgaar's Fantasy Map Generator, you can customize this scale to fit the needs of your project.

The map generator operates on a scale that you can adjust through the `Units` settings. The scale impacts the way distances are measured and displayed on the map. This is used for states area, routes, zones and the size of the world.

## Units editor

The Units settings appear following the path Menu → Tools → edit Units or using the shortcut `SHIFT` + `Q`. Once you open the Units Editor, you see several options.

- Distance
- Altitude
- Temperature
- Population

In the icons at the bottom of the Units editor are more options:

- Linear ruler on the map
- Curve ruler, opisometer
- Route opisometer
- Planimeter
- Remove all rulers. A popup appears that reads: Are you sure you want to remove all placed rulers?
  If you just want to hide rulers, toggle the Rulers layer off in Menu. And buttons remove and cancel.
- Restore default unit settings. Set values to default but don't remove rulers.

### Distance

Here you can select the unit your maps are going to use. You can select from:

- Mile (mi). This is a land mile
- Kilometer (km)
- League (lg) this is a land league. Like all units, it will be used in all distances in the map, Whether sea or land
- Versta (vr). Old Russian length unit
- Nautical mile (nmi)
- Nautical league (nlg)
- Custom name

Area unit name is just to customize the name of that unit.

### How to add a distance unit

When you click on "custom name" a popup appears A popup will appear that says "Provide a custom name for a distance unit" and has an input field that says "type a text". Type in the name of your unit and press confirm. If you want to exit this popup without saving, press cancel.

### How to change the scale of your map

The value who decides how big is your map is the ratio between pixels and your distance unit. By default this ratio is: "1 pixel is 3 km" but you can change it on the slider. Accepted values are from 0.01 where your whole map is only a few tens of units (10-20 units) to 19.91. that makes your map a few something units.

### Altitude

The altitude section has a dropdown to select the name of the unit used for height and a slider. This measures sea depth, mountains, lake depth and more. When you click on layers preset → heightmap, the map tooltip will show the heights in the unit that you choose in altitude.

The dropdown has these units to choose from:

- Feet (ft)
- Meters (m).
- Fathom (f).
- Custom name. It opens a pop with an input text field. Write your unit name and click confirm.

The height exponent goes from 1.5 to 2.2 and affect higher numbers more. The default value of height exponent is 1.8. Here is an example of heights with different exponents.

- Island coast. Exp: 1.5 → 11 m. 1.8 → 25 m. 2.2 → 34 m. An average of that cell above the sea level.
- Central lands. Exp: 1.5 → 225 m. 1.8 → 665 m. 2.2 → 2819 m.
- High mountain. Exp: 1.5 → 743 m. 1.8 → 2785 m. 2.2 → 16233 m.

These heigths can vary with each map and place and can be modified on the heightmap editor too.

### Temperature

It lets you choose the temperature unit.

- Degree Celsius (°C)
- Degree Fahrenheit (°F)
- Kelvin (K)
- Degree Rankine (°R)
- Degree Delisle (°De)
- Degree Newton (°N)
- Degree Réaumur (°Ré). The melting and boiling points of water are defined as 0 and 80 degrees.
- Degree Rømer (°Rø). The freezing point of pure water 7.5 degrees and the boiling point of water as 60 degrees.

### Population

Here you can change the amount of people and population ratio live in the map.

- 1 population point corresponds to... by default 1000. Min 10. Max 10000. This represents how many people are in the map. Any number of population in the map is represented in "population points" not in exact number of people.
- Urbanization rate. Burg population relative to all population. This does not modify the total population or decrease rural population. It only increases the people at burgs and makes the percentage of people living at burgs higher. By default: 1. Min: 0.01. Max: 5.
- Urban density. Average people per building in medieval fantasy city generator. By default: 10. Min: 1. Max: 200.

## Rulers

Click on a ruler to delete it or the button with a bin to remove all rulers.

### Linear rulers.

When you click on the ruler icon a new linear ruler is created in the middle of the screen.

- Drag the whole ruler clicking on the label and holding.
- Extend the ruler clicking on the dot at one of the two ends of the ruler. Hold the mouse button to extend and move the mouse to control the direction.
- Click anywhere inside the ruler to create a ruler anchor point. A circle appears at that anchor point to indicate it. Even if you move the ruler at the edges, it will remain fixed at the anchor point. This allows you to measure contours that are not straight. Click on the same point to delete that anchor point.
- You can hold `CTRL` and drag the dot at one of the ends to keep the previous endpoint as an anchor point and extend the ruler.
- Click on the endpoint to delete that and return to previous, shorter length of the ruler.
- Click on an anchor point and hold `SHIFT` to move the anchor point only in horizontal or vertical of the origin point. This is called to "keep the axial direction".

### Curve ruler, opisometer

When you click the opisometer ruler you, your cursor changes to a cross. You need to click and hold on the origin of your ruler and draw the opisometer on the map.

- Meanwhile your cursor is a cross, you can hold `SHIFT` to disallow path optimization.
- Click and hold on the label or the line to drag the opisometer.
- Click on the circle at the end to extend the opisometer.

### Route opisometer

When you click the opisometer ruler you, your cursor changes to a cross. You need to click and hold on the origin of your ruler and draw the opisometer on the map.

- Draw the opisometer to follow the length of a route.
- Click on the circle at the end to extend or shorten the opisometer.

### Planimeter

When you click the planimeter button, your cursor changes to a cross. You need to click and hold on the origin of your ruler and draw the planimeter on the map.

- The planimeter will cover a 2D area.
- Meanwhile your cursor is a cross, you can hold `SHIFT` to disallow path optimization.
- A number with your "area unit" set in Units Editor → Distance → Area unit will appear. By default is "square" that appears as Km2.

## Routes

In Layers → Routes you can show/hide routes on your map. In Style → select element "routes" you can change the how your routes look. Clicking on a route show their "edit route" menu. This have name, group, length and a row of buttons.

- Name. Write on the text field to rename. Click speaker icon to make the software speak the name in audio. Click earth globe to create a new random name.
- Group. Open the dropdown list to choose from a group. Click on pencil to open "edit route groups". You can add, remove and style route groups.
- Length. Automatically calculated length appears in the length unit that you choose in units editor.
- Pin. Create a new route selecting route cells.
- Chain. Click to join the route to another route that starts or ends at the same cell.
- Broken chain. Click on a control point to split the route there.
- Graphic. Show the elevation profile for the route.
- Edit free text notes (legend) for the route.
- Lockpad. Click to lock route and prevent changes to it by regeneration tools.
- Remove route. Shortcut: `delete` key.

Clicking on menu → tools → edit → routes opens the "Routes Overview" menu. This shows a list of all routes on a table with sortable headers. Sorted by default by length. Click on a table header to change the sorting criteria. The table shows: Target icon, route name, route group, length, edit, lock and remove icons.

- Clicking on the target icon focus the map on the selected route.
- Edit icon opens the "edit route" menu.

At the botton there is:

- Total routes number.
- Average length of a route in distance units.
- Refresh the editor.
- Pin. Create a new route selecting route cells.
- Save routes-related data as a text file (.csv).
- Lock or unlock all routes.
- Remove all routes.

## Scale bar

At the bottom of your map you can see a scale bar. The scale bar is automatically adjusted to the distance unit you are using, the size of your world and the zoom you have. You can customize it at: Style → select element → Scale Bar.

## Grids

Grid can be used to refer to several things. Grids due to the voronoi cells that create the map or aesthetic grids that are placed on top. Voronoi cell grids are discussed on another page, here we will talk about the aesthetic grids that can be activated and deactivated. These are called "overlay grids" or "grid overlay".

to show/hide the grid go to: menu → layers → grid. You can customize the look and features of your grid in style → select element → grid. In style you can customize:

- **Opacity** of the grid. From 0 invisible to 1 visible.
- **Type**.
  - Hex grid (pointy). Hexagons. They are joined horizontally at the sides. This makes them have two vertical vertices.
  - Hex grid (flat). They are joined vertically at the sides. They show two horizontal vertices.
  - Square grid. Squares parallel to the ground.
- **Scale**. Default 1. Min: 0.1. Max: 10. Set the scale of the grid overlay.
- Next to the scale is the **distance** between grid cell centers in map scale. Default: scale 1 → 75 km. Min: 0.1 → 7.5 km. Max: 10 → 750 km.
- **Shift by axes**. The input fields are shifting in X or Y axis in pixels. By default 0, 0.
- **Stroke color** of the grid. Color selector.
- **Stroke width**. The line that draws the grid. By default 0.5. Min: 0. Max: 5.
- **Stroke dash**. Write number on the input field to select a pattern. The first number is the length of the line, the second is the length of the blank space. Linecap is the border of the line. Choose from inherit, butt, round, square for linecap.
- **Filter**. Apply predefined filters on the grid element.
- **Clipping**. No clipping (appear everywhere), clip water (appear only on land), clip land (appear only on water).

### Scale on the grids

All grids have the same distance between cell centers. By default this scale is 1. You can change both "grid scale" and units editor → "distance scale" (how many units is one pixel) to fit your needs.

![Hex grid flat](https://github.com/user-attachments/assets/1877f0bb-829b-49ff-9caa-66e41d450329 "Hex grid (flat)")
![Hex grid pointy](https://github.com/user-attachments/assets/39b63f0b-e809-4589-aecc-72e17b04566c "Hex grid (pointy)")
![Square grid](https://github.com/user-attachments/assets/f22b0835-5ced-4685-9091-8274365a7e4a "Square grid")
![Square 45 degrees](https://github.com/user-attachments/assets/65c99617-a85b-43a3-9909-8d9ab96e4389 "Square rotated 45 degrees")

In a square grid rotated 45 degrees, the cells are in diagonal.

![Square grid truncated](https://github.com/user-attachments/assets/1e4ccc9b-d78f-4e4c-ad0f-18c02c73b2ca "Truncated square grid")

In this truncated square grid, the distance between two squares and two octagons is the same.

![Square tetrakis](https://github.com/user-attachments/assets/47033791-91e5-4bf7-b947-f277def3712d "Tetrakis square grid")

In this tetrakis square grid, each square is divided by four smaller squares and eight triangles.

![Triangular horizontal](https://github.com/user-attachments/assets/5f259831-f034-46ca-ad59-bb6d9eb48434 "Triangle grid (horizontal)")
![triangular vertical](https://github.com/user-attachments/assets/5cfb88ca-713c-45ee-b00e-4cb5ed0684e4 "Triangle grid (vertical)")
![Trihehagonal grid](https://github.com/user-attachments/assets/ea12005a-8151-4f01-ad06-78ca76746698 "trihexagonal grid")
![Rhombille grid](https://github.com/user-attachments/assets/aee10502-6b49-419f-be8f-2bdc1fce00fd "Rhombille grid")

The distance of the ruler is the same on all images. To know more about these tilings, you can read the [wikipedia page on euclidean uniform tilings.](https://en.wikipedia.org/wiki/List_of_Euclidean_uniform_tilings).

---

# URL parameters

Here is a list of parameters you can add to URL in order to set generator options and control its behavior on load. It can be used to share exactly the same generated map without need to send the file, or even to show exact place on that map.

## Azgaar's Fantasy Map Generator parameters:

- `maplink` - open .map file from the provided URL, use like [`https://azgaar.github.io/Fantasy-Map-Generator/?maplink=https://dl.dropboxusercontent.com/s/xgs3y1awlokio7x/Atlas%20046.map`](https://azgaar.github.io/Fantasy-Map-Generator/?maplink=https://dl.dropboxusercontent.com/s/xgs3y1awlokio7x/Atlas%20046.map). Due to browser security restrictions, it works only for servers that allow CORS (e.g. DropBox, but not Google Drive)
- `seed` - map seed, actual map generation depends not only on seed, but also on options and map size. So to share the same map via a link you need to use it like [`http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default`](http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default). It will work only if codebase is the same, so till the next generator update. To share exactly the same map you have to send a .map file (e.g. via `maplink`)
- `options` - set to `default` to allow generator to ignore options set by user. It's required for sharing the same map, see above
- `width`, `height` - map canvas size in pixels
- `scale` - map zoom level, where `0.5` is 50% zoom, `1` is 100%, `2` is 200% and so on
- `x`, `y` - point coordinates that should be focused. Obviously works only if `scale` is greater than 1. Try [`http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default&scale=8&x=768&y=361`](http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default&scale=8&x=768&y=361)
- `burg` - burg id or name to focus on, works only if `scale` is greater than 1. Try [`http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default&scale=8&burg=2`](http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default&scale=8&burg=2)
- `cell` - cell if to focus on, Try [`http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default&scale=8&cell=1000`](http://azgaar.github.io/Fantasy-Map-Generator/?seed=123456789&width=1536&height=722&options=default&scale=8&cell=1000)

## Watabou's Medieval Fantasy City Generator parameters:

This parameters are used by MFCG and not intended to be set manually.

- `from`- if equals to `MFCG`, generator will consider the URL as coming from MFCG
- `size` - MFCG city size, equals to population point in FMG
- `coast` - `1` if city is on a coastline
- `port` - `1` if city is a port
- `river` - `1` if city is on a river

---

# User Interface

By default, when you open the software, a random map is generated (according to the saved, customizable settings).

This can be changed, so that when the software is opened, it will display the last saved map (instead of generating a new one).

What is shown to the user when opening the software, is the map (with a certain composition of layers that will be described later), its scale, as well as a button that allows opening the tab menu of fmg.

The first time the software is activated, the layers shown on the map are of the predefined layer group "Political Map".
The following times what will be active is the predefined group of layers that you selected last time.

# Controls

Understanding the different types of controls that make up the user interface can contribute to a general understanding of fmg.

For example, the paint brush button appears in many windows, and in all of them it has a similar meaning - opening the element in the design tab. Instead of explaining every time what this button does, in each separate window, you can describe the operation of the button once, for all the places where it appears.

## General controls

These elements appear in many places in the UI, for example in the various windows.

### Windows

Such as the various editors.

### Legends

A panel that displays a description of a certain element.

This is a non-closeable window. Instead, it closes automatically when the mouse cursor leaves the element to which the legend belongs.

For example, when going over a regiment, it will be displayed for example:

### Tables

Such as the tables that appear in the overview windows.

#### Features

##### Sorting

If the element has a numerical value, it will be possible to sort in ascending or descending order.

If the element has an alphabetical value, it will be possible to sort alphabetically, in ascending or descending order.

The sorting criterion will appear next to the column of the table, for example:

![image](https://github.com/user-attachments/assets/5e4e133e-01d7-4941-9ea0-cef7df0c6f84)

Here the biome column in the biomes table is shown, with the table sorted by biome names, in alphabetical order.

### Buttons

#### Adding an element

![image](https://github.com/user-attachments/assets/2ccbfb57-d902-436e-810f-c61acd5883d6)

#### Deleting an element

![image](https://github.com/user-attachments/assets/493af060-3d18-420a-9d07-2c32d26cb032)

#### Play an element name

![image](https://github.com/user-attachments/assets/5786f119-11dc-4024-9db8-8bcd65f580d4)

#### Opening a style editor for an element

![image](https://github.com/user-attachments/assets/3aa4cb9f-ae2e-47cd-866f-1e7763d6a9b3)

#### Focus on an element

![image](https://github.com/user-attachments/assets/f0e2a635-cfaf-46d9-b3f7-a5f830910091)

#### Opening a wiki guide for an element

![image](https://github.com/user-attachments/assets/7ef572ad-d0c6-42f5-b658-0bc8b6b8295e)

#### Lock buttons

![image](https://github.com/user-attachments/assets/8dc84a92-a4fb-4d86-a95e-936530493261)

If you want the next map to have a fixed value, and not regenerated, you can lock a certain value using a lock button. The button is found, for example, in the options tab, but also in some editors.

In general, whoever wants to generate a map with fixed characteristics, he should lock everything possible.

## Unique controls

### Map

The map, that can be presented in several view mode.

### Bottom label tooltip

When moving over a certain element (on the map or in the windows), a description of it will appear below.

In addition, if it is relevant, a description of what can be done with the control will appear.

In addition, if it is relevant, a shortcut for activating the control will appear.

For example, when going over the layer control of the rivers:

![image](https://github.com/user-attachments/assets/edc9e00f-70a0-4e6a-89cd-f8bf0545a0c3)

### Opening the tab menu

By clicking on the upper left triangle.

Or by the keyboard shortcut Tab.

![image](https://github.com/user-attachments/assets/9c405153-520b-4d16-8b3e-6e38064f4a1e)

### New map generators

By hovering over the upper left triangle, a new map generator command will open.
or by the shortcut F2.

![image](https://github.com/user-attachments/assets/544497f4-4fa1-4a8f-88c0-31a8fc8c7282)

## Zooming

There are several way to zoom:

Using the mouse by Double click,

Using the keyboard by Pressing + (plus) and - (minus), or press numbers (from 0-9, The higher the number, the higher its zoom level).

There is also support for touch screen and mouse pad.

## Moving on the map

By dragging the mouse, or by the navigation keys (right, left, up and down).

# Menu

FMG menu contain 5 tabs: Layers, Style, Option, Tools, About.

Any of the tabs contain the Lower Menu Section.

## Increasing the menu size

Increase by shortcut keys ctrl & +, and decrease by ctrl & -.

## Tabs shortcuts

### Layers Tabs shortcuts

Ctrl + click on a certain toggle will turn on the layer and will also navigate to the style editor tab configured for the element of the layer.

For example, if you click on the rivers layer with ctrl pressed, you will navigate to:

![image](https://github.com/user-attachments/assets/87cff977-2385-4626-a97c-6ec5dc59db6c)

## Lower section

The buttons on the Lower Menu Section is in any of the menu tabs.

## New Map

Generates a new map (according to the settings in the Options tab).

The map size depends on the screen size, as fmg tries to fill the entire screen. Alternatively, you can edit the resolution in the "options" tab.

## Export

Export the map to a certain format.

### Download image

There are several buttons for downloading images in different formats (svg, png, jpeg).

Note: Generator uses pop-up window to download files. Please ensure your browser does not block popups.

There is also a buttons to download a zip of several tiles (in a separate png file) of the map. After click the tile button, a new window will pop up, allowing the user to select number of Columns and Rows.

### Export to GeoJSON

There are several buttons for downloading some data in GeoJSON formats (data can be cells, routes, rivers, markers.

### Export To JSON

There are several buttons for downloading some data in JSON formats.

## Save

Saving a file in .map format, which contains the current state of the map, so that we can load it in the future using the load button.

### Save options

#### Machine

Save to local PC.

#### Dropbox

Saving to the dropbox account, if connected.

#### Browser

Saving to the browser's memory.

## Load

Loading a previously saved map.

The buttons correspond to those of the save window.

## Reset Zoom

Reset zoom to its initial position.

# Layers

Additional layers of information can be displayed on the map to enrich it.

A map without layers will be a completely white map, only showing the land on the surface of the ocean, and lakes if there are any.

Each layer is a toggle, which means that clicking on a certain layer will cause it to be displayed or removed from the map.

For example, the "Rivers" layer will display the rivers on the map. Clicking again on the rivers layer will hide them.

There are layers that cannot be represented together with others (For example, Heightmap and Biomes layers).

You can drag the different layers and thus change their display order.

When a certain layer is pressed, and you hover over some element in the map that is relevant to the layer, the lower label will display relevant information. For example, if the Heightmap layer is on, hovering over a cell will display its height.

In addition, you can press ctrl and a certain layer, to navigate to the style tab when it is configured to customize the display of the layer you clicked on. For example, if you click on the rivers layer, you will navigate to:

![image](https://github.com/user-attachments/assets/87cff977-2385-4626-a97c-6ec5dc59db6c)

## Layers classification

### Opaque layers

Texture, Heightmap, Biomes.

Each of these layers is drawn on the entire map in an opaque manner, that is, it comes at the expense of the other, so that they hide each other.

The one that will be displayed is the one that appears last in the order of the layers (to change, you can drag the layers and change their display order).

The other layers can hide parts of other layers, but they cannot hide the entire map.

Note: you can change the design of the above layers so that they are not opaque, this is in the style editor tab. But by default, the above layers are opaque.

### semi opaque layers

Religion, Cultures, States, States, Temperature.

These layers are drawn on the entire map, but they do not completely hide the above 3 layers, and they can work with each other without full hiding.

## Presets

A dropdown that displays a set of predefined layers.

For example, the group of layers called "political map", contains (and toggle) the layers of the states, borders, rivers, etc.

Available presets: Political map, Cultural map, Religions map, Provinces map, Biomes map, Heightmap, Physical map, Places of interest, Military map, Emblems, Pure landmass.

## Biomes

This layer shows the division of the land into biomes.

When this layer is on, when hovering a cell, its biome will be displayed in the tooltip. Biome is a term from the field of ecology that describes a large-scale ecosystem that is characterized by environmental conditions such as climate, the type of soil, and the flora and fauna that characterize it. For example, tropical rainforests, deserts, tundra, savannas, etc. Each of the biomes is characterized by a unique ecosystem of living species found in it naturally and in the environmental conditions that prevail there.

Default biomes:

- Tropical rainforest
- Tropical seasonal forest
- Hot desert
- Savanna
- Grassland
- Temperate rainforest
- Temperate deciduous forest
- Cold desert
- Taiga
- Tundra
- Glacier
- Wetland

In general, the biomes are derived from the topographical map (which can be edited in tools or options), and from the temperature (which can be edited in configure world).

## Icons

This layer shows the icons of the burgs. When this layer is on, when hovering a burg icon, an offer to edit it will be displayed, along with its name and its population will be displayed in the tooltip.

## Heightmap

This layer shows the topographical map. When this layer is on, when hovering a cell, its height will be displayed in the tooltip. A topographic map describes the height (relative to sea level) of the map at the various points (cells). All secondary data (rivers, cities, countries, markers, etc.) depend on the topographical map, so it is of great importance.

Each cell is associated with a height, which is a number between 0 and 100.

# Style tab

The style tab allows applying design settings, both in general to the entire map, and privately to certain elements, such as the color of elements, their degree of opacity, etc.

This is the final stage in the preparation of the map, after all its logic is ready.

After choosing an element for design (eg Anchor Icons or biomes), you can customize its design with a variety of controls.

For many of the elements there are the same design controls with the same functionality, below is their breakdown:

![image](https://github.com/user-attachments/assets/f4277f5d-e45a-440c-83f6-52f2e02cab71)

Sometimes, elements are divided into groups. For example, by default, lakes are divided into 6 groups: fresh water, salt water, dry, etc.

You can customize the display of each of the groups separately, for example, a freshwater lake can be drawn in blue, and a saltwater lake can be drawn in red.

Note that next to the group, there is a number that records how many elements there are from the group.

### Opacity

![image](https://github.com/user-attachments/assets/df451f6d-70f1-4ee7-9599-31f5f4839d23)

A slider that determines the degree of opacity of the element (value between 0 and 1).

If equal to 0, the color of the element will (mostly) be the color of what is below it, for example the color of the river will be the color of the land on which it flows (and if there is a layer that colors the land, it will be the army). Note that there are exceptions, for example for a lake, the color of the element will be the color of the sea.

### Shift by axes

Allows you to shift the layer on the x-axis and the y-axis.

### Filter

![image](https://github.com/user-attachments/assets/9ae43d89-f729-4ef2-b554-653d295afd5a)

Contains additional display settings on the element, according to a filter. Each filter has certain display settings.

For example, Blur 1 creates a light blur around the element, while Blur 3 creates a heavier blur, while Blur 10 completely blurs the element, effectively hiding it.

This is known as a filter, because you put a "lens" over the object that makes a visual effect on it, for example a blur, or a wrinkled look. It could be called "effect" (on the element).

Filters: none, Blur 0.2, Blur 1, Blur 3, Blur 5, Blur 7, Blur 10, Splotch, Blurred Splotch, Shadow 2, Shadow 0.1, Shadow 0.5, Outline, Pencil, Turbulence, Paper, Crumpled, Grayscale, Sepia, Dingy.

### Clipping

![image](https://github.com/user-attachments/assets/333f4c01-e7b6-4282-9518-b7f2da6433a2)

Dropdown that allows you to choose whether the layer will apply to the land, the sea, or both.

#### No clipping

The layer will apply to the entire map, including the sea and including the land.

#### Clip water

The layer will only apply to the land.

#### Clip land

The layer will only apply to the sea.

### Common style controls (Fill related)

#### Fill color

![image](https://github.com/user-attachments/assets/d4a949b7-2336-4ac7-a50f-fbc402be9bb7)

Sets the element's fill color.

### Common style controls (Stroke related)

Stroke is the line that surrounds the element.

#### Stroke color

![image](https://github.com/user-attachments/assets/7890332c-1d38-402a-b1c5-6967ee451142)

Sets the fill color of the stroke.

#### Stroke width

![image](https://github.com/user-attachments/assets/9b3cf590-42ec-4f4b-adce-952aae223698)

Slider that determines the width of the stroke.

The lower the value, the thinner the line will be, to the point of non-existence, and conversely, the higher the value, the thicker the line will be.

### Common style controls (Font related)

#### Font size

![image](https://github.com/user-attachments/assets/347bad04-5b45-451f-8f29-d6c58c0844db)

Determines the font of the text relevant to the element.

### Common style controls (dotted lines related)

#### Stroke dash

![image](https://github.com/user-attachments/assets/6d467a97-a379-4e5a-9d69-d775ab850b23)

This style control contains 2 parts of data.

The number determines the spacing between the dash marks that make up the dashed line.

Value 0 determines that there will be no spaces. The higher the value, the greater the distance between dashes.

The dropdown allows you to determine the appearance of the ends of the dash endcaps that make up the dashed line.

Butt – The dashes have no effect. The line ends exactly at the ends of its starting and ending points, without extending the line beyond these ends.

Square - There is a small addition along the line. More precisely, adds a rectangle at the end of the line, where the width of the rectangle is half the width of the line and the height is equal to the width of the line.

Round - Expand the line at its end using a semicircle, whose diameter is equal to the width of the line.

Inherit - takes the varian of its parent elements, basically a default value.

## Style presets

![image](https://github.com/user-attachments/assets/0fd99cd1-5e52-4d70-b137-379bda19ecb3)

Dropdown that allows you to choose a set of design settings for all elements.

Each of those sets defines its design settings for each of the elements, for example the ancient preset defines the texture of the land to be with the image setting to be ancient small, and the Clipping setting to be No clipping.

Available presets: Default, Ancient, Gloom, Pale, Light, Watercolor, Clean, Atlas, DarkSeas, Cyberpunk, Night, Monochrome.

**+ button** Allows you to add your own set of settings.

## Global filters

![image](https://github.com/user-attachments/assets/15994c62-1bed-4068-a99d-7d32008892a2)

Allows global filters to be applied to all map elements: Grayscale, Sepia, Dingy, Tint.

## Heightmap styling

Contains some common style controls and the following controls:

### Terracing slider

![image](https://github.com/user-attachments/assets/1f09d100-1fa4-484a-b00b-ee9d2f066720)

A slider that determines the Terracing power(value between 0 and 20), That determines the strength of the rating between the different colors.

The higher the value, the greater the separation between different colors, which gives the impression of a three-dimensional topographical map.

The above effect is achieved through the visual effect of giving a "shadow" to the height cells, so that it will appear that they are one on top of the other, or "creating terraces" in separate planes, each plane at its own height level.

### Reduce layers

![image](https://github.com/user-attachments/assets/340ae86e-74ec-417b-9c76-8c7caba850b6)

A slider that controls the reduction rate value (value between 0 and 10).

The elevation map consists of "elevation layers".

The reduction rate determines the number of displayed layers (inversely - the higher the reduction rate, the smaller the number of displayed layers).

The higher the number of layers, the more uneven the map will look.

The smaller the number of layers, the more uniform the map will look, when neighboring height cells will be the same color more likely.

Note: this does not affect the height of the map cells, only the display. If you hover neighboring cells on the map, you will see that although they appear to have the same height because of the color, the [Lower Menu Section](<https://github.com/Azgaar/Fantasy-Map-Generator/wiki/2.3.2-(Menu)-Lower-Section>) will show a different height.

Increasing the reduction rate value increases the performance of fmg.

### Simplify line

![image](https://github.com/user-attachments/assets/077c1f69-0f8a-4452-80a7-eb1ca3ff7203)

A slider that determines the value of the line simplification rate.

When this value is reset, the borders of the topographic map colors will be according to the borders of the map cells.

The higher the value of the line simplification rate, the more the borders of the colors of the topographic map will merge with each other, in such a way that the borders of the colors curve towards each other.

When the value of the line simplification rate is high, the height is illustrated in a better way, thanks to the illusion of depth that the effect gives. For example, the bright orange color is "above" the green color, and above it there is a ring of a darker orange color, and above it there are rings of army red, and above it there are rings of a dark burgundy color.

Increasing the value of the line simplification rate increases the performance of fmg.

### Line style

![image](https://github.com/user-attachments/assets/7f04c7e4-402f-495f-bd75-9c7668b839e5)

Dropdown that allows you to choose the line style of the "height cells". In this context, a "height cell" is a cell painted with a color that represents a certain height.

To understand what the slider does, you should turn on the cell layer.

#### Curved

In this view, the "elevation cells" extend approximately next to the map cells, so instead of extending exactly over the cell lines, the lines are rounded, to create a more natural look.

#### Linear

In this view, the "elevation cells" span just above the map cells.

#### Rectangular

In this view, the "elevation cells" extend approximately next to the map cells, so that instead of extending exactly over the cell lines, the lines are "squared", to create a more pixelated look.

### Color scheme

![image](https://github.com/user-attachments/assets/f727305e-9d04-4314-a84d-d8ad292b8f96)

Dropdown that allows changing the color scheme, so that each height will be represented in a different color according to the color scheme.

For example, in the default set (Bright), the dark burgundy color represents the highest cell, while in the natural set, the highest cell will have a white color. And so each height will have a slightly different color in the different color schemes.

Available color schemes: Bright, Light, Natural, Green, Olive, Livid, Monochrome.

# Map Settings

These settings are applied when you generate a new map.

- Canvas size: Map size in pixels. The button on the left resets the default size. Note: There is no way to change the map size after the map is created and it is always recommended to use the default value.

- Map seed: A number that defines the generation of random values. Every time you generate a new map, a new seed number is set here, which together with the size of the map and the options in the map settings menu, creates a random map. Please note – for 2 maps that have the same map size and the same options and the same number of seed, the maps will be identical. The button on the left side allows you to browse between seed values ​​of previous generators.

- Points number: A slider that defines the number of points (cells). The higher the number of points, the more detailed the map is, and is divided into more cells (where each cell is smaller), but on the other hand it negatively affects performance. In terms of performance, the recommended value is 10k. If your computer is powerful and you don't experience performance issues, choose a higher value until you experience performance issues.

- Map name: This will be the name under which the map will be saved. On the right, there is a button that allows you to replace with a new generator name.

- Year and era: The current year and the name of the current era. Don't affect a lot as there is no time simulation in the FMG.

- Heightmap: Opens the topographic map selector, which is intended for selecting a template intended for creating a topographic map. For example an archipelago, a group of continents, etc. You can create your own template, or modify the existing templates.

- Cultures: a slider that determines the number of cultures.

- Cultures set: Dropdown that contains the group of cultures that will be thrown into the map.

- States number: A slider that determines the number of countries.

- Provinces ratio: A slider that determines the percentage of burgs who have their own districts. A province is a sub-unit of a country, and can be edited in the province editor.

- Size variety: A slider that determines the degree of variation in the territory of the countries. The lower the value, the more uniform the country regions.

- Growth rate: A slider that defines how far countries and civilizations will expand into neutral lands after a generation. The lower its value, the more land will remain politically neutral.

- Towns number. A slider that determines the number of towns.

- Religions number: A slider that determines the number of religions and sects.

- States labels: Dropdown that defines whether full names or a shortened version of the state names are displayed when there is no room for the full name.

---

# Working offline

**Fantasy Map Generator** is a web-tool, but it can be run offline if previously downloaded. Download [the archive](https://github.com/Azgaar/Fantasy-Map-Generator/archive/master.zip) and unzip it to any folder. Then you need to run a local web-server, follow [the instructions](Run-FMG-locally).

_Known Limitations:_

- Additional Fonts are not available
- Additional Textures are not available

You can also try an **Electron desktop application** - download [an archive](https://github.com/Azgaar/Fantasy-Map-Generator/releases) for your architecture, unzip and run the _Azgaar's Fantasy Map Generator.exe_.
