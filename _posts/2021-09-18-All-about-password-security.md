---
title: Password security and how to choose passwords
published: true
---

More and more security breaches are beign exploited these days, and some of these lead to password leakages, which means that the user's passwords are made public or shared.

In this post I'll cover what's the best practices to carry out when choosing a password, and how to manage all the passwords for all of your accounts.

First of all, I'll address how to deal with passwords for the average user, and afterwards I'll get into some detail of which attacks can be performed, the math involving password strength, and some extra tip to make your account security more robust.

## Dealing with passwords

Let's get onto it, the three factors that affect the strength of your password are the following :

<ul>
  <li>Length of the password</li>
  <li>Character set that will be used</li>
  <li>Predicatbility</li>
</ul>

To get more into detail, you can keep reading and discover which kind of attacks can affect you and what the math behind password security is, but if you only want to have secure passwords without knowing the reason, these are the guidelines to follow :

* * *
Password must be more than 15 characters long.
It needs to include some number, symbol and capital letter.
It doesn't have to be anything related to you (e.g. your favourite team, your birthday, etc.) and it can't be composed of common words such as "password" or "hello".
It isn't a good idea either to substitute letters for numbers as in "h3ll0w0rld", this will be the first thing an attacker tries when performing a dictionary attack (which you can learn about down below).
* * *

Some extra thing that you should take into consideration when choosing your password is the convenience of remembering and typing it. So for example, "QrN}x#hCyBQ7t2)>" if a great password in terms of its robustness, but it is really hard for humans to remember random character strings, and a pain to type it in every time you want to access something.

With all of this said, I'd like to introduce you to password managers, which consist of software that creates and stores an encrypted database of your different passwords. A password manager will keep all of your passwords safe and you will only need to remember the master password to access the database, this is increcibly helpful since it opens the possibility to have randomly generated passwords for all of your accounts, such as "chm(6PM@d+dHHT%q". This way, you only need to follow the convenience guideline for your master password, and use a website to generate all of your other passwords (you won't need to remeber them since they are securely stored).

<img src="https://s3.amazonaws.com/neowin/news/images/uploaded/2017/06/1497027603_keepass_screenshot_2017.jpg" class="border" />

As a last tip for the average user : NEVER RE-USE PASSWORDS. For real, do not ever use the same password for more than one site, each site handles its security differently, and if you use the same password for a dubious website that can have a security breach and for your Amazon account, well, you don't need me to tell you what will happen.

## Types of attacks

To know how passwords can be cracked, first you need to learn how they are stored. The most common way (and the most secure one) passwords are stored inside a company's database is with a hash of the password.
A hashing function or algorithm is a mathematical function that is easy to do in one way, but nearly impossible to undo, it converts a random input into a fixed-length output. What this does is allowing passwords to be stored with a different form than their original one, and it makes it hard for someone who sees this hash to guess what the original password was.

<img src="https://github.com/peixetlift/peixetlift.github.io/blob/master/assets/2021-09-18%2010_15_24-hashing%20function%20-%20Google%20Search%20%E2%80%94%20Mozilla%20Firefox.png" class="border" />

In conclusion, if you hash the same input twice, you will get the same output, but if you have the output you cannot obtain the input. Therefore the way of guessing a password is by hashing some string and comparing this hash to the hash of the original password (provided the attacker has somehow obtained the password hash). This hash comparison is the same procedure that takes place when you enter your password to log in to a site, it calculates the hash of what you entered, and compares it to the hash stored in the database (which means that the attacker can directly try combinations of characters in the log in form of the site without needing to calculate hashes if the website doesn't have any protection).

Getting deeper into detail, let's break down what kinds of attacks you can fall victim of :

<ul>
  <li>Brute force attack</li>
  <li>Dictionary attack</li>
</ul>

### Brute forcing

What a brute force attack consists of, is essentially trying every single combination possible until access is granted. This means trying the combinations :

<ol>
  <li>a</li>
  <li>b</li>
  <li>...</li>
  <li>aa</li>
  <li>ab</li>
  <li>...</li>
  <li>aaa</li>
  <li>aab</li>
  <li>aac</li>
</ol>

This process might seem endless, but since it is a computer and not a human who will be running it, it can literally take seconds to crack a password if it the password isn't long enough and the character set doesn't fit the broadness requirements.

### Dictionary attacks

These kind of attacks are a variant of the brute force ones, they also consist of trying many combinations, but this time the combinations won't be every possible string, they will be a list of common words and patterns taken from lists of previously leaked passwords, common english words, etc. This is why unpredictability of our passwords is of huge importance.

<img src="https://cdn.arstechnica.net/wp-content/uploads/2013/03/IMG_1667.jpg" class="border" />

Once again, millions of combinations can be tried in seconds, so it's not about trying to get fancy with passwords such as "adam12354" instead of "adam12345" (which by the way is a terrible password since it doesn't meet the length, character set or predictability requirements).

## Math

This will probably be the most intense section of the post, by reading this you'll learn to compare the strength of different passwords and the number of combinations that an attacker has to try in order to crack your password.

### Entropy

To talk about password robustness, a little insight into the concept of Shannon's entropy shall be made.
In information theory, entropy is, by definition, the average level of information or uncertainty that a process can output. For example, The entropy of the result of throwing a biased coin will be lower than that of a normal coin, because the outcome we're expecting from the biased coin is easier to guess before the coin has been thrown.
Entropy is calculated through the formula :

<img src="https://www.researchgate.net/profile/Jodi-Kearns/publication/34995295/figure/fig16/AS:669479310729234@1536627714604/Shannons-original-Entropy-equation.png" class="border" />

Thus a standard's coin entropy is H = 1/2 because p = 1/2 and (1 - p) = 1/2 (being p the probability of heads and (1 - p) the one of tails), while a biased coin entropy will be something between 0 and 0'5, depending on p.

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