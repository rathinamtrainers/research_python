# Speaker Notes — Session 2 · Numbers, Variables & Expressions

*Python Mastery — Part 1: Foundations · Week 2. Read aloud, top to bottom.*

---

## Segment 1 · Recap of Week 1 + today's goal (0:00–0:08)

### Slide 1 — Numbers, Variables & Expressions

Hello again everyone, and welcome back to Python Mastery, Part 1, Foundations. This is Week 2, and today we're getting into the actual stuff of programming — numbers, the values your programs work with, and the names you give those values. As always we're live on Microsoft Teams and we're recording, so if anything goes by quickly you'll be able to come back and watch it again whenever you like. Let's get started.

### Slide 2 — Recap / where we got to last week

Before we dive in, let me take just a moment to remind you where we left things last week, because today builds directly on it. Last week you installed the whole toolchain — Python 3.14, and the tools uv, Ruff, and VS Code. You met the REPL, that interactive prompt where you type one line and Python answers you straight away. You learned to run a script two different ways, by its filename and with the dash-m form. And you scaffolded your very first project, taskapp, using uv init, and you tidied it up with Ruff. Everything you set up last week is exactly what we use today. So if you've got your environment working, you are completely ready for this session.

### Slide 3 — Last week, in one breath

Here's that recap in one quick summary so it's fresh in your mind: Python 3.14 installed, the REPL where you talk to Python, scripts run two ways, and the taskapp project created and formatted. The two things I'll actually want open today are your REPL and your taskapp project. If you didn't quite finish last week's lab, don't panic — you can still follow along today and catch up on the setup afterward from last week's recording.

### Slide 4 — Today's goal

So here's our map for the next two hours. By the end of today you'll be able to use Python's four number types — integers, floats, complex numbers, and booleans. You'll be able to tell apart the three kinds of division — true division, floor division, and modulo — and use the power operator. You'll be able to look at an expression and predict the answer by understanding operator precedence. You'll understand how variables really work, how names point at values, and what the special value None is for. And finally, you'll learn about the single most famous surprise in all of programming — floating-point imprecision — and the three standard ways to defend against it. That's a lot of genuinely useful ground, so let's begin with the numbers themselves.

---

## Segment 2 · Numeric types, operators, precedence (0:08–0:30)

### Slide 5 — Numbers & operators

Let's start at the foundation of everything: the values your programs compute with. We'll look at the kinds of numbers Python understands and the operators that work on them.

### Slide 6 — The four numeric types

Python has four built-in types for numbers, and I want you to know all four by name even though we'll use two of them constantly and the other two only occasionally. The first is the integer, written `int`, which is for whole numbers — things like forty-two, or negative seven. And here's something genuinely lovely about Python: integers have unlimited size. In many other languages, integers overflow once they get too big, but in Python you can compute ten to the power of a hundred and Python just hands you the full, exact answer with every digit. The second type is the float, short for floating-point number, which is for numbers with a decimal point — three point one four, or two point zero. The third is the complex number, which has a real part and an imaginary part, written with a `j`, like two plus three-j. You'll rarely need complex numbers unless you're doing certain kinds of maths or engineering, but it's good to know Python has them built in. And the fourth type is the boolean, written `bool`, which is just `True` or `False`. Now here's a quirk worth knowing: in Python, a boolean is actually a special kind of integer. `True` behaves like the number one and `False` behaves like the number zero. So if you add `True` plus `True`, Python genuinely answers two. That looks strange the first time you see it, but it follows naturally once you know booleans are integers underneath. And if you ever want to ask Python what type a value is, you just type `type` and the value in brackets, and it tells you.

### Slide 7 — Arithmetic operators

