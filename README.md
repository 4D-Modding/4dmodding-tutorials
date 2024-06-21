# 4D-Modding Basic Tutorials
Some Basic Tutorials on modding 4D Miner using the 4D-Modding framework. This ONLY includes the basics, like installing 4DModLoader and creating a mod project.

------------

### Links:
 - [4D-Modding website](https://www.4d-modding.com/ "4D-Modding website")
 - [4D-Modding Discord](https://discord.gg/AmGKpYXBwX "Discord Server")
 - [4dm.h (the modding headers)](https://github.com/4D-Modding/4dm.h "4dm.h (the modding headers)")
 - [4DMod Example](https://github.com/4D-Modding/4dmod-example "4DMod Example")
 - [4D-Fly Mod Source-Code](https://github.com/4D-Modding/4DFly "4D-Fly")
 
------------

## Where to begin?

 First of all, you have to install 4DModLoader. For that you need to go to the [4D-Modding website](https://www.4d-modding.com/ "4D-Modding website") and download the 4DModLoader Installer.

 After you have installed the 4DModLoader, you can install mods into the `./mods/` directory and play.

## How to create mods?

 To write mods, you need to have at least some knowledge of C++ and how games work. You also require Visual Studio 2022 to actually build the code. (or you could use the GitHub Actions workflow I added recently, which will compile the mod .dll for you each time you push new changes to the repo)

 To create mods, first, go to the [4DMod Example](https://github.com/4D-Modding/4dmod-example "4DMod Example") repo and use it as a template to create your own repo. Call it whatever you want your mod to be called. You can make the repo private or public (just normal GitHub stuff).

 After you have created a repo for your mod based on the [4DMod Example](https://github.com/4D-Modding/4dmod-example "4DMod Example"), you can `git clone --recursive` your mod repository.
 In there you will find a `4DMod.sln` file. That's a Visual Studio 2022 project/solution file, which you will need to open.

 And now you are ready to start coding the mod!

 ### Hooking

 *Hooking* is when you override one of the game functions to add your own code into it, either before or after the original code (or removing the original code completely)

 There are a few hooks by default in the 4DModExample `main.cpp`
 You can either stick to those or remove them (tho do not remove the StateIntro::init() one. since it initializes some important stuff)

 There are a few macros for creating hooks:
  - `$hook(returnType, cl, function, ...)` Creates a hook to a member function (__thiscall). The one you are going to use most of the times.
  - `$hookByFunc(returnType, className, func, ...)` A variation of the `$hook` macro. Instead of hooking function by `className::functionName` you directly pass it a value from `Func` namespace. Useful for cases when the class you are trying to hook is inside `_Nested`. I will try to get rid of this macro next update. But it is what it is now.

  For both of those you have a pointer called `self`, which basically plays the role of `this`

  - `$hookStatic(returnType, cl, function, ...)` Creates a hook to a static function (__fastcall).
  - `$hookStaticByFunc(returnType, func, ...)` A variation of the `$hookStatic` macro. Instead of hooking function by `className::functionName` you directly pass it a value from `Func` namespace. Useful for cases when the class you are trying to hook is inside `_Nested`. I will try to get rid of this macro next update. But it is what it is now. 

  For all of those, to call the original function's code, you just call `original()`.

  For example hooks, check the 4DModExample hooks.
