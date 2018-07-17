
- [Reconnection](#reconnection)
- [Main](#main)
- [Free Spins](#free-spins)
- [Cascades](#cascades)
- [Picker](#picker)

Order of events
===============

Reconnection
------------

```
generic:configuration
generic:show
generic:availableActions
```

Main
----

Pre-spin events
```
generic:bonusPrize?
generic:tokenAwarded?
```

Spin event - **ALWAYS SENT**
```
generic:spin
```

Panel modifier events. This happens pre-evaluation
```
[generic:symbolMovement|generic:symbolOverride]*
```

Winning event
```
generic:winnings?
```

Feature start events
```
generic:freeSpinsStart?
generic:pickerStart?
generic:cascadesStart?
```

How a feature is triggered events
```
generic:featureTriggeredWinnings
generic:featureTriggeredByToken
```


Monetary winnings from state change event
```
generic:stateChangeMonetaryWinnings?
```

Panel overrider events - this happens post-evaluation
```
[generic:symbolMovement|generic:symbolOverride]*
generic:overriderSpin
```

Symbol marker events
```
[generic:symbolMarker]*
```

Available actions event - **ALWAYS SENT**
```
generic:availableActions
```



Free spins
----------

Pre-spin events
```
generic:bonusPrize?
generic:tokenAwarded?
```

Spin event - **ALWAYS SENT**
```
generic:spin
```

Panel modifier events. This happens pre-evaluation
```
[generic:symbolMovement|generic:symbolOverride]*
```

Winning event
```
generic:winnings?
```

Free spin progress event
```
generic:freeSpinsProgress
```

Feature start events and free spins retrigger/end events
```
generic:freeSpinsRetrigger?
generic:freeSpinsStart?
generic:pickerStart?
generic:cascadesStart?
generic:freeSpinsEnd?
```

How a feature is triggered events
```
generic:featureTriggeredWinnings
generic:featureTriggeredByToken
```

Monetary winnings from state change event
```
generic:stateChangeMonetaryWinnings?
```

Panel overrider events - this happens post-evaluation
```
[generic:symbolMovement|generic:symbolOverride]*
generic:overriderSpin
```

Symbol marker events
```
[generic:symbolMarker]*
```

Available actions event - **ALWAYS SENT**
```
generic:availableActions
```



Cascades
--------

Pre-spin events
```
generic:bonusPrize?
generic:tokenAwarded?
```

Spin event - **ALWAYS SENT**
```
generic:spin
```

Panel modifier events. This happens pre-evaluation
```
[generic:symbolMovement|generic:symbolOverride]*
```

Winning event
```
generic:winnings?
```

Free spin progress event
```
generic:cascadesProgress
```

Feature start events and cascade end event
```
generic:freeSpinsStart?
generic:pickerStart?
generic:cascadesStart?
generic:cascadesEnd?
```

How a feature is triggered events
```
generic:featureTriggeredWinnings
generic:featureTriggeredByToken
```

Monetary winnings from state change event
```
generic:stateChangeMonetaryWinnings?
```

Panel overrider events - this happens post-evaluation
```
[generic:symbolMovement|generic:symbolOverride]*
generic:overriderSpin
```

Symbol marker events
```
[generic:symbolMarker]*
```

Available actions event - **ALWAYS SENT**
```
generic:availableActions
```


Picker
------

Pick event - **ALWAYS SENT**
```
generic:pick
```

Pick update events
```
generic:pickerLevelUpdate
generic:pickProgress
```

Feature start events and picker end event
```
generic:freeSpinsStart?
generic:cascadesStart?
generic:freeSpinsEnd?
generic:pickerEnd?
```

How a feature is triggered events
```
generic:featureTriggeredWinnings
generic:featureTriggeredByToken
```

Monetary winnings from state change event
```
generic:stateChangeMonetaryWinnings?
```

Available actions event - **ALWAYS SENT**
```
generic:availableActions
```