Now let's meet the operators — the symbols that do arithmetic. The first three are exactly what you'd expect from a calculator: plus for addition, minus for subtraction, and the asterisk for multiplication. The interesting ones are the three that all relate to division, because Python deliberately gives you three different division operators for three different jobs. The first is a single forward slash, and that's true division — it gives you the full decimal answer, and here's the key thing to remember: true division always gives you a float, always, even when the numbers divide evenly. So four divided by two gives you two point zero, not just two, because the slash always produces a float. The second is a double forward slash, and that's floor division — it divides and then rounds down to a whole number, throwing away any fractional part. The third is the percent sign, and despite looking like a percentage, in programming this is the modulo operator, which gives you the remainder after division. And finally there's a double asterisk, which is the power operator — two double-asterisk ten means two raised to the power of ten. So look at the table: seven slash two is three point five, seven double-slash two is three, seven percent two is one, and seven double-asterisk two is forty-nine.

### Slide 8 — `//` and `%` travel together

I want to spend an extra moment on floor division and modulo, because they're the two that beginners find least intuitive, and they almost always travel together as a pair. Think of it like this. If you have seventeen of something and you want to put them into groups of five, floor division tells you how many full groups you get — seventeen double-slash five is three full groups. And modulo tells you how many are left over that didn't fit into a full group — seventeen percent five is two left over. Three full groups of five, with two left over — and notice three times five plus two gives you back seventeen. That's the relationship. This pair is incredibly useful in real programs: converting total seconds into minutes and seconds, working out change from a payment, splitting items into rows, anything where you divide something into whole chunks and care about the leftover.

### Slide 9 — Mixing types & precedence

Two more important ideas before we go to the demo. The first is what happens when you mix types in one calculation. If you add an integer and a float together — say two plus three point zero — Python widens the answer to the more general type, the float, so you get five point zero. The rule is that the narrower type, the integer, gets promoted to the wider type, the float, so they can be combined. The second idea is precedence, which is the order Python applies operators in. Just like in school maths, some operators bind more tightly than others. The power operator is strongest. Then multiplication and all the division operators come next. Then addition and subtraction come last. And when two operators are at the same level, Python works left to right. So two plus three times four is fourteen, not twenty, because the multiplication happens before the addition. If you actually wanted the addition first, you wrap it in parentheses — open-bracket two plus three close-bracket times four gives you twenty. And honestly, here's my advice for life: when you're not absolutely certain about precedence, just add parentheses. They cost you nothing, they never change a correct answer, and they make your intention crystal clear to anyone reading your code, including yourself in six months.

---

## Segment 3 · Demo 3 — REPL calculator & precedence (0:30–0:55)

### Slide 10 — Let's use Python as a calculator

Right — let's stop talking about operators and actually use them. Let me share my screen and we'll turn Python into a calculator together and watch precedence at work.

### Slide 11 — Watch this, Demo 3

Here's what I'm going to do so you know what to watch for. I'll run each of the division operators side by side so you can see exactly how they differ — true division, floor division, modulo, and the power operator. Then I'll show you precedence directly, comparing what happens with and without parentheses. I'll mix an integer and a float so you can watch the result type change. And then I'll show you two ways of assigning to variables — augmented assignment, where you update a variable based on its own value, and multiple assignment, where you set several names at once. Keep your eyes on the screen; you'll rebuild all of this in your lab.

### Slide 12 — The operators, side by side

So here I am at the REPL. Let me run the four key operators on the same pair of numbers so the difference is unmistakable. I type seven slash two, press enter, and Python gives me three point five — the full decimal answer, and notice it's a float. Now seven double-slash two, and I get three — floor division has thrown away the fractional part and given me a whole number. Now seven percent two, and I get one — that's the remainder, because two goes into seven three times with one left over. And finally seven double-asterisk two, and I get forty-nine — seven squared. So there they are together: the single slash gives a float, the double slash floors to a whole number, the percent gives the remainder, and the double-asterisk raises to a power. Four operators, four very different jobs.

### Slide 13 — Precedence in action

