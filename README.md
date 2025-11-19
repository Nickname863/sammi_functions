### Features
The SAMMI devs kinda refuse to add an easy way to create functions, so we have to use some fatures to build around that. You could also use extensions butthat would require you  to have Bridge open for your new commands to work. This method SHOULD work without bridge open from my tests.


# Calling Buttons in SAMMI with parameters and returns!
### Developer Mode
For a part of this process we need to access SAMMIs Developer mode.
Without it we will later not be able to set Button Instance variable.
[img]

### Debugging with SAMMI
Sammi offers a HTML file called "Bridge". Located in your SAMMI install folder under ".\bridge\bridge.html"
If you open this file in your Browser, it will connect to your SAMMI instance and allow you to call some test functions. Bridge is also used for extensions, but this Guide will not go into them. Bridge is not needed for our approach and does not have to be running constantly. I would reconmend not closing the tab while working though, as it will generate new fake user IDs.
[Image]



### Preparing files 
This example uses and ini File, for my purposes i will place it at 
"C:\Testfiles\viewerPoints.ini" the preconfigured Deck uses this path. I would reconmend,
to use a global variable for these kinds of paths, but for my convenience i decided against it.

#### User Id in Ini
To make this example work you need to insert the correct user id in your ini. Sadly Bridge doesn't straight up tell you what you User ID is.
Luckily it is easy to figure out.
In the Settings tab simply activate logging (you might not want to show this on stream when using extensions because it will log everything the extensions send back and forth). Now the user ID will be visible in the Log under the Settings page as "user_id".
Replace this id with the two in the example ini if you use it.

## Preparing a command
Create a command Button. After creating it ricghtclick it and edit it's settings.
These are a bit of a prteference, as you might want them to Queue or Overlap (consider race cionditions in that case). However we do not want the Button to persist the local variables, every press should be unique.

Now set a trigger. I simply choose "!command *".


Inside the Button we need to Pull the "user_id"
[IMG]

Now we need to create and Object that we will transfer to our "function Button".
For that simply create one with "Create Object" and set some properties with "Set Object Variable" in my case userId and price
[IMG]


Now we get slowly to the meat and potatos of the whole operation. We will utilise "Trigger extension Triggers" to fire an extension trigger an send and object with it. This is very "Trigger Button" but allows uns to provide parameters. The first Parameter will be the Button Id for my example i will use "canAffordAndSpendButton", we have not created the Button we are going to call, but we soon will.

[IMG]

Now we come to the reason why we set the button to not retrain values. By using "Wait until Variable exists" we can wait until our "function Button" returns a value to us and then continue. Thats pretty much the first command done. Do not forget to exit the Button if the user can not afford to spend points.
[IMG]

Time to set up the function Button. Create a Button and give it the ID "canAffordAndSpendButton", you can call it whatever you want.

Here a very important Part, you need to add an extension Trigger

[IMG]

Naturally you also do not want this Button to have persising variables
[IMG]

In the Button you can pull all the input variables (price, user_id) and we will also opull the "button_id" and "instance_id" of the calling Button, we need these to later set a Variable inthe Original Button.
[IMG]
The next Part is all just logic to read the ini nothing special
[IMG]

And finally the Magic sauce, and the Reason we had to turn on Dev mmode at the beginning of the Guid. We will use "Set Button Instance Variable" to set a new Variable in the Original Button. For this we need the Button_Id, a instance_id that is unique to a button press and of course the name of the variable we ant to set. For us that is "canAffordAndSpend"
[IMG]
The remainder is just the spending logic.
[IMG]

Now we can call our Command with Bridge and it should work completly. Good luck and have fun.
