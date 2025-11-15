My Journey Down the Local LLM Rabbit Hole

I've been hearing about local LLMs forever. As a Business Analyst, the idea of an AI assistant on my own PC to help draft templates and user stories—completely offline—was too good to pass up. I've got a pretty beefy machine (Ryzen 9 9900x, RTX 5080), so I figured, "how hard can it be?"

Famous last words.

Chapter 1: Ollama - The Developer's Gateway

My first stop was Ollama. Everyone said it was the easiest way to get started. They weren't wrong. A few commands in my terminal, and I was downloading a 16GB Gemma 3 model. It felt like magic.

But then I hit a wall. It's... a command-line tool. It's amazing for developers who want to hook it up to other code, but I just wanted a simple chat workbench. I also learned that my 16GB model was now "stuck" in Ollama's world. Other apps couldn't see it. That was lesson #1: All these AI apps are like little walled gardens; they don't share their models.

Chapter 2: LM Studio - The Workbench I Wanted

This led me to LM Studio. This was exactly what I was looking for. It's a proper GUI where you can search for models, download them, and just... chat. I had to re-download my models (this time as .gguf files), but I was finally ready to get to work generating my BA and PM templates.

Or so I thought. My next problem was speed. I have an RTX 5080. This stuff should be fast.

Chapter 3: Hitting the "Speed Trap" (And How I Fixed It)

At first, things were zippy. But I got greedy. I went into the settings and saw "Context Length" (which is just the model's short-term memory). I thought, "More is always better!" and cranked it all the way up to 131,072 tokens.

The model immediately slowed to a crawl. I mean, it was painfully slow.

After a lot of frustration, I finally figured it out: By maxing out the context, I was choking my GPU. My 5080 was spending all 16GB of its VRAM just reserving that massive, empty memory block. This left almost nothing for the actual processing.

That was my big "aha!" moment. Here's my "go-fast" checklist now:

GPU Offload: Set this to "Max". This forces as much of the model as possible onto the GPU's fast VRAM.

Context Length (The "Speed Trap"): For 99% of tasks, I set this to 8192. This is still huge, but it leaves plenty of VRAM for the GPU to work, making it incredibly fast.

Flash Attention: Turn this ON. It's a free speed boost for modern NVIDIA cards.

CPU Threads: I set this to 12 to match my Ryzen 9 processor.

Suddenly, I had a lightning-fast AI for all my text-based template work.

Chapter 4: AnythingLLM & The Multimodal Nightmare

I got ambitious. I wanted to analyze a bunch of images and have the AI write a compiled document. This led me to AnythingLLM, which is built to "chat" with your documents (or images).

This was a whole new level of "why doesn't this work?"

First, I had to figure out how to connect LM Studio (as a server) to AnythingLLM.

Then I got a "Model does not support images" error. I learned that I needed a special "Vision" (VL) model.

Then, when I tried to upload 9 images, I got a "Context overflow: 4096 tokens" error.

It turned out I needed a model that was both a vision model and had a massive context window.

My Final Toolkit for BA Work

After all that troubleshooting, my local AI setup is now something I use every day. Here's my final toolkit:

For Fast, Daily Text & Templates:

Gemma 3 27B or Llama 3.3 70B. I run them in LM Studio with an 8192 context window. They're incredibly fast and smart enough for 90% of my work (drafting user stories, writing test cases, creating smaller templates).

For Analyzing Lots of Documents/Images:

Qwen2.5-VL-7B-Instruct-GGUF. This is my "multimodal" workhorse. It supports vision and has a giant 128k context window, which fixes all the "overflow" errors. I run this in LM Studio and connect it to AnythingLLM to analyze all my images at once.

It was a journey, but it's amazing what you can get running on your own machine once you figure out which settings to tweak.
