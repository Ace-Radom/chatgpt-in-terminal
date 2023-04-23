## Chat with GPT in Terminal

[![English badge](https://img.shields.io/badge/%E8%8B%B1%E6%96%87-English-blue)](./README.md)
[![简体中文 badge](https://img.shields.io/badge/%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-Simplified%20Chinese-blue)](./README.zh-CN.md)
[![Platform badge](https://img.shields.io/badge/Platform-MacOS%7CWindows%7CLinux-green)]()
[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg)](https://github.com/RichardLitt/standard-readme)

This project enables chatting with ChatGPT in the terminal.

Markdown content in answers is rendered as beautifully formatted rich text.

Supports history retrieval with the up arrow key, optional multi-line questions, and tokens counting.

Slash (/) commands are available in the chat box to toggle multi-line submit mode, undo the last question and answer, modify the system prompt and more, see the available commands below for details.

Supports saving chat messages to a JSON file and loading them from the file.

![example](https://github.com/xiaoxx970/chatgpt-in-terminal/raw/main/README.assets/small.gif)

Uses the [gpt-3.5-turbo](https://platform.openai.com/docs/guides/chat/chat-completions-beta) model, which is the same model used by ChatGPT (Free Edition), as default.

## Changelog

### 2023-04-23
Released `gpt-term` on [Pypi](https://pypi.org/project/gpt-term/), started version control. No need to clone the project locally anymore, simply use the `pip` command to install gpt-term.

<details>
  <summary>More Change log</summary>

### 2023-04-15

- Added the ability to create a line break in single-line mode using `Esc` + `Enter`

### 2023-04-13

- Added the function of generating and setting the terminal title in the background, and now the client will use the summary of the first question content as the terminal title

### 2023-04-09

- Add filename generate function, client will suggest the summary of the first question as filename when save command executed.

### 2023-04-05

- Add `/delete` command to delete the first question and answer in this chat to reduce token.

### 2023-04-01

- Add `/copy` command to copy the last reply's content to the clipboard
- Add streaming output mode, enabled by default, use `/stream` to switch

### 2023-03-28

- Add `--model` runtime argument and `/model` command to choose / change AI models.

### 2023-03-27

- Added `--key` runtime argument to select which API key in the `.env` file to use.

### 2023-03-23

- Added slash (/) command functionality
- Added `--load` runtime argument to load previously saved chat history
- Modified program structure and interaction methods, changing the original `input()` function to the `prompt_toolkit` library's input interface, supporting multi-line input, command-line completion, and other features.
- Improved error handling mechanisms, added chat history backup, logging, and other features, enhancing the program's reliability and fault tolerance.
- Refactored code logic and function structure, improving modularity and readability.

</details>

## Preparation

1. An OpenAI API key. You need to register an OpenAI account and obtain an API key.

   OpenAI's API key can be generated on the page opened by clicking "View API keys" in the upper right corner of the homepage, direct link: https://platform.openai.com/account/api-keys

   ![image-20230303233352970](https://github.com/xiaoxx970/chatgpt-in-terminal/raw/main/README.assets/image-20230303233352970.png)

2. [Python](https://www.python.org/downloads/) version 3.7 or higher.

## Installation


1. Install `GPT-Term` using `pip`

   ```shell
   pip3 install gpt-term
   ```

2. Configure the API Key

   ```shell
   gpt-term --set-apikey YOUR_API_KEY
   ```

   > If you don't configure the API Key now, you can enter it when prompted during runtime.

## Update

To update `GPT-Term` to the latest version, run the following command in your terminal:

```sh
pip3 install --upgrade gpt-term
```

> If there is a new version, `GPT-Term` will prompt the user to update upon exiting.

## How to Use

Run with the following command:

```shell
gpt-term
```

When entering a question in single-line mode, use `Esc` + `Enter` to start a new line, and use `Enter` to submit the question.

> Original chat logs will be saved to `~/.gpt-term/chat.log`

### Available Arguments

| Arguments     | Description                     | Example                                       |
| ------------- | ------------------------------- | --------------------------------------------- |
| -h, --help    | show this help message and exit | `chat.py --help`                              |
| --load FILE   | Load chat history from file     | `chat.py --load chat_history_code_check.json` |
| --key API_KEY | choose the API key to load      | `chat.py --key OPENAI_API_KEY1`               |
| --model MODEL | choose the AI model to use      | `chat.py --model gpt-3.5-turbo`               |
| -m, --multi   | Enable multi-line mode          | `chat.py --multi`                             |
| -r, --raw     | Enable raw mode                 | `chat.py --raw`                               |
| --set-apikey KEY        | Set the OpenAI API key                                   | `gpt-term --set-apikey sk-xxx`   |
| --set-timeout SEC       | Set the maximum wait time for API requests               | `gpt-term --set-timeout 10`      |
| --set-gentitle BOOL     | Set whether to auto-generate a title for the chat        | `gpt-term --set-gentitle True`   |
| --set-saveperfix PERFIX | Set the save prefix for chat history files               | `gpt-term --set-saveperfix chat_history_` |
| --set-loglevel LEVEL    | Set the log level: DEBUG, INFO, WARNING, ERROR, CRITICAL | `gpt-term --set-loglevel DEBUG`  |

> Multi-line mode and raw mode can be used simultaneously

### Configuration File

The configuration file is located at `~/.gpt-term/config.ini` and is autogenerated. It can be modified using the program's `--set` option or edited manually.

The default configuration is as follows:

```ini config.ini
[DEFAULT]
# API key for OpenAI
OPENAI_API_KEY=

# The maximum waiting time for API requests, the default is 30s
OPENAI_API_TIMEOUT=30

# Whether to automatically generate titles for conversations, enabled by default (generating titles will consume a small amount of tokens)
AUTO_GENERATE_TITLE=True

# Define the default file prefix when the /save command saves the chat history. The default value is "./chat_history_", which means that the chat history will be saved in the file starting with "chat_history_" in the current directory
# At the same time, the prefix can also be specified as a directory + / to allow the program to save the chat history in a folder (note that the corresponding folder needs to be created in advance), for example: CHAT_SAVE_PERFIX=chat_history/
CHAT_SAVE_PERFIX=./chat_history_

# Log level, default is INFO, available value: DEBUG, INFO, WARNING, ERROR, CRITICAL
LOG_LEVEL=INFO
```

### Available Commands

- `/raw`: Display raw text in replies instead of rendered Markdown format

  > After switching, use the `/last` command to reprint the last reply

- `/multi`: Enable or disable multi-line mode, allowing users to enter multi-line text

  > In multi-line mode, use `Esc` + `Enter` to submit the question
  >
  > If pasting multi line text, single-line mode can also paste properly

- `/stream`: disable or enable stream mode

   > In stream mode, the answer will start outputting as soon as the first response arrives, which can reducing waiting time. Stream mode is on by default.

- `/tokens`: Display the total tokens spent and the tokens for the current conversation

  > GPT-3.5 has a token limit of 4096; use this command to check if you're approaching the limit

- `/usage`: Display the API credits summary

  > This feature may not be stable. If it fails to operate, you can visit the [usage page](https://platform.openai.com/account/usage) to view further information.

- `/model`: Show or change the Model in use

  > `gpt-4` , `gpt-4-32k` , `gpt-3.5-turbo` are supported by default. when using other models you need to change the API endpoint in code.

- `/last`: Show the last reply

- `/copy` or `/copy all`: Copy the last reply's content to the clipboard

  - `/copy code [index]`: Copy the `index`-th code block from the last reply's content to the clipboard

    > If `index` is not specified, the terminal will print all code blocks and ask for the number of the one to be copied

- `/delete` or `/delete first`: delete the first question and answer in the current chat

     > When the token is about to reach the upper limit, the user will be warned, and when the upper limit has been exceeded, it will be asked whether to delete the first message

   - `/delete all`: delete all messages

- `/save [filename_or_path]`: Save the chat history to the specified JSON file

  > If no filename or path is provided, the client will generate one, and if generation fails, the filename `chat_history_YEAR-MONTH-DAY_HOUR,MINUTE,SECOND.json` is suggested on input.

- `/system [new_prompt]`: Modify the system prompt

- `/title [new_title]`: Set terminal title for this chat

  > If new_title is not provided, a new title will be generated based on first question

- `/timeout [new_timeout]`: Modify API timeout.

  > The default timeout is 30 seconds, it can also be configured by setting `OPENAI_API_TIMEOUT=` in the `~/.gpt-term/config.ini` file.
  
- `/undo`: Delete the previous question and answer

- `/version`: Display the local and remote versions of `GPT-Term`

- `/help`: Display available commands

- `/exit`: Exit the application

### Exit Words

In the chat, use exit words to end the current session. Exit words include:

```python
['再见', 'bye', 'goodbye', '结束', 'end', '退出', 'exit', 'quit']
```

Exit words will be sent as a question to ChatGPT, and the application will exit after GPT replies.

You can also use `Ctrl-D` or `/exit` to exit immediately.

Upon exit, the token count for the chat session will be displayed.

> Current price: $0.002 / 1K tokens, Free Edition rate limit: 20 requests / min (`gpt-3.5-turbo`)

## Dependencies

Thanks to the following projects for providing strong support for this script:

- [requests](https://github.com/psf/requests): For handling HTTP requests
- [pyperclip](https://github.com/asweigart/pyperclip): A cross-platform clipboard operation library
- [rich](https://github.com/willmcgugan/rich): For outputting rich text in the terminal
- [prompt_toolkit](https://github.com/prompt-toolkit/python-prompt-toolkit): Command-line input processing library
- [sseclient-py](https://github.com/btubbs/sseclient-py): For implementing streaming output of answers
- [tiktoken](https://github.com/OpenAI/tiktoken): A library for calculating and processing OpenAI API tokens

## Contributing

Feel free to dive in! [Open an issue](https://github.com/xiaoxx970/chatgpt-in-terminal/issues/new) or submit PRs.

### Contributors

This project exists thanks to all the people who contribute. 
<a href="https://github.com/xiaoxx970/chatgpt-in-terminal/graphs/contributors"><img src="https://opencollective.com/chatgpt-in-terminal/contributors.svg?width=890&button=false" /></a>

## Project Structure

```sh
.
├── LICENSE                   # License
├── README.md                 # Documentation
├── chat.py                   # Script entry point
├── config.ini                # API key storage and other settings
├── gpt_term                  # Project package folder
│   ├── __init__.py
│   └── main.py               # Main program
├── requirements.txt          # List of dependencies
└── setup.py
```

## License

This project is licensed under the [MIT License](LICENSE).