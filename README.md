#### Warning
I should just put this at the top, because i do not know how you run your setup. But if you swithc into dev mode it will refresh your decks and clear all temporary variables.
This should not be really that much of an issue i think. But some people do stuff like never restarting their PC, so if you rely on some temp variables in your specific setup to be filled (so you never close SAMMI) i do not want to be the reason you reset them.
Take care. I do have an example Deck attached, but without reading the Guide the deck makes no sense anyway and afterward you won't need the example Deck.

[![](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/warning.PNG)](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/warning.PNG)


Also i am no SAMII expert so if i did something weird it might just be inexperience.
Oh and if it is too clunky for you don't feel pressured to use this method.


Quick Rundown For you:

- Calling Button and "function Button" should have the setting "Local Variables Persist" set to OFF
- Calling Button needs to use "Trigger Extension Triggers" and "Wait until Variable exists"
- "Function Button" needs a trigger of type "Extension trigger" the message is basically the name used to call the function
- Inside the "Function Button" you need to use "Set Button Insatnce Variable" to set a variable in the caller, you can pull the "button_id" and "instance_id"






### Features
The SAMMI devs kinda refuse to add an easy way to create functions, so we have to use some fatures to build around that. You could also use extensions but that would require you to have Bridge open for your new commands to work. This method SHOULD work without bridge open from my tests.

I have included an Example Deck in the Folder Deck that can be importet and will work, it SHOULD work.


# Calling Buttons in SAMMI with parameters and returns!
### Developer Mode
For a part of this process we need to access SAMMIs Developer mode.
Without it we will later not be able to set Button Instance variable. You enable it by clicking 10 on refresh in the settings. 
(Dev Commands you add will not vanish from your deck and you can move and copy them like normal.)

[![](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/activate_developer_mode.gif)](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/activate_developer_mode.gif)

### Debugging with SAMMI
Sammi offers a HTML file called "Bridge". Located in your SAMMI install folder under ".\bridge\bridge.html"
If you open this file in your Browser, it will connect to your SAMMI instance and allow you to call some test functions. Bridge is also used for extensions, but this Guide will not go into them. Bridge is not needed for our approach and does not have to be running constantly. I would reconmend not closing the tab while working though, as it will generate new fake user IDs.
[![](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/bridge_ui.PNG)](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/bridge_ui.PNG)



### Preparing files 
This example uses and ini File, for my purposes i will place it at 
"C:\Testfiles\viewerPoints.ini" the preconfigured Deck uses this path. I would reconmend,
to use a global variable for these kinds of paths, but for my convenience i decided against it.

#### User Id in Ini
To make this example work you need to insert the correct user id in your ini. Sadly Bridge doesn't straight up tell you what you User ID is.
Luckily it is easy to figure out.
In the Settings tab simply activate logging (you might not want to show this on stream when using extensions because it will log everything the extensions send back and forth). Now the user ID will be visible in the Log under the Settings page as "user_id".
Replace this id with the two in the example ini if you use it.
[![](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/getrUserId.gif)](https://raw.githubusercontent.com/Nickname863/sammi_functions/refs/heads/main/images/getrUserId.gif)

## Preparing a command
Create a command Button. After creating it ricghtclick it and edit it's settings.
These are a bit of a prteference, as you might want them to Queue or Overlap (consider race cionditions in that case). However we do not want the Button to persist the local variables, every press should be unique.

[![](https://github.com/Nickname863/sammi_functions/blob/main/images/commandButtonSettings.PNG?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/commandButtonSettings.PNG?raw=true)

Now set a trigger. I simply choose "!command *".
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/commandTriggers.PNG?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/commandTriggers.PNG?raw=true)

Inside the Button we need to Pull the "user_id"
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/01_pullUserId.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/01_pullUserId.png?raw=true)

Now we need to create and Object that we will transfer to our "function Button".
For that simply create one with "Create Object" and set some properties with "Set Object Variable" in my case userId and price
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/02_createObject.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/02_createObject.png?raw=true)


Now we get slowly to the meat and potatos of the whole operation. We will utilise "Trigger extension Triggers" to fire an extension trigger an send and object with it. This is very "Trigger Button" but allows uns to provide parameters. The first Parameter will be the Button Id for my example i will use "canAffordAndSpendButton", we have not created the Button we are going to call, but we soon will.

[![](https://github.com/Nickname863/sammi_functions/blob/main/images/03_extension_trigger.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/03_extension_trigger.png?raw=true)

Now we come to the reason why we set the button to not retrain values. By using "Wait until Variable exists" we can wait until our "function Button" returns a value to us and then continue. Thats pretty much the first command done. Do not forget to exit the Button if the user can not afford to spend points.
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/04_waitfor_value.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/04_waitfor_value.png?raw=true)

## Function Button
Time to set up the function Button. Create a Button and give it the ID "canAffordAndSpendButton", you can call it whatever you want.

Here a very important Part, you need to add an extension Trigger

[![](https://github.com/Nickname863/sammi_functions/blob/main/images/05_add_trigger.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/05_add_trigger.png?raw=true)

Naturally you also do not want this Button to have persising variables
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/06_settings.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/06_settings.png?raw=true)

In the Button you can pull all the input variables (price, user_id) and we will also opull the "button_id" and "instance_id" of the calling Button, we need these to later set a Variable in the Original Button.
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/07_pulls.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/07_pulls.png?raw=true)
The next Part is all just logic to read the ini nothing special
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/08_logic_stuff.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/08_logic_stuff.png?raw=true)

And finally the Magic sauce, and the Reason we had to turn on Dev mmode at the beginning of the Guid. We will use "Set Button Instance Variable" to set a new Variable in the Original Button. For this we need the Button_Id, a instance_id that is unique to a button press and of course the name of the variable we ant to set. For us that is "canAffordAndSpend"
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/09_logic_stuff.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/09_logic_stuff.png?raw=true)
The remainder is just the spending logic.
[![](https://github.com/Nickname863/sammi_functions/blob/main/images/10_logic_stuff.png?raw=true)](https://github.com/Nickname863/sammi_functions/blob/main/images/10_logic_stuff.png?raw=true)

Now we can call our Command with Bridge and it should work completly. Good luck and have fun.