Now let me show you precedence live, because this is where people get caught out. I type two plus three times four. Now before I press enter — what do you think it'll be? If you said fourteen, you've got it: the multiplication runs first, three times four is twelve, plus two is fourteen. Now watch what parentheses do. I type open-bracket two plus three close-bracket times four, and now I get twenty, because the brackets force the addition to happen first, five times four. Same numbers, same operators, completely different answer, all decided by precedence. And here's a subtle one that surprises even experienced programmers: I type two double-asterisk three double-asterisk two. You might expect it to work left to right, two to the third is eight, then eight squared is sixty-four. But the power operator is special — it actually groups right to left. So Python computes three to the power of two first, which is nine, and then two to the power of nine, which is five hundred and twelve. That right-to-left behaviour is unusual, so it's worth remembering, and once again, if you ever want a different grouping, just reach for parentheses.

### Slide 14 — What you just saw

Let's pause and take stock of what we just did. We saw that the single slash always returns a float, while floor division and modulo return whole numbers when you start with whole numbers. We saw that the double-asterisk is the power operator and that, unusually, it groups from the right. We saw that mixing an integer and a float widens the answer to a float. And we saw that parentheses override precedence and, just as importantly, make your code easier to read. If any of that flew by, it's all in the recording and you'll do every bit of it yourself in the lab. Now let's talk about the names we attach to these values.

---

## Segment 4 · Variables, assignment, None, input/casting (0:55–1:12)

### Slide 15 — Variables & None

So far we've been computing values and letting Python throw the answer away. Now let's learn how to hold onto a value by giving it a name — that's what variables are for — and we'll meet a special value called None.

### Slide 16 — Variables: names bind to objects

Here's the single most important idea about variables in Python, and if you come from another language this might be slightly different from what you're used to, so listen closely. When you write x equals five, you are not putting the number five into a box called x. Instead, you are creating a name, x, and pointing it at the value five. The name is a label, a sticky note, attached to an object. Why does this distinction matter? Because it means a name can be re-pointed at any time, even to a completely different kind of value. I can say x equals five, and then immediately say x equals "now a string", and Python is perfectly happy — the name x simply stops pointing at the number and starts pointing at the text. There's no fixed type attached to the name itself. Now, two convenient shortcuts. The first is augmented assignment: when you write x plus-equals three, that's just a shorthand for x equals x plus three — take whatever x currently is, add three, and re-point x at the result. And the same pattern works with minus-equals, times-equals, and the others. The second shortcut is multiple assignment: you can write a comma b equals one comma two and set both names in a single line. And this gives you a beautifully clean way to swap two values — a comma b equals b comma a swaps them in one go, with no temporary variable needed. In many other languages that swap takes three lines; in Python it's one.

### Slide 17 — None, the absence of a value

Now let me introduce a special value called None. None is Python's way of representing "nothing here" — the deliberate absence of a value. And I want to be really clear about what it is not, because beginners mix these up. None is not the number zero. It is not an empty piece of text. It is not False. It is its own distinct thing that means "no value at all." You'll use it constantly: as a starting placeholder for a variable before you have a real value for it, as the answer when a function has nothing meaningful to return, and as a sensible default. One small but important habit: when you want to check whether something is None, you write `is None`, using the word "is", rather than the double-equals you'd use for comparing numbers. So you'd write "if result is None". The reason is that None is a single, unique object in all of Python, and "is" asks exactly the right question — "is this the None object?" Get into that habit now and it'll serve you for your whole Python life.

### Slide 18 — Input is always text

Here's a point that trips up almost every single beginner, so let me make it loud and clear. When you ask the user to type something in, using the input function, what you get back is always text — always a string — even if the user typed digits. So if you ask for someone's age and they type four-two, what Python hands you is not the number forty-two, it's the text "four-two". And that matters enormously, because if you then try to add one to that text, Python doesn't do maths — it gives you a TypeError, complaining that it can't combine text and a number. The fix is to convert, or "cast", the text into a number before you compute with it. You wrap it in int, with brackets, to turn it into an integer, or in float to turn it into a floating-point number. So int of age, plus one, correctly gives you forty-three. Forgetting to cast the result of input is, without exaggeration, the number one bug I see from beginners in this part of the course. So whenever you read a number from the user, your very next thought should be: cast it.

