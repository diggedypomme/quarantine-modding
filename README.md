# In course of updating! this might take me a few days so please be patient, but let me know if you have a particular part you need advice on

# Modding Quarantine 1994
Documenting my findings on modding the 1994 game Quarantine

This is the github repo to go with my subreddit : https://www.reddit.com/r/QuarantineModding/  
And youtube video: https://www.youtube.com/watch?v=nTVZKp6tR-Y

Note that this is a work in progress and I will keep updating it as I fix up and get my code and explanations uploaded. This may take some time.

## Quarantine:
I have been playing around with a game that made a big impact on me as a kid, the 1994 game Quarantine. Think Doom in a Taxi, with huge maps and sandbox gameplay. This github is a work in process to document my findings, and share the scripts that I used to do so. I am currently in the process of documenting all of this, which will take some time, so this will be filled out as I go.

> **Note** that this is the first time that I have worked on any of this, so my methods might not be correct, but I got there in the end. I am open to any suggestions for improvements, or for new findings.

## Main summary:
- .MAP - The game has 5 maps, each of which have the name "*CITY.MAP". Please see the mapping section for more info.  
- .BLK - The map file refers to tile references. These are configured within similarly named .BLK files. Please see the BLK section for more into  
- .SPR - The "sprite" files contain the image data for walls and the in game objects/enemies  
- .IMG - The IMG files contain the texture data for hud images and floors  
- .GAM - The .GAM files contain the file saves.  

### Maps
- **The game maps are large 2d arrays. Each number in the array corresponds to one "tile", with 0 being a blank tile. The numbers go up to 65,535 (the largest value for an unsigned 16 bit int).**  
For each map there are actually 128 different model setups (this could just be a section of road, or maybe a couple of buildings etc). Each of these have 4 different texture sets.  
The first 128 (0-127) are the first texture set. The next 128 follow the same texture set. I'm not sure what the difference is between 10 and 138 for example, but I assume this is something to do with floors changing.  
For my map previewer, as I had the screenshots generated, I simplified this by condensing down the 0-127 range.

The first two bytes (UINT16LE, little-endian) in the file are the width. The next two are the height, then the following data contains all of the map data itself. 

### Sprites
The sprite files cover the in game objects and walls. They are stored in `.SPR` files, each of which contain multiple images. The sprite data is in hex files, which first list the number of images within the file, then the x,y sizes for each, then the raw data for the files themselves.

For example:
03 50 50 60 60 70 70 60 60 60 ....

- `03` converted to hex would be `3`. `0c` would be `12`. That's the number of images within this sprite.  
- `50` in hex is `80` in decimal, so the first image is **80x80 pixels**.  
- `60` in hex is `96` in decimal, so the second image is **96x96 pixels**.  
- `70` in hex is `112` in decimal, so the third image is **112x112 pixels**.

The data after this is the raw image data. So for the above example:  
- If it's **80x80**, the next **6400 decimal numbers** are the first image.  
- The next **9216** are the second image.  
- The next **12544** are the third image.

There are no delimiters within the image data itself; you need to split it based on the lengths defined from the starting sizes.

### Palettes
The `.IMG` files relate to the floor and HUD (see section on `.IMG`), but I noted that when making edits to these files, it ended up affecting the palette for the sprites on that level. With that in mind, I found that you can select the relevant floor `.IMG` for that level, extract the palette from it, and then apply this palette to the sprites in question.

### IMG extraction.
- I got the tip for this by looking at https://moddingwiki.shikadi.net/wiki/Quarantine - "*.IMG GIF with modified signature ("IMAGEX" instead of standard "GIF87a")"
- This is done by opening the IMG in a hex editor and changing the start block to read GIF87a. This can then be read with paint, or any other image processing tool. Note that the palettes are used for the sprites, so you NEED to retain the palette when you save it. I found Gimp to be the best tool for doing this.

### Save game hacking
- These are just long hex files, again 16 bit little endian. I don't know the correct way to know what refers to which variables, however I wrote a script using pyautogui which loads the game, then runs through a bunch of different setups - standing still for x, moving for x, shooting for x) and then compares these to see what changed across them. This can then highlight relevant areas for you to investigate changing. This worked well, so I can now edit the main gun ammo, and the x coord. I need to work out exactly how to map this coord onto the main map, and I also need to work out the y coord, and the rotation.. I'd like to be able to set a spawn point within the map, and then toggle back to the game but have it write the coord to a save so you can just load this to continue from there.  
This will probably be one of the last things I upload because I feel the other things are higher priority.

### Tile setups
-to be updated, but basically to get some explanation here:

-the first number is 01. Not sure of the relevance of this.  
-Then after this it is the number of tiles (128).  
-After this it then goes through each of the tiles one by one, giving the values for "Floor Count", "Wall Count","Texture data", "Sprites"

Floor: 18 bytes  
Wall: 18 bytes  
Texture data : 2  
Sprites: 12 bytes  

There is no delimiter, so you need to work out the length of each subsequent tile by calculating the sum of:  
- number of floors * 18  
- number of walls * 18  
- Texture data *2  
- sprite count * 12

By working out the start stops for each of the tiles, you can then parse the data. I will add more info on this shortly.

### Doom
to be updated , but see the doom explanation subfolder for queries as to why I did it with all actors, and the pros and cons of doing it differently.
