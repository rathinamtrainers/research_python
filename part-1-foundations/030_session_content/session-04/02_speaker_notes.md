# Speaker Notes — Session 4 · Control Flow & Pattern Matching

*Python Mastery — Part 1: Foundations · Week 4. Read aloud, top to bottom.*

---

## Segment 1 · Recap of Week 3 + today's goal (0:00–0:08)

### Slide 1 — Control Flow & Pattern Matching

Hello everyone, and welcome back — this is Week 4 of Python Mastery, Part 1, Foundations, and as always we're live on Microsoft Teams and the session is being recorded, so if anything moves too quickly today you can come straight back to it afterwards and watch it as many times as you need. Today is, I think, one of the most exciting weeks so far, because today is the week your programs learn to think. Up to now your code has run straight down the page, top to bottom, doing the same thing every time. After today, your programs will be able to make decisions, repeat work, and react differently depending on what they're given. This is the week Python stops being a calculator and starts being a program. So let's get going.

### Slide 2 — Last week, Strings & Text

Let me just take thirty seconds to remind you where we were last week, because today builds directly on it. Last week was all about strings and text. You learned to reach into a string by position with indexing, and to pull out slices with that square-bracket colon notation — things like s of one colon four, and the lovely s of colon colon minus one to reverse a string. You learned that string methods like strip, split, join, and lower always hand you back a brand-new string, because strings can't be changed in place. And you learned to format output beautifully with f-strings and that little format mini-language, lining numbers up in columns with things like colon, greater-than, eight, point, two, f. We even took a peek at the brand-new t-strings and at the difference between bytes and str. So you can now build and shape text. Today, we make your code decide what to do with it, and repeat work as many times as you like.

### Slide 3 — Today's goal

So here's our map for the next two hours. First, we'll make decisions — branching with if, elif, and else, and a really important idea called truthiness, which is about what Python considers true and false. Then we'll learn to repeat work with loops: the for loop, the while loop, and a little helper called range. Then we'll learn to steer those loops — how to skip a turn, how to stop early, and a clever loop-else clause that catches a lot of beginners by surprise in a good way. And finally, the star of the show: structural pattern matching, the match and case statement, which lets you branch not just on a value but on the shape of your data. By the end you'll have everything you need for this week's lab — a guessing game and a command menu. Let's start with decisions.

---

## Segment 2 · `if` / `elif` / `else`, truthiness, boolean logic (0:08–0:26)

### Slide 4 — Branching with if, elif, else

This is the most fundamental decision tool in the language, and it reads almost exactly like English. Look at the example: if score is greater than or equal to ninety, the grade is an A; else, if it's at least eighty, the grade is a B; otherwise, the grade is a C. Now, there are a few things I want you to notice carefully here, because they're new and they matter. The very first thing is the indentation — the lines that belong to each branch are pushed in by four spaces. In Python, that indentation is not just for looks; it is how Python knows which lines belong inside the if. There are no curly braces, no "begin" and "end" keywords like in some other languages — the indentation is the structure. If you're coming from Java or C, this will feel strange for about a day and then completely natural. The second thing: you can have as many elif branches as you like, zero or many, and the else at the end is optional. And the third, really important thing: Python checks these branches from the top down, and the very first one that's true wins. The moment a branch matches, Python runs it and skips all the rest entirely. So order matters — you put your most specific conditions first.

### Slide 5 — Truthiness, what counts as true

Now here's an idea that's quietly one of the most Pythonic things in the whole language, and it's called truthiness. When you put a value inside an if, Python doesn't just accept things that are literally True or False — it asks "is this value truthy or falsy?" and decides from there. And there's a small, memorable list of values that Python treats as falsy — meaning they behave like False in an if. Look at the list on screen: the number zero, the float zero-point-zero, the empty string, the empty list, the empty dictionary, the empty tuple, an empty set, the special value None, and of course False itself. Every single other value is truthy. So an empty string is falsy, but a string with anything in it is truthy. An empty list is falsy, but a list with even one item is truthy. Look at the example: I check "if name", where name is an empty string. Because the empty string is falsy, that branch is skipped and we fall through to the else, printing "no name given". This is wonderfully handy — instead of writing "if the length of name is greater than zero", idiomatic Python just writes "if name". Once this clicks, your code gets shorter and clearer everywhere.

### Slide 6 — Boolean logic and chained comparisons

