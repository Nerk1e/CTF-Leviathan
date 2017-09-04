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
