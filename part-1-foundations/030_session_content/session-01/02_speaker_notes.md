# Speaker Notes — Session 1 · Getting Started & the Python Model

*Python Mastery — Part 1: Foundations · Week 1. Read aloud, top to bottom.*

---

## Segment 1 · Welcome & course map (0:00–0:10)

### Slide 1 — Getting Started & the Python Model

Hello everyone, and welcome — this is the very first session of Python Mastery, Part 1, Foundations, and I'm really glad you're here. We're live on Microsoft Teams, and as with every session this one is being recorded, so if anything goes by too fast today, you'll be able to come back and watch it again as many times as you like. My name is Rajan, I'll be your trainer for all sixteen weeks, and over those sixteen weeks we are going to take you from "I have never written a single line of code" all the way to writing real, clean, working Python programs. Today is week one — getting started and understanding how Python actually works. So take a breath, get comfortable, and let's begin.

### Slide 2 — Welcome / How this course works

Before we touch any code, I want to spend just a few minutes telling you how this whole thing is going to run, because once you understand the rhythm, every week after this one is going to feel easy and familiar. So let's talk about how the course works.

### Slide 3 — The Python Mastery Series

The course you're in is called Part 1, Foundations, and it's the first of four parts in a series. Think of it as a staircase. Part 1, the one we're doing now, is sixteen weeks and it's all about the foundations — the core language and the everyday tools. After this comes Part 2, which is intermediate Python and object-oriented programming, then Part 3 is advanced, and Part 4 is expert-level. You don't need to think about those today at all. I just want you to see that we have a clear, complete path, and by the end of Part 1 you will be able to write proper Python and you'll have built a real command-line application with your own hands. So even though we start at absolute zero today, we finish this part with something you can show people.

### Slide 4 — How every week runs

Now here's the shape of every single week, and this is important so let me say it slowly. Each session is two hours, it's live, and it's recorded. In that live time, I do two things: I explain a concept, and then I show it to you working by typing it live on my screen while you watch. What I do not do is ask you to type along with me during the session — there's no in-session hands-on. And that's deliberate, not lazy. The reason is that the real practice, the part where the learning actually sticks, happens after the session, when you sit down on your own with the recording playing on one side and the lab guide on the other, and you rebuild what you just watched me do, at your own pace, pausing whenever you need to. That recording is not just a nice extra — it is genuinely part of your study material. If you get lost at any point today, the fix is simple: scrub back to the exact moment and watch it again.

### Slide 5 — The loop, every week

So here's the loop, and it repeats every single week. First, I explain the concept. Then I show you a live demo of that concept working. Then, after the session, you do a lab where you rebuild that same demo yourself and push it a little further. And finally there's a checkpoint — usually you submit your lab — which is how I know, even without being in the room with you, that the whole group is keeping up before we move on to next week's material. Concept, demo, lab, checkpoint. That's the heartbeat of this course. The beautiful thing about it is that the lab always rebuilds what the demo just showed, so you're never thrown into something you haven't seen me do first.

### Slide 6 — What you need

What do you actually need to take part? Honestly, very little. You need a laptop — Windows, Mac, or Linux all work perfectly — and it just has to be a machine where you're allowed to install software, because in your lab this week you'll be installing a few small free tools. And that's it. You do not need any prior programming experience. If you have never coded before in your life, you are exactly who this course is built for, and we are starting from the very floor. Now, some of you might already know another language — maybe Java, or C#, or JavaScript, or Go. If that's you, wonderful, you'll move a bit faster, and every now and then in the demos I'll deliberately point out where Python does something differently from what you're used to, so you get value too. But nobody gets left behind here.

### Slide 7 — Today's goal

