# Resources

## Roblox Lua Style Guide

The Roblox Lua Style guide is the baseline for best practices. It is a necessity to go through this guide first and then come back to learn more.
[Roblox Lua Style Guide](https://roblox.github.io/lua-style-guide/)

## Framework
______________________________

As of July 2021, Wonder Works Studio is currently using the [Knit Framework](https://sleitnick.github.io/Knit/) as our bread and butter for all of our games on the Roblox platform. 

Knit luckily is not a strict structured framework and more of a modular library. Knit is designed in a way that lets the developers decide their own structure while still accessing all of the same functionality and benefits it has to offer. 

While we may design our own framework in the future, it would end up very similar in design to the way Knit functions now which is perfect for our current uses. 

I have dedicated a [section](knit.md) to go over some basics of Knit but it is very important to read up on the framework through its actual documentation.

[Knit Framework](https://sleitnick.github.io/Knit/)


## DataStore Library
______________________________

As with Knit, we are using another open-sourced project for our datastore systems. [Profile Service](https://madstudioroblox.github.io/ProfileService/) has a lot of benefits such as session-locking with features like MetaTags and GlobalUpdates when adding new content to the project. We could also create our own DataStore system in the near future but this currently has everything we could possibly need and more at its current capabilities. 

[Profile Service](https://madstudioroblox.github.io/ProfileService/)

Feel free to make any modifications to adapt on a per-project basis. This could include web-service, MemoryStore, or any additional cloud service features.

## Roact, Rodux, And Their Integration
______________________________

`
As with everything in this guide, we are only glancing at the surface here with the introduction of these libraries. You will have to test out using these libraries yourself and really play around with how things operate and function. There are plenty of resources at your disposal and your coworkers will be more than willing to assist you with any questions you may have.
`

#### Roact

Roact is a Roblox Lua UI Library which serves the purpose of handling the creation and usage of the player's user interface. Since Roact was built to be very similar to Facebook's React Library, it has a lot of the same principles and functionality that makes it an industry standard. 

If you're new to Roact, I would look at this [devforum guide](https://devforum.roblox.com/t/roact-the-ultimate-ui-framework/796618) to begin the learning process. As you will soon learn, using Roact takes **much longer** to implement depending on the project. So why do we use it? The perks of Roact become clear as your project takes shape. Roact helps you define what your UI should be doing compared to how it should do it. This means less unknown bugs, more manageable code, and easy reusability. The perks become even more transparent when combined with Rodux.


[Roact](https://roblox.github.io/roact/)


#### Rodux

Similar to how Roact was formed, Rodux is based off it's predecessor Redux with the same purpose in mind; to be the ideal state library. As the word 'state' is defined, we can declare the condition of our application at a specific time and that will never change unless we change it. This means one source of truth - it's state. If a round-based game is in intermission then it's state would be set to intermission. The rest of the game's code would have no way to define it's own 'truth'.

Obviously with a medium to large project you may have several states. Is a specific UI window open? What tool is the player holding? What location is the player in? Rodux tackles this by creating 'The Store'. 

Store has a state, a reducer, and a dispatcher. 

The state is a simple dictionary to define the data of the store.
```lua
    local state = {
        location = "Texas",
        inventoryWindowOpen = false,
        weaponHeld = "",
    }
```


Then the dispatcher handles all of the incoming **action objects** and sends them to the reducer. The dispatcher can be considered a middle man and a **middleware** can be setup to catch all actions and modify the action before being sent to the reducer.

The reducer takes in an **action object** and the current **state** of the store and returns a new state based off of the action.

[Rodux](https://roblox.github.io/rodux/)


#### Roact-Rodux Integration

While Roact and Rodux can be used separately for their own specific uses, they can and should be used together. Defining the current state of the user's interface prevents a lot unknown errors and simplifies the long-term development process. 

[Roact-Rodux](https://roblox.github.io/roact-rodux/)


