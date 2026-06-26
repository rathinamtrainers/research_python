# Speaker Notes — Session 3 · Strings & Text

*Python Mastery — Part 1: Foundations · Week 3. Read aloud, top to bottom.*

---

## Segment 1 · Recap of Week 2 (0:00–0:08)

### Slide 1 — Strings & Text

Hello everyone, and welcome back — this is Week 3 of Python Mastery, Part 1, Foundations, and today is all about strings and text. We're live on Microsoft Teams as always, and the session is being recorded, so if anything moves quickly, you can come straight back to this recording later and rebuild it at your own pace. So far we've taught Python how to do arithmetic; today we're going to teach it to handle words, and by the end you'll be producing neat, professional-looking output. Let's get going.

### Slide 2 — Recap / where we left off

Before we open anything new, let me take just a couple of minutes to remind you of where we got to last week, because today builds directly on it. Let's recap Week 2.

### Slide 3 — Last week, Numbers

Last week we worked with numbers. We met the numeric types — `int` for whole numbers, `float` for decimals, `complex`, and `bool`, which is really just a special kind of integer. We learned the three different division operators and why they're different: a single slash gives you true division with a decimal result, a double slash gives you floor division that throws away the fractional part, and the percent sign gives you the remainder. We saw exponentiation with the double star, and we talked about operator precedence — the order Python does things in. We learned that a variable in Python is really just a name bound to an object, and that `None` is Python's way of saying "there's no value here." And crucially, we learned that floating-point numbers are approximate — that `0.1 plus 0.2` famously doesn't give you exactly `0.3` — and we learned our three defences: rounding, careful formatting, and `math.isclose`. So you can already use Python as a calculator. Today we make it talk — we make it produce text.

### Slide 4 — Today's goal

So here's our map for the next two hours. By the end of today you'll be able to index and slice strings — that is, reach into a piece of text and pull out exactly the part you want — and you'll know the handful of string methods you'll use every single day. You'll understand why strings are immutable, which sounds technical but is actually a simple and helpful idea. You'll learn to build clean, formatted output using f-strings and what's called the format mini-language, which is how you make numbers and columns line up beautifully. You'll meet a brand-new kind of string in Python 3.14 called a t-string, and you'll be able to say how it differs from an f-string. And finally we'll demystify the difference between text and bytes, and explain why something called encodings exists. That's a lot of useful ground, so let's dive in.

---

## Segment 2 · str literals, indexing, slicing, methods, immutability (0:08–0:33)

### Slide 5 — Strings, up close

Let's start with the absolute basics of what a string is and how you reach into it. Literals, indexing, slicing, and the everyday methods — that's this section.

### Slide 6 — What a string is

So, what is a string? A string is simply an ordered sequence of characters — it's how Python holds text, whether that's a single word, a whole sentence, or an entire document. You create one by wrapping text in quotes, and here's a nice thing about Python: single quotes and double quotes mean exactly the same thing, so `'hello'` in single quotes and `"hello"` in double quotes are identical. Use whichever is more convenient — for example, if your text contains an apostrophe, you'd reach for double quotes so the apostrophe doesn't confuse Python. When you need text that spans multiple lines, you use triple quotes — three quote marks at each end — and everything in between, including the line breaks, becomes part of the string. Now, sometimes you need special characters that you can't easily type, and for those we use escape sequences: a backslash followed by `n` means a newline, a backslash followed by `t` means a tab, and a double backslash means an actual backslash character. And finally, there's a handy thing called a raw string: if you put the letter `r` right before the opening quote, Python treats every backslash literally and doesn't interpret it as an escape. That's incredibly useful for things like Windows file paths, where backslashes are everywhere and you don't want them turning into newlines and tabs by accident.

### Slide 7 — Indexing

Now let's reach into a string. Every character in a string has a position, called its index, and you access it with square brackets. Here's the one thing that trips up absolutely every beginner, so let me say it clearly: counting starts at zero, not one. So for the word "Python", position zero is the capital P, position one is the y, and so on. If you ask for `word[0]` you get back `'P'`. Now here's a lovely Python convenience: you can also count from the right-hand end using negative numbers. `word[-1]` gives you the very last character, the n; `word[-2]` gives you the second-to-last. This saves you from having to know the exact length just to grab the last character. One caution: if you ask for an index that's beyond the end of the string, Python will stop and raise an `IndexError`, which is just its polite way of telling you there's no character at that position.

