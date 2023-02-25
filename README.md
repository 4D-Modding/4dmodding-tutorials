# 4D Modding: Basic Tutorials
This is a tutorial for modding 4D Miner with the 4D Modding framework.

------------

### Links:
 - [4D Modding web-page](https://gdpseditor.com/4dmodding/ "4D Modding web-page")
 - [4D Modding Discord](https://discord.gg/AmGKpYXBwX "Discord Server")
 - [4dm.h (modding headers/framework)](https://github.com/4D-Modding/4dm.h "4dm.h (modding headers/framework)")
 - [4DMod-Example](https://github.com/4D-Modding/4dmod-example "4DMod-Example")
 - [4DFly Mod Source-Code](https://github.com/4D-Modding/4DFly "4DFly")
------------

### Basic Tutorial:

#### Making a Mod Project:

First, clone the [4DMod-Example](https://github.com/4D-Modding/4dmod-example "4DMod-Example") project, or use it as repo template. (I recommend using Visual Studio 2022 for making mods).

Then, open `main.cpp`, where you can find some basic defines at start of the file.
```cpp
//#define DEBUG_CONSOLE // Uncomment this if you want a debug console

// Mod Name. Make sure it matches the mod folder's name
#define MOD_NAME "4DMod"
#define MOD_VER "1.0"
```
If you want to have a debug console window to use `std::cout` or `printf`, you need to define `DEBUG_CONSOLE`. Make sure to comment it out before sharing the mod.

You can also see the `MOD_NAME` and `MOD_VER` defines. They are used by 4DModding-Core to load a mod and show it on the mod list. You need to change these defines to *your own* mod name and version. Additionally, when sharing and using the mod, the mod folder's name needs to match `MOD_NAME` in order for the mod to work correctly.

### Basics (Hooking):

One of the most important concepts to understand is **Hooking**, which is necessary to **hook** into game functions in order to modify the game.

The 4D Modding framework uses the [MinHook library](https://github.com/TsudaKageyu/minhook "MinHook library") for hooking.
To be exact, it uses the [MultiHook](https://github.com/m417z/minhook "MultiHook") branch.

In the `Main_Thread` function there is already a basic example of hooking. For example, hooking the `Player::update()` function:
```cpp
Hook(reinterpret_cast<void*>(base + idaOffsetFix(0x7EB40)), reinterpret_cast<void*>(&Player_update_H), reinterpret_cast<void**>(&Player_update));
```
Let's look at the arguments...

The first argument is the **address/offset** of the function in the game (or `target`). More info about **offsets** can be found [below](#obtaining-a-function-offset). The `target` argument is used to replace the function with our custom "`detour`" function, which is provided by the next argument.

The second argument is the **Hook Function** (or `detour`). It's a pointer to our custom function with *our own* code. This function supposed to have the same arguments as the original game function.

The last argument is the **Original Function** (or `original`). It's basically just a pointer to the original game function that we are replacing with `detour`. You can use it inside of the custom function to call original function before or after your own code.

Here is an example of `detour` and `original` functions:
```cpp
// original function
void(__thiscall* Player_update)(Player* self, GLFWwindow* window, World* world, double dt); 

// detour/hook function
void __fastcall Player_update_H(Player* self, GLFWwindow* window, World* world, double dt) 
{
	// Your code that runs every frame here (it only calls when you play in world, because its Player's function)

	Player_update(self, window, world, dt);
}
```
(also make sure to change `_thiscall*` in `original` function to `_fastcall*` if the original function is static)

----

### Obtaining a Function Offset

You probably remember the `target` argument, which is the address/offset of a function in the game, right?

So... how do you get one?

Well first of all, you can to go into the `4dm.h` headers and find the offset you need, which is the easiest way.

But what if the offset isn't there? (since 4dm.h is still in development and doesnt have everything)

In that case, you need to use some kind of RE (Reverse Engineering) tool.

For example: 
 - [IDA Pro](https://hex-rays.com/ida-pro/ "IDA Pro"). It isnt free. ~~But you can easily pirate it :trollface:~~
 
 or

 - [Ghidra](https://github.com/NationalSecurityAgency/ghidra "Ghidra") which is free.

Im not going to explain **how** to do this stuff. If you want, just google it lol.

----

There will be more tutorials later. But for now this is it.

You can ask questions in [4D Modding Discord Server](https://discord.gg/AmGKpYXBwX "Discord Server"), if you have any.