The last piece of decision-making is combining conditions, and Python gives you three plain English words to do it: and, or, and not. Look at the first example: if age is at least eighteen and the person has a ticket, let them enter — both conditions must be true. The "or" word needs just one side to be true. And "not" simply flips a condition — "if not done" means "if done is false, keep going". These read so naturally that you barely have to think about them. And then there's a lovely Python feature in the last example, called chained comparison. I've written "if zero is less than or equal to hour, less than twenty-four". In most languages you'd have to write that as two separate comparisons joined with "and", but Python lets you chain them exactly the way you'd write it in mathematics. It means the same thing — zero is less than or equal to hour, and hour is less than twenty-four — but it reads like a number line, which is much friendlier. So that's decisions: if, elif, else, truthiness, and the boolean words. Now let's make code repeat.

---

## Segment 3 · `for` / `while` / `range`, break / continue / pass, loop-else (0:26–0:43)

### Slide 7 — for loops and range

There are two kinds of loop in Python, and the first one, the for loop, is the one you'll reach for most often. A for loop says "for each item in this collection, do the following". Look at the first example: "for ch in the string a-b-c" walks through that string one character at a time, so ch is "a", then "b", then "c", and we print each one. The for loop doesn't count — it walks through whatever you give it, item by item. Now, very often you do want to count, to do something a fixed number of times, and that's where range comes in. "for i in range of five" gives you the numbers zero, one, two, three, four. And here's the one thing everyone trips on at first, so let me say it clearly: range stops before the number you give it. Range of five gives you five numbers, but they're zero through four — it does not include five itself. You can also give range a start and a stop, like range two to ten, and even a step, like range zero to ten stepping by two, which gives you the even numbers. So: the for loop walks any sequence, and range is how you generate a run of numbers to loop over.

### Slide 8 — while loops and sentinels

The second kind of loop is the while loop, and it works on a completely different idea. A for loop runs a known number of times — once per item. A while loop runs an unknown number of times: it keeps going as long as some condition stays true, and stops the moment that condition becomes false. Look at the first example: I set count to three, and while count is greater than zero, I print it and then subtract one. So it prints three, two, one, and then count hits zero, the condition is false, and the loop stops. Now I want to flag the single most common beginner bug with while loops, so please remember this: inside the loop, something must move you toward the exit. In that example, the "count minus equals one" line is doing that job — without it, count would stay at three forever and the loop would never end. That's called an infinite loop, and we've all written one. The second example shows a really common real-world pattern: while True, which loops forever on purpose, but inside we read a line of input and break out the moment the user types "quit". That word "quit" is what we call a sentinel — a special value that signals "we're done". So the rule of thumb is: use a for loop when you know how many times, and a while loop when you're waiting for something to happen.

### Slide 9 — break, continue, pass

Now let's learn to steer a loop from the inside, because sometimes you don't want to run every iteration straight through. There are three little keywords for this. The first is continue, and it means "I'm done with this particular item, skip straight to the next turn of the loop". In the example, if n is negative, we continue, which jumps past everything below and starts the next iteration — so negatives never reach the process line. The second is break, and it's stronger: break means "stop the whole loop right now and get out". In the example, the moment n equals our stop value, we break, and the loop ends entirely, no matter how many items were left. So continue skips one, break leaves for good. And the third keyword, pass, is the odd one out — it does absolutely nothing. It's a placeholder. Python sometimes requires a statement where you haven't written the real code yet, and pass fills that hole so the code is valid while you think. You'll use continue and break constantly; pass you'll use occasionally as scaffolding. Keep them straight and your loops will do exactly what you mean.

### Slide 10 — The loop-else clause

Now here's a feature that surprises almost everyone, and once you understand it, you'll find it genuinely elegant: loops in Python can have an else clause. Yes, an else — on a loop, not just on an if. And here's exactly what it does: the else block runs when the loop finishes normally, all the way through, without ever hitting a break. If the loop does break out early, the else is skipped. Let me read the example because it's the perfect use case. We loop over our inventory looking for a target item. If we find it, we print "found it" and break. And then — lined up with the for, not inside it — there's an else that prints "not found". So if the item was in the inventory, we break, and the else never runs. But if we get all the way through the whole list without ever finding it and breaking, the loop finishes normally, and the else fires, telling us "not found". This is the classic search-and-report-failure pattern, and it saves you from messy little "found" flag variables. I know the word "else" on a loop reads oddly — a lot of people wish it were called "nobreak" — but read it as "the loop finished without breaking" and it'll make sense every time. Right — we've covered loops and their controls. Let me show you all of this running live.

