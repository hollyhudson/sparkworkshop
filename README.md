ToDo
===

Look up python simple server.

Goals
===

After the workshop, students will be able to:

* Connect a SparkCore to their wifi and claim it on the spark.io website.
* Write a program in the spark.io IDE and flash their cores.
* Use curl from the command line to control the onboard LED.
* Use Javascript to login to the spark servers and track an access token cookie
* Call a function on the sparkcore from the outside
* Input:
  * Read from buttons
  * Read from knobs
* Output:
  * Drive external LEDs and RGB LEDs
  * Control servos 
* Time/clocks
  * Sync time with server
  * Schedule alarms
* Audio (demo rather than worked example)

Curriculum
===

1) Connect core and flash with code to make the onboard LED blink. (using online IDE)
	* uses cut-and-paste code for HI and LOW
	* talk about IO pins (set pin to IO mode, etc)
2) Go over network diagram for the parts of the system.  Discuss access token and core ID, and show how to get them.
3) Use curl to call a function on the core to change the color of the onboard LED.
	* show how this works via function in code downloaded in 1
4) Pull up breadboard and attach a switch, doing remedial breadboard explanation as necessary.
5) Use curl to read the state of the switch. (done with curl)
6) Now we begin writing code in the IDE and javascript (IDE work can be done outside of the IDE, and we will show those interested how to do so after class.)
7) 

Command Line Guide
===

`spark login`  log into the spark website

`spark help`  get a list of commands

`spark list`  get a list of the cores you own, and whether or not they're online

`spark flash YOURCORENAME demo1`  send your code to a spark core


Example programs
===
You can either paste the source code for these programs into the web IDE
at [spark.io](https://spark.io/) or you can use the
[command line utilities](http://docs.spark.io/cli/).  The command line
has the advantage of allowing you to keep your source code under git or hg
so that you can track questions.


Demo 1
---
Just flashes the blue LED on the board at 1 Hz.  You can do this
through the IDE, or on the command line.  For the command line,
find out your core name with the `list` command and then call the
`flash` command on that name:

    spark list
    spark flash CORE_NAME demo1


Demo 2
---
Exposes a function to allow a remote process (for instance `curl`
on the command line or javascript in a webpage) to turn the LED on
or off.  Also has a variable to allow a remote read of the LED
state. Flash it via the IDE or the command line with:

    > spark flash CORE_NAME demo1

And then use the command line utility to turn the LED on, to check
the state, turn it off, as well as see how it handles invalid input:

    > spark call CORE_NAME set_led 1
    > spark get CORE_NAME led_state
    > spark call CORE_NAME set_led 0
    > spark call CORE_NAME set_led blah

You can also use curl to do this query, although you will need two pieces
of information: your private access token and your core ID.

![Device ID](images/core-id.png)

For the access token, click on the Gear Box icon and copy the token.

![Access Token](images/access-token.png)

For the device id, select the Cores icon (looks like a target reticle)
and then click the dropdown next to the core name to get the id.

With these two pieces of info, you can use `curl` to turn on the LED by
doing an HTTP `POST` with the access token and argument as form
elements.  The `-d` or `--data` argument sends the specified data
in a POST request to the http server.

    > DEVICE_ID=(paste the hex device id token...)
    > ACCESS_TOKEN=(paste the hex access token...)
    > curl "https://api.spark.io/v1/devices/$DEVICE_ID/set_led" \
	-d access_token=$ACCESS_TOKEN \
	-d args="1"

    > curl "https://api.spark.io/v1/devices/$DEVICE_ID/set_led" \
	-d access_token=$ACCESS_TOKEN \
	-d args="0"

You can read the value of the variable by doing an HTTP `GET`, passing
in the access token as a query parameter.  `curl` does `GET` by
default:

    > curl "https://api.spark.io/v1/devices/$DEVICE_ID/led_state?access_token=$ACCESS_TOKEN"


Demo 3
---
This one is not a new program to flash, but a Javascript program to run
in your browser that will talk to the core.  It uses the
[Spark Javascript API](http://docs.spark.io/javascript/) and will require
you to login to get the access token.

If you have multiple devices, they will be listed and you can select
which one to toggle the LED on.  If you use `curl` or some other method
to toggle the LED, it should update live on the web page.


Demo 4
---
Tracks the current time and reads an analog value from a knob connected
to A0.  The javascript interface also demonstrates using cookies to store
the login information so that you don't need to keep putting in your
spark.io details.


Demo 5
---


