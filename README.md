# The Soul of Etria

Credits for this project go to:  
Fredrick Farouk -- Solo Coder and Manager,  
Hassan Saheb -- Main Spriter and Voice Actor, and  
Omar Alighieri -- Trailer Editor and Assistant Spriter.

---

## Development Timeline

I worked on this over the winter holiday at the end of 2025.  

This project took a lot of my time, roughly four hours per day for several weeks, as I was the only coder.

---

## Technical Overview

Many features in the game were at least somewhat challenging to implement, so here are some descriptions.

---

### Procedural Generation

A highlight of the game was the biome generation using noise maps.

I created four noise maps: height, moisture, temperature, and latitude.
Originally, I used Perlin noise, but realised its edges were too jagged and it did not make for clean biome transitions, making me switch to OpenSimplex noise.

Then, I constructed a classifier to take in the four noise values at a point and determine the best biome. This was hardcoded with several if statements.

After, the biomes are converted into the colour of the cell on the world grid. This conversion is dependent on the current *SOUL* value.

---

### _SOUL_ and Rippling

Rippling was quite complicated. To make it look good, I wanted the ripple to move through the ground to the enemy, which required several functions to avoid this glitching out terrain generation, and to allow collision detection with these ripples.

Making the inside of the change colours and zoom out was even more complicated, as in the same instance a certain biome could have two different colours. The key idea to resolve this was to generate a new map for the current chunk (with the updated soul colours) and pass that into the ripple. As the ripple moves, the old circumference (after the circumference expands) has every cell inside it be replaced from the map to the new map. This continues until the ripple reaches the farthest corner. The issue with this was a minor bug where moving into a different chunk while the ripple is active leads to the new map replacing the old map in the *wrong position*, leading to a visual bug. I decided that patching this would take up too much time that could be spent adding features.

Furthermore, _SOUL_ changing also affect the world generation itself, not just the overlay. The biome of an area actually changes (height is increased and moisture is decreased), leading to more barren areas at very low _SOUL_.

Regarding collision detection, I used some basic trigonometry to find the circumference of the circle. At every point on the chunk, we calculate its relative position to the ripple's origin, find the hypoteneuse and see if its within the thickness of the circle to the radius.

Another instance of trigonometry helping out was (π+arctan2(dy,-dx))mod2π to find the direction to the NPC in the Arrow class.

---

### Sound and Event Handling

Such a complex game was not meant to be made in `tkinter`, so lag was expected. However, since many different things were going on simultaneously, I needed to utilise `tkinter`'s after ID system. Essentially every moving object has its own after ID.

Many issues arose from garbage collection not collecting things when I wanted it to, and forgetting to clear the after ID was one of the reasons for this. To solve this, I imported the `gc` module to tell me when things were still stored in memory.

Regarding sound, I allotted channels for each specific task. Soundtracks went into channel 0, NPC dialogue went to channel 1, sound effects went into channel 2, boss damage sounds went into channel 3 and cutscene audios used channel 4.

---

### NPC Dialogue

This was probably the most annoying game feature to implement. Firstly, for the sake of the plot my teammates had come up with, I needed each NPC to
a) have multiple dialogues,
b) have branching trees of potential dialogue depending on the player's decisions,
c) allow the player to speak in response to the NPC,
d) have branching trees *in the middle of one dialogue* depending on a player's decision,
e) take textual input from a player, and
f) output sound.

To do this, I came up with a system. Here is an example of calling an NPC class:

