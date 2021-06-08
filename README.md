# Simple-Matrix-Bot-Lib
(Version 1.3.x)

simplematrixbotlib is a Python 3 library for quickly building Matrix bots. It uses [matrix-nio](https://github.com/poljar/matrix-nio) as its Matrix client library.

[View on Github](https://github.com/KrazyKirby99999/simple-matrix-bot-lib) or [View on PyPi](https://pypi.org/project/simplematrixbotlib/) or
[View docs on readthedocs.io](https://simple-matrix-bot-lib.readthedocs.io/en/latest/)

# Installation
### To use it, simplematrixbotlib can be either installed from pip or downloaded from github.</br>
Installation from pip:
```
python -m pip install simplematrixbotlib
```
Download from github:
```
git clone --branch master https://github.com/KrazyKirby99999/simple-matrix-bot-lib.git
```

# Example Usage
```python
import simplematrixbotlib as botlib
import os

creds = botlib.Creds("https://home.server", "user", "pass")
bot = botlib.Bot(creds)

prefix = '!'

async def echo(room, message):
    """
    Example function that "echoes" arguements.
    Usage:
    example_user- !echo say something
    echo_bot- say something
    """
    match = botlib.MessageMatch(room, message, bot)
    if match.not_from_this_bot() and match.prefix(prefix) and match.command("echo"):
        await bot.api.send_text_message(room.room_id, match.args)

bot.add_message_listener(echo)

bot.run()
```
More examples can be found [here](examples).

# Features
## Complete:
- ### Login to homeserver - bot automatically login upon the execution of bot.run() 
    ```python
    import simplematrixbotlib as botlib
    
    creds = botlib.Creds("home.server", "user", "pass")
    bot = botlib.Bot(creds)
    bot.run() #Logs in during the execution of this line
    ```
- ### Join room on invite - bot automatically join rooms that the bot is invited to upon execution of bot.run(), or upon invite if the bot is running
- ### Send message - bot can send messages in response to other messages, and can also run other code in response to messages as well as filter the messages that the bot responds to
    ```python
    import simplematrixbotlib as botlib
    
    creds = botlib.Creds("https://home.server", "user", "pass")
    bot = botlib.Bot(creds)

    async def say_something_to_a_message_not_from_bot(room, message): #Must be an "async" function with (room, message) arguments
        if not message.sender == bot.async_client.user_id: #Optional, prevents the bot from reacting to its own messages
            await bot.api.send_text_message(room.room_id, "something") #Send a message containing "something" to room
    bot.add_message_listener(say_something_to_a_message_not_from_bot) #Listen for messages, can have as many message listeners as needed, each added using bot.add_message_listener

    bot.run()
    ```
- ### Execute action based on criteria - "match filters" can be used to  specify which messages for the bot to respond to
    ```python
    import simplematrixbotlib as botlib
    import os

    creds = botlib.Creds("https://home.server", "user", "pass")
    bot = botlib.Bot(creds)

    prefix = '!' #Create prefix for commands

    async def echo(room, message):
        """
        Example function that "echoes" arguements.
        Usage:
        example_user- !echo say something
        echo_bot- say something
        """
        match = botlib.MessageMatch(room, message, bot) #Create an object of the botlib.MessageMatch class
        if match.not_from_this_bot() and match.prefix(prefix) and match.command("echo"): #Add match filters
            await bot.api.send_text_message(room.room_id, match.args)#Execute action
    bot.add_message_listener(echo)

    bot.run()
    ```
## In Progress:
- ### Add examples

## Planned:
- ### Add more match filters
- ### Preserve sessions
- ### Support for Encrypted Rooms
- ### More

# Dependencies
## Python:
- ### matrix-nio >= 0.18.2
## External:
- ### Python >= 3.7
