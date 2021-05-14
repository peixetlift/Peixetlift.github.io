
Hello everyone! This post will be about a famous bash script the purpose of which is to harm a system by consuming a big amount of its memory.

This script is commonly known as "Fork Bomb" and this is how it works :

<ol>
  <li> We create a function called `:`</li>
  <li> We write the function's code, which will be calling itself recursively and creating two child processes, which can't be terminated by themselves because they are running in the background</li>
  <li> We call the function to trigger the bomb</li>
</ol>

```
:(){ :|: & };:
```

### Detailed explanation

<ul>
  <li>`:()` creates/defines the function with name `:`</li>
  <li> Inside `{}`we write the code, which will be `:|: &` --> `:` calls the function, `|` pipes its output into another call to `:`, and we tell it to run in the background with `&`</li>
  <li> More specifically, what `&` does is disowning the function, which makes the child processes unable to be auto-killed when the father process is terminated. You can find more information [in here](https://datacadamia.com/lang/bash/process/ampersand)</li>
  <li> Finally, we use the `;` as a command separator and we run the function with ':'</li>
</ul>
