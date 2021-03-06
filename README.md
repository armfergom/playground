Table of Contents
=================

- [Game Actions](#game-actions)
  - [generic:spin](#genericspin)
- [Game Events](#game-events)
  - [generic:availableActions](#genericavailableactions)
  - [generic:cascadesEnd](#genericcascadesend)
  - [generic:cascadesProgress](#genericcascadesprogress)
  - [generic:cascadesStart](#genericcascadesstart)
  - [generic:configuration](#genericconfiguration)
  - [generic:featureTriggeredWinnings](#genericfeaturetriggeredwinnings)
  - [generic:show](#genericshow)
  - [generic:spin](#genericspin)
  - [generic:symbolMovement](#genericsymbolmovement)
  - [generic:winnings](#genericwinnings)
- [Custom Types](#custom-types)
  - [CascadeState](#cascadestate)
  - [FreeSpinState](#freespinstate)
  - [MainState](#mainstate)
  - [PickerState](#pickerstate)
  - [Coord](#coord)
  - [Panel](#panel)
  - [Winning](#winning)
  - [Transition](#transition)


Game Actions
============

This section describes the actions that can be sent to the server and some examples.

generic:spin
------------

The message to send to the server to perform a spin of the panel.


### Fields

| Name      | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| betAmount | `long` | The panel bet to perform the spin with. |

#### Example

```
{"name":"generic:spin","gameSessionId":"","action":"{\"betAmount\":100}","type":"game-action"}
```

Game Events
===========

This section describes the events the server is able to produce and some examples.

generic:availableActions
------------------------

Message sent to the client after every action indicating what actions are allowed in the next round.

**This message is only sent during reconnections.**

### Fields

| Name             | Type           | Description                                       |
| ---------------- | -------------- | ------------------------------------------------- |
| availableActions | `List<String>` | The list of available actions for the next round. |
| state            | `String`       | The name of the current state.                    |

#### Example

```
{"availableActions":["generic:spin"],"state":"MAIN_CASCADES","type":"game-event","name":"generic:availableActions"}
```

generic:cascadesEnd
-------------------

Message sent to the client indicating that cascades have just ended.


### Fields

| Name      | Type           | Description                                         |
| --------- | -------------- | --------------------------------------------------- |
| stateName | `String`       | The name of the cascades state that has just ended. |
| context   | `CascadeState` | The state with which the cascades ended.            |

#### Example

```
{"stateName":"MAIN_CASCADES","context":{"bet":100,"panel":{"reels":[{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["NITRO"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"winnings":[],"cascadeLevel":1,"multiplier":1},"type":"game-event","name":"generic:cascadesEnd"}
```

generic:cascadesProgress
------------------------

Message sent to the client indicating the progress of the cascades phase. Sent after each cascade spin.


### Fields

| Name      | Type           | Description                                 |
| --------- | -------------- | ------------------------------------------- |
| stateName | `String`       | The name of the cascades state in progress. |
| context   | `CascadeState` | The current state of the cascades phase.    |

#### Example

```
{"stateName":"MAIN","context":{"bet":100,"panel":{"reels":[{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["NITRO"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"winnings":[],"cascadeLevel":1,"multiplier":1},"type":"game-event","name":"generic:cascadesProgress"}
```

generic:cascadesStart
---------------------

Message sent to the client indicating that cascades have just started.


### Fields

| Name      | Type           | Description                                           |
| --------- | -------------- | ----------------------------------------------------- |
| stateName | `String`       | The name of the cascades state that has just started. |
| context   | `CascadeState` | The initial cascades state for the triggered phase.   |

#### Example

```
{"stateName":"MAIN_CASCADES","context":{"bet":100,"panel":{"reels":[{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"winnings":[],"cascadeLevel":0,"multiplier":1},"type":"game-event","name":"generic:cascadesStart"}
```

generic:configuration
---------------------

Message sent to the client in every connection with relevant engine configuration information.

**This message is only sent during reconnections.**

### Fields

| Name              | Type                | Description                                            |
| ----------------- | ------------------- | ------------------------------------------------------ |
| minBet            | `long`              | The minimum bet allowed in the game.                   |
| maxBet            | `long`              | The maximum bet allowed in the game.                   |
| symbols           | `Set<String>`       | The set of available symbols in the game.              |
| symbolMultipliers | `Map<String, Long>` | A prize multiplier associated with each symbol if any. |

generic:featureTriggeredWinnings
--------------------------------

Message sent to the client indicating that a feature has been triggered because there were certain winnings.


### Fields

| Name      | Type            | Description                                                   |
| --------- | --------------- | ------------------------------------------------------------- |
| stateName | `String`        | The name of the triggered state.                              |
| winnings  | `List<Winning>` | The list of winnings that caused the feature to be triggered. |

#### Example

```
{"stateName":"MAIN_CASCADES","winnings":[],"type":"game-event","name":"generic:featureTriggeredWinnings"}
```

generic:show
------------

Message sent to the client in every connection with information about the current state of the game. Should be used for recreate the last state the player had in the game.

**This message is only sent during reconnections.**

### Fields

| Name            | Type                         | Description                                                                          |
| --------------- | ---------------------------- | ------------------------------------------------------------------------------------ |
| state           | `String`                     | The name of the current state.                                                       |
| stateFeature    | `FeatureType`                | The type of feature the current state represents: free spins, cascades, picker, etc. |
| panel           | `Panel`                      | The last panel shown in the game.                                                    |
| betAmount       | `long`                       | The last bet amount used.                                                            |
| winnings        | `List<Winning>`              | The list of last winnings produced.                                                  |
| mainState       | `MainState`                  | The current state of the game in the MAIN state.                                     |
| freeSpinsStates | `Map<String, FreeSpinState>` | For each active free spin state name, the current game state.                        |
| cascadeStates   | `Map<String, CascadeState>`  | For each active cascade state name, the current game state.                          |
| pickerStates    | `Map<String, PickerState>`   | For each active picker state name, the current game state.                           |
| markersApplied  | `Map<String, List<Coord>>`   | For each marker that are applied at this moment, the affected coordinates            |

generic:spin
------------

Message sent to the client after every spin action with the result of the spin.


### Fields

| Name      | Type    | Description                                                                                                                                     |
| --------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| betAmount | `long`  | The bet amount used in the last spin action.                                                                                                    |
| panel     | `Panel` | The panel that have been evaluated and used to generate the winnings.                                                                           |
| basePanel | `Panel` | Base panel that was generated before applying any kind of modifiers, it will be identical to the field panel if there was no modifiers applied. |

#### Example

```
{"betAmount":100,"panel":{"reels":[{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"basePanel":{"reels":[{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"type":"game-event","name":"generic:spin"}
```

generic:symbolMovement
----------------------

Message sent to the client indicating that certain symbols have been moved from one place to another. The order in which this event is sent is important as it can happen before and after the evaluation of the panel.


### Fields

| Name          | Type               | Description                                           |
| ------------- | ------------------ | ----------------------------------------------------- |
| originalPanel | `Panel`            | The panel before the symbols are moved.               |
| newPanel      | `Panel`            | The resulting panel after the symbols are moved.      |
| transitions   | `List<Transition>` | The set of symbol movements that occurred.            |
| componentName | `String`           | The name of the panel modifier that has been applied. |

#### Example

```
{"originalPanel":{"reels":[{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"newPanel":{"reels":[{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["NITRO"]},{"symbols":["GEAR"]},{"symbols":["ROCKET"]},{"symbols":["ROCKET"]}]},"transitions":[{"from":null,"to":{"reel":0,"row":0}},{"from":null,"to":{"reel":1,"row":0}},{"from":null,"to":{"reel":2,"row":0}},{"from":null,"to":{"reel":3,"row":0}}],"componentName":"AvalancheReelsModifier","type":"game-event","name":"generic:symbolMovement"}
```

generic:winnings
----------------

Message sent to the client when winnings have been produced.


### Fields

| Name                | Type            | Description                           |
| ------------------- | --------------- | ------------------------------------- |
| winnings            | `List<Winning>` | The list of produced winnings.        |
| monetaryWinningsSum | `long`          | The total monetary winnings produced. |

#### Example

```
{"winnings":[{"winningType":"LINE","prizeType":"MONEY","winnings":100,"mainSymbol":"GEAR","occurrences":4,"id":"1-0-3","symbols":["GEAR","GEAR","GEAR","GEAR"],"coords":[{"reel":0,"row":0},{"reel":1,"row":0},{"reel":2,"row":0},{"reel":3,"row":0}]}],"monetaryWinningsSum":100,"type":"game-event","name":"generic:winnings"}
```

Custom Types
============

This section types used in the above actions and events.

CascadeState
------------

The last state of the game in a given cascade state.

### Fields

| Name         | Type            | Description                                             |
| ------------ | --------------- | ------------------------------------------------------- |
| bet          | `long`          | The last bet amount used in the cascade game.           |
| panel        | `Panel`         | The last panel shown in the cascade game.               |
| winnings     | `List<Winning>` | The list of last winnings produced in the cascade game. |
| cascadeLevel | `int`           | The current cascade level in a given cascade game       |
| multiplier   | `long`          | The last prize multiplier used in the cascade game.     |

FreeSpinState
-------------

The last state of the game in a given free spin state.

### Fields

| Name                         | Type                             | Description                                                                      |
| ---------------------------- | -------------------------------- | -------------------------------------------------------------------------------- |
| bet                          | `long`                           | The last bet amount used in the free spin game.                                  |
| panel                        | `Panel`                          | The last panel shown in the free spin game.                                      |
| winnings                     | `List<Winning>`                  | The list of last winnings produced in the free spin game.                        |
| multiplier                   | `long`                           | The last prize multiplier used in the free spin game.                            |
| freeSpinRemaining            | `int`                            | The number of free spins to complete the free spin game.                         |
| symbolsInPanelsStopCondition | `List<Pair<Set<String>, Coord>>` | The symbols to appear in given coordinates so that the free spin game completes. |

MainState
---------

The last state of the game in the MAIN state.

### Fields

| Name       | Type            | Description                                          |
| ---------- | --------------- | ---------------------------------------------------- |
| bet        | `long`          | The last bet amount used in the MAIN game.           |
| panel      | `Panel`         | The last panel shown in the MAIN game.               |
| winnings   | `List<Winning>` | The list of last winnings produced in the MAIN game. |
| multiplier | `long`          | The last prize multiplier used in the MAIN game.     |

PickerState
-----------

The last state of the game in a picker state.

### Fields

| Name                       | Type                                             | Description                                                                                         |
| -------------------------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------- |
| bet                        | `long`                                           | The bet amount with which the picker game was reached.                                              |
| currentLevel               | `int`                                            | If the picker game has levels, the current level of the picker game.                                |
| items                      | `List<List<PickAwardWrapper>>`                   | The list of items picked in each level of a picker game.                                            |
| accumulatedTotalWinnings   | `long`                                           | The accumulated monetary winnings at a given point of the picker game.                              |
| unusedLevelEffectsPerLevel | `List<Map<LevelEffect, List<PickAwardWrapper>>>` | The list of level effects that have been picked per level that have not yet been used for anything. |
| picksPerformed             | `int`                                            | The total number of picks performed in a picker game.                                               |
| picksRemaining             | `int`                                            | The number of remaining picks to perform to complete a picker game.                                 |

Coord
-----

The representation of a panel coordinate.

### Fields

| Name | Type  | Description                       |
| ---- | ----- | --------------------------------- |
| reel | `int` | The reel index of the coordinate. |
| row  | `int` | The row index of the coordinate.  |

Panel
-----

A representation of a panel in a slot game.

### Fields

| Name  | Type         | Description                               |
| ----- | ------------ | ----------------------------------------- |
| reels | `List<Reel>` | The list of reels a panel is composed of. |

Winning
-------

Represents a winning after a panel evaluation. Winnings do not necessarily need to be monetary.

### Fields

| Name        | Type           | Description                                                                                                     |
| ----------- | -------------- | --------------------------------------------------------------------------------------------------------------- |
| winningType | `WinningType`  | The type of winning. LINE, SCATTER, WAY, PICKER, GROUP.                                                         |
| prizeType   | `PrizeType`    | The type of prize awarded. MONEY, FEATURE_START, ITEM_COLLECTED, MISC.                                          |
| winnings    | `long`         | If the winning is monetary, this indicates the actual amount.                                                   |
| mainSymbol  | `String`       | The main symbol that produced the winning.                                                                      |
| occurrences | `int`          | The number of occurrences of the symbol that produced the winning.                                              |
| id          | `String`       | An identifier for the winning.                                                                                  |
| symbols     | `List<String>` | The actual list of symbols that produced the winning. May all be the same or itcould also include wild symbols. |
| coords      | `List<Coord>`  | The coordinates where the symbols that produced the winnings are in the evaluated panel.                        |

Transition
----------

Represents the a symbol movement in a panel.

### Fields

| Name | Type    | Description                                                                                          |
| ---- | ------- | ---------------------------------------------------------------------------------------------------- |
| from | `Coord` | The position from which the symbol is going to move. Null if the symbol comesfrom outside the panel. |
| to   | `Coord` | The position where the symbol will be moved to.                                                      |
