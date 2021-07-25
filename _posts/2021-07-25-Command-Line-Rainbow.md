
title: Command Line Rainbow
published: true
---

Hi again! In this post I'll explain some code that I've developed to mess with the `cmd` colours on windows.

<img src="https://user-images.githubusercontent.com/74323383/126896875-0ae9ac47-6c8d-45e7-a427-db4c3e22cc03.png" class="border" />

The funciton of the script is mainly to draw kind of a rainbow, and it adapts to every window size.
I'll be using two separate files for this :

* <span class="green">rainbow.bat</span>
* <span class="green">parse.pl</span>

>The `.bat` extension is used for batch files, which are, in short, "shell scripts for the windows cmd". [This excellent page](https://www.robvanderwoude.com/)  will teach you everything you want to know and much more (for real, those people are crazy).<br>
>The `.pl` extension is the one used for Perl scripts. If you are interested in learning the language, you can visit [this site](https://www.perltutorial.org/).<br>

## <span class="pink">rainbow.bat</span>

Let's brake down this script in two parts : obtaining the number of columns that the window has (A.K.A. how many characters fit in one line) and displaying the rainbow.

### Number of columns

To check the number of columns that the window is using, the command `mode` will be very useful. The documentation of the command is [here](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mode).

With `mode con` we can output some info about the size of the window. It's been very useful to me to redirect this info into a temporary separate file `ModeTemp.txt` (it will be deleted when the process ends) in order to parse it afterwards with `parse.pl`, but I'll get into the parser later. For now, let's just assume that we can obtain the number of columns with the other script.
* * *
```bat
mode con >> ModeTemp.txt
```
* * *

Once obtained, the number of columns has to be assigned to a variable so that it can be used later on. This seems trivial, but it is definetly not when the scripting language is batch.

To accomplish this variable assignment, it is required to loop through the parser's output, which will be stdout. When the loop is finished, the variable `cols` will be set to the last line that it read.

* * *
```bat
for /f "tokens=*" %%a in ('perl parse.pl') do (
    set cols=%%a
)
```
* * *

Some pretty detailed information about how this `for /f` works can be found [here](https://ss64.com/nt/for_f.html).

### Displaying the rainbow

Here is where the real magic happens. I'll have to break this down into different sections once again so that it can be well understood :

The first thing I'm doing after obtaining the columns is checking if the number is odd or even.
* * *
```bat
set /A mod=%cols% %% 2
if %mod%==1 set /A cols-=1
```
When loop ends :
```bat
if %mod%==1 echo [%a%m [0m
```
* * *
>This step will let me add a "speed feature" by echoing either one or two "coloured blank spaces" on each loop iteration.<br>
>In case the number is even, no changes are required because by echoing 2 characters at a time, we'll reach the end of the line.<br>
>However, if it is an odd number and no modifications are made, the whole line + 1 character will be echoed when `speed=2`, thus colouring the next line.<br>
>The required change in this last case is pretty simple, when the number of columns is odd, substract 1 to it and echo outside of the loop.<br>

Let's now get to this famous loop:

To be able to make a determined amount of iterations in batch, one option is to loop through a range with `for /L`. Once again, for more info look at [this website](https://ss64.com/nt/for_l.html).

Depending on the "speed" (that we obtain through stdin), each iteration will either echo 1 character `cols` number of times, or 2 characters `cols`/2 number of times.

There is one more thing to be understood before looking at the code : <span class="pink">how do you use colours?</span> `cmd` can display colours thanks to <span class="green">ANSI sequences</span>, which you can learn more about [checking out this page](https://www.robvanderwoude.com/ansi.php).

* * *
```bat
for /L %%n in (1 %speed% %cols%) do (
  if %speed%==1 echo | set /p="[%a%m [0m"
  if %speed%==2 echo | set /p="[%a%m  [0m"
)
set /A a+=1
if %a%==47 set /A a=41
```
* * *
>The variable `a` you can see is used to modify the colour, once an entire line is printed in `<ESC>[%a%m`, `a` is incremented and the loop starts back again, now with the colour `a + 1`.

You may be asking yourself where is this loop re-started with a different color, and you're right, let's get to that :

In batch file there exist these things called `labels`, which you can think of as kind of a checkpoint, you can use `go to <label>` and the program will execute the code that is under that `label`.
The name of the only label I use in the script is `colorLoop`, and it is what lets us go back to the beginning of the loop with a different value of `a` every time.

Here you can see the full code of <span class="pink">rainbow.bat</span> :
* * *
```bat
@echo off
set /A a=41

mode con >> ModeTemp.txt

set /p speed="Select speed : [1/2]"
for /f "tokens=*" %%a in ('perl parse.pl') do (
    set cols=%%a
)

set /A mod=%cols% %% 2
if %mod%==1 set /A cols-=1

del ModeTemp.txt

:colorLoop
for /L %%n in (1 %speed% %cols%) do (
  if %speed%==1 echo | set /p="[%a%m [0m"
  if %speed%==2 echo | set /p="[%a%m  [0m"
)
if %mod%==1 echo [%a%m [0m
set /A a+=1
if %a%==47 set /A a=41
goto :colorLoop
```
* * *
>In case you're wondering, `@echo off` is used to turn off echo, this way the commands won't be displayed through the terminal

<style>
  .border {   
  border-width: 0px;
  border-color: #7FFF00;
  border-style: groove;
  box-shadow: 0px 0px 8px #ADFF2F} 
 .green {
 text-shadow : 0px 0px 4px #ADFF2F }
 .pink {
 text-shadow : 0px 0px 4px #ff6699 }
</style>

