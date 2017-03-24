# LuaJIT for 3DS

Note: The JIT does not work yet. The FFI does, though. However: there's a catch. It's kind of a hack.

Essentially, what you need to do is create a global function called "FFI\_DLSYM".

For instance:

```lua
function FFI_DLSYM(name)
    if name == 'printf' then
        return 0x2398472938 -- or whatever
    end
end
```

Obviously, it would be quite painful to have a huge if-chain with all the possible symbol names. Typically, I do something like this:

```lua
SYMBOLS = {}
function FFI_DLSYM(name)
    return SYMBOLS[name]
end
```

And then, from the C side, I'd do this:

```c
lua_getglobal(L, "SYMBOLS");
lua_pushstring(L, "printf");
lua_pushnumber(L, (uintptr_t)&printf);
lua_settable(L, -3);
```

## To compile:

Install devkitPro/devkitARM. Then,

```
./compile
```
