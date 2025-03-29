## Sprite editing. -  Note that this page is still in progress.
### Explanation
- **Explanation as to how the sprites work
The sprite files cover the in game objects and walls. They are stored in .SPR files, each of which contain multiple images. The sprite data is in hex files, which first list the number of images within the file, then the x,y sizes for each, then the raw data for the files themselves

For example:
03 50 50 60 60 70 70 60 60 60 ....

03 converted to hex would be 3. 0c would be 12. That's the number of images within this sprite.
50 in hex is 80 in decimal, so the first image is 80x80 pixels
60 in hex is 96 in decimal, so the second image is 96x96 pixels
70 in hex is 112 in decimal, so the third image is 112x112

The data after this is the raw image data. So for the above example, if it's 80x80, the next 6400 decimal numbers are the first image. The next 9216 are the second image, the next 12544 are the third image. 

There are no delimiters within the image data itself, you need to split it based on the lengths defined from the starting sizes

Note that once you extract this data you will have the image data, but without the palette. 

- **Palettes
The IMG files relate to the floor and hud (see section on IMG) but I noted that when making edits to these files, it ended up affecting the palette for the sprites on that level. With that in mind, I found that you can select the relevant floor IMG for that level, extract the palette from it, and then apply this palette to the sprites in question.
I will look at adding some code and explanation for this shortly, as when doing it myself I extracted these separately in python first, but I don't want to share any of the original game code, therfore I need to make it so that you can just drag and drop one of the gif files to auto extract the palette. This may take me a little while to get updated into the code, so bear with me.

- **Editor
As this is currently still in progress, and I need to combine and rework some of these tools, however the current breakdown of the tools are as follows:

jsviewer.html - a really simple tool that lets you paste in a string of hex, set the width, and then preview this with a random colour palette. This is superceded, but leaving here for reference as it might be of use to people
imageconvert.html - This will let you load a png file, will apply the palette to it, and then has an export image. This will export the image *into browser local storage*. This can then be imported in the main sprite editor. I know this is a janky setup, and will need to be updated, but that's just the way I have it working for now that was used during my own testing.
1_simple_html_to_open_sprite6.html - This is the main sprite editor. I hard coded in the palette which I will need to strip for sharing here, so I need to get this updated, but I'll put the current code here for now. Please see the following screenshots for explanations:
