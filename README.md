
# FunctionWise - The Calculator for Learners, powered by AI.

A calculator that shows its work. Instead of just handing you an answer, FunctionWise gives you the answer and then explains why it works, step by step, so you actually learn the idea behind it.

It runs entirely on your own computer. A single HTML file talks to a small language model (Qwen3 0.6B) running locally through [Ollama](https://ollama.com). No account, no API key, and no internet needed after the one-time model download.

## Features

- **Answer plus explanation.** Every result comes with a short, numbered walkthrough that names the rule behind each step, such as order of operations or inverse operations.
- **Trustworthy arithmetic.** For standard expressions, a built-in safe calculator computes the real answer first. The model is told to explain that answer, not to invent its own, which matters because very small models can be shaky at arithmetic.
- **Word-style problems too.** Things like `solve 3x+5=20` or `what is 15% of 80` are handled by the model directly, and the page labels them so you know to double-check.
- **Practice built in.** Each explanation ends with a "Try this" problem.
- **Streaming responses.** The explanation appears word by word as the model writes it.
- **Private and offline.** Nothing leaves your machine.
- **Single file.** No build step, no dependencies, no server of its own.

## Requirements

- A computer that can run Ollama (Windows, macOS, or Linux)
- About 1 GB of free disk space for the model
- A modern browser (Chrome, Edge, Firefox, Safari)

## Setup

1. **Install Ollama.** Download it from [ollama.com](https://ollama.com) and install it like a normal app. On Windows and macOS it starts in the background automatically.
    
2. **Download the model.** Open a terminal (PowerShell or Command Prompt on Windows, Terminal on macOS) and run:
    
    ```
    ollama pull qwen3:0.6b
    ```
    
3. **Open the app.** Double-click `FunctionWise.html`. It opens in your browser.
    
4. **Check the status dot** in the top right corner. Green with "Ollama ready" means you're good to go.
    

## Using it

Type a problem into the box, or use the on-screen keypad, then press **Enter** or **Explain it**. The example chips under the keypad are a quick way to see how it behaves.

### Expressions the built-in calculator understands

|Feature|How to write it|
|---|---|
|Add, subtract, multiply, divide|`+` `-` `*` or `×` `/` or `÷`|
|Powers|`^` (for example `2^10`)|
|Parentheses|`( )`|
|Percent|`50%` is treated as `0.5`|
|Square root|`sqrt(144)`|
|Trigonometry (degrees)|`sin(30)` `cos(60)` `tan(45)`|
|Logarithms|`ln(x)` natural log, `log(x)` base 10|
|Absolute value|`abs(-7)`|
|Pi|`pi`|

Anything the calculator can't parse is passed to the model to solve and explain on its own.

### Two kinds of results

- **"checked by the calculator"**: the answer was computed by the built-in parser. You can rely on it. The model only writes the explanation.
- **"worked out by the model, double-check it"**: the model produced the answer itself. A 0.6B model is helpful for explanations but can make mistakes, so verify important results.

## Settings

Open **Settings and troubleshooting** at the bottom of the page.

|Setting|Default|Notes|
|---|---|---|
|Ollama address|`http://localhost:11434`|Change it if Ollama runs on another port or machine|
|Model|`qwen3:0.6b`|Swap in a larger model such as `qwen3:4b` for richer explanations (run `ollama pull qwen3:4b` first)|

Settings reset when you reload the page.

## Troubleshooting

**The status dot is red and says it can't reach Ollama.** Make sure Ollama is running (open the app again), then refresh the page. If it stays red, your browser may not be allowed to talk to Ollama. Quit Ollama completely and start it with the allowed origins set:

- macOS or Linux:
    
    ```
    OLLAMA_ORIGINS="*" ollama serve
    ```
    
- Windows PowerShell:
    
    ```
    $env:OLLAMA_ORIGINS="*"; ollama serve
    ```
    

Keep that terminal open while you use FunctionWise, then refresh the page.

**The dot says Ollama is on but the model isn't pulled.** Run `ollama pull qwen3:0.6b` and confirm the model name in Settings matches exactly.

**"Model not found" error.** Same fix: pull the model, or correct the name in Settings.

**The explanation is empty or odd.** Try rephrasing the problem. Very small models are sensitive to wording. A larger model usually helps.

**It works in my browser but not when the page is hosted online.** Hosted pages are blocked from calling `localhost` by browser security rules. Open the file locally instead.

## How it works

1. Your input is run through a small recursive-descent parser (no `eval`). If it is a valid arithmetic expression, the exact result is kept.
2. The app sends a request to Ollama's `/api/chat` endpoint with thinking mode turned off, so replies are fast.
3. If a verified answer exists, the prompt tells the model to explain it without changing it. Otherwise the model is asked to give the answer first and then explain.
4. The response is streamed in, cleaned of any reasoning tags, escaped for safety, and rendered as simple formatted steps.

## Limitations

- A 0.6B model is tiny. Explanations are good for foundational math but can get shaky on advanced topics like calculus or proofs.
- Model-solved problems are not verified. Always double-check them.
- Settings are not saved between sessions.
- Implicit multiplication such as `2(3+4)` is not supported by the built-in calculator. Write `2*(3+4)`.

## Ideas for the future

- Graphing for functions
- An "explain it like I'm five" toggle
- Solved-problem history and streaks
- Saved settings

## Credits

Built with [Ollama](https://ollama.com) and [Qwen3](https://qwenlm.github.io/) by the Qwen team.

## About Linked AI

Linked AI is a project exploring the capabilities of artificial intelligence and beyond.

We build, test, and open-source experiments across AI, intelligent systems, APIs, and emerging technology.