### Slide 8 — Slicing

Indexing gives you one character; slicing gives you a whole run of them — a substring. You write it with a colon inside the brackets: `word[1:4]` means "give me the characters from position 1 up to, but not including, position 4." That "not including the end" part is the second thing that catches beginners, so hold onto it: the start is included, the stop is excluded. If you leave out the number before the colon, Python assumes you mean "from the very beginning," so `word[:2]` gives you the first two characters. If you leave out the number after the colon, it means "all the way to the end." And there's a third, optional number — the step — which says how many characters to jump each time. The most famous use of the step is `word[::-1]`, with a step of minus one, which walks through the string backwards and so reverses it. So with just indexing and slicing you can pull out any character or any chunk of a string you like.

### Slide 9 — High-value string methods

Now, strings come with a whole toolbox of built-in methods — little functions attached to the string that do useful things — and while there are dozens, you'll reach for the same handful constantly, so let me introduce them. `.strip()` removes any spaces or whitespace from the start and end of a string, which is brilliant for cleaning up messy input. `.lower()`, `.upper()`, and `.title()` change the case — all lowercase, all uppercase, or Title Case With Each Word Capitalised. Then there's a pair that work together beautifully: `.split()` takes a string and breaks it into a list of pieces wherever it finds a separator you choose, like a comma; and `.join()` does the exact opposite, taking a list of pieces and gluing them back into one string with a separator of your choice. `.replace()` swaps one piece of text for another throughout the string. `.find()` and `.index()` tell you where a piece of text appears. And `.startswith()` and `.endswith()` answer yes-or-no questions about how a string begins or ends. Learn these few and you'll handle ninety percent of everyday text work.

### Slide 10 — Strings are immutable

Now here's a concept that sounds intimidating but is genuinely simple and helpful: strings are immutable. Immutable just means "cannot be changed." Once you've created a string, you cannot reach in and change one of its characters. If you try — if you write `word[0] = "J"` to try to swap the P for a J — Python refuses and raises a `TypeError` that says, quite literally, "'str' object does not support item assignment." So if strings can't be changed, how do all those methods work? Here's the key insight: every string method returns a brand-new string and leaves the original completely untouched. When you call `.upper()`, it doesn't modify your string — it hands you back a fresh, upper-cased copy. This is actually why we can chain methods together so cleanly, which you'll see in a moment: each method produces a new string, and the next method acts on that. So immutability isn't a restriction that gets in your way; it's the thing that makes string handling predictable and safe.

---

## Segment 3 · Demo 5 — slicing & method chaining (0:33–0:55)

### Slide 11 — Let's clean up some messy text

Alright — that's enough theory. Let's open Python and actually clean up some genuinely messy text together, so you can see all of this working. Let me share my screen.

### Slide 12 — Watch this, Demo 5

Here's what I'm about to do, so you know what to watch for. I'm going to start with a deliberately messy string — `"  Buy Milk, Eggs, Bread  "`, with extra spaces around it and mixed-up casing. I'll clean it up by chaining methods together: first stripping the spaces, then lowercasing, then splitting it on the commas into a list of separate items. Then I'll pick out a single word and use indexing and slicing on it — grabbing its first character, its last character, its first three letters, and even reversing it. Then I'll rebuild a clean, tidy string from the pieces using join. And right at the end, I'll prove to you that through all of this, the original string never changed at all — that's immutability in action. Watch closely; you'll rebuild every bit of this in your lab.

### Slide 13 — Method chaining, step by step

So here I am at the REPL. I'll put our messy text into a variable called `raw` — notice the extra spaces at both ends and the mixed casing. Now watch me chain methods. I type `raw.strip().lower()` and press enter, and look — in one line, Python trimmed the surrounding spaces and lowercased everything, giving me `'buy milk, eggs, bread'`. That's two methods working one after another: strip handed its result straight to lower. Now let me split this into separate items. I'll build a list where I take `raw.strip().split(",")` to break it on the commas, and I'll strip each piece as I go to remove the leftover spaces around the words. The result is a clean list: `'Buy Milk'`, `'Eggs'`, `'Bread'` — three tidy items. Now let me show you indexing and slicing on a single word. If I take the word `'Bread'`, then position zero is `'B'`, position minus one is `'d'`, the slice from the start to three gives me `'Bre'`, and the slice with a step of minus one reverses it to `'daerB'`. And finally, let me glue my clean list back into a single string: I take `", "` — a comma and a space — and call join on it with my list, and out comes a perfectly formatted `'Buy Milk, Eggs, Bread'`. Now here's the punchline. Let me print the original `raw` variable again — and look, it still has all its original spaces and capitals, completely unchanged. Every method I called returned a new string; not one of them touched the original. That is immutability, and that's exactly why chaining is safe.

