# 4D Modding: Basic Tutorials
This is tutorials for modding 4D Miner with 4D Modding framework

------------

### Links:
 - [4D Modding web-page](https://gdpseditor.com/4dmodding/ "4D Modding web-page")
 - [4D Modding Discord](https://discord.gg/AmGKpYXBwX "Discord Server")
 - [4dm.h (modding headers/framework)](https://github.com/Tr1NgleDev/4dm.h "4dm.h (modding headers/framework)")
 - [4DMod-Example](https://github.com/Tr1NgleDev/4dmod-example "4DMod-Example")
 - [4DFly Mod Source-Code](https://github.com/Tr1NgleDev/4DFly "4DFly")
------------

### Basic Tutorial:

#### # Making mod project:
First, Clone [4DMod-Example](https://github.com/Tr1NgleDev/4dmod-example "4DMod-Example") project. (I recomend using Visual Studio 2022)

Then in the project you open `main.cpp`

There you can find some basic defines at start of file.
```cpp
//#define DEBUG_CONSOLE // Define that if you want debug console

// Mod Name. Make sure its same as mod's folder name
#define MOD_NAME "4DMod"
#define MOD_VER "1.0"
```
If you want to have debug console window to use `std::cout` or `printf`, you should define `DEBUG_CONSOLE`. Make sure to comment it before sharing the mod.

You can also see `MOD_NAME` and `MOD_VER` defines. They are used for 4DModding-Core to load mod and show it on list. You are supposed to change these defines to your mod name and version. Also when sharing and using the mod, mod folder is supposed to have the same name as `MOD_NAME` for the mod to work correctly.

### # Basics (Hooking):
One of the main things to know is **Hooking**

You need it to Hook into game functions and do stuff to game.

4D Modding framework uses [MinHook library](https://github.com/TsudaKageyu/minhook "MinHook library") for hooking.
To be exact, it uses [MultiHook](https://github.com/m417z/minhook "MultiHook") branch.

In `Main_Thread` function there is already basic example of hooking.
For example: Hooking `Player::update()` function
```cpp
Hook(reinterpret_cast<void*>(base + idaOffsetFix(0x7EB40)), reinterpret_cast<void*>(&Player_update_H), reinterpret_cast<void**>(&Player_update));
```
Lets look at arguments..

First argument is **address/offset** to function in the game (or `target`). There will me more info about **offsets** later. It uses `target` to replace it with our custom "`detour`" function.

Which is the next argument. **Hook Function** (or `detour`). Its a pointer to our custom function with our own code. This function supposed to have same arguments as the original game function.

Last argument is **Original Function** (or `original`). Its basically just a pointer to original game function that we replaced with `detour`. You can use it inside of the custom function to call original function after or before your code.

Example of `detour` and `original` functions:
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

You probably remember `target` argument which is address/offset to a function in game, right?

So... How do you get one?

Well first of all you are able to go into `4dm.h` headers and find offset you need. Which is the easiest way.

But what if there isnt one? (since 4dm.h is still in development and doesnt have everything)

Then you need to use some kind of RE (Reverse Engineering) tool.

For example: 
 - [IDA Pro](https://hex-rays.com/ida-pro/ "IDA Pro"). It isnt free. ~~But you can easily pirate it :trollface:~~
 
 or

 - [Ghidra](https://github.com/NationalSecurityAgency/ghidra "Ghidra") which is free.

Im not going to tell **how** to do this stuff. If you want, just google it lol.

----

There will be more tutorials later. But for now this is it.

You can ask questions in [4D Modding Discord Server](https://discord.gg/AmGKpYXBwX "Discord Server"), if you have any.
