# SirChatalot

This is a Telegram bot that uses the OpenAI [ChatGPT API](https://platform.openai.com/docs/guides/chat) to generate responses to messages. I just wanted to test the API and I thought that a Telegram bot would be a good way to do it. Some things can be unnecessary complicated. 

This bot can also be used to generate responses to voice messages. Bot will convert the voice message to text and will then generate a response. Speech recognition is done using the OpenAI [Whisper model](https://platform.openai.com/docs/guides/speech-to-text). To use this feature, you need to install the [ffmpeg](https://ffmpeg.org/) library. Voice message support won't work without it.

## Getting Started
* Create a bot using the [BotFather](https://t.me/botfather).
* Clone the repository.

### Automatic steps (for Linux)
* Run the command `./first_run.sh` from the root directory of the repository. This will install the required packages, create the configuration file and start the bot.
  * Script will ask you to enter the Telegram token and the OpenAI secret key as well as other optional parameters. You can also change them later in the `./data/.config` file (see *Configuration*).
  * [ffmpeg](https://ffmpeg.org/) will not be installed automatically. You need to install it manually. Voice message support won't work without it.

### Manual steps
* Install the required packages by running the command `pip install -r requirements.txt`.
* Install the [ffmpeg](https://ffmpeg.org/) library for voice message support (for converting .ogg files to other format) and test it calling `ffmpeg --version` in the terminal. Voice message support won't work without it.
* Create a `.config` file in the `data` directory using the `config.example` file in that directory as a template.
* Run the bot by running the command `python3 main.py`.

`Whitelist.txt`, `banlist.txt`, `.config`, `chat_modes.ini`, are stored in the `./data` directory. Logs rotate every day and are stored in the `./logs` directory.

Bot is designed to talk to you in a style of a knight in the middle ages by default. You can change that in the `./data/.config` file (SystemMessage).

There are also some additional styles that you can choose from: Alice, Bob, Charlie and Diana. You can change style from chat by sending a message with `/style` command, but your current session will be dropped. 
Styles can be set up in the `./data/chat_modes.ini` file. You can add your own styles there or change the existing ones.

## Configuration
The bot requires a configuration file to run. The configuration file should be in [INI file format](https://en.wikipedia.org/wiki/INI_file) and should contain the following fields:
```
[Telegram]
Token = 0000000000:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
AccessCodes = whitelistcode,secondwhitelistcode
RateLimitTime = 3600
GeneralRateLimit = 100

[OpenAI]
SecretKey = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
ChatModel = gpt-3.5-turbo
ChatModelPrice = 0.002
WhisperModel = whisper-1
WhisperModelPrice = 0.006
Temperature = 0.7
MaxTokens = 500
AudioFormat = wav
SystemMessage = You are a helpful assistant named Sir Chat-a-lot, who answers in a style of a knight in the middle ages.
MaxSessionLength = 15
ChatDeletion = False
```
* Telegram.Token: The token for the Telegram bot.
* Telegram.AccessCodes: A comma-separated list of access codes that can be used to add users to the whitelist. If no access codes are provided, anyone who not in the banlist will be able to use the bot.
* Telegram.RateLimitTime: The time in seconds to calculate user rate-limit. Optional.
* Telegram.GeneralRateLimit: The maximum number of messages that can be sent by a user in the `Telegram.RateLimitTime` period. Applied to all users. Optional.
* OpenAI.SecretKey: The secret key for the OpenAI API.
* OpenAI.ChatModel: The model to use for generating responses (Chat can be powered by `gpt-3.5-turbo` for now).
* OpenAI.ChatModelPrice: The [price of the model](https://openai.com/pricing) to use for generating responses (per 1000 tokens, in USD).
* OpenAI.WhisperModel: The model to use for speech recognition (Speect-to-text can be powered by `whisper-1` for now).
* OpenAI.WhisperModelPrice: The [price of the model](https://openai.com/pricing) to use for speech recognition (per minute, in USD).
* OpenAI.Temperature: The temperature to use for generating responses.
* OpenAI.MaxTokens: The maximum number of tokens to use for generating responses.
* OpenAI.AudioFormat: The audio format to convert voice messages (`ogg`) to (can be `wav`, `mp3` or other supported by Whisper). Stated whithout a dot.
* OpenAI.SystemMessage: The message that will shape your bot's personality.
* OpenAI.MaxSessionLength: The maximum number of user messages in a session (can be used to reduce tokens used). Optional.
* OpenAI.ChatDeletion: Whether to delete the user's history if conversation is too long. Optional.

Configuration should be stored in the `./data/.config` file. Use the `config.example` file in the `./data` directory as a template.

## Styles
Bot supports different styles that can be triggered with `/style` command.  
You can add your own style in the `./data/chat_modes.ini` file or change the existing ones. Styles are stored in the INI file format.  
Example:
```
[Alice]
Description = Empathetic and friendly
SystemMessage = You are a empathetic and friendly woman named Alice, who answers helpful, funny and a bit flirty.

[Bob]
Description = Brief and informative
SystemMessage = You are a helpful assistant named Bob, who is informative and explains everything succinctly with fewer words.

```
Here is a list of the fields in this example:
* Alice or Bob: The name of the style. 
* Description: Short description of the style. Is used in message that is shown when `/style` command is called.
* SystemMessage: The message that will shape your bot's personality. You will need some prompt engineering to make it work properly.

## Using GPT-4
You can use GPT-4 if you got an access to it. To do that, you need to change the `OpenAI.ChatModel` and change `OpenAI.ChatModelPrice` field to `ChatModelPromptPrice` and `ChatModelCompletionPrice` (Prompt and completion prices are different for GPT-4) in the `./data/.config` file:
```
...
[OpenAI]
ChatModel = gpt-4
; ChatModelPrice = 0.002 - delete this line
ChatModelPromptPrice = 0.03
ChatModelCompletionPrice = 0.06
...
```
ChatModelPrice calculates for the whole message, so it is not representative in this case. Use ChatModelPromptPrice and ChatModelCompletionPrice instead. They calculate for the prompt and completion separately.

Models can be found here: https://platform.openai.com/docs/models/gpt-4  
Prices can be found here: https://openai.com/pricing

Using GPT-4 will require more money, but it will also give you more power. GPT-4 is a much more powerful model than GPT-3.5-turbo. It capable of generating longer responses and can be used for more complex tasks.

*Note:* GPT-4 is still in limited beta and is not available to everyone. You need to get an access to it first. You can do that by filling out the form here: https://openai.com/waitlist/gpt-4-api  
Also, you can not use GPT-4 image input right now, it will be available in the future (I'll update bot when it will happen).

## Running the Bot
To run the bot, simply run the command `python3 main.py`. The bot will start and will wait for messages. 
The bot has the following commands:
* `/start`: starts the conversation with the bot.
* `/help`: shows the help message.
* `/delete`: deletes the conversation history.
* `/statistics`: shows the bot usage.
* `/style`: changes the style of the bot from chat.
* `/limit`: shows the current rate-limit for the user.
* Any other message (including voice message) will generate a response from the bot.

Users need to be whitelisted to use the bot. To whitelist yourself, send an access code to the bot using the `/start` command. The bot will then add you to the whitelist and will send a message to you confirming that you have been added to the whitelist.
Access code should be changed in the `./data/.config` file (see *Configuration*).
Codes are shown in terminal when the bot is started.

## Whitelisting users
To add yourself to the whitelist, send the bot a message with one of the access codes (see *Configuration*). The bot will then add you to the whitelist and will send a message to you confirming that.
Alternatively, you can add users to the whitelist manually. To do that, add the user's Telegram ID to the `./data/whitelist.txt` file. 
If no access codes are provided, anyone who not in the banlist will be able to use the bot.

## Banning Users
To ban a user you should add their Telegram ID to the `./data/banlist.txt` file. Each ID should be on a separate line. 
Banlist has a higher priority than the whitelist. If a user is on the banlist, they will not be able to use the bot and the will see a message saying that they have been banned.

## Rate limiting users
To limit the number of messages a user can send to the bot, add their Telegram ID and limit to the `./data/rates.txt` file. Each ID should be on a separate line.
Example:
```
123456789,10
987654321,500
111111,0
```
Rate limit is a number of messages a user can send to the bot in a time period. In example user with ID 123456789 has 10 and user 987654321 has 500 messages limit. User 111111 has no limit (overriding `GeneralRateLimit`).  
Time period (in seconds) can be set in the `./data/.config` file in `RateLimitTime` variable in `Telegram` section (see *Configuration*). If no time period is provided, limit is not applied.  
General rate limit can be set in the `./data/.config` file in `GeneralRateLimit` variable in `Telegram` section (see *Configuration*). If no general rate limit is provided, limit is not applied for users who are not in the `rates.txt` file. To override general rate limit for a user, set their limit to 0 in the `rates.txt` file.  
Users can check their limit by sending the bot a message with the `/limit` command. 

## Deleting Conversation History
To delete the conversation history on the server, send the bot a message with the `/delete` command. The bot will then delete the conversation history and will send a message to you confirming that the history has been deleted. After that it will be a new conversation from the bot's point of view.
Conversation history in the Telegram chat will not be affected.

## Generating Responses
To generate a response, send the bot a message (or a voice message). The bot will then generate a response and send it back to you.

## Using Docker
You can use Docker to run the bot. You need to build the image first. To do that, run the following command in the root directory of the project after configuring the bot (see *Configuration*):
```
docker compose up -d
```
This will build the image and run the container. You can then use the bot as described above.  
To stop the container, run the following command:
```
docker compose down
```

## Warinings
* The bot stores the whitelist in plain text. The file is not encrypted and can be accessed by anyone with access to the server.
* The bot stores chat history in as a pickle file. The file is not encrypted and can be accessed by anyone with access to the server.
* Configurations are stored in plain text. The file is not encrypted and can be accessed by anyone with access to the server.
* The bot can store messages in a log file in a event of an error. The file is not encrypted and can be accessed by anyone with access to the server.
* The bot temporarily stores voice messages in `./data/voice` directory. The files are deleted after processing (successful or not), but can remain on the server if the event of an error. The files are not encrypted and can be accessed by anyone with access to the server.
* The bot is not designed to be used in production environments. It is not secure and was build as a proof of concept and for ChatGPT API testing purposes.
* The bot will try to continue conversation in the event of reaching maximum number of tokens by creating summary of the conversation and using it as a prompt for the next response. This can lead to the bot anwering poorly.
* The bot is using a lot of read and write operations with pickle files right now. This can lead to a poor performance on some servers if the bot is used by a lot of users. Immediate fix for that is mounting the `./data/tech` directory as a RAM disk, but in a event of a server shutdown, all data will be lost.
* Use this bot at your own risk. I am not responsible for any damage caused by this bot.

## License
This project is licensed under [GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html). See the `LICENSE` file for more details.

## Acknowledgements
* [OpenAI ChatGPT API](https://platform.openai.com/docs/guides/chat) - The API used for generating responses.
* [OpenAI Whisper API](https://platform.openai.com/docs/guides/speech-to-text) - The API used for speech recognition.
* [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot) - The library used for interacting with the Telegram API.
* [FFmpeg](https://ffmpeg.org/) - The library used for converting voice messages.
* [pydub](https://github.com/jiaaro/pydub) - The library used for finding the duration of voice messages.