`hassan = NPC(
        name="hassan",
        pos=hassan_spawn_point,
        sprite_name_normal="hassan_sprite_normal",
        sprite_name_flipped="hassan_sprite_flipped",
        dialogue_lines = [[
        {"text": "Woah! Is that an adventurer?"},
        {"text": "Woah! You startled me there. What would a snide little individual like you want with the great Hassan?"},
        {"text": "I met your friend Omar, and he told me you had a \"sacred key\".",
         "speaker": "player"},
        {"text": "Oh really? Friend, huh. Is that what he tells people? Okay then, I'll help you, but only on one condition."},  # this is parsed as an f-string in the class
        {"text": "Well, alright! What's that?",
         "speaker": "player"},
        {"text": "You bring me Omar, and let me have him. You see, me and him go way back. Best of friends, we were. Then, things got messy."},
         {"text": "Why? What happened?",
          "speaker": "player"},
        {"text": "He betrayed me; tried to leave me for dead at the hands of goblins! But I escaped. So what'll it be? Me, or Omar?\n\nType a for me, and b for Omar. Choose wisely.",
         "input": {
             "validator": lambda v: v in ("a", "b"),  # only allow "a" or "b"
             "save_to": "hassan_betray_choice",
            "required_message": "You must type 'a' or 'b'!"}},
        {"text": "Something feels off.",  # little hack
         "speaker": "player",
         "audio": "no"},
        {"text": "Something feels off.",  # little hack
        "speaker": "player",
        "audio": "no"}
            ]],
     appended_dialogue_lines = [  # for first encounter
         {"a": [{"text": "Okay. I'll see what I can do.",
         "speaker": "player"},
        {"text": "I thought so."},
        {"text": "Bring me Omar; you'd better.", "audio": "no"}],
        "b": [{"text": "I don't believe you. Omar would never!",
         "speaker": "player"},
        {"text": "Oh really? Well then. YOU PICKED WRONG!"},
        {"text": "What?",
         "speaker": "player", "audio": "no"}]},
        {"a": [{"text": "I mean, you could've brought him alive at least. But... it wouldn't have made much of a difference. HA!"},  # second encounter
        {"text": "You're welcome, I guess.",
        "speaker": "player"},
        {"text": "Here. Take my key. You've earned it.", "audio": "no"}]}
        ],
    prox1=20,
    prox2=7,
    chosen_adder="hassan_betray_choice"
)`

As you can see, the dialogue is implemented by a massive system of nested dictionaries, including at the highest level full dialogues and at the lowest level individual strings, with each string encoding the speaker, whether audio should play and whether an input is required (more specifically, what exactly is required in that input to avoid security errors, and a message if an input is invalid).
The exact way I implemented all of this can be seen in the `NPC` class.

---

These were just some examples of the technical hurdles in this project.

## Lessons Learned

There are a few things I learned, one of which being that I should maintain multiple files always. My intention was to pack a full game into one file so that installation would be easy, but I understand now this precaution mostly led to difficulty in coding.  

I also found that I should probably stick to using the intended tools for the job; Python's `tkinter` is not very fast, and for a 2D game with many features, that was an issue.  

It was a lot of fun to try to push `tkinter` to its limits though.

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

The key feature of the game, as indicated by the title, is the "_SOUL_" feature. This is a "health bar" of sorts that sits at the top of the screen. The player's SOUL heavily affects generation with less water, hotter biomes, darker and redder colours, more dangerous surroundings and more enemies at lower _SOUL_.  

Further leaning into the "Ripple Effect", losing or gaining SOUL (from healing potions, etc.) sends out a "ripple". This is an effect that moves directly through the ground in a different colour depending on the player's current _SOUL_. It is difficult to describe in the email, however it is more clear in the trailer and the actual game. It serves as the player's main attack on bosses and enemies.

To avoid making the synopsis too technical, I will not go into details on how the infinite world is stored while avoiding (heavy) lag, how exactly the OpenSimplex noise generation is implemented, how NPCs, Bosses and Items are made into general class structures for ease of continuation, how the game updates continuously without a general builtin update method in python, or anything else to that effect. For those types of details, see the code and my (sometimes) helpful comments.

In general, the game has too many features to discuss in a short synopsis, but hopefully the main purpose has been outlined here. More detail is shown in the trailer and gameplay video, and of course, you can always use the executable to play the game yourself. Have fun!

## Usage

Clone the repository and run `The Soul of Etria.py`.
