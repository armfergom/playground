Table of Contents
=================

- [Game Actions](#game-actions)
  - [generic:spin](#genericspin)
- [Game Events](#game-events)
  - [generic:availableActions](#genericavailableactions)
  - [generic:cascadesEnd](#genericcascadesend)
  - [generic:cascadesProgress](#genericcascadesprogress)
  - [generic:cascadesStart](#genericcascadesstart)
  - [generic:featureTriggeredWinnings](#genericfeaturetriggeredwinnings)
  - [generic:freeSpinsEnd](#genericfreespinsend)
  - [generic:freeSpinsProgress](#genericfreespinsprogress)
  - [generic:freeSpinsRetrigger](#genericfreespinsretrigger)
  - [generic:freeSpinsStart](#genericfreespinsstart)
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
{"availableActions":["generic:spin"],"state":"MAIN","type":"game-event","name":"generic:availableActions"}
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
{"@type":"CascadesEndEvent","stateName":"CASCADES","context":{"bet":100,"panel":{"reels":[{"symbols":["GG","GG","GG"]},{"symbols":["GG","CC","TRIGGER"]},{"symbols":["AA","BB","CC"]},{"symbols":["AA","DD","BB"]},{"symbols":["GG","AA","GG"]}]},"winnings":[],"cascadeLevel":2,"multiplier":3},"type":"game-event","name":"generic:cascadesEnd"}
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
{"stateName":"CASCADES","context":{"bet":100,"panel":{"reels":[{"symbols":["GG","GG","FF"]},{"symbols":["CC","FF","TRIGGER"]},{"symbols":["BB","CC","FF"]},{"symbols":["AA","DD","BB"]},{"symbols":["GG","AA","GG"]}]},"winnings":[{"winningType":"LINE","prizeType":"MONEY","winnings":30,"mainSymbol":"FF","occurrences":3,"id":"13","symbols":["FF","FF","FF","DD","GG"],"coords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":2},{"reel":3,"row":1},{"reel":4,"row":2}],"winningSymbols":["FF","FF","FF"],"winningCoords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":2}]}],"cascadeLevel":1,"multiplier":2},"type":"game-event","name":"generic:cascadesProgress"}
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
{"@type":"CascadesStartEvent","stateName":"CASCADES","context":{"bet":100,"panel":{"reels":[{"symbols":["GG","EE","FF"]},{"symbols":["FF","EE","TRIGGER"]},{"symbols":["FF","EE","WILD"]},{"symbols":["AA","DD","BB"]},{"symbols":["GG","AA","GG"]}]},"winnings":[],"cascadeLevel":0,"multiplier":1},"type":"game-event","name":"generic:cascadesStart"}
```

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
{"@type":"FeatureTriggeredByWinningsEvent","stateName":"CASCADES","winnings":[{"winningType":"LINE","prizeType":"FEATURE_START","winnings":25,"mainSymbol":"EE","occurrences":3,"id":"1","symbols":["EE","EE","EE","DD","AA"],"coords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":1},{"reel":3,"row":1},{"reel":4,"row":1}],"winningSymbols":["EE","EE","EE"],"winningCoords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":1}]},{"winningType":"LINE","prizeType":"FEATURE_START","winnings":25,"mainSymbol":"EE","occurrences":3,"id":"15","symbols":["EE","EE","WILD","DD","AA"],"coords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":2},{"reel":3,"row":1},{"reel":4,"row":1}],"winningSymbols":["EE","EE","WILD"],"winningCoords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":2}]}],"type":"game-event","name":"generic:featureTriggeredWinnings"}
```

generic:freeSpinsEnd
--------------------

Message sent to the client indicating that free spins have just ended.


### Fields

| Name      | Type            | Description                                          |
| --------- | --------------- | ---------------------------------------------------- |
| stateName | `String`        | The name of the free spin state that has just ended. |
| context   | `FreeSpinState` | The state with which the free spins ended.           |

#### Example

```
{"@type":"FreeSpinsEndEvent","stateName":"FREESPINS","context":{"bet":100,"panel":{"reels":[{"symbols":["EE","AA","FF"]},{"symbols":["BB","FF","AA"]},{"symbols":["WILD","DD","BB"]},{"symbols":["EE","AA","AA"]},{"symbols":["AA","AA","FF"]}]},"winnings":[{"winningType":"LINE","prizeType":"MONEY","winnings":45,"mainSymbol":"FF","occurrences":3,"id":"5","symbols":["FF","FF","WILD","AA","FF"],"coords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":0},{"reel":3,"row":1},{"reel":4,"row":2}],"winningSymbols":["FF","FF","WILD"],"winningCoords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":0}]},{"winningType":"LINE","prizeType":"MONEY","winnings":45,"mainSymbol":"FF","occurrences":3,"id":"19","symbols":["FF","FF","WILD","EE","AA"],"coords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":0},{"reel":3,"row":0},{"reel":4,"row":0}],"winningSymbols":["FF","FF","WILD"],"winningCoords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":0}]}],"multiplier":3,"freeSpinRemaining":0,"freeSpinsConsumed":10,"symbolsInPanelsStopCondition":[],"freeSpinsAccumulatedWinnings":10740},"type":"game-event","name":"generic:freeSpinsEnd"}
```

generic:freeSpinsProgress
-------------------------

Message sent to the client indicating the progress of the free spin phase. Sent after each free spin.


### Fields

| Name      | Type            | Description                                  |
| --------- | --------------- | -------------------------------------------- |
| stateName | `String`        | The name of the free spin state in progress. |
| context   | `FreeSpinState` | The current state of the free spin phase.    |

#### Example

```
{"stateName":"CASCADES_FS","context":{"bet":100,"panel":{"reels":[{"symbols":["AA","EE","TRIGGER"]},{"symbols":["CC","EE","FF"]},{"symbols":["WILD","TRIGGER","GG"]},{"symbols":["EE","BB","AA"]},{"symbols":["DD","GG","DD"]}]},"winnings":[{"winningType":"LINE","prizeType":"MONEY","winnings":75,"mainSymbol":"EE","occurrences":3,"id":"14","symbols":["EE","EE","WILD","BB","GG"],"coords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":0},{"reel":3,"row":1},{"reel":4,"row":1}],"winningSymbols":["EE","EE","WILD"],"winningCoords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":0}]}],"multiplier":3,"freeSpinRemaining":9,"freeSpinsConsumed":1,"symbolsInPanelsStopCondition":[],"freeSpinsAccumulatedWinnings":75},"type":"game-event","name":"generic:freeSpinsProgress"}
```

generic:freeSpinsRetrigger
--------------------------

Message sent to the client indicating that free spins have just been retriggered.


### Fields

| Name      | Type            | Description                                                     |
| --------- | --------------- | --------------------------------------------------------------- |
| stateName | `String`        | The name of the free spin state that has just been retriggered. |
| context   | `FreeSpinState` | The new free spin state for the retriggered free spins.         |

#### Example

```
{"@type":"FreeSpinsRetriggerEvent","stateName":"FREESPINS","context":{"bet":100,"panel":{"reels":[{"symbols":["EE","TRIGGER","BB"]},{"symbols":["AA","WILD","BB"]},{"symbols":["AA","WILD","AA"]},{"symbols":["BB","DD","WILD"]},{"symbols":["DD","AA","FF"]}]},"winnings":[{"winningType":"LINE","prizeType":"MONEY","winnings":1500,"mainSymbol":"BB","occurrences":4,"id":"7","symbols":["BB","BB","WILD","WILD","FF"],"coords":[{"reel":0,"row":2},{"reel":1,"row":2},{"reel":2,"row":1},{"reel":3,"row":2},{"reel":4,"row":2}],"winningSymbols":["BB","BB","WILD","WILD"],"winningCoords":[{"reel":0,"row":2},{"reel":1,"row":2},{"reel":2,"row":1},{"reel":3,"row":2}]},{"winningType":"LINE","prizeType":"MONEY","winnings":75,"mainSymbol":"EE","occurrences":3,"id":"16","symbols":["EE","WILD","WILD","DD","DD"],"coords":[{"reel":0,"row":0},{"reel":1,"row":1},{"reel":2,"row":1},{"reel":3,"row":1},{"reel":4,"row":0}],"winningSymbols":["EE","WILD","WILD"],"winningCoords":[{"reel":0,"row":0},{"reel":1,"row":1},{"reel":2,"row":1}]},{"winningType":"LINE","prizeType":"MONEY","winnings":300,"mainSymbol":"BB","occurrences":3,"id":"17","symbols":["BB","WILD","WILD","DD","FF"],"coords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":1},{"reel":3,"row":1},{"reel":4,"row":2}],"winningSymbols":["BB","WILD","WILD"],"winningCoords":[{"reel":0,"row":2},{"reel":1,"row":1},{"reel":2,"row":1}]}],"multiplier":3,"freeSpinRemaining":7,"freeSpinsConsumed":8,"symbolsInPanelsStopCondition":[],"freeSpinsAccumulatedWinnings":5625},"type":"game-event","name":"generic:freeSpinsRetrigger"}
```

generic:freeSpinsStart
----------------------

Message sent to the client indicating that free spins have just started.


### Fields

| Name      | Type            | Description                                            |
| --------- | --------------- | ------------------------------------------------------ |
| stateName | `String`        | The name of the free spin state that has just started. |
| context   | `FreeSpinState` | The initial free spin state for the triggered phase.   |

#### Example

```
{"@type":"FreeSpinsStartEvent","stateName":"FREESPINS","context":{"bet":100,"panel":{"reels":[{"symbols":["DD","TRIGGER","FF"]},{"symbols":["GG","FF","TRIGGER"]},{"symbols":["EE","TRIGGER","BB"]},{"symbols":["GG","WILD","FF"]},{"symbols":["FF","GG","FF"]}]},"winnings":[],"multiplier":1,"freeSpinRemaining":10,"freeSpinsConsumed":0,"symbolsInPanelsStopCondition":[],"freeSpinsAccumulatedWinnings":0},"type":"game-event","name":"generic:freeSpinsStart"}
```

generic:show
------------

Message sent to the client in every connection with information about the current state of the game. Should be used for recreate the last state the player had in the game.

**This message is only sent during reconnections.**

### Fields

| Name            | Type                         | Description                                                                                                                                                                                                                                                                  |
| --------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| state           | `String`                     | The name of the current state.                                                                                                                                                                                                                                               |
| stateFeature    | `FeatureType`                | The type of feature the current state represents: free spins, cascades, picker, etc.                                                                                                                                                                                         |
| panel           | `Panel`                      | The last panel shown in the game.                                                                                                                                                                                                                                            |
| betAmount       | `long`                       | The last bet amount used.                                                                                                                                                                                                                                                    |
| winnings        | `List<Winning>`              | The list of last winnings produced.                                                                                                                                                                                                                                          |
| mainState       | `MainState`                  | The current state of the game in the MAIN state.                                                                                                                                                                                                                             |
| freeSpinsStates | `Map<String, FreeSpinState>` | For each active free spin state name, the current game state.                                                                                                                                                                                                                |
| cascadeStates   | `Map<String, CascadeState>`  | For each active cascade state name, the current game state.                                                                                                                                                                                                                  |
| pickerStates    | `Map<String, PickerState>`   | For each active picker state name, the current game state.                                                                                                                                                                                                                   |
| markersApplied  | `Map<String, List<Coord>>`   | For each marker that are applied at this moment, the affected coordinates                                                                                                                                                                                                    |
| initialPanel    | `Panel`                      | The initial panel of the game, it is used in games were there are modifiers to apply betweenrounds and the player changes the bet level, this will invalidate the modifiers if he spins and thispanel will be used as base in the slot Machine Spin, so it should be showed. |

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
{"betAmount":100,"panel":{"reels":[{"symbols":["CC","AA","CC"]},{"symbols":["EE","CC","CC"]},{"symbols":["FF","FF","BB"]},{"symbols":["FF","BB","GG"]},{"symbols":["BB","CC","FF"]}]},"basePanel":{"reels":[{"symbols":["CC","AA","CC"]},{"symbols":["EE","CC","CC"]},{"symbols":["FF","FF","BB"]},{"symbols":["FF","BB","GG"]},{"symbols":["BB","CC","FF"]}]},"type":"game-event","name":"generic:spin"}
```

generic:symbolMovement
----------------------

Message sent to the client indicating that certain symbols have been moved from one place to another. The order in which this event is sent is important as it can happen before and after the evaluation of the panel.


### Fields

| Name          | Type                     | Description                                           |
| ------------- | ------------------------ | ----------------------------------------------------- |
| originalPanel | `Panel`                  | The panel before the symbols are moved.               |
| newPanel      | `Panel`                  | The resulting panel after the symbols are moved.      |
| transitions   | `Collection<Transition>` | The set of symbol movements that occurred.            |
| componentName | `String`                 | The name of the panel modifier that has been applied. |

#### Example

```
{"originalPanel":{"reels":[{"symbols":["GG","GG","FF"]},{"symbols":["CC","FF","TRIGGER"]},{"symbols":["BB","CC","FF"]},{"symbols":["AA","DD","BB"]},{"symbols":["GG","AA","GG"]}]},"newPanel":{"reels":[{"symbols":["GG","GG","FF"]},{"symbols":["CC","FF","TRIGGER"]},{"symbols":["BB","CC","FF"]},{"symbols":["AA","DD","BB"]},{"symbols":["GG","AA","GG"]}]},"transitions":[{"from":{"reel":0,"row":0},"to":{"reel":0,"row":1}},{"from":{"reel":1,"row":0},"to":{"reel":1,"row":1}},{"from":{"reel":2,"row":0},"to":{"reel":2,"row":2}},{"from":null,"to":{"reel":0,"row":0}},{"from":null,"to":{"reel":1,"row":0}},{"from":null,"to":{"reel":2,"row":0}},{"from":null,"to":{"reel":2,"row":1}}],"componentName":"AvalancheReelsModifier","type":"game-event","name":"generic:symbolMovement"}
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
{"winnings":[{"winningType":"LINE","prizeType":"MONEY","winnings":25,"mainSymbol":"EE","occurrences":3,"id":"1","symbols":["EE","EE","EE","DD","AA"],"coords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":1},{"reel":3,"row":1},{"reel":4,"row":1}],"winningSymbols":["EE","EE","EE"],"winningCoords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":1}]},{"winningType":"LINE","prizeType":"MONEY","winnings":25,"mainSymbol":"EE","occurrences":3,"id":"15","symbols":["EE","EE","WILD","DD","AA"],"coords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":2},{"reel":3,"row":1},{"reel":4,"row":1}],"winningSymbols":["EE","EE","WILD"],"winningCoords":[{"reel":0,"row":1},{"reel":1,"row":1},{"reel":2,"row":2}]}],"monetaryWinningsSum":50,"type":"game-event","name":"generic:winnings"}
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

| Name                         | Type                             | Description                                                                                                                                                                          |
| ---------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| bet                          | `long`                           | The last bet amount used in the free spin game.                                                                                                                                      |
| panel                        | `Panel`                          | The last panel shown in the free spin game.                                                                                                                                          |
| winnings                     | `List<Winning>`                  | The list of last winnings produced in the free spin game.                                                                                                                            |
| multiplier                   | `long`                           | The last prize multiplier used in the free spin game.                                                                                                                                |
| freeSpinRemaining            | `Integer`                        | The number of free spins to complete the free spin game, null if the consumption condition is not based in the number of spins, for example, the occurrence of a symbol in the panel |
| freeSpinsConsumed            | `int`                            | The number of free spins consumed so far                                                                                                                                             |
| symbolsInPanelsStopCondition | `List<Pair<Set<String>, Coord>>` | The symbols to appear in given coordinates so that the free spin game completes.                                                                                                     |
| freeSpinsAccumulatedWinnings | `long`                           | The accumulated winnings during the entire free spin phase.                                                                                                                          |

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

| Name           | Type           | Description                                                                                                                                     |
| -------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| winningType    | `WinningType`  | The type of winning. LINE, SCATTER, WAY, PICKER, GROUP.                                                                                         |
| prizeType      | `PrizeType`    | The type of prize awarded. MONEY, FEATURE_START, ITEM_COLLECTED, MISC.                                                                          |
| winnings       | `long`         | If the winning is monetary, this indicates the actual amount.                                                                                   |
| mainSymbol     | `String`       | The main symbol that produced the winning.                                                                                                      |
| occurrences    | `int`          | The number of occurrences of the symbol that produced the winning.                                                                              |
| id             | `String`       | An identifier for the winning.                                                                                                                  |
| symbols        | `List<String>` | The list of symbols in the winning combination. May include symbols that arepart of a winning line but not part of the actual winning.          |
| coords         | `List<Coord>`  | The list of coordinates in the winning combination. May include coordinates that are part of a winning line but not part of the actual winning. |
| winningSymbols | `List<String>` | The actual list of symbols that produced the winning. May all be the same or itcould also include wild symbols.                                 |
| winningCoords  | `List<Coord>`  | The coordinates where the symbols that produced the winnings are in the evaluated panel.                                                        |

Transition
----------

Represents the a symbol movement in a panel.

### Fields

| Name | Type    | Description                                                                                          |
| ---- | ------- | ---------------------------------------------------------------------------------------------------- |
| from | `Coord` | The position from which the symbol is going to move. Null if the symbol comesfrom outside the panel. |
| to   | `Coord` | The position where the symbol will be moved to. Null if the symbol goes outsidethe panel.            |

Event sequences
===============

The different event sequences that can be produced in the game.

Event sequence 1
----------------

```
generic:spin
generic:winnings
generic:cascadesStart
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 2
----------------

```
generic:spin
generic:symbolMovement
generic:cascadesProgress
generic:cascadesEnd
generic:availableActions

```


Event sequence 3
----------------

```
generic:spin
generic:availableActions

```


Event sequence 4
----------------

```
generic:spin
generic:symbolMovement
generic:winnings
generic:cascadesProgress
generic:availableActions

```


Event sequence 5
----------------

```
generic:spin
generic:winnings
generic:freeSpinsStart
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 6
----------------

```
generic:spin
generic:winnings
generic:freeSpinsProgress
generic:cascadesStart
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 7
----------------

```
generic:spin
generic:freeSpinsProgress
generic:availableActions

```


Event sequence 8
----------------

```
generic:spin
generic:symbolMovement
generic:cascadesProgress
generic:cascadesEnd
generic:freeSpinsEnd
generic:cascadesStart
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 9
----------------

```
generic:spin
generic:symbolMovement
generic:winnings
generic:cascadesProgress
generic:freeSpinsStart
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 10
-----------------

```
generic:spin
generic:winnings
generic:freeSpinsProgress
generic:freeSpinsRetrigger
generic:availableActions

```


Event sequence 11
-----------------

```
generic:spin
generic:freeSpinsProgress
generic:freeSpinsEnd
generic:availableActions

```


Event sequence 12
-----------------

```
generic:spin
generic:freeSpinsProgress
generic:freeSpinsEnd
generic:cascadesStart
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 13
-----------------

```
generic:spin
generic:symbolMovement
generic:cascadesProgress
generic:cascadesEnd
generic:freeSpinsRetrigger
generic:availableActions

```


Event sequence 14
-----------------

```
generic:spin
generic:symbolMovement
generic:cascadesProgress
generic:cascadesEnd
generic:freeSpinsRetrigger
generic:featureTriggeredWinnings
generic:availableActions

```


Event sequence 15
-----------------

```
generic:spin
generic:symbolMovement
generic:cascadesProgress
generic:cascadesEnd
generic:freeSpinsEnd
generic:availableActions

```