---

## Segment 4 · Demo 7 — loops & range patterns (0:43–1:05)

### Slide 11 — Let's run every loop, live

Let me share my screen, and we'll run through every kind of loop and every control we just talked about, so you can see exactly how they behave. Watch closely — you'll rebuild all of this in your lab this week.

### Slide 12 — Watch this, Demo 7

Here's what I'm going to show you in this first demo. I'll start with for loops driven by range, and a for loop walking through a string, so you see both the counting style and the walking style. Then I'll write a while loop that counts down to zero using a sentinel. After that I'll bring in our loop controls — I'll use continue to skip the even numbers, and break to stop the loop early once we've gone far enough. And finally I'll show you that loop-else clause in action, with a little search that prints "not found" only when the loop never found what it was looking for. Keep your eye on which lines run and which get skipped.

### Slide 13 — Loops in action

So here I am at the prompt. Let me start with a for loop over a range that has a step: for i in range zero to six stepping by two. I run it, and look — it prints zero, two, four. It started at zero, stepped by two each time, and stopped before six, just as we said range always does. Now let me show you continue and break working together. I'll loop over range of ten, and inside I'll say: if n is even — that's n modulo two equals zero — continue, skip it. Then: if n is greater than five, break, stop entirely. Otherwise, print n. So watch what comes out: one, three, five. The even numbers got skipped by continue, the odd numbers one, three, and five got printed, and then when we reached seven — which is greater than five — break stopped the whole loop before it could print anything more. So you can see continue quietly skipping turns and break decisively ending the loop. That combination — skip some, stop at a limit — is something you'll use all the time.

### Slide 14 — Search with loop-else

Now let me show you that loop-else clause we talked about, because seeing it run makes it click. I'll set a target of seven. Then I loop over the list one, three, five, and inside the loop I check: if n equals the target, print "found" and break. And lined up with the for, I add an else that prints "not found". Now I run it. And what do we get? "not found". Why? Because seven was never in our list — one, three, and five all failed the test, we never hit that break, the loop ran all the way to the end normally, and so the else fired. Now, just to prove it, imagine the target were three instead: the loop would find it, print "found", break out, and the else would be completely skipped — we'd never see "not found". So the else is your "the loop finished without finding anything" handler. No flag variable, no extra bookkeeping — just a clean else on the loop. Beautiful, isn't it?

### Slide 15 — What you just saw

So let's take stock of that first demo. We saw range driving counted loops, and a for loop walking straight through the characters of a string. We saw a while loop repeating until its condition flipped to false. We saw continue skip a single iteration and keep the loop alive, and break leave the loop entirely the moment we hit our limit. And we saw the loop-else clause run only when no break happened — our clean way to handle "we looked and didn't find it". If any of that went by quickly, remember it's all in the recording and you'll rebuild every line of it yourself this week. Now let's move on to the most powerful branching tool in modern Python.

---

## Segment 5 · `match` / `case` — all pattern kinds + guards (1:05–1:27)

### Slide 16 — Pattern matching

We're now going to meet structural pattern matching — the match and case statement. This is a relatively modern addition to Python, and it's genuinely one of the nicest features in the language for writing clear, readable branching code. Let me explain what it is and why you'd want it.

### Slide 17 — Why match / case

So, why do we need match and case when we already have if, elif, else? Here's the thing. If-elif chains are perfect when you're asking simple yes-or-no questions. But often your data has structure — it's a list of a certain length, or a dictionary with certain keys — and you want to branch on what shape it has, and pull pieces out of it at the same time. That's exactly what match and case is built for. It doesn't just ask "is this equal to that?" — it asks "does this match this shape?", and if it does, it can reach in and grab the pieces for you in the same line. It's been a stable part of the language since Python three-ten, and it's unchanged in our version, three-fourteen. Look at the little teaser on screen: we match on a status code, and we have a case for four hundred, a case for four-oh-four, and then a case with just an underscore. That underscore is special — it's the wildcard, and it matches absolutely anything, so it's your catch-all at the bottom. Let's go through every kind of pattern you can write, because each one unlocks something useful.

