---
title: Command Line Rainbow
published: true
---

Hi again! In this post I'll explain some code that I've developed to mess with the cmd colours on windows.

The funciton of the script is mainly to draw kind of a rainbow, and it adapts to every window size.
I'll be using two separate files for this :

* rainbow.bat
* parse.pl

>The `.bat` extension is used for batch files, which are, in short, "shell scripts for the windows cmd". [This excellent page](https://www.robvanderwoude.com/)  will teach you everything you want to know and much more (for real, those people are crazy).
>The `.pl` extension is the one used for <span class="green">Perl</span> scripts. If you are interested in learning the language, you can visit [this site](https://www.perltutorial.org/).

<span class="pink">rainbow.bat</span>
* * *
```
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
Let's brake down this script in two parts : obtaining the number of columns that the window has (A.K.A. how many characters fit in one line) and displaying the rainbow.


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

