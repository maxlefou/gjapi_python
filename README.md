# Game Jolt Trophy for Python

Original file by vncastanheira@gmail.com
version 1.1

#### This program is free software: you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License

This is a script and a sample that lets you use Game Jolt trophies and scoring in your Ren'Py games.
You can find a sample game that uses it here: https://github.com/maxlefou/GameJolt_API_for_RenPy_Sample_Game If you are new with python and Ren'Py coding, I suggest you to get an eye on the sample game's code while reading this README file.

## Basic instructions
This document will explain you how to use the GameJolt API in Ren'Py in the most basic way. I'm not really a python coder, just having small notions of it, but I could make this. It means it's the most simpliest way to make work the API in Ren'Py. If you are an advanced python developper, feel free to get inspired and go further into it. ^^
I will also assume you are starting a game right away from the launcher. It might work with games already made and advanced but it should work anyway. Just be careful of the additions you put into it and make backups !
Also, i can't guarantee a good support on it because I am myself a n00b at it. Oh, and excuse my poor english, it's not my main langage.
Anyway, let's start.

### The file py_gjapi.rpy
This file is a basic python script that includes a class that will interact with the GameJolt API. It has been made by vncastanheira and you can find the script alone here: https://github.com/maxlefou/gjapi_renpy It is a fork of the file py_gjapi.py made by vncastanheira that you can find here: https://github.com/vncastanheira/gjapi_python

As is, the script should work with anything in python out of the box. But sadly, for Ren'Py, it's not the case. It needs a conversion. Fortunately, a very simple conversion. (this is why this fork is made) 
Simply copy this file into the game folder of your game.

### Editing options.rpy
Now that the main script is there, we're gonna add some data in options.rpy. It's very easy, again. At the very end of the file, add the following :

persistent.GJGameKey = "<i>game's public key</i>"<br>
persistent.GJPrivateKey = "<i>game's private key</i>"<br>
persistent.GJusername = ""<br>
persistent.GJtoken = ""

in the first two lines here, put the public key and private key of your game. Very important. It's one of the 4 things the API absolutely needs to work !
And that's all.. Yep…

### Setting the things in script.rpy
Now we're gonna edit the script.rpy file.
At the beginning of the game, we must indicate what values we want to send to GameJolt in order to connect to it later. First we can only give the public and private keys of the game but it's ok, we'll see later for the username and token.

First, we're going to define some variables that we'll use later :

define GJusername = None<br>
define GJtoken = None<br>
define GJscore = 0<br>
define GJcheck1 = None<br>
define GJcheck2 = None

Now, at the first action of label start:, put the line below :

$ GJset = GameJoltTrophy(GJusername, GJtoken, persistent.GJGameKey, persistent.GJPrivateKey)

This sets a variable named GJset. This will be used for all interactions with the API. It saves the current username, the current token and the two keys of the game. We put it in first so we can set the game keys to it.

Now we need a way to indicate the username and token of the user. There is multiple ways of doing that in Ren'Py, but for this one, I use the renpy.input. This command is often used to set the name of a character or stuff.

$ GJusername = renpy.input("Type here your username and press Enter.", "")<br>
$ GJtoken = renpy.input("Now, type here your game token and press Enter.", "")

Now that it's done, we're going to set the persistent variables from their normal ones, just in case.

$ persistent.GJusername = GJusername<br>
$ persistent.GJtoken = Gjtoken

Now we're going to interract directly with the commands of GJset. They require all the same thing : being in a python block. Like the following that will authenticate the username and token :

python:<br>
&nbsp;&nbsp;GJset.changeUsername(persistent.GJusername)<br>
&nbsp;&nbsp;Gjset.changeUserToken(persistent.Gjtoken)

Be careful with the indentation too !

Now we can do a check of the authentication, to see if the credentials are good. The authenticateUser command is made for that. It will return either True or False depending if the authentication failed or not.

python:<br>
&nbsp;&nbsp;GJcheck1 = GJset.authenticateUser()<br>
&nbsp;&nbsp;if GJcheck1 == True:<br>
&nbsp;&nbsp;&nbsp;&nbsp;"Yay it worked!"<br>
&nbsp;&nbsp;else:<br>
&nbsp;&nbsp;&nbsp;&nbsp;"Oh no! It didn't worked!"
    
Now, in the same way, we're going to open the session. You can do it directly in the block « if GJcheck1 == True » or you can use labels and jumps.

python:<br>
&nbsp;&nbsp;GJcheck2 = GJset.openSession()<br>
&nbsp;&nbsp;if GJcheck2 == True:<br>
&nbsp;&nbsp;&nbsp;&nbsp;"Yay it worked!"<br>
&nbsp;&nbsp;else:<br>
&nbsp;&nbsp;&nbsp;&nbsp;"Oh no! It didn't worked!"

Now that the session is open, we can at last use trophies and scoring. YAY ! Oh, by the way, to close the session, just use the code below :

python:<br>
&nbsp;&nbsp;GJset.closeSession()<br>
&nbsp;&nbsp;persistent.GJusername == None<br>
&nbsp;&nbsp;persistent.GJtoken == None<br>
&nbsp;&nbsp;GJusername == None<br>
&nbsp;&nbsp;GJtoken == None

### Add trophies
This one is insanely simple. Here's the example code :

python:<br>
&nbsp;&nbsp;Gjset.addAchieved(<i>trophy id</i>)

Simply put this and put the trophy's ID.

### Add scores
This is pretty simple too. Unless you have multiple tables. Let's suppose the score you want your users to submit is in the variable Gjscore :

python:<br>
&nbsp;&nbsp;GJset.addScores(GJscore, GJscore, table_id=<i>table id</i>, extra_data='', guest=False, guestname='')

In this setting, it will send the score set in Gjscore in the table id with the most basic options. To know more about this and the storage data part, you can check on the script's sources.
