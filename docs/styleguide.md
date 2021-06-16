# Programming Style Guide

Welcome to the Wonder Works Programming Style Guide.

This is heavily inspired by [Roblox Lua Style guide](https://roblox.github.io/lua-style-guide/) with a few differences. It is worth reading through [that guide](https://roblox.github.io/lua-style-guide/) first then adjusting to this guide.

______________________________

## Why Do We Have A Style Guide

Working in a company means collaborating with a lot of different individuals. 
At Wonder Works, you will be working side by side with a lot of programmers who might join your project to assist you, give you guidance, or to take over the project themselves.

In order for this to be streamlined, everyone's code needs to follow a similar structure. 
From readability to agreed upon best practices; everyone needs to be on the same page.

______________________________


## Structure

#### File Names

All file names need to specifically relate to the file's purpose; written in `PascalCase`.

<span style="color:red">Bad:</span>
```
-- File's purpose: To handle all Interactions on the server
File's Name: serverPrompts

Why:
It could possibly handle more than just proximity prompts and does not state what kind of file it is.
```

<span style="color:green">Good:</span>
```
-- File's purpose: To handle all Interactions on the server
File's Name: InteractionService

Why:
States it is a Service, in PascalCase, while specifically stating its purpose.
```

______________________________

#### File Purpose

Every file within your application should have a single main purpose. CurrencyService should relate only to currency. DataService should relate only to data. 

______________________________

#### Source File Format

Each  file needs to follow this general format with very little exceptions.
```
-- Module Documentation (briefly describe the purpose of this module)

-- Service References

-- Framework require

-- Constant & Local variables

-- CreateService / CreateController / Class Creation (Knit.CreateService / Knit.CreateController / Class.new())

-- Module code

-- Return module
```

#### Classes

Classes should have a specific purpose and use while following this template code:
```lua
local MyClass = {}
MyClass.__index = MyClass

function MyClass.new()
    local self = setmetatable({
        _destroyed = false
    }, MyClass)
    
    return self
end


function MyClass:Destroy()
    if self._destroyed then
        return
    end

    self._destroyed = true
end
```

Each class needs the `Destroy` method so that it can be cleaned up properly by Maids. Also in general the class should most likely have its own maid that is cleaned
in the `Destroy` method to make sure no memory leaks remain after it has served its use.
______________________________

## Readability

First and foremost - your code needs to look pleasing to the eye. If someone glances at your code they should be able to get a gist of what your intentions were and not be too overwhelmed. 


______________________________

#### Variable Naming Schemes

All variables should be local and should be as descriptive as needed to clearly state their purpose.

<span style="color:red">Bad:</span>

```lua
local function createPart(arg1, arg2, arg3)
    local newPart: BasePart = Instance.new("Part")
    newPart.Name = arg1
    newPart.Size = arg2
    newPart.Position = arg3

    return newPart
end
```

<span style="color:green">Good:</span>

```lua
local function createPart(name: String, size: Vector3, position: Vector3)
    local newPart: BasePart = Instance.new("Part")
    newPart.Name = name
    newPart.Size = size
    newPart.Position = position

    return newPart
end
```

**Variables that retrieve services** using `game:GetService` should be in `PascalCase`

```lua
local RunService = game:GetService("RunService")
```

**Constant variables** (variables that, without a doubt, never change) that hold general information should be in `UPPER_SNAKE_CASE`

```lua
local TREASURE_SPAWNS = 10
```

Otherwise, **all other variables** will be in `camelCase`

```lua
local playerName = "WonderWorksDeveloper1"
```

______________________________

#### Function Naming Schemes

**Local functions** should be written in `camelCase`

```lua
local function createPart()

end
```

**Public Methods** (methods that could be accessed from a different script) should be written in `PascalCase` 

```lua
function InteractionService:HandlePrompt()

end
```

**Private Methods** (methods that should only be used inside this specific source file) should be written with an `_underscoreCamelCase`
```lua
function InteractionService:_handlePrompt()

end
```

______________________________

#### Spacing

Each segment of your code needs to be spaced out accordingly.

After requiring your services and framework module there should be a **single space**. Then another **single space** after requiring your other modules and delcaring any other local variables.

Example:
```lua
local ProximityPromptService = game:GetService("ProximityPromptService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Knit = require(ReplicatedStorage.Knit)

local RemoteEvent = require(Knit.Util.Remote.RemoteEvent)

local InteractionService = Knit.CreateService {
    Name = "InteractionService";
    Client = {
        Triggered = RemoteEvent.new(),
        HoldBegan = RemoteEvent.new(),
        HoldEnded = RemoteEvent.new()
    };
}
```

There should also be **two spaces** in between each function.

```lua
function InteractionService:KnitStart()

end

-- 2 spaces
function InteractionService:KnitInit()

end
```

Inside each scope you should separate each 'section' of code by a **single space**. 

Example:

```lua
function InteractionService:_onPromptTriggered()
    local variableOne = 1
    local variableTwo = 2
    local variableThree = 3

    if variableOne == 1 then
        print("success")
    else
        print("???")
    end

    if variableTwo >= 2 then
        local secondExampleVariable = true

        if secondExampleVariable then
            -- still correct spacing
        end
    end
end
```

______________________________

#### Commenting


Commenting is always a weird topic with differing opinions. What is a good comment compared to a bad comment? When are you supposed to comment and when should the code just speak for itself? Hopefully this helps lay those questions to rest.

##### <span style="color:green">Good Comments</span>

Good comments are things you're required to do and do well. Your team is depending on you to be clear and concise, not only with your code but also with your intentions. If you have ever had someone review your code then you definitely know how two people can view the same line of code differently.

As a general rule of thumb - intent comments should be included in almost every segment of code that will reduce the time it takes to understand it.
Just because you know what is *supposed* to happen does not mean everyone will know what is *supposed* to happen.
Example:
```lua

function InteractionService:_onPromptTriggered(prompt: ProximityPrompt)
    -- run prompt's action if the prompt is valid
    if self._prompts[prompt] then
        if self._prompts[prompt].action then
            self._prompts[prompt].action()
        end
    end
end
```

This is obviously a very very small example. This should be taken more seriously when you have a several lines of code after the initial if statement and a developer might have to take several minutes to even grasp what is going on.


- Comments that document the overall purpose of this file. Although the file name should already do that job, it helps to add in more context that is precise and to the point.
Example:
```lua
--[[
    PartService will create different types of parts that hold special properties.
]]
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Knit = require(ReplicatedStorage.Knit)
```

##### <span style="color:red">Bad Comments</span>

- Comments that add more lines without adding more understanding to the current code.
Example:
```lua
local function createPart()
    -- I would like to move this to PartService soon maybe?

    local newPart = Instance.new("Part")
    return newPart
end
```

- Long Comments that defeat the purpose of a "**quicker** and better understanding"
Example:
```lua
local function createPart(arg1, arg2, arg3)
    --[[
        So createPart takes in 3 arguments, name, size, and position.
        Name refers to what I'll name the part
        Size refers to the size of the part (how big it is!)
        Position refers to where the part will be in the world

        Then this function will create a new part, set those 3 properties, and return the new part!

        Afterwards you can modify the part as you see fit and delete it after it has been used
    ]]
end
```

- Restating code. You should never state what your code is already specifically saying; only its intent. Bad example:
```lua
local function createPart(name, size, position)
    -- create part with Instance.new() then set its properties 
    local newPart = Instance.new("Part")
    newPart.Name = name
    newPart.Size = size
    newPart.Position = position
    return newPart
end
```

______________________________

#### Type Checking

While tedious, type checking is greatly preferred everywhere BUT only required on your function arguments. 

```lua
function InteractionService:_onPromptTriggered(prompt: ProximityPrompt)
    local variableOne: number = 1
    local variableTwo: number = 2
    local variableThree: number = 3
end
```

You don't have to add the ```--!strict``` tag at the beginning of each script though unless you want to ensure you're not making any mistakes. This is just for readability purposes. Yes, I may see that ```variableOne``` is ```1``` but when the code can be potentially hundreds of lines it is hard to just tell what this argument is used for and why. Defining the type helps with any possible confusion.


[For more guidance and assistance check this link.](https://devforum.roblox.com/t/type-checking-for-beginners/1027242)
