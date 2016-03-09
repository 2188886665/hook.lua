# hook.lua

*Module for [hooking](https://en.wikipedia.org/wiki/Hooking) and intercepting function calls.*

Hooks can be added to any function. If multiple hooks are added to a single function,
then each hook is executed in the order they have been added. If any hook returns
anything other than `nil`, the execution of further hooks and
the actual (intercepted) function is stopped. The returned values of the hook is
returned to the original caller of the function.


## API References

*The following references are based on [hook.lua](https://github.com/MrVallentin/hook.lua) being included using `local hook = require("hook")`.*

```lua
hook.add(func, hook)
```
> Adds a hook to a function, the hook is a function which will be called prior to the
> actual (intercepted) function.
>
> If a function has multiple hooks added, each hook
> will be executed in the order they have been added.
>
> If any hook returns anything other than `nil`, the execution of further hooks and
> the actual (intercepted) function is stopped. The returned values of the hook is
> returned to the original caller of the function.

```lua
hook.call(func, ...)
```
> Executes each hooked function, until a hook returns anything other than `nil`.
> Then the returned values of the hook is returned to the original caller of
> `hook.call()`.
>
> The varargs would be the hooked function's parameters. As when calling the hooked
> function, the arguments is what will be passed through the varargs.

```lua
hook.remove(func, hook)
```
> Remove a hook from a function.


## Example


```lua
local hook = require("hook")


local function test(msg)
	print("test(\"" .. tostring(msg) .. "\")")
end


local function testHook(msg)
	print("testHook(\"" .. tostring(msg) .. "\")")
	
	if msg == "stop" then
		return true
	end
end

local function testHook2(msg)
	print("testHook2(\"" .. tostring(msg) .. "\")")
end


test = hook.add(test, testHook)
test = hook.add(test, testHook2)


test("Hello World")
-- Prints:
-- testHook("Hello World")
-- testHook2("Hello World")
-- test("Hello World")


test("stop")
-- Prints:
-- testHook("stop")


hook.call(test, "Hooks Only")
-- Prints:
-- testHook("Hooks Only")
-- testHook2("Hooks Only")


test = hook.remove(test, testHook)


test("Hello Again")
-- Prints:
-- testHook2("Hello Again")
-- test("Hello Again")
```


#### License

This module is shared under the MIT license, and is therefore free to use, shared, distribute and modify.
See [LICENSE](http://github.com/MrVallentin/hook.lua/LICENSE) for more details.
