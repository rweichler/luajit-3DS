# LuaJIT for 3DS

Note: The JIT does not work yet. The FFI does, though. However: there's a catch. It's kind of a hack.

Essentially, what you need to do is create a global table called "SYMBOLS", where the keys are the names of the C functions/variables, and the values are the addresses.

For instance:

```lua
SYMBOLS = {}
SYMBOLS['printf'] = 0x9812734 -- or whatever
```

Typically, you'd expose this from the C side, like so:

```c
lua_getglobal(L, "SYMBOLS");
lua_pushstring(L, "printf");
lua_pushnumber(L, (uintptr_t)&printf);
lua_settable(L, -3);
```

## To compile:

```
./compile
```