### Slide 18 — Literal, capture and wildcard

Let's start with the three simplest patterns. The first is a literal pattern — that's just an exact value, like the case "quit" on screen. If the thing we're matching is exactly the string "quit", that case runs. Simple, like an equals check. The second is the capture pattern, and this one trips people up, so listen carefully. When you write a plain name in a case — like "case other" on screen — that is not a comparison against a variable called other. Instead, it matches anything, and it grabs whatever was matched and stores it in a new variable called other, which you can then use. So a bare name captures the value rather than comparing to it. That's a really important distinction: a literal like "quit" compares, but a bare name like "other" captures. And the third is the wildcard, the underscore. It matches anything, just like a capture, but it deliberately throws the value away — it doesn't bind it to a name. You use the underscore as your final "anything else" case, when you just want to handle the leftovers and don't care about the value. So: literals compare, names capture, underscore catches-all and discards.

### Slide 19 — Sequence and or-patterns

Now things get powerful. You can match on the shape of a sequence — a list or a tuple. Look at the first case: case, open bracket, "add", comma, task, close bracket. This matches only when the thing we're matching is a sequence of exactly two items, where the first item is the literal string "add" — and when it matches, it captures the second item into a variable called task. So in one line, we've checked the length, checked the first element, and grabbed the second element. That's the magic of structural matching. The second case shows an or-pattern, using the vertical bar: case, list with "list", bar, list with "ls". That means "match if it's the one-item list 'list', OR the one-item list 'ls'" — the vertical bar lets a single case handle several alternatives, so you don't repeat the body. And the third case shows the star pattern: case, open bracket, "tags", comma, star-rest. The star-rest soaks up all the remaining items into a list called rest, however many there are. So if someone types "tags urgent home work", "tags" matches the literal, and rest becomes the list of all the words after it. Length-checking, value-checking, and capturing — all woven into one readable pattern.

### Slide 20 — Mapping patterns and guards

Two more pattern kinds and you'll have the full toolkit. The first is mapping patterns, for matching dictionaries. Look at case, curly brace, "cmd", colon, c. That matches any dictionary that has a key called "cmd", and it captures that key's value into a variable c. And importantly, it doesn't care about extra keys — as long as the "cmd" key is there, it matches, and any other keys in the dictionary are simply ignored. That's exactly what you want when you're processing structured data where you only care about certain fields. The second feature here is the guard, and it's a wonderful refinement. A guard is an extra "if" condition you bolt onto the end of a case. Look at the example: case, list with "done" and id, then "if id dot isdigit". This case only matches when the input is a two-item list starting with "done" AND the captured id is made of digits. So the pattern checks the shape, and the guard adds an extra runtime condition on top. If the shape matches but the guard is false, Python moves on to the next case. Guards let you be as precise as you need to be.

### Slide 21 — The order rule

Before we demo this, one critical rule that ties everything together: order matters, enormously. Just like with if-elif, Python tries your cases from the top down, and the very first one that matches wins — it runs that case and stops looking. So you must always put your specific patterns above your general ones. Look at the example: case zero is checked first, then case int — which matches any integer — then case underscore, the catch-all, at the very bottom. If you put case int before case zero, then the number zero would match the int case first and your special zero handling would never run. So the discipline is: most specific at the top, most general at the bottom. And I strongly encourage you to always end a match with a case underscore, your catch-all, so that no input ever silently falls through unhandled. That habit will save you from a lot of confusing bugs. Right — let's see a real match statement do real work.

---

## Segment 6 · Demo 8 — match / case command dispatcher (1:27–1:50)

### Slide 22 — Let's route a command by its shape

Let me share my screen again. We're going to build something that ties together everything we just learned about pattern matching — a little command dispatcher that takes whatever a user types and routes it to the right action based on its shape. This is exactly the pattern your capstone application will use for its menu, so this demo is one to watch carefully.

### Slide 23 — Watch this, Demo 8

Here's the plan. I'm going to write a function that takes a command string, splits it into words, and then runs a match on that list of words. I'll write a case for adding a task, a case for marking a task done — with a guard to make sure the id is actually a number — a case for listing that accepts either "list" or "ls" using an or-pattern, a case for a dictionary input, and finally a catch-all case with the underscore. Then I'll feed it several different inputs live and we'll watch each one get routed to exactly the right branch. You'll see literal, capture, sequence, or-, and mapping patterns, plus a guard, all working together in one statement.

