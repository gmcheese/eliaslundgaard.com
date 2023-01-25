---
categories: ["projects"]
date: 2023-01-25T12:55:05-04:00
tags: ["projects"]
title: "How to Supercharge Siri with GPT-3"
show-meta: false
featured_image: /gpt3-siri/siri-gpt.png
toc: false
---

![Create API key](/gpt3-siri/siri-gpt.png)

How many times have you used Siri and thought: "That just doesn't cut it"? Even though Siri is useful for mundane tasks such as settings timers or turning of smart home devices, functionality often falls short at simple questions.

You've probably already heard too much about [OpenAI's](https://openai.com) [ChatGPT](http://chat.openai.com), which blows Siri's more "soft" qualities - such as answering questions, out of the water. So why not use both?

In this post I'll guide you through the steps to set up a GPT-3 with chat capabilities, and make it accessible through Siri. I'll keep it as simple as possible - you should be able to follow these steps with limited technical knowledge!

## Extending Siri with GPT-3

Apple's [Shortcuts](https://support.apple.com/en-gb/guide/shortcuts/welcome/ios) app is one of the few attempts Apple has done at opening up their platforms. Even though we cannot directly modify Siri, we can use Siri as a proxy to access GPT-3 through OpenAI's API. This method will also work with Siri interaction through HomePod.

## What you need

* An [OpenAI account](https://openai.com/login) with credits available. Don't worry, you get $18 in free credits when signing up for the first time!
* An iOS device. iPad, iPhone, Macbook, anything is good as long as it has [Siri enabled](https://support.apple.com/en-gb/guide/iphone/iph83aad8922/ios) and the [Shortcuts App](https://apps.apple.com/us/app/shortcuts/id915249334).

## Getting the API key

First, let's get you API key from OpenAI. This is the secret key that will allow you to connect to OpenAI's GPT-3, and send and receive requests. The key is linked to your account and will use your credits, so keep it secret!

Got to you OpenAI dashboard by logging in on the [login page](https://openai.com/login). In the top right corner, click your profile avatar and select "**View API keys**".

![Create API key](/gpt3-siri/api-key-menu.png)

You will now be directed to the API keys page. Click "**Create new secret key**" and copy it.

## Create the Shortcut

Now for the fun part! We will create a shortcut in the shortcuts app. The shortcut will:

1. Ask for a input via voice
2. Send the voice input as a prompt to GPT-3
3. Receive the response from GPT-3
4. Read the response out loud.

Head to the shortcuts app and click the **+** icon to create a new shortcut.

### Naming the shortcut

If you want to access the shortcut through Siri, you need to give it a proper name. Activating shortcuts through Siri works by activating siri, and then saying the shortcut name. For a shortcut named "My Shortcut", you would activate it by "Hey Siri... My Shortcut". That sounds a bit weird, so instead I named my GPT-3 bot "Coda" (It chose this name itself), and named the shortcut **Ask Coda**. This means you will activate the shortcut with "Hey Siri... Ask Coda". Much nicer!

### Ask for Voice Input

In the new shortcut your making, create an "Dicate Text" action. On iOS and MacOS this will start text dictation through the Keyboard's dictation. On HomePod, dictation will run through the HomePod itself. On iOS and Homepod, dication will be done in the same language as the system language. You may need to change this to English, unless you want to further modify GPT-3 to understand your language.

![dicate text](/gpt3-siri/dictate-text.png)

Now, create a "Set Variable" action below the dictate text, and set the variable name to `PROMPT`, and the variable to `Dictated Text`.

### Engineering the Prompt

GPT-3 requires context to provide good answers. Prompt engineering is the answer, and is a field in great development. I follow OpenAI's playground example of creating a chatbot:

```
The following is a conversation with an AI assistant called Coda. The assistant is helpful, creative, clever, and very friendly. It's answers are short and concise, end expressed as spoken language.

Human: Hello, who are you? END OF QUESTION
Coda: I am an AI called Coda, your personal assistant. How can I help you today?

Human : PROMPT END OF QUESTION
Coda :
```

This gives the bot a minimal context for what is expected of it. Feel free to play around! You can make it sound like a cowboy, only answer in riddles and much more.

When using dictation, punctuation and questionmarks are not added to the prompt. This is why we add "END OF QUESTION" at the end of each question by the human.

Take the prompt text above and copy it into a "Text" action. Replace "`(Your message)`" with the variable `PROMPT`.

Add a new "Variable" and set it's name to `INPUT` and it's value to "Text" you just created.

![dicate text](/gpt3-siri/prompt-engineering.png)

### Send the prompt to GPT-3

Now that we have the prompt, we're ready to send it off! First, create a "Text" action, and set it's text to the API key you generated in the previous step. It looks something like this: `sk-xxxxxxxxxxxxxxxxxxxxxxxx`. Then create a "Set Variable" action, and set it's name to `API_KEY`, and it's value to "Text".

Now, create a "URL" action, and set it's value to `https://api.openai.com/v1/completions`.

The next part is a bit tricky if you have limited experience with web development, so hang tight!

Create a "Get Contents of URL" shortcut, and set it's "Method" to `POST`. Under "Headers", create two entries:

1. One with the **Key**: "Authorization" and value "Bearer ", and then your `API_KEY` variable.
2. One with the **Key**: "Content-Type" and value "application/json".

Now, under "Request Body", add the following values:

| Key         | Type     | Value |
|--------------|-----------|------------|
| model | Text      | text-curie-001        |
| prompt      | Text | The `INPUT` variable      |
| temperature      | Number | 0.9     |
| max_tokens      | Number | 150      |
| top_p      | Number | 1.0      |
| frequency_penalty      | Number | 0      |
| presence_penalty      | Number | 0.6      |
| stop      | Array | "Human:" and "Coda:"      |

Most of these parameters are taken from OpenAI's chat playground.

![request](/gpt3-siri/request.png)

### Get the Response

Now that the prompt has be sent to OpenAI for processing, we need to take the returned value and extract GPT-3's answer. The response is in json format, and all we need to do is get the right value.

First, add a "Get Dictionary from Input" action and set the input to "Contents of URL".

Then, add a "Get Dictionary Value" action and set it to get Value for "choices" in the Dictionary from before.

Add another "Get Dictionary Value" and set it to get Value for "text" in the response from above.

Then create a "Set Variable" action, and set it's name to `RESPONSE` and it's value to the "Dictionary Value" from before.

![request](/gpt3-siri/extract.png)


### Speak it Out!

Finally, add a "Speak Text" action, and set the value to the `RESPONSE` variable from before.

All done! You should now be able to activate GPT-3 with "Hey Siri, ask Coda" and then ask whatever you want.

Full configuration here:

![request](/gpt3-siri/full-config.png)

## Future Improvements

I'll list some of the improvements you can make to make it even better.

### Chat History

One of the greatest features with ChatGPT is it's ability to keep track of the conversation. Currently we can only do one human question per session, so it would make sense to investigate how we could utilize loops to keep the conversation going. It could also be interesting to add more contextual information to the initial prompt, like who the owner is, what interests they have and more. Finally, adding dynamic metadata such as Homekit Devices, weather information etc. would be a natural next step.

### Parameter Tuning

Currently we're using the default configuraiton from OpenAI's playground. It could be interesting to play around with the values to optimize the results.

That's it! I hope you found this read interesting and made it run on your own device.