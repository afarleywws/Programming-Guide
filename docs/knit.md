# Knit Framework

[Knit Framework](https://sleitnick.github.io/Knit/)


#### Why Knit

Knit luckily is not a strict structured framework and more of a modular library. Knit is designed in a way that lets the developers decide their own structure while still accessing all of the same functionality and benefits it has to offer.

Knit can easily be added to pre-existing code or start the creation of an application that is future-proof when it comes to new updates and features.


#### How Knit Works


| Name | Description |
| ----------- | ----------- |
| Service | Server-Side Code that has a specific purpose. Initialized during Knit.Start() |
| Controllers | Client-Side Code that has a specific purpose. Initialized during Knit.Start() |
| Modules | Separate ModuleScripts that are used as either classes, information storage, libraries, or multi-usage functions |


Check out this [getting started](https://sleitnick.github.io/Knit/gettingstarted/) documentation to learn more but at startup all Knit needs is to run it's Start() function **once** on both the server and client.

```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Knit = require(ReplicatedStorage.Knit)

Knit.Start():Catch(warn)

-- Start() returns a Promise that catches any error with the initial startup that then uses warn to output the error.
```

While this is how you start Knit, it will need more code before it is close to production-ready.


#### Services

Services are meant to have a specific purpose and only relate to that purpose. DataService would have all battle-related functionality. CurrencyService would have all currency-related functionality. 

Services only need to require Knit, declare the service, and initialize the service. `KnitInit()` method is fired during the startup of `Knit.Start()` which means KnitInit cannot yield or it will prevent other code from running. After all KnitInit methods are ran, `KnitStart()` methods are ran on their own which means they can yield if needed.


```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Knit = require(ReplicatedStorage.Knit)


local CurrencyService = Knit.CreateService {
    Name = "CurrencyService",
    Client = {},
}


function CurrencyService:KnitStart()
    -- declare variables, wait for children, etc.
end


function CurrencyService:KnitInit()
    -- no yielding
end

```

If I wanted to have the client communicate with the service and get information back from it I could set up a method that is exposed to the client.

```lua
-- on the server from the Service ModuleScript

function CurrencyService.Client:GetInformation()
    return "some information"
end


-- on the client from a LocalScript

local CurrencyService = Knit.GetService("CurrencyService")
local information = CurrencyService:GetInformation()
```

#### Controllers

Controllers follow the exact same structure as Services.


```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Knit = require(ReplicatedStorage.Knit)


local MovementController = Knit.CreateController {
    Name = "MovementController";
}


function MovementController:KnitStart()
    -- declare variables, wait for children, etc.
end


function MovementController:KnitInit()
    -- no yielding
end

```


#### Util

Knit comes with a lot of neat libraries at your disposal. 

##### Maid

[Maid](https://medium.com/roblox-development/how-to-use-a-maid-class-on-roblox-to-manage-state-651bf74de98b) is a your most useful tool for tracking and disposing of potential memory leaks. It will disconnect events, destroy objects, you name it! Read up on it more [here](https://sleitnick.github.io/Knit/util/maid/). It is really easy to use and take advantage of so make sure to do so.

##### Promises

[Promises](https://eryn.io/roblox-lua-promise/lib/) allows for a safe way to catch values that may exist in the future but might not exist now. Not only does this safeguard you and your code but it prevents yielding until a value may exist. Useful for managing asynchronous code.


```lua
local HttpService = game:GetService("HttpService")

local function httpGet(url)
	return Promise.new(function(resolve, reject)
		local success, result = pcall(HttpService.GetAsync, HttpService, url)

		if success then
			resolve(result)
		else
			reject(result)
		end
	end)
end

httpGet("https://www.google.com"):Then(function(result)
    -- if successful then
end):Catch(function(error)
    -- rejected, lets handle rejection
end)

print("The rest of the script is not waiting on this result! Wooo")
```


##### Signal and RemoteSignal and RemoteProperty

Lets group these together for simplicity. 

`Signal` and `RemoteSignal` are wrappers for BindableEvents and RemoteEvents respectively. They provide more functionality for developers to take advantage which saves us time in the long run. 

`RemoteProperty` are in a way similar to RemoteEvents but instead expose a read-only `Value` to the client that is updated whenever it is set. The client can `:Get()` this Value at any point and also setup a `.Changed` connection to know as soon as the Value object is changed.

All three of these should be used instead of BindableEvents. and RemoteEvents. If you're wondering "what about RemoteFunctions?" refer back to the `Services` portion of this page.

[Signal](https://sleitnick.github.io/Knit/util/signal/)


[RemoteSignal](https://sleitnick.github.io/Knit/util/remotesignal/)


[RemoteProperty](https://sleitnick.github.io/Knit/util/remoteproperty/)