---

## Segment 5 · Demo 4 — Floating-point surprises & fixes (1:12–1:37)

### Slide 19 — Now, the famous floating-point surprise

Alright — it's time for one of my favourite things to show beginners, because it genuinely looks like Python is broken until you understand it, and then it makes perfect sense. Let me share my screen again.

### Slide 20 — Watch this, Demo 4

Here's the plan. I'm going to add nought-point-one and nought-point-two in the REPL, and the answer is going to surprise you — it will not be nought-point-three. I'll explain in one plain paragraph why that happens, and I promise it's not Python being faulty. Then I'll show you the three standard defenses: rounding the result, comparing with a function called math-dot-isclose, and formatting for display. And to tie it all together, I'll read two numbers from the user, cast them, and compute a restaurant tip — exactly the kind of thing your lab asks for. Watch closely.

### Slide 21 — The surprise

So here we go. I type nought-point-one plus nought-point-two and press enter — and look at that. Instead of nought-point-three, Python gives me nought-point-three-zero-zero-zero-zero-zero-zero-zero-zero-zero-zero-zero-zero-zero-zero-four. A tiny sliver of error has crept in. And to drive it home, I'll ask Python directly: is nought-point-one plus nought-point-two equal to nought-point-three? And Python says False. Now, your first reaction might be that Python can't do basic arithmetic, but that's not it at all. Here's the plain explanation. Computers store numbers in binary, in base two. And just as you can't write one-third exactly as a decimal — it's nought-point-three-three-three-three forever — most everyday decimal fractions, including nought-point-one and nought-point-two, can't be written exactly in binary either. So Python stores the very closest binary value it can, which is almost but not perfectly nought-point-one, and when you add two of these almost-perfect values, the tiny errors add up and peek out at the far end of the number. And here's the really important part: this is not a Python flaw. Almost every programming language behaves exactly the same way, because they all use the same standard for storing floating-point numbers. So floats are approximate, and once you accept that, you just learn to work with it.

### Slide 22 — The three defenses

So how do we work with it? Three standard defenses, and you'll reach for these constantly. The first is round. If I type round, and inside the brackets nought-point-one plus nought-point-two, comma two — that two means "round to two decimal places" — Python gives me a clean nought-point-three. So when you want a tidied value to keep using, round it. The second defense is for when you need to compare two floats. Remember, comparing with double-equals failed. Instead, you import the math module and use math-dot-isclose. I type math-dot-isclose, and inside the brackets nought-point-one plus nought-point-two, comma nought-point-three — and now Python says True. What isclose does is check whether two numbers are close enough to be considered equal, within a tiny tolerance, which is exactly the right question to ask about approximate values. So the rule is simple and worth memorising: never compare floats with double-equals; use math-dot-isclose. The third defense is for display. When you just want to show a number nicely to a user, you format it — here I use an f-string with a colon-dot-two-f format, and it shows nought-point-three-zero, a clean two-decimal string. We'll go much deeper on this formatting next week with strings, but I want you to see it now so you know the option exists. So: round to get a clean value, isclose to compare, and format to display.

### Slide 23 — What you just saw

Let's take stock of the floating-point story. We saw that nought-point-one plus nought-point-two is not exactly nought-point-three, because floats are approximate — they're stored as binary fractions and most decimals don't fit exactly. We learned the golden rule: never compare floats with double-equals, always use math-dot-isclose. We saw that round gives you a clean value and formatting gives you clean display. And we tied it back to input: the user's typing always arrives as text, so you cast it with int or float before you do any maths. Those few habits will save you from a whole category of confusing bugs. Now let's open it up for your questions.

