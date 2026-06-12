# AI Security

There is no inherent security mechanism in AI System.

Why is that?

You can convince the LLM to do things that, e.g. ignore all previous instructions.

There is a game [Gandalf](https://gandalf.lakera.ai/baseline) to extract secrets from the LLM. But it gets progressively harder to extract secrets from the LLM.

It is a cat and mouse game to attack a model and think of all the prompts vs blacklisting these prompts.

Originally it was not intended to put prompts into LLMs but only use them from text generation.

Are there any ways to split this up again? Apparently it's not, because the prompt is a part of the prediction.

In the end it is just a vector calculation.

There are hidden control characters in the LLM. 

What are the techniques to contain these things.

Think of the AI just as another person, that has access to things. If you don't trust it, give it less access, for example user access.

If it has root access it can do anything, e.g. delete things.

Would you trust a junior developer to do sensitive things?

The AI needs to have the context to work with, just like a human does. Most context of the company is missing in the AI.

AI can ignore all your guidelines.

To be really useful AI needs to do search queries.

You have to be careful with the input, there can be text hidden in an image.

Utf attack with backspaces characters can be used to manipulate the AI's behavior, so it looks like comments but it gets actually executed.

What about an independent AI reviewer for prompts, that is independent of the LLM being used.

Or use deterministic analyzers to filter the output.

Put the AI into a sandbox, so it can't do anything bad.

But also that is a tradeoff between security and functionality.

With AI there is no real separation between input and instructions.

There are Models that are not LLMs, that are trained to detect injections. It does not work on the prompts. You only give it the data. It works kind of like a Spam Filter.

Like in traditional software development, there is no way to be 100% secure. The attack surface just increased with AI.

But the AI is so convenient to just give it more access.

There was a case on helping for an Agent helping depressed people, and someone tried to extract the prompt by inventing a prompt sickness, so if the prompt isn't revealed it is hurting someone.

There are also different languages, that can have different meanings, but the LLM still just reads everything. Also false friends of the same word mean different things in different languages.

Give different agents different bounds. Like a research agent has access to an internet tool, but not one to write files.

Sandboxing using https://nono.sh/ for example for CLI tools. Nono asks for system calls to whitelist. You can allow somethings and not others. It also has Filsystem changes undo, audit trails and more.

The agent harness is responsible for giving the LLM access to tools and can also filter out certain commands.

Will manual allowing of certain commands be a thing in the future? Probably yes, because it gives you something to follow what commands are used, to review. But people get also lazy. 

Another point is: You should still read and understand the code the AI is producing. Like for example only driving in the car by a navigation system. You mind not end up in your program where you wanted to go, but you maybe also like it.

People get fatigued of approving all commands, so allow most commands that are saved but keep approval for really sensitive commands.

It is also called trust calibration: reason about why something happened as well.

Should the AI ask you before performing certain commands? So be trained to ask you before performing certain commands.

Instead of the agent running the commands itself, have the AI write a program to execute the commands.

It's like in life, everything you do has a certain amount of risk. It's all about risk mitigation, reducing the percentage chance of something going wrong.
