# Configure fonts, colors and the toolbar

`config` command is provided to allow users to configure the fonts, the color of the text and the background, and the toolbar.

### In a word

```
usage: config [-s font size][-n font name][-b background color][-f foreground 
color][-c cursor color][-dgpr]
```

* `-s font size`: change the size of the text
* `-n font name`: change the font
* `-b background color`: change the background color
* `-c cursor color`: change the color of the cursor
* `-d`: do not save the changes and go back to the precious status
* `-g`: apply the change to all the open windows
* `-p`: save the change to apply it permanently
* `-r`: go back to the initial status: white background and black text

### Font

First you have to prepare your own console font manually.&#x20;