So what specifically are we going to achieve today? By the end of this session you'll understand how to install Python 3.14 and open its new interactive prompt, called the REPL. You'll know how to run a Python file two different ways. You'll be able to explain, in one plain sentence, what people mean when they say "the interpreter runs bytecode" — and I promise that will be painless. And you'll see how to scaffold a real project using a tool called uv, and tidy it up with a tool called Ruff. Don't worry about those names yet; we'll meet them properly later. That's our map for the next two hours.

---

## Segment 2 · What Python is, the 3.14 release, the mental model (0:10–0:30)

### Slide 8 — What Python is / where 3.14 fits

Let's start with the most basic question of all: what actually is Python? Let's answer that properly before we run anything.

### Slide 9 — What is Python?

Python is a high-level, general-purpose programming language. Let me unpack those two phrases because they matter. "High-level" means it's designed to be read and written by humans easily — it reads almost like structured English, and it takes care of a lot of fiddly, low-level housekeeping for you, like managing the computer's memory, so you can focus on the actual problem you're solving. "General-purpose" means it isn't locked to one job — the same language is used for writing small scripts, building websites, crunching data, automating boring tasks, and powering artificial intelligence. You learn one language, and a lot of doors open. Now, one more word you'll hear me say: CPython. When people say "Python," the standard version that almost everyone installs and runs is called CPython — that's the official build, and that's the one we're going to install and use throughout this course. So when I say Python, I mean CPython, the standard build.

### Slide 10 — Python 3.14, current stable

A quick word on versions, because in this course we are very precise about this. The version we use is Python 3.14. The very first 3.14 release came out in October 2025, and the current stable point release, as of right now, is 3.14.6, which arrived in June 2026. We pin the entire course to 3.14, deliberately, so that every demo I show you reproduces exactly the same way on your machine — no surprises from version differences. And 3.14 is a genuinely nice release to learn on, because it brought some friendly improvements we'll actually use. The big one for today is a much-improved interactive prompt, which you'll see in just a moment. Later in the course we'll meet a brand-new kind of string called a t-string, in week three, and some friendlier error-handling syntax in week eleven. You don't need to remember any of that now — I'm just planting flags so you know good things are coming.

### Slide 11 — How Python runs your code

Now I promised you the "interpreter runs bytecode" sentence, so let's earn it, and I'll keep this completely jargon-free. When you write Python, you type plain text into a file — let's say a file called hello dot py. That text is your source code; it's just words you wrote. When you run that file, two things happen, automatically, without you doing anything special. First, Python quietly translates your text into something called bytecode, which is a compact set of instructions that Python understands well but that you and I would find unreadable. Second, a part of Python called the interpreter reads that bytecode and carries it out, one instruction at a time. That's it. You never do this translation by hand — it's all automatic, and it happens the instant you run your file. So if anyone ever asks you what Python does with your code, here's your one sentence: Python turns your text into bytecode, and then the interpreter runs that bytecode. That's the whole mental model, and it's all you need at this stage.

### Slide 12 — REPL vs script — two ways to run

There are two completely different ways you'll interact with Python, and I want you to be clear on the difference because we'll use both, today and every week. The first is the REPL — that's an interactive prompt where you type one line, hit enter, and Python answers you immediately. It's a conversation. You type two plus two, it says four, right there and then. The REPL is fantastic for exploring, for testing a quick idea, for asking "what does this do?" But it forgets everything the moment you close it. The second way is a script — that's when you write your code into a file, save it, and run the whole file. A script is for code you want to keep and run again and again. So a simple way to remember it: the REPL is for exploring, scripts are for keeping. In a minute I'll show you both, side by side, so the difference becomes obvious.

---

## Segment 3 · Demo 1 — REPL tour + first script (0:30–0:55)

### Slide 13 — Let's open Python and talk to it

Alright, enough talking about Python — let's actually open it up and have a conversation with it. Let me share my screen, and we'll do this together.

### Slide 14 — Watch this, Demo 1

