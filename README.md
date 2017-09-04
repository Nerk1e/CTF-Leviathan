# CTF-Leviathan
OverTheWire Leviathan

![ctf leviathan1](https://user-images.githubusercontent.com/31230311/29848806-f03e7d88-8cf0-11e7-848f-3411c3241bee.png)

![ctf leviathan2](https://user-images.githubusercontent.com/31230311/29848816-fcfaede0-8cf0-11e7-9695-f125dbb8515c.png)

```markdown
#Leviathan 0

#First we need to log into the level using ssh given and the username

ssh leviathan.labs.overthewire.org -p2223 -l leviathan0

#the level starting instructions gives us the password

leviathan0

#Since there is not any instructions besides to check the directories
#I start to look at the files with ls and ls -la

ls 

ls -la

#I can now see there is some files
#but I also want to see what cat /etc/leviathan_pass does

cat /etc/leviathan_pass

#it says that it is a directory
#lets go in and read the directory

cd /etc/leviathan_pass

#we can now see that there are all the levels and passwords. Cool beans
#so there are 7 levels and passwords
#lets see if any of the files early hold anything like ".backup"
#I need to exit then log back in with the password to go back to the files
#then access .backup

cd .backup

#this shows some files and bookmarks
#it mentions leviathan1 so lets grep to quick find leviathan1
#and see what's in leviathan1 bookmarks

grep leviathan1 bookmarks.html

#The quick find reads 
#" "http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A> "
# "rioGegei8m " is next password
```


![ctf leviathan3](https://user-images.githubusercontent.com/31230311/29848824-093f4b96-8cf1-11e7-9964-de98b3244812.png)

```markdown
#Level 1

#lets log into the ssh with the new password

ssh leviathan.labs.overthewire.org -p2223 -l leviathan1

rioGegei8m

#lets see if we can snoop around like the first level using ls -la

ls -la

#there is a file that is highlighted in red saying "check"
#lets cd check to see what's inside it

cd check

#that didnt work, so lets ./check

./check

#it's asking for a password
#typing in leviathan1 doesn't work so we may need to look around
#let's try ltrace ./check that can show 
#the background string code that is looking
#for the password

ltrace ./check

#it is showing the code and asking for the password again.
#when you put in the wrong code it shows the password it is looking for in
#the " strcmp " which stands for string to compare, and it is comparing 
#the bad password I typed in, with the right password which is " sex " 
#so lets ./check again and type in the right password

./check

sex

#the section up next is blank so we need to see the user.
#to see who the user is that we are logged in as
#you can use " id " or "whoami"
#I'll use " id "

id

#now it shows 
#" uid=12001(leviathan1) gid=12001(leviathan1) euid=12002(leviathan2) groups=12002(leviathan2),12001(leviathan1) "
#this shows the users who were last in
#similar to the first level, lets cat the "/etc/leviathan_pass"
#and see if anything is in leviathan2 since it is showing up in the 2nd userid

cat /etc/leviathan_pass/leviathan2

#it shows a password " ougahZi8Ta "
```

![leviathan4](https://user-images.githubusercontent.com/31230311/30011834-f20e5770-9108-11e7-9665-31e6e6a178ea.png)
![leviathan5](https://user-images.githubusercontent.com/31230311/30011840-0023c62e-9109-11e7-8925-b2cef0422ed4.png)


```markdown
#Level 2


#We are ready for level 2 and logging in and password

ssh leviathan.labs.overthewire.org -p2223 -l leviathan2

ougahZi8Ta

#Alright, we are back in the void again and must ls or ls -la to see
#what we can do next

ls

#After ls it shows a red highlighted word saying "printfile"

ls -la

#shows the other files but still the red highlighted "printfile"
#cd ./printfile does not do anything, but lets try cat

cat printfile

#symbols and words show up, some of it seems encrypted
#lets see what print file does with ./printfile

./printfile

#says ***file printer*** usage: ./printfile filename
#so in a way using ./printfile is just like using cat
#but it is saying "You can't have that file"
#and trying a leviathan2 file says permission denied
#I am going to make a folder called "ladybug" 
#and a txt file called "derp" in the temporary file
#then run a ltrace and see what comes up in the background script

mkdir /tmp/ladybug && touch /tmp/ladybug/file\ derp.txt

cd /tmp/ladybug

ls -la

ltrace ~/printfile "file derp.txt"

#we can see that there is "access(derp.txt,4)
#it seems like it is checking to see if what is being typed matches up
#to whatever the "4" stands for
#we know that there is cat on file derp
#so we need to exploit "file" and bring the password into our tmp file
#lets try to symbolic link the password "file" to our tmp file
#using the "/etc/leviathan_pass/leviathan3"

ln -s /etc/leviathan_pass/leviathan3 /tmp/ladybug/file

ls -la

#we can see it so lets try printfile and "file derp.txt"

~/printfile "file derp.txt"

#it worked and now we got the password

Ahdiemoo1j

```

```markdown
#Level 3

ssh leviathan.labs.overthewire.org -p2223 -l leviathan3

Ahdiemoo1j

#well this is a very short and unique level
#if we ls -la it 
#it will show level3 file

ls -la

#if we type ./level3 it will ask for a password

./level3

#we do not know the password

ltrace

#if we ltrace then it will show the code and have strcmp("h0no33", "kakaka") 
#type kakaka for password

kakaka

#it will be wrong and then it will show strcmp("kakaka\n", "snlprintf\n")   
#if you type snlprintf it will say correct and say
# puts("[You've got shell]!"[You've got shell]!

snlprintf

#so snlprintf seems to be the password needed to open ./level3
#type ./level3 and put the password in then ask whoami

./level3

whoami

#typing in whoami will show the username leviathan4
#then type cat /etc/leviathan_pass/leviathan4 for password to next level

cat /etc/leviathan_pass/leviathan4

#password is vuH0coox6m
```

```markdown
#Level 4


#log in and use the password

ssh leviathan.labs.overthewire.org -p2223 -l leviathan4

vuH0coox6m

#we see a trash file so lets dig into it
#we can use cd .trash since ./trash will not work

cd .trash

#we are in, let's ls -la to see whats in the trash file

ls -la

#there is a bin in the trash....
#lets see whats in the trash bin

./bin

#there is a binary output that we can decode
# " 01010100 01101001 01110100 01101000 00110100 01100011 01101111 01101011 01100101 01101001 00001010"
#Using Binary to Ascii Text Converter site http://www.binaryhexconverter.com/binary-to-ascii-text-converter
#the output reads " Tith4cokei "
```

```markdown
#Level 5


#log in and use the password

ssh leviathan.labs.overthewire.org -p2223 -l leviathan5

Tith4cokei

#now that we are logged in, lets take a look around

ls -la

#we see a highlighted file called leviathan5
#we cannot get in as easily as the last level with cd or ./ 
#so we need to go another route
#we do see that there is a /tmp/file.log so that is a clue
#we are making a symlink taking the tmp file and looking for the password in leviathan6

ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log

#now lets see if there is a code like last file with ./ and use the leviathan5 file

./leviathan5

#we got a password " UgaoFee4li "
```

```markdown
#Level 6

#before logging in, make sure to download nano " sudo apt-get install nano "
#Log into the level and put in the password

ssh leviathan.labs.overthewire.org -p2223 -l leviathan6

UgaoFee4li


#let's see what's in here with ls -la

ls -la

#we see leviathan6 file similar to what level5 had
#i did ./leviathan6

./leviathan6

#it is asking for a 4 digit code we don't have
#trying to cd into it will not work
#using ltrace we can see there is " <4 digit code> "
#we may need to make a tmp file to work around into it using script
#so far kali linux does not like nano sh so needed to write a manual script

for i in {0000..9999}; do ./leviathan6 $i; done

#this makes us look through the code for specific pins between 0000-9999 to find for the 4 digit
#if we type whoami it will let us know we are now inside the leviathan6 file
#if we cat the password, we can cat for the password now

cat /etc/leviathan_pass/leviathan7

#password is ahy7MaeBo9

#We are here. We finally got to level 7
```


```markdown
#Level 7


#Let's log in one final time with the ssh and password

ssh leviathan.labs.overthewire.org -p2223 -l leviathan7

ahy7MaeBo9

#Let's ls -la

ls -la

#there is a final file called CONGRATULATIONS
#Let's see if anything is in it with cat

cat CONGRATULATIONS

#This is what it says
#Well Done, you seem to have used a *nix system before, now try something more serious.
#(Please don't post writeups, solutions or spoilers about the games on the web. 
#Thank you!)

#ehhhhh.... We finished. :)
```