### Slide 14 — What you just saw

So let's take stock of what just happened. We took one messy string and reshaped it entirely by chaining methods together, each one handing its result to the next. We saw that split turns a string into a list of pieces, and join does the reverse, turning a list back into a single string. We used indexing and slicing to pull out exactly the characters we wanted from a word. And most importantly, we confirmed that the original string stayed exactly as it was the whole time, because string methods always return new strings rather than changing the old one. If any of that went by quickly, remember it's all in the recording, and you'll do it yourself this week.

---

## Segment 4 · f-strings & the format mini-language; t-string intro (0:55–1:17)

### Slide 15 — Formatting output

Now we move to what is honestly one of the most satisfying parts of learning Python: making your output look good. We're going to learn f-strings and the format mini-language — the tools that turn raw values into neat, aligned, professional output.

### Slide 16 — f-strings, the everyday tool

Let me introduce the f-string, which is the tool you'll use to build text out of values more than any other. An f-string is just a normal string with the letter `f` placed right before the opening quote. The magic is that inside it, anywhere you put a value in curly braces, Python replaces those braces with the actual value when it builds the string. So if I have a variable `name` holding `"Eggs"` and a variable `qty` holding `12`, then the f-string `f"{name}: {qty}"` becomes `'Eggs: 12'`. And it's not limited to plain variables — you can put any expression inside the braces. You can do arithmetic, like `f"{qty * 2}"`, or call a method, like `f"{name.upper()}"`, and Python works it out and drops the result straight in. One important detail to file away for later today: an f-string evaluates immediately and gives you back an ordinary, finished `str` — the text is fully built right then and there. Keep that fact in mind, because it's exactly what's going to distinguish an f-string from a t-string in a little while.

### Slide 17 — Self-documenting {value=}

Here's a small f-string feature that you will fall in love with the moment you start debugging your own programs. If you put an equals sign right after the expression inside the braces, the f-string prints both the expression itself and its value. So `f"{qty=}"` doesn't just give you `12` — it gives you `'qty=12'`, showing you the name and the value together. And it works with expressions too: `f"{qty * 2 =}"` shows you both `qty * 2` and the answer, `24`. Why is this so wonderful? Because when you're trying to figure out why your program is misbehaving, instead of writing out `print("qty", qty)` and carefully keeping the label and the value in sync by hand, you just write `f"{qty=}"` and Python labels it for you automatically. It's a tiny feature that saves you enormous amounts of fiddling, and once you start using it you'll wonder how you managed without it.

### Slide 18 — The format mini-language

Now for the part that makes your output look truly polished: the format mini-language. Inside the braces of an f-string, after the value, you can add a colon and then a little specification that controls exactly how the value is displayed. Let me walk you through the pieces you'll use most. First, width and alignment. If you write a value followed by `:<10`, Python pads it with spaces on the right so the whole field is at least ten characters wide, left-aligned. A greater-than sign instead, like `:>8`, right-aligns it within eight characters — which is exactly what you want for numbers in a column. And a caret, like `:^6`, centres it. Second, thousands grouping: if you put a comma in the spec, like `{1234567:,}`, Python inserts thousands separators and gives you `1,234,567` — instantly readable. Third, fixed decimal places: `.2f` means "show this as a float with exactly two decimal places," so `3.14159` formatted with `.2f` becomes `3.14`. And the real power comes from combining these: something like `:>10,.2f` means right-aligned, ten wide, with thousands grouping and two decimals, all at once. That single little spec is how you turn a column of raw numbers into a clean financial-looking report.

### Slide 19 — Let's format a tidy report line

Let me show you all of this producing a genuinely tidy report line, and then I'll introduce you to the new t-string. Let me share my screen again.