Here's what I'm about to do, so you know what to look for. I'm going to launch the Python 3.14 REPL and you'll notice it has colours and it's smart about completing things. I'll do a little arithmetic, work with some text, and use the print command. Then I'll show you a neat trick where I start typing "import ma", press the Tab key, and watch Python finish the word "math" for me. I'll ask Python for help on a built-in. And then I'll switch from the conversational REPL to writing my very first saved script, a file called hello dot py, and I'll run it two different ways. Keep your eyes on the screen — you'll rebuild every bit of this in your lab.

### Slide 15 — The new 3.14 REPL

So here I am at the prompt. I type two plus two, press enter, and Python immediately answers four — no "run" button, no waiting, it just responds. Now watch the colours as I type; notice the text "Python" lights up as a string, and when I call dot upper on it, Python hands me back the same word in capitals. This is the new 3.14 REPL doing its syntax highlighting automatically, which makes what you type much easier to read. Now here's the trick I love showing beginners. I'm going to start typing a command to bring in some extra maths abilities — I type "import ma" and then, instead of finishing the word, I just press Tab. And look — Python completes it to "import math" for me. That autocompletion is new and on by default in 3.14, and it's a lovely little helper. One more thing: if you ever want to know what something does, you type help, and in brackets the name of the thing — here, help of len — and Python reads you its own documentation right there in the prompt. And when you're done exploring and want to leave, you simply type exit, with the empty brackets, or press Control-D. That's the REPL — a place to play.

### Slide 16 — One file, two ways to run

Now let me show you the other side: a real script. I've created a tiny file called hello dot py, and all it contains is one line that prints "Hello, Python 3.14". Now down here in the terminal, I run it the first way: I type python space hello dot py, and there's my message printed out. That's running the file by its name, by its path. Now I'll run the exact same file a second way: python space dash m space hello — and notice I dropped the dot py this time. And look, identical output. So what's the difference? The first form, python hello dot py, says "run this specific file." The second form, python dash m hello, says "find the module named hello and run it." Right now they look the same and give the same result, and that's fine — I'm planting a seed. That dash-m form is going to become really important in week nine when we start organising our code into packages. For today, just notice: same file, two ways to run it, same result.

### Slide 17 — What you just saw

So let's just pause and take stock of what we did there. We saw that the REPL answers you instantly, which makes it perfect for trying things out and getting quick feedback. We saw that pressing Tab completes import names for us, and that help opens up Python's own documentation right inside the prompt. Then we saw that a script is simply saved code — code that lives in a file so you can run it over and over. And we ran that script two ways, by its filename and as a module with dash-m, and got the same result both times. If that all makes sense, you've got the core of today already. If any of it went by quickly, remember — it's all in the recording, and you'll rebuild it yourself this week.

---

## Segment 4 · The modern toolchain — uv and Ruff (0:55–1:10)

### Slide 18 — The modern toolchain

Now we're going to level up just slightly, from talking to Python to setting up a proper project, and for that we'll meet two modern tools. Let me explain why we even bother with tools on day one.

### Slide 19 — Why tools on day one?

You might be thinking: I've only just written one line in a file, why are we talking about tools already? Fair question. Here's the reason. Real programs are almost never a single loose file sitting on your desktop. Even a beginner project very quickly needs two simple things. The first is isolation — the idea that each project you build gets its own clean, separate setup, so that one project's needs never tangle up with another's. The second is tidy code — having your code formatted consistently and having something quietly catch silly mistakes before they bite you. Now, I could let you discover the pain of not having these and then rescue you in week ten, but that's not kind. Instead, we build these good habits from day one, so that by the time they really matter, they're already second nature and completely invisible to you. So let me introduce the two tools that give us isolation and tidiness for free.

### Slide 20 — uv, the project manager

The first tool is called uv — three letters, lowercase, made by a company called Astral. uv is one single, blazingly fast tool that does several jobs that used to need a whole pile of separate tools. It can install Python itself for you — there's literally a command, uv python install 3.14, that fetches and sets up the right Python. It can scaffold a brand-new project for you with uv init, which we'll see in a moment. It can run your project with uv run. And later, in week ten, it'll manage the extra libraries your project depends on, and lock them down so your project is reproducible. For today, the headline is just this: uv is the tool that creates and runs our project, and it replaces a confusing pile of older tools with one clean command.

