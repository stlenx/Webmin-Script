#!/bin/sh

#My boyfriend made me comment this artwork. This is so sad, forced labor.

echo "Instalation for Webmin, are you sure you want to continue? [y/n]"
read response

#Check the user's response using a poggy woggy switch statement

case $response in

y)	echo "Installing Webmin"

	#Count will be use to check if the sources file contains the necesary line
	count=0
	echo "Checking sources file"
	while read line; do
	if [ "$line" = "deb http://download.webmin.com/download/repository sarge contrib" ]
	then
		#If the line was found then we must exit and make the count 0.
		#I don't think break here does a good job as i believe it just exits from the if instead of the while but whatever idk how to do that :P
		echo "Repository was found"
		count=0
		break
	else
		#If the line was not found we'll increment the count.
		((count=count+1))
	fi

	done < /etc/apt/sources.list

	#We now check if count is bigger than 0, if it is it means the sources file does not contain the line we require, so we just add it.
	if [ $count -gt 0 ]
	then
		echo "Repository was not found, adding..."
                echo "deb http://download.webmin.com/download/repository sarge contrib" >> /etc/apt/sources.list
	fi

	installed=$(dpkg -l | grep webmin)

	#Checking if webmin is already installed
	if [ "$installed" = "" ]
	then
		echo "Downloading webmin key..."

		#Get key bullshit idk i just took this from the teacher's website idk why we need this but cool.
        	wget -qO - http://www.webmin.com/jcameron-key.asc | apt-key add -

		echo "Key downloaded and added. Installing webmin"

	        #Hopefully if all the before steps have been successful :pray:
	        apt update
	        apt install webmin -y
	else
		echo "Looks like you already have Webmin installed. The script will stop now"
	fi

	;;
n)	echo "Not installing Webmin then lol"
	#LOL you trying to execute the wrong script :rofl:
	;;
"i guess")
	echo "You guess? Come back when youre sure of what youre doing sYs aDmIn"
	#Yeah this guy is just haha funny look how funny i am no fucking way im so funny
	;;
*)	echo "I did not understand. Terminating process"
	#What the atcual fuck did you just write in the console you undeveloped monkey. It was a simple yes or no, i don't even ask you to put the full word just the letter y or n smh my head.
	;;
esac