### Slide 20 — Watch this, Demo 6

Here's the plan for this demo. I'm going to build an aligned report row using the format mini-language — fixing the width of each column so everything lines up — and I'll show you the thousands grouping and the self-documenting equals trick along the way. Then comes the new part: I'm going to write that exact same interpolation as a t-string, with a `t` prefix instead of an `f`, and we'll discover that it does something quite different. I'll use `type()` to reveal that a t-string doesn't give us a finished string at all — it gives us a `Template` object. And I'll explain in one clear sentence what t-strings are actually for. This is the freshest, most cutting-edge thing in today's session, so watch closely.

### Slide 21 — Aligned report row

So here at the REPL, let me set up three values: a `name` of `"Eggs"`, a `qty` of `12`, and a `price` of `3.5`. Now I'll build a report row with fixed-width fields. I write `f"{name:<10}{qty:>4}{price:>8.2f}"`. Let me read that spec out: the name is left-aligned in a field ten characters wide, the quantity is right-aligned in four characters, and the price is right-aligned in eight characters with exactly two decimal places. And when I run it, look at the result — the columns line up perfectly, and the price shows as `3.50` with its two decimals. Now let me show you grouping on a big number: `f"{1234567.5:>12,.2f}"` gives me `1,234,567.50` — right-aligned, twelve wide, with thousands commas and two decimals. The reason everything lines up so neatly is that every field has a fixed width, so no matter how long or short the actual value is, the column stays the same size. This is exactly the technique your lab asks you to use to print an aligned table.

### Slide 22 — f-string vs t-string

Now here's the new and exciting part. Let me take a simple interpolation — `name` holding `"Eggs"` — and write it first as an f-string: `f"Item: {name}"`. As we already know, that immediately gives me the finished string `'Item: Eggs'`. Now watch what happens when I change just one letter — I swap the `f` for a `t` and write `t"Item: {name}"`. Instead of a finished string, Python hands me back something called a `Template`. And if I wrap it in `type()` to ask Python what it actually is, it tells me it's a `string.templatelib.Template` — a completely different kind of object from a plain string. So the headline difference is this: an f-string immediately gives you a finished `str`, the text already built. A t-string gives you a `Template`, which is an object that keeps the static parts of your text and the values you interpolated separate, so that you — or more usually, a library you're using — can inspect and process them before the final text is ever assembled.

### Slide 23 — Why t-strings exist

So why on earth would you want a Template instead of a finished string? Let me give you the one-sentence reason, because in Part 1 we just want you to recognise t-strings, not master them. Because a Template keeps the fixed parts of your text and the interpolated values separate, library code can step in and sanitise or transform those values before the text gets stitched together. And the classic, important payoff is security. When you build, say, a database query or a chunk of web-page HTML by jamming user-supplied values straight into an f-string, you open the door to nasty bugs called SQL injection and cross-site scripting, where a malicious value changes the meaning of your query or your page. Because a t-string lets a library see the values separately first and clean them, it can shut that door. T-strings are brand new — they arrived in Python 3.14 under a proposal called PEP 750. For our purposes in Part 1, the goal is simply this: you should be able to look at a `t"..."` and say "that's a t-string, it makes a Template rather than a string, and it exists for safe handling of interpolated values." That's it — recognition, not mastery.

### Slide 24 — What you just saw

Let's take stock. We used a single f-string to produce an aligned row with fixed-width columns, thousands grouping, and two decimal places — output that looks genuinely professional. We saw the self-documenting equals trick print both an expression and its value for easy debugging. And then we changed one letter, `f` to `t`, and discovered that a t-string produces a `Template` object rather than a finished string — and we learned the one-sentence reason that's useful: it lets values be handled safely before the text is built. You can now state, in your own words, how a t-string differs from an f-string, which is exactly what this part of the lab will ask of you.

---

## Segment 5 · bytes vs str, encodings basics + Q&A (1:17–1:50)

### Slide 25 — Bytes & encodings

There's one more piece of the text story I want to give you today, and it's the piece that connects text to the real world of files and networks: the difference between bytes and strings, and the idea of an encoding. Let me explain why text needs a translation step at all.

### Slide 26 — str vs bytes