### Slide 21 — Ruff, format and lint

The second tool is called Ruff, also from Astral, and it is astonishingly fast. Ruff does two jobs for us. The first is formatting: you run ruff format, and it automatically rewrites your code into a clean, consistent style — the right spacing, the right quotes, sensible line lengths — so you never have to fuss about that by hand, and everybody's code looks the same. The second job is linting, which is a slightly odd word that just means checking your code for likely mistakes and messiness: you run ruff check, and it points out problems before they become bugs. And here's a lovely convenience — you don't even need to install Ruff separately. Because we have uv, you can run Ruff on demand by typing uvx ruff format or uvx ruff check, and uv fetches and runs it for you. Like everything in Part 1, both uv and Ruff are completely free and open-source. Now let's see them turn a loose idea into a real project.

### Slide 22 — Let's turn a loose idea into a real project

So let me share my screen again, and this time we're going to create an actual project from nothing and tidy it up. Here we go.

---

## Segment 5 · Demo 2 — uv init project + Ruff (1:10–1:35)

### Slide 23 — Watch this, Demo 2

Here's the plan for this demo so you know what's coming. I'm going to type uv init taskapp, and a complete little project is going to appear out of thin air. We'll have a look inside it together at the few files it creates. Then I'll run the project to prove it works. After that, I'm going to do something slightly mischievous — I'll paste in some deliberately ugly, badly-spaced code, and then I'll run Ruff's formatter and watch it clean the whole thing up instantly. And finally I'll run Ruff's checker, let it spot a problem, and fix it. Watch closely, because this project we're about to create has a special role.

### Slide 24 — What uv init creates

I type uv init taskapp and press enter, and just like that, uv has created a folder called taskapp with everything a real project needs. Let me walk you through what's inside, because you'll see these same files in every project from now on. There's a file called dot python dash version, and that simply pins this project to Python 3.14, so anyone who opens it uses the right Python. There's a README, which is where you'd describe your project. There's main dot py, which is a ready-to-run starter file — uv has even put a small bit of code in it that prints a friendly hello. And there's pyproject dot toml, which is the project's main settings file, holding its name and details. Now let me run it: I type uv run main dot py, and there's our greeting — "Hello from taskapp!" Notice I didn't have to set anything up; uv quietly created an isolated environment behind the scenes and ran the project inside it. And here's the special part: this taskapp project is not throwaway. This is the seed of your capstone. Over the next sixteen weeks, this exact project is going to grow, week by week, into a complete task-and-notes manager application that you'll build with your own hands. So it starts, humbly, right here.

### Slide 25 — Ruff in action

Now let me show you Ruff cleaning things up. I've pasted some genuinely messy code into the file — inconsistent spacing, odd quote marks, the kind of thing that's hard to read. Now I type uvx ruff format, and look at that — it says "1 file reformatted," and when I open the file again, everything's been tidied: clean spacing, consistent quotes, proper layout, all done for me in a fraction of a second. That's the formatter. Now let me run the checker: I type uvx ruff check. If everything's healthy, it gives me a cheerful "All checks passed!" But if there's a real problem in the code — say I've imported something I never use — ruff check will point right at it and tell me what's wrong. And for the safe, obvious fixes, I can add dash dash fix, like ruff check dash dash fix, and Ruff will go ahead and correct them for me automatically. Format keeps your code pretty; check keeps your code honest.

### Slide 26 — What you just saw

Let's take stock again. In just a couple of commands, uv init built us a complete, runnable project in seconds — no manual setup at all. uv run executed it inside its own clean, managed environment. Then Ruff stepped in: the formatter tidied our messy code instantly, and the checker verified the code was sound. And most importantly, we now have taskapp — the project we're going to nurture and grow across all sixteen weeks of this course. So from one loose idea to a real, tidy, running project. That's exactly what your lab this week asks you to do for yourself.

