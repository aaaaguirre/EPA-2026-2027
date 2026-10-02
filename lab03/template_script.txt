#!/bin/bash

# this is a comment

# for loop to count to 10
for c in {1..5}; do
	echo "Count: $c"

	# if does not use == it uses -eq
	# note the spaces around if [ ]
	if [ $c -eq 3 ]; then
		echo "found the third item"
	fi
done

# how do we pass parameters from the command line
# into this bash script. 
# we use the notation $1, $2 etc to represent
# the first, second etc parameter into this script
if [ -z $1 ]; then
	echo "You didn't pass any paraemters to $0"
else
	echo "You passed in $1 to $0"
fi

# heres a brand new command: 
# it calls ps -ef, then pipes it into word counter
# then stores the result in ct
ct=$(ps -ef | wc -l)
echo "There are $ct processes running on this machine"

# we could put the counting code ito a special function
# lets call that function counter()

counter() {
	ct=$(ps -ef | wc -l)
	echo "There are $ct processes running on this machine"
}

# now we call the counter function
counter

# lets pass a parameter into counter function
counter 5
# but the counter function doesn't yet use the value passed
# how can we get a function to use a parameter?

# here's another example
counter_v2() {
	echo "the parameter passed into this function is $1"
}

counter_v2 5

# lets look at a more complex example
# suppose we want to monitor the file system for 
# the %age of used space.

# this looks very complex
# note the use of 2 pipes (|) and three commands - this is a form of chaining
used_percent=$(df -h | awk 'FNR==2{print $5}' | sed 's/%//')
echo "the root partition is $used_percent percent used"
# lets walk through it and have a look
