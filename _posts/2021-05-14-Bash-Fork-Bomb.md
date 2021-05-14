---
title: Bash Fork Bomb
published: true
---

Hello everyone! This post will be about a famous bash script the purpose of which is to harm a system by consuming a big amount of its resources.

This script is commonly known as "Fork Bomb" and this is how it works :

* We create a function called `:`
* We write the function's code, which will be calling itself recursively and creating two child processes, which can't be terminated by themselves because they are running in the background (this loop will be run forever)
* We call the function to trigger the bomb


<span class="green"> Bash code : </span>
<br>

***
```
:(){ :|: & };:
```
***
<br>
## Detailed explanation

<span class="pink">1.-</span> `:()` creates the function with name `:`<br>
<span class="pink">2.-</span> Inside `{}`we write the code, which will be `:|: &` 
> `:` calls the function, `|` pipes its output into another call to `:`, and we tell it to run in the background with `&`<br>

<span class="pink">3.-</span> More specifically, what `&` does is disowning the function, which makes the child processes unable to be auto-killed when the father process is terminated. You can find more information [in here](https://datacadamia.com/lang/bash/process/ampersand)<br>
<span class="pink">4.-</span> Finally, we use the `;` as a command separator and we run the function with `:`<br>

<span class="pink" style="font-size:35px"><b> DISCLAIMER </b></span><br>

Please do not use this command unless you are ready to see your PC crash and reboot it.<br>
The command doesn't require super-user privileges to run.<br>

I hope this little article was of good interest to you, see you later :)<br>
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