---

## Segment 6 · Q&A + install troubleshooting (1:35–1:50)

### Slide 27 — Q&A + install help

Now I want to spend a little time on the bit that genuinely trips beginners up the most — not the code, but getting Python installed in the first place. So let me walk through the common gotchas per operating system, and then I'll take your questions.

### Slide 28 — Install gotchas

Let's go operating system by operating system, because each one has its own little quirk. If you're on Windows, the cleanest modern way is to install Python from the Microsoft Store, which gives you Python and keeps it updated automatically. If instead you download the classic installer from python dot org, there is one box you absolutely must tick on the very first screen — the one that says "Add Python to PATH." If you miss that box, your computer won't be able to find Python afterward, and that's the single most common Windows problem I see. On a Mac, you can download the installer package, the dot pkg file, from python dot org and double-click through it, or you can let uv install Python for you with uv python install 3.14. On Linux, your system probably already has some Python, but it's often an older version, so the safest bet is again uv python install 3.14 to get exactly 3.14. And honestly, the simplest path that works the same on all three systems is this: install uv first, then let uv fetch Python for you. One tool, three platforms, no fuss.

### Slide 29 — python vs python3 vs py

One more thing that confuses absolutely everyone at the start: what command do you actually type to run Python? It varies by system, annoyingly. On a Mac or Linux, it's often python3, and you can be specific with python3.14. On Windows, there's a handy launcher called py, so you'd type py dash 3.14 to pick the version. The good news is that once you're working inside a uv project, you sidestep all of this entirely — you just type uv run, and uv uses the right Python automatically, so there's nothing to guess. And here's a tip that will save you a lot of frustration: if you type a command and the computer says something like "command not found," nine times out of ten it is not that Python is broken — it's that PATH issue, the computer simply not knowing where to look. Keep that in mind and you'll debug most install problems yourself. Now — what questions do you have? This is the perfect moment, because once you leave today you'll be installing all of this on your own machine.

---

## Segment 7 · Wrap + Lab 1 brief (1:50–2:00)

### Slide 30 — Wrap-up / this week's homework

We're coming to the end, so let me hand you off to your homework — your first lab — and walk you through exactly what it asks.

### Slide 31 — Lab 1, set up and prove your toolchain

Here's Lab 1, and it mirrors everything we did today. Step one: install the four pieces of the toolchain — Python 3.14, uv, Ruff, and VS Code as your editor. Step two: open the REPL and evaluate three expressions, just like we did, to get comfortable talking to Python. Step three: write your own hello dot py and run it both ways, by filename and with dash-m. Step four: run uv init taskapp to scaffold your very own project, run it, and format it with Ruff. And step five: submit your proof — screenshots of your REPL session and your script running, along with your formatted uv project. The whole lab is just you rebuilding Demo 1 and then Demo 2 at your own pace. The full written guide, with every command spelled out, is in your lab document, and the recording of today is right there to follow along with. Take your time, pause as often as you need, and don't worry if it takes a couple of tries — that's completely normal and it's exactly how you learn.

### Slide 32 — Next week

Looking ahead: next week is Week 2, Numbers, Variables, and Expressions, where Python starts to feel like a calculator with superpowers. And here's why this week matters so much — next week we'll be working right inside the REPL and the taskapp project you set up today. So please do get your environment working this week; come to Week 2 with Python installed and running, because absolutely everything we do from here on builds on the foundation you're laying right now.

### Slide 33 — See you in the lab

That's a wrap for Week 1. Thank you all so much for being here for the very first session — you've taken the hardest step, which is starting. Go set up your tools, have fun talking to the REPL, and I'll see you in the lab and back here next week. Now, are there any last questions before we close?