So far, everything we've done has been with `str` — human-readable text, made of characters like letters, digits, and emoji. But here's the thing your computer never forgets and we usually do: deep down, a computer doesn't store characters. It stores numbers — specifically, small numbers from zero to two hundred fifty-five, called bytes. And when your program reads a file from disk, or receives data over a network, what actually arrives is a stream of those raw bytes, not characters. Python represents that raw form with a separate type called `bytes`, which you'll recognise because it prints with a little `b` in front of the quotes. So we have two different worlds: the `str` world of human text, and the `bytes` world of raw storage. And because they're genuinely different, we need an agreed rule for converting between them — a rule that says which character maps to which bytes. That rule is called an encoding.

### Slide 27 — Encode / decode with UTF-8

Let me make this concrete. Take the word "café" — notice that last character, the e with the accent, which isn't a plain English letter. If I call `.encode("utf-8")` on it, Python converts my text into bytes — and look, most of the characters become one byte each, but that accented e turns into two bytes, `\xc3\xa9`. That conversion from text to bytes is called encoding — it's text going out to storage. And the reverse, taking those bytes and calling `.decode("utf-8")` to turn them back into the original text "café", is called decoding — storage coming back to text. Encode goes string to bytes; decode goes bytes to string. And the encoding I keep naming, UTF-8, is the one you should reach for essentially always — it's the worldwide standard, it can represent every character from every language plus emoji, and it's the sensible default unless someone specifically tells you otherwise. The reason I'm planting this today is that the moment we start reading and writing real files, in Week 12, this text-versus-bytes distinction becomes something you handle directly — and you'll be glad you already understand it.

### Slide 28 — What you just saw

So to sum up this last piece: text and stored bytes are two genuinely different things — characters versus raw numbers. An encoding is the agreed rule that maps between them. The `.encode()` method takes you from text to bytes, and `.decode()` takes you from bytes back to text. And UTF-8 is the standard encoding you'll use by default. This will all click into place even more firmly when we open real files in a few weeks. Now, before we wrap up, this is a perfect moment for your questions — anything at all about strings, indexing, slicing, formatting, t-strings, or encodings. What's on your mind? Don't be shy; if something didn't quite land, someone else is almost certainly wondering the same thing, and this is the best time to ask while it's fresh.

---

## Segment 6 · Wrap + Lab 3 brief (1:50–2:00)

### Slide 29 — Wrap-up / this week's homework

We're coming to the end, so let me hand you off to your homework — Lab 3 — and walk you through exactly what it asks, so you know precisely what to do this week.

### Slide 30 — Lab 3, a text-report formatter

Here's Lab 3, and it mirrors everything we did today. You're going to write a script called `report.py` that holds a few rows of data, each one a name, a quantity, and a price. First, you'll clean up any messy fields using the stripping and slicing techniques from Demo 5 — the method chaining we did on that grocery string. Then, using f-strings and the format mini-language from Demo 6, you'll print those rows as a neatly aligned table, where the columns line up and the prices show two decimal places with thousands grouping for the larger numbers. And finally, to cement your understanding of today's newest idea, you'll add a short comment in your code sketching how one of those lines would look written as a t-string, and noting that its type would be a Template rather than a string, and why. So the whole lab is you rebuilding Demo 5 and then Demo 6 at your own pace. The full written guide, with every step spelled out, is in your lab document, and today's recording is right there to follow along with. Take your time, pause whenever you need, and don't worry if your columns don't line up perfectly on the first try — adjusting the widths until it looks right is part of the learning.

### Slide 31 — Next week

Looking ahead: next week is Week 4, Control Flow and Pattern Matching, where your programs start making decisions and repeating themselves. And here's how today connects: we'll use the strings you've learned to handle today to build the conditions our programs test and the data our loops chew through. In fact, the `match`/`case` feature we'll meet next week is brilliant at parsing command strings — exactly the kind of comma-separated, messy text you cleaned up in today's demo. So everything you've learned about pulling text apart and putting it back together feeds straight into next week.

### Slide 32 — See you in the lab

That's a wrap for Week 3. You've gone from doing arithmetic to producing clean, formatted, professional-looking text, and you've even had a peek at one of the most modern features in all of Python. Go and build that report formatter, enjoy making those columns line up, and I'll see you in the lab and back here next week. Now — are there any last questions before we close?
