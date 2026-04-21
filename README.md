# The Soul of Etria

## Credits

Credits for this project go to:  
Fredrick Farouk -- Solo Coder and Manager,  
Hassan Saheb -- Main Spriter and Voice Actor, and  
Omar Alighieri -- Trailer Editor and Assistant Spriter.

---

## Development Timeline

I worked on this over the winter holiday at the end of 2025.  

This project took a lot of my time, roughly four hours per day for several weeks, as I was the only coder.

---

## Lessons Learned

There are a few things I learned, one of which being that I should maintain multiple files always. My intention was to pack a full game into one file so that installation would be easy, but I understand now this precaution only led to difficulty in coding.  

I also found that I should probably stick to using the intended tools for the job; Python's tkinter is not very fast, and for a 2D game, that was an issue.  

It was so much fun to try to push tkinter to its limits though.

The prompt for the hackathon was "Ripple Effect", which may give context on some game features and some points in the synopsis.

---

## Synopsis

Here is the synopsis I presented in my submission:

---

This project, The Soul of Etria, is a 2D open-world procedurally generated adventure game written entirely in Python over the course of two months of hard work by the team, primarily using the tkinter library. This is unusual because tkinter is a graphical user interface (GUI) toolkit typically used for simple applications and menus, rather than real-time games, which are usually built with libraries such as pygame, or game engines that handle all game logic for the user such as Unity or GoDot. Despite this, tkinter is used here to handle rendering, input, and interface elements, with pygame.mixer used only for audio, as it gives a specific terrain generation "pixel-art"/old-school style that I couldn't find something else that could replicate faithfully.  

The game contains many special features, such as:

- Several Non-Player Character (NPC) dialogues that use player text input, the player's decision changing the entire plotline [Ripple Effect] (e.g. choosing Omar or Hassan at the second dialogue),  
- The ability to pick up, use and drop various items into their locations such as the potion and the poem,  
- The fitting of a full game, including sprite files and audio files into a single Python file, NPC and general enemy spawning logic that depends on biomes,  
- A "regenerate" button that allows the player to select their "seed"; the same "seed" always makes the same map. Players may therefore share enjoyable seeds with special terrain features with each other to find new biome structures,  
- Arrows that constantly spin around the player, indicating the direction (and distance: bigger arrow means closer) of their key destinations, usually NPCs (lots of trigonometry here),  
- Various buttons including a Help menu, Credits menu, Arrow menu (which turns on and off specific unwanted arrows to avoid cluttering the screen) and a Regenerate/Seed entry menu for regenerating a seed,  
- Multiple boss fights with different special attacks for each boss,  
- Playing the game in any way is shown on the Discord sidebar (unless the player does not have a discord account), along with the player's current soul,  
- An objectives bar indicating the player's current goal,  
- A hidden feature (clicking the biome indicator expands it to show the current temperature, latitude, moisture, and height),  
- Enemies and bosses spawn dynamically depending on biome and proximity, each with unique attack patterns,  
- The soundtrack AND game icon adapts to the player’s _SOUL_ level (explained below), reinforcing the game’s atmosphere,  
- and many more.  

---

## Core Mechanic: _SOUL_ System

The key feature of the game, as indicated by the title, is the "_SOUL_" feature. This is a "health bar" of sorts that sits at the top of the screen. The player's SOUL heavily affects generation with less water, hotter biomes, darker and redder colours, more dangerous surroundings and more enemies at lower _SOUL_.  

Further leaning into the "Ripple Effect", losing or gaining SOUL (from healing potions, etc.) sends out a "ripple". This is an effect that moves directly through the ground in a different colour depending on the player's current _SOUL_. It is difficult to describe in the email, however it is more clear in the trailer and the actual game. It serves as the player's main attack on bosses and enemies.

To avoid making the synopsis too technical, I will not go into details on how the infinite world is stored while avoiding (heavy) lag, how exactly the OpenSimplex noise generation is implemented, how NPCs, Bosses and Items are made into general class structures for ease of continuation, how the game updates continuously without a general builtin update method in python, or anything else to that effect. For those types of details, see the code and my (sometimes) helpful comments.

---

In general, the game has too many features to discuss in a short synopsis, but hopefully the main purpose has been outlined here. More detail is shown in the trailer and gameplay video, and of course, you can always use the executable to play the game yourself. Have fun!

## Usage

Clone the repository and run "The Soul of Etria.py". That is it.