### Slide 24 — The dispatcher

So let me write the function. I'll call it dispatch, and it takes a command string. The first thing I do is match on command dot split, which breaks the string into a list of words. Now the cases. First, case, "add", star-words — so any command starting with "add", and star-words captures everything after it; I join those words back together and return "added:" with the text. Next, case "done", id, with a guard "if id dot isdigit" — this only matches a two-word "done" command where the second word is all digits, and I return "completed" with that id. Next, case list with "list", bar, list with "ls" — my or-pattern, so either word works, and I return "showing all tasks". Then a case for the empty list — that's when someone just pressed enter without typing anything — and I gently return "type a command". And finally, case underscore, my catch-all, returning "unknown command" for anything that didn't fit. Notice how readable this is: each case is one line describing a shape, and the whole set of possible commands is laid out like a little table. That clarity is the whole point of match.

### Slide 25 — One input, the right route

Now let me run it with a few different inputs and watch the routing. First I call dispatch with "add buy milk". The "add" case matches, star-words captures "buy" and "milk", and I get back "added: buy milk". Now dispatch with "done 7" — the "done" case matches, the guard checks that "7" is all digits, which it is, and I get "completed number 7". Now I'll try "ls" — that hits my or-pattern and returns "showing all tasks", and note that "list" would do exactly the same thing. And finally let me throw it something nonsensical — dispatch with "frobnicate" — and it falls all the way through to the catch-all and returns "unknown command", which is exactly the graceful behaviour we want. One match statement, five different command shapes, each routed perfectly. And as I said — this is precisely the dispatcher shape your capstone's menu will be built on, so you're seeing your future code right here.

### Slide 26 — What you just saw

Let's take stock of that demo. We built one match statement that routed five completely different command shapes to five different actions. We used a literal pattern for the command words, capture patterns to grab the task text, a sequence pattern with a star to soak up a variable-length tail, an or-pattern so one case handled two spellings, and a guard to make sure an id was really a number. And we ended with a case underscore that caught everything unexpected and handled it gracefully, so no input ever crashed our dispatcher. That's the power of structural pattern matching: clear, safe, readable branching on the shape of your data. You'll lean on this for the rest of the course.

---

## Segment 7 · Q&A + wrap + Lab 4 brief (1:50–2:00)

### Slide 27 — Wrap-up

We're coming to the end, so let me take your questions in a moment, but first let me hand you off to your homework — Lab 4 — and walk you through exactly what it's asking for.

### Slide 28 — Lab 4, game and menu

Here's Lab 4, and it has two parts that together exercise everything we did today. The first part is a number-guessing game. You'll use a while loop to keep asking the player to guess, you'll break out of that loop the moment they guess correctly, and — this is the nice touch — you'll add a loop-else that runs only when they run out of tries without guessing, to tell them they've lost and reveal the number. So that's your while loop, your break, and your loop-else, all in one fun little program. The second part is a file called menu dot py, which reads a command line and routes it with a match statement, exactly like the dispatcher I just showed you. Your menu should handle at least four different command shapes, plus an unknown fallback using the case underscore. And then you submit both scripts, along with a few sample runs showing them working, to our course channel on Microsoft Teams. The whole lab mirrors Demo 7 for the game and Demo 8 for the menu, so the recording walks you through every piece. Take your time, pause as often as you need, and remember it's completely normal to make a few attempts before it clicks.

### Slide 29 — Next week

Looking ahead: next week is Week 5, Functions, Scope, and Type Hints, and it's a real turning point in the course. Up to now we've been writing logic loosely; next week we learn to wrap that logic — the decisions and loops you mastered today — into clean, reusable, named functions. In fact, the match-based menu you write this week will become a tidy, typed, documented function next week, so the work you do in this lab feeds directly forward. So please do get Lab 4 working this week — it's the foundation we build functions on top of.

### Slide 30 — See you in the lab

That's a wrap for Week 4. You've taught your programs how to make decisions, how to repeat work, and how to react to the shape of their input — that's an enormous amount of real programming power, and your code can do genuinely interesting things now. Go and have fun with the guessing game especially; it's the first program in this course that actually plays back with you. I'll see you in the lab and back here next week. Now — what questions do you have? This is the perfect time to ask.