---

## Segment 6 · Q&A + common arithmetic gotchas (1:37–1:50)

### Slide 24 — Q&A + gotchas

Before we wrap up I want to gather together the handful of mistakes that catch beginners most often in exactly this material, so you can sidestep them in your lab, and then I'll take whatever questions you have.

### Slide 25 — Common arithmetic gotchas

Let me walk through the five gotchas that come up again and again. First: five divided by two with a single slash is two-point-five, not two — remember the single slash always gives you a float, so if you were expecting a whole number, you wanted the double slash. Second, a subtle one: floor division always rounds toward negative infinity, meaning downward on the number line, so negative seven double-slash two is negative four, not negative three — it rounds down, away from zero, when the result is negative. That surprises people, so keep it in mind. Third: if you try to convert text that contains a decimal point straight to an integer — int of "three-point-five" — Python errors, because int can't parse a decimal string. The fix is to go to float first and then to int if you really need a whole number. Fourth: dividing by zero raises a ZeroDivisionError and stops your program, so if there's any chance the divisor could be zero, you check for it first — we'll learn the proper way to handle errors like this in Week 11, but for now, just be aware. And fifth, the one we just covered: never compare floats with double-equals; use math-dot-isclose. Keep these five in your back pocket and your calculator lab will go smoothly.

### Slide 26 — A first touch of math

And one last friendly thing before we close: a quick peek at the math module, which you've already seen me use for isclose. The math module is part of Python's standard library — it comes free with every install — and it's full of the mathematics you'd otherwise have to write yourself. You just say "import math" once, and then you have it all. Need a square root? math-dot-sqrt of one-four-four gives you twelve-point-zero. Need to round down or up to a whole number deliberately? math-dot-floor and math-dot-ceil do exactly that. Want the value of pi? It's right there as math-dot-pi. I'm only showing you a taste today, but the lesson is bigger than these few functions: whenever you find yourself about to write some fiddly mathematics by hand, pause and check whether the math module already has it, because very often it does. Alright — what questions do you have? Now is a great time, because you'll be writing your own calculator this week.

---

## Segment 7 · Wrap + Lab 2 brief (1:50–2:00)

### Slide 27 — Wrap-up / this week's homework

We're at the end, so let me hand you off to this week's lab and walk you through exactly what it asks of you.

### Slide 28 — Lab 2, a safe little calculator

Here's Lab 2, and it pulls together everything we did today into one small, satisfying program. Step one: create a new file called calculator-dot-py, and put it right beside your taskapp from last week. Step two: read two numbers from the user with input, and immediately cast them with float, because — say it with me — input always gives you text. Step three: compute and print their sum, their difference, their floor-division, their remainder using modulo, and a percentage. Step four, and this is the important one: round your results sensibly so there's no floating-point noise spilling out at the user — use round, just like in Demo 4. And step five: submit your script along with its output for the sample inputs given in the lab guide. The whole lab is really just you rebuilding Demo 3 for the operators and Demo 4 for the safe rounding, at your own pace. The full written guide spells out every step, and today's recording is right there to follow along with. Take your time and pause whenever you need to.

### Slide 29 — Next week

Looking ahead to next week: Week 3 is Strings and Text, where we take these numbers and turn them into clean, nicely formatted output using something called f-strings. And here's why today matters for next week — the casting and rounding you practise this week are exactly what you'll need to format real values into tidy reports. So everything connects. Get comfortable with numbers this week, and strings will feel like a natural next step.

### Slide 30 — See you in the lab

That's a wrap for Week 2. You've now got the value layer of Python under your belt — the number types, the operators, how variables really work, and how to keep floating-point from biting you. Go and build that calculator; it's a genuinely fun little program and it'll cement everything from today. I'll see you in the lab and back here next week. Any last questions before we close?
