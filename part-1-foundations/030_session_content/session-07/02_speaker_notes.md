# Speaker Notes — Session 7 · Sets & Dictionaries

*Python Mastery — Part 1: Foundations · Week 7. Read aloud, top to bottom.*

---

## Segment 1 · Recap of Week 6 + today's goal (0:00–0:08)

### Slide 1 — Sets & Dictionaries

Hello everyone, and welcome back — this is Week 7 of Python Mastery, Part 1, Foundations. We're live on Microsoft Teams as always and the session is being recorded, so if anything today goes past you, you can come back to this recording afterwards and replay it as many times as you like while you do the lab. Today we add the two containers that, honestly, you will use for the rest of your Python life. Lists were last week's workhorse. Dictionaries are *the* workhorse — if I had to give up every container in Python except one, I'd keep the dictionary without hesitating. And alongside it we'll meet the set, which is a small, sharp tool that solves a specific problem beautifully. By the end of today your task app stops being a list you have to search through, and becomes a keyed model you can look things up in instantly. Let's get into it.

### Slide 2 — Recap & today

First, let me put today in context — where we are in the journey.

### Slide 3 — Last week, Lists & Tuples

Thirty seconds on where we were last week, because today is built right on top of it. Last week you learned lists — the container that changes. You appended to them, inserted into them, removed and popped from them, sorted them. You learned the aliasing trap, which is the one that bites everybody: when you write b equals a, you don't get a second list, you get a second *name* pointing at the same list, so changing one changes the other, and the fix is b equals a dot copy. And then you learned tuples — the little immutable records — and you modelled a task as a tuple of id, title, and done. You learned to unpack them, so tid, title, done equals task pulls all three out in one line, and you learned the lovely swap idiom, a, b equals b, a. So as of last week you can hold your tasks in order, in a list. Today we're going to hold them by key, and you'll feel immediately why that's better.

### Slide 4 — Today's goal

Here's the map for the next two hours. We start with sets — what they are, the algebra you can do with them, and the one job they're perfect for, which is uniqueness. Then I'll show you all of that running live. After that we move to dictionaries, which is the bigger half of today: how to build them, how to get things out of them safely, how to loop over them, and a few behaviours that surprise people. We'll talk about how you actually choose between a list, a set, and a dict, because that's a real decision you'll make constantly. Then we'll meet three tools from the collections module — Counter, defaultdict, and namedtuple — which are the kind of thing that, once you've seen them, you wonder how you lived without. And I'll demo all of that on our task app. Then Q&A, and I'll walk you through this week's lab. Let's start with sets.

---

## Segment 2 · Sets & set algebra (0:08–0:26)

### Slide 5 — Sets

Sets first. Uniqueness, membership, and a little bit of algebra.

### Slide 6 — What is a set?

A set is an unordered collection of unique, hashable values. Let me unpack that sentence, because every word in it is doing work. Look at the example: tags equals, curly braces, "work", "urgent", "email". That's a set of three tags.

First word: **unique**. A set physically cannot hold a duplicate. If you add something that's already in there, nothing happens — no error, no second copy, the set just stays as it is. That's not a limitation, that's the entire point of the container.

Second word: **unordered**. There's no first element. You cannot write tags square-bracket zero — that's an error. You can't slice it. The set has no notion of position at all.

Third word: **hashable** — and I'll come back to that properly when we do dictionaries, because it's the same rule for both. For now, read it as "simple, unchanging values" — strings, numbers, tuples. Yes.

And the payoff for giving up order and duplicates is **speed**. Asking "is work in tags" on a set is almost instant, no matter how big the set gets. So here's the way to hold it in your head: a set answers the question *"which things do I have?"* It does not answer *"what order are they in?"* If your question is the first one, a set is the right tool.

### Slide 7 — Making a set, and the curly-brace trap

Now let me show you how to make one, and warn you about a genuine trap on the way. The first line is easy: tags equals curly braces with some values in them — that's a set. Straightforward.

Second line: empty equals set, open paren, close paren. That is the *only* way to make an empty set. And now look at the third line, because this is the trap: oops equals curly braces with nothing inside them. You would think, quite reasonably, that empty curly braces make an empty set. They don't. Empty curly braces make an empty **dictionary**.

Why? History, purely. Dictionaries existed in Python before sets did, and dictionaries got the curly braces first. When sets arrived later they had to share the syntax, and dict already owned the empty case. So it's not logic, it's seniority. And the reason I make a fuss about it is that it fails *silently* — you don't get an error, you get a dict, and then three lines later something confusing happens. So burn this in: for an empty set, you must write set with parentheses.

One more nice thing on that slide: set of the string "hello" gives you the set h, e, l, o. Notice there's only one l, because sets don't do duplicates. Set doesn't wrap the string, it *iterates* it — it walks the string character by character and puts each character in.

### Slide 8 — Adding and removing

Three methods to know. The first is **add** — tags dot add "home" puts one thing in. And here's the nice property: add is idempotent, which is a fancy word for "calling it twice is the same as calling it once". If "home" is already in there, add does nothing at all. It doesn't complain, it doesn't duplicate. That means you never have to check before you add, which makes a lot of code shorter.

Then there are two ways to take something out, and the difference matters. **Discard** removes it if it's there and quietly does nothing if it isn't. **Remove** takes it out if it's there, and raises a KeyError if it isn't. Same job, different attitude. Discard says "make sure this is gone, I don't care whether it was there". Remove says "this should have been there — shout at me if it wasn't". Pick the one that says what you actually mean. Most of the time, in my experience, you want discard.

### Slide 9 — Set algebra

Now here's where sets earn their keep, and this is the bit people genuinely enjoy. Sets let you do algebra — real, mathematical set operations, the Venn-diagram ones you may have met at school — with single-character operators.

I've got two sets: mine, holding work, urgent, email; and yours, holding urgent, home, email. Look at the table. **Pipe** is union: everything in either set. **Ampersand** is intersection: only the things in both. **Minus** is difference: things in mine that are not in yours. And **caret** — the little hat character — is symmetric difference: things in exactly one of them, but not in both.

Now, the reason I love these is that each one answers a real question in one character. What tags do we use between us? That's union. What tags do we both use? Intersection. What do I use that you don't? Difference. What do we disagree on? Symmetric difference. Without sets, every one of those is a loop with an if inside it. With sets it's one line that reads like the question.

And if the symbols feel cryptic while you're learning, they all have word-spelled equivalents — dot union, dot intersection, dot difference. Use whichever you find clearer; nobody will judge you.

But there is one real difference between the two forms, and it's worth knowing. The **methods** will accept any iterable — you can pass a list, or even a string, to dot intersection and it'll work, because it converts for you. The **operators** insist on an actual set on both sides. So set of "abc" ampersand the string "cbs" is a TypeError, but set of "abc" dot intersection of "cbs" happily gives you b and c. That's deliberate on Python's part — the operators are strict so you don't accidentally do set algebra against a string you meant to be a set. So: symbols when both sides are genuinely sets, methods when you want the convenience of converting as you go.

### Slide 10 — The dedupe idiom

This is the single most common thing you'll ever do with a set, so I want to give it its own moment. Look at the list: work, urgent, work, email, urgent, work. Six items, with lots of repetition. Now watch — unique equals set of tags. That's it. That's the whole deduplication. You get back work, urgent, email. Six became three, and I didn't write a loop, I didn't check anything, I just changed the container. The duplicates had nowhere to live.

Now, there's a catch, and it's the thing I most want you to take from this slide. You just threw away the order. The set has no idea what came first. So if you want to *display* those tags — put them on screen for a human — you almost always want the last line: sorted of set of tags. That gives you back a proper list, in alphabetical order: email, urgent, work. And I want you to think of that as a pair, almost as one idiom: **set to remove duplicates, sorted to get an order back.** You'll write sorted-of-set an enormous number of times in your career.

### Slide 11 — Why sets are fast

Let me tell you why membership on a set is so quick, because the reason is genuinely interesting and it explains the rules we've been bumping into.

When you ask "is this in a list?", Python has no choice but to look. It checks the first item, then the second, then the third, all the way along until it finds it or runs out. If the thing you want is at the end of a hundred-thousand-item list, Python touches all hundred thousand.

A set doesn't search at all. When you put a value into a set, Python computes a **hash** of it — a number derived from the value itself — and uses that number to decide where to store it. So when you later ask "is work in here?", Python hashes "work" again, gets the same number, goes straight to that spot, and looks. One step. It doesn't matter whether the set holds ten things or ten million.

Look at the numbers on the slide, which I measured on this machine, on Python 3.14.6. Finding the worst-case item in a hundred-thousand-item list took about 924 microseconds. The same question against a set took about 30 nanoseconds. That's roughly thirty thousand times faster — and here's the important part: make the collection ten times bigger and the list gets ten times slower, while the set stays flat. I'll run that live for you in a moment because I don't expect you to take it on trust.

And *this* is why set elements have to be hashable. Python has to compute that number from the value. If the value could change after you stored it, the hash would go stale and Python would be looking in the wrong place forever. So the price of the speed is: only unchanging values allowed.

### Slide 12 — Sets are unordered, really

I said sets are unordered a few minutes ago, and I want to come back to it hard, because there's a version of this that catches people out and it's worth two minutes now to save you an hour later.

Look at the slide. I typed the same set literal — work, urgent, email — and Python displayed it in one order. Then I ran it *again*, same set, same three strings, and it came out in a different order. Same set. Different display. Nothing changed.

Now why? Modern Python deliberately randomises the hash of strings each time the interpreter starts. It's a security measure — it stops an attacker feeding your program values engineered to collide and slow it to a crawl. The consequence for you is that the *display order* of a set of strings is genuinely arbitrary, and can differ from run to run on the very same machine.

So here's the rule, and it's a firm one: **never rely on the order a set displays in.** Don't write a test that expects a particular order. Don't paste a set's output into your homework as though it's fixed. And if you need a dependable order — for output, for a report, for comparing two runs — wrap it in sorted. That's the fix, it's one word, and it's always available.

Now — you might be sitting there thinking "hang on, my dict output looks stable". It is, and that's a real difference between the two, and we'll get to exactly why when we do dictionaries in a bit. For now: sets don't promise you an order. Let me show you all of this actually running.

---

## Segment 3 · Demo 13 — set algebra & dedupe (0:26–0:48)

### Slide 13 — Watch this: Demo 13

Right — let me share my screen and let's stop talking about sets and use some.

### Slide 14 — Demo 13, what we'll do

Here's what I'm about to do in the REPL. I'll build two sets of tags — mine and yours — and run all four algebra operators on them so you can see the answers. Then I'll take that repetitive tag list and collapse it with set, and bring an order back with sorted. Then I'll actually time membership on a set against a list, live, so you can see those numbers are real and not something I made up on a slide. And at the end I'll try to put a list inside a set, so you can see exactly what Python says when you break the hashable rule — and it's worth seeing, because Python 3.14 gives you a noticeably better error message there than any earlier version did.

### Slide 15 — Set algebra, live

So here are my two sets, and let's ask our four questions.

Sorted of mine pipe yours — everything either of us uses — gives me email, home, urgent, work. All four tags, no duplicates, even though email and urgent were in both.

Sorted of mine ampersand yours — what we have in common — gives email and urgent. Just the overlap.

Sorted of mine minus yours — what I have that you don't — gives just work. Notice that's not symmetric: yours minus mine would give home instead. Order of operands matters for difference, exactly like subtraction.

And sorted of mine caret yours — what exactly one of us has — gives home and work. The things we disagree about. Notice email and urgent have vanished from that answer, because we both have them.

Now, you'll notice I'm wrapping every single one of these in sorted. That's deliberate, and it's the thing I told you two minutes ago. Without sorted, these would come out in an arbitrary order — and in fact, if you run this yourself right now, and then run it again, you may well see the raw sets come out differently each time. The contents will always be identical. The order is not something to lean on. So when I want you to *see* an answer, I sort it.

Let me now do the dedupe. Here's the tag list — six items, lots of repeats. Length of tags is six. Set of tags, and length of that is three. One function call did what would have taken a loop and a condition.

And now let me time it, because I want to prove the speed claim. I'm building a list of a hundred thousand numbers and a set of the same hundred thousand, and I'm asking each of them for the very last item — the worst possible case for the list, because it has to walk the whole thing. The list takes somewhere around 900 microseconds. The set takes about 30 nanoseconds. Look at the units — microseconds against nanoseconds. That's a factor of tens of thousands. And if I made both ten times bigger, the list number would grow by ten and the set number would barely move.

Last thing. Let me try to put a list inside a set and see what happens. There it is: TypeError — cannot use a list as a set element, unhashable type list. Now read that message, because it's lovely, and it's new. In Python 3.13 and everything before it, that error just said "unhashable type: list" and left you to work out where and why. Python 3.14 tells you exactly what you were doing — using it as a set element — and then gives you the technical reason. That's one of those small quality-of-life improvements in 3.14 that you'll appreciate at eleven at night when something's broken. And the fix, when you hit it, is a tuple: tuples are immutable, so they're hashable, so they can live in a set.

### Slide 16 — What you just saw

So, to draw a line under sets. Four operators answered four genuinely different questions about two collections, each in one line. Set of a list deduplicated it in a single call — you traded order away and got uniqueness back. Membership on a set was thousands of times faster than on a list, and that gap widens as things grow. And sets only hold hashable things, which is the price you pay for that speed — no lists inside sets.

That's sets. They're a small tool with one clear job. Now let's meet the big one.

---

## Segment 4 · Dictionaries — methods, iteration, choosing a container (0:48–1:10)

### Slide 17 — Dictionaries

Here it is — the dictionary. Keyed data, and the workhorse of the language.

### Slide 18 — What is a dict?

The dictionary. This is the most important container in Python, and I want to explain it with the thing you already have — last week's task list.

Last week your tasks lived in a list of tuples. Now suppose I ask you: mark task number seven as done. What does your code have to do? It has to walk the list, checking each task's id, until it finds seven. You're *searching*. And that's silly, because you already know exactly which one you want — you said its name.

A dictionary fixes that. It maps a **key** to a **value**. Look at the example: tasks is a dict where key one maps to a little dict with a title, a done flag, and some tags, and key two maps to another one. And now look at that last line — tasks square-bracket one, square-bracket title, gives you "Buy milk" immediately. No walking, no searching, no loop. You asked for task one and Python went straight there — for exactly the same hashing reason that made sets fast, because a dict is built on the same machinery.

Two rules. The **keys** are unique and hashable — same rule as set elements, same reason. The **values** are anything you like at all, with no restrictions. And notice what I've done here: each value is *itself* a dict. That's the dict-of-dicts shape, and it's extremely common, because it means each task can carry as many fields as it needs. That's the model your task app is going to use from today all the way to the capstone.

### Slide 19 — Access, and missing keys

Now, getting things out — and this is a genuine fork in the road, so let's be careful.

The first line, tasks square-bracket one, gives you the task. Fine. The second line, tasks square-bracket ninety-nine, where there is no ninety-nine — that's a KeyError, and your program **stops**. It crashes. It doesn't give you None, it doesn't give you empty, it raises.

The third line is the alternative: tasks dot get ninety-nine. That gives you back None. No crash. And the fourth line, get with a second argument, gives you back whatever default you chose — here the string "none" — instead of None. And the last line, ninety-nine in tasks, just asks the question and gives you True or False without fetching anything.

Here's how I want you to think about the choice, because it's not "get is the safe one, always use get". It's about what you're claiming. Square brackets say *"this must be there"* — and if it isn't, that's a bug, and crashing loudly is exactly the right behaviour, because it tells you immediately. Dot get says *"this might be there, and that's fine"*. Both are correct in the right place. If you use get everywhere out of fear, you'll turn real bugs into silent Nones that surface somewhere else entirely, three functions away, and you'll have a much worse afternoon. So say what you mean.

### Slide 20 — The everyday methods

Quickly through the ones you'll use constantly. Assigning to a key — tasks bracket five equals something — adds it if it's new, and overwrites it without a word if it isn't. There's no separate "add" method; assignment is it.

Setdefault is the odd-sounding one. It means "give me the value at this key, and if there's nothing there, put this in first and then give me that". So it's get-or-insert, in one step. It's handy, though in a minute I'll show you defaultdict, which is usually nicer for the grouping job people reach for setdefault to do.

Update merges another dict in — the other dict's keys win where they collide. Pop removes a key and hands you back its value, raising KeyError if it's not there — and just like the set, giving pop a second argument makes it safe and gives you that default instead. And len of a dict tells you how many keys are in it — not how deep it goes, just the top-level count.

### Slide 21 — Iterating a dict

Looping over a dict — and there's a surprise in the very first line, so watch for it.

If you write "for tid in tasks", what do you get? You get the **keys**. Not the values, not pairs — the keys. That catches nearly everyone once, because you look at that line and think you're iterating "the dict", and it feels like you should get the contents. What you get are the keys. Python's reasoning is that the keys *are* what the dict is indexed by, and it's consistent with "ninety-nine in tasks" checking keys as well. But it's a surprise the first time.

If you want the values, say so: for title in tasks dot values.

And then the third one — for tid, task in tasks dot items — is the one you'll use most, so let me slow down. Items gives you both at once: each turn of the loop hands you a little tuple of key and value, and we unpack it straight into two names in the for line. That's last week's tuple unpacking, showing up again exactly where it's useful. You get the id and the task together, no lookups inside the loop. When you're reading real Python, dot items in a for line is one of the most common things you will ever see.

### Slide 22 — Views are live

This is a subtle one that I'd rather you meet from me now than from a bug later.

Look at the screen. I call tasks dot keys and put it in a variable called ks. It shows me dict_keys of one, two, three. Then I add a fourth task to the dict — and I don't touch ks at all. And then I look at ks again, and it says one, two, three, **four**.

I never reassigned it. That's because keys, values, and items don't hand you a *copy* — they hand you a **view**. A view is a live window onto the dict. It's not a photograph, it's a pane of glass. Look through it later and you see whatever's there now.

Most of the time this is exactly what you want, and it's efficient, because Python isn't copying your whole dict every time you loop over it. But there are two things to know. First, if you genuinely want a frozen snapshot — a list that won't change under you — wrap it: list of tasks dot keys. Now you have your own copy and it's yours.

And second — this is the one that actually bites — **don't add or remove keys while you're looping over a dict.** You'll get a RuntimeError telling you the dictionary changed size during iteration. And now you know why: you were looking through a live window while somebody rearranged the room. The fix is that same one: loop over list-of-tasks-dot-keys, so you're walking your own copy, and then it's perfectly safe to modify the real dict inside the loop.

### Slide 23 — Keys must be hashable

Here's that hashable rule again, now for dict keys, and it's exactly the same rule for exactly the same reason.

Look at the first line: I try to use a list as a dict key, and I get TypeError — cannot use a list as a dict key, unhashable type, list. And look at the second: I use a *tuple* of the same two strings as a key, and it works perfectly.

So what's the difference? A list can change. A tuple cannot. Python needs to hash the key to know where to file the value, and if the key could change afterwards, that filing would be wrong and the value would be lost inside its own dict. So Python refuses at the door.

The hashable things are the ones that can't change: strings, integers, floats, booleans, tuples, frozensets. The unhashable ones are the ones that can: lists, dicts, and sets. And the practical takeaway is a nice one — if you ever want a compound key, something like a coordinate pair or a name-and-date combination, reach for a **tuple**. Tuples as dict keys are a genuinely useful trick, and they work precisely because tuples are frozen.

One more time on that error message: notice it says "as a dict key" on this slide and it said "as a set element" on the set slide. Python 3.14 tailors the message to what you were actually doing. Earlier versions said "unhashable type: list" in both cases and let you figure it out. Small thing, real improvement.

### Slide 24 — Dicts keep insertion order

Now let me settle the question I left hanging when we did sets.

Look at the example. I make a dict with "b" first and "a" second, and I list its keys, and I get b then a. Not sorted — b comes before a. It came out in the order I **put things in**. And that is guaranteed. Not "usually works", not "happens to be true on my machine" — it is a promise of the language, since Python 3.7. You can rely on it, permanently, in every conforming Python.

So we have this slightly odd-looking pair of facts, and I want to make them stick: **dicts promise you insertion order. Sets promise you nothing.** Both are built on hashing, so it's reasonable to ask why they differ, and the answer is just implementation history — the dict got rebuilt around 2016 in a way that made it both faster and ordered, essentially for free, and the language then promised the ordering. The set never got that treatment. So don't try to reason it out from first principles; just remember the two facts. Dict: ordered by insertion, guaranteed. Set: no order, don't look.

And if you want your dict alphabetical rather than in insertion order, it's the same tool as always — sorted of the dict gives you sorted keys, and sorted of dict dot items gives you sorted pairs.

### Slide 25 — A preview: comprehensions

Very quickly, and I do mean quickly, because this is next week's headline and I only want you to recognise the shape when you see it in my demo.

Look at the first line: curly braces, tid colon t-of-title, for tid, t in tasks dot items. That builds a whole new dictionary — mapping each id to just its title — in one expression. That's a dict comprehension. And the second line, with no colon in it, builds a set of every tag across every task, in one expression. That's a set comprehension.

That's all I want today: when one of these scrolls past in the demo, I want you to think "that's building a container in one line" rather than "what on earth is that". Next week we pull them apart properly and you'll write them yourself.

### Slide 26 — Choosing a container

So now you know three containers, and the honest question is: which one do I use? Because that's a decision you'll make every single time you store anything.

Here's the diagram, and the key insight is the question at the top. Don't ask what the data looks like. Ask what you're going to **do** with it.

If you need to keep things in order, and duplicates are meaningful — three separate log entries that happen to say the same thing are three real events — that's a **list**.

If you only care whether something is present, and duplicates are noise you want gone, and you'll be asking "is this in here?" a lot — that's a **set**.

If you have a natural identifier and you want to fetch things by it — a task by its id, a user by their email, a count by its name — that's a **dict**.

And here's the tell, the thing I really want you to leave with. If you ever catch yourself writing a for loop over a list whose only purpose is to find the one item with a matching id — **stop**. That loop is the container telling you it's the wrong container. That's a dict wearing a list's clothes. That is precisely the loop we're deleting from your task app in this week's lab.

### Slide 27 — Counter

Now three tools from the collections module, which is part of the standard library — nothing to install, it ships with Python. These are specialised dicts, built for jobs so common that somebody wrote them once so you never have to again.

The first is **Counter**, and it does exactly what it says. You hand it any collection of things and it tells you how many of each. Look: Counter of that list of tags gives me Counter of urgent two, work one, home one. It counted them. That's it. That's the whole thing — one line, no loop, no if.

Two more bits worth having. Most_common of two gives you the top two as a ranked list of pairs, highest first — instant leaderboard. And then look at the last one, because it's my favourite: counts of "never-seen", a key that was never in the data at all, gives you **zero**. Not a KeyError. And that's exactly right, isn't it? The number of times you saw something you never saw is zero. A Counter is a dict, so you'd normally expect a KeyError there — this one is deliberately friendlier, because for counting, zero is the sensible answer.

And notice the second line on that slide, because it's a distinction I want you to file away for about three minutes' time: when you read a missing key from a Counter, it answers zero and it does **not** add that key to the counter. It just answers the question and leaves. Hold that thought — the next tool behaves differently, and the contrast is the point.

### Slide 28 — defaultdict

The second tool is **defaultdict**, and it solves the grouping problem. Let me set the problem up, because otherwise the solution looks like magic.

Suppose I want, for each tag, a list of the task ids that carry it. With a plain dict I have to write: is this tag already a key? If not, create it with an empty list first. Then append. Every single time. That "is it there yet, if not create it" dance shows up in front of every append, and it's pure noise — it has nothing to do with what I'm trying to do.

Defaultdict deletes the dance. Look at the code: by_tag equals defaultdict of list. Then I just loop and append. No checking. When I touch a key that doesn't exist, the defaultdict calls that factory — the word "list" I handed it — to make a fresh empty list, puts it there, and *then* does my append. The key appears exactly when I need it.

Note carefully what I passed: the word list, with **no parentheses**. I'm handing over the function itself so the defaultdict can call it later, each time. If you write list with parentheses you'd be calling it now and passing in one single list, which is not what you want at all.

And now the warning on that slide, because this one is genuinely surprising: **reading a missing key creates it**. If you just look at by_tag of "nonsense" — you don't assign, you only look — you get an empty list back *and* "nonsense" is now permanently a key in your dict. That's the trade you made: you asked for keys to spring into existence on touch, and touching includes reading.

And *there's* the contrast I asked you to hold on to from the Counter slide. Read a missing key from a Counter and it says zero and adds nothing. Read a missing key from a defaultdict and it hands you an empty list and quietly keeps it. Two dict subclasses from the same module, opposite behaviour on the identical action — so it's genuinely worth knowing which one you're holding.

So if you only want to *check* whether a tag is there, use the in operator — "nonsense in by_tag" — because that asks without touching, and it won't create anything. And there's one more that surprises people: dot get does **not** use the factory. By_tag dot get of a missing tag gives you None, not an empty list, and inserts nothing. That's not a bug — the factory only fires for square brackets. So if you want the empty list, use brackets; if you want to peek without consequences, use get or in.

### Slide 29 — namedtuple

The third tool, briefly. Remember last week's task tuple — id, title, done? It worked, but the code that read it was miserable: task bracket one meant the title, and you had to just *know* that. Six months later, you don't.

Namedtuple fixes it. You declare the shape once — Task equals namedtuple with fields id, title, done — and now you build them like normal, but you read them by name. T dot title gives you "Buy milk". And when you print one, it tells you what it is: Task, id equals one, title equals Buy milk, done equals False. Compare that to the bare tuple, which just showed you three anonymous values and left you guessing.

And the lovely part is that it's still a tuple underneath. It's immutable, it unpacks, you can still index it if you want — you've lost nothing and gained readability. So think of it as a tuple that documents itself. It's a stepping stone, really: in Week 14 we get proper classes, and you'll see that namedtuple was the halfway house all along.

Right. Let me put all of this together on the actual task app.

---

## Segment 5 · Demo 14 — dict-of-tasks + `Counter` / `defaultdict` (1:10–1:38)

### Slide 30 — Watch this: Demo 14

Screen coming up — and this is the demo that changes your task app for good, so this is the one to rewatch when you sit down to the lab.

### Slide 31 — Demo 14, what we'll do

Here's the plan. I'm going to build the tasks dict keyed by id — the model that stays with us from today to the capstone. I'll show you get, and items, and adding and removing keys. Then I'll pull every tag out and rank them with Counter. Then I'll group task ids by tag with a defaultdict, so you see that no-key-checking magic in action. And I'll finish with a task as a namedtuple so you've seen the shape.

### Slide 32 — Tallies and groups, live

So here's the dict, keyed by id, each value a little dict with a title, a done flag, and a list of tags.

Watch this — tasks bracket one, bracket title. "Buy milk". Straight there. Compare that to last week, where I'd have looped through a list checking ids. That loop is gone. It's not shorter, it's *absent*.

Now let me loop it properly with items — id and task together, one line each, nicely formatted.

Now, the tags. Every task has a list of them, and I want them all in one flat list. There's the flattening — every tag from every task — and you can see the repetition: urgent appears again and again, home twice. Now: Counter of that. And there's my answer — urgent three, home two, shopping one, work one. Ranked, highest first, in one line.

And notice the Counter came out ranked without me sorting it, which — after everything I said about sets not having an order — deserves a word. A Counter is a **dict**, not a set. Dicts keep insertion order, so a Counter's output is stable and reproducible, and its display is helpfully ordered by count. Most_common of two gives me the top two: urgent with three, home with two. That's your tag leaderboard, and that's a line of your lab, right there.

Now the grouping. By_tag equals defaultdict of list. And now watch how little code this is — for each task, for each of its tags, append the id to by_tag of that tag. That's it. No checking whether the tag exists. No creating empty lists. And there's the result: shopping maps to task one, urgent maps to one, two and four, work to two, home to three and four. Every tag now knows its tasks.

Let me show you the trap I mentioned, live, because it's better seen than described. Watch the keys — that's my list of tags. Now I'm going to *read* a key that doesn't exist: by_tag of "does-not-exist". I get an empty list back, as promised. But now look at the keys again — "does-not-exist" is in my dictionary. I didn't assign to it. I only looked at it. So when you want to check, use in, which asks without touching. When you want to add, use square brackets, which creates.

Last thing — the namedtuple. Task equals namedtuple with id, title, done. Build one, and there it is: t dot title reads "Buy milk", and printing it shows every field with its name attached. Nice and self-describing.

### Slide 33 — What you just saw

Let me pull that together. You saw a dict-of-dicts keyed by id, where fetching a task took no searching whatsoever. You saw items give you the key and the value in one clean loop. You saw Counter rank every tag in one line, with a missing key answering zero instead of raising. And you saw defaultdict group ids by tag with no key-checking at all — and you saw its one sharp edge, that reading a missing key creates it.

And I want to say this clearly: this keyed model is not a demo. This is your task app now, from today through to the capstone in Week 16. In Week 12 this exact shape is what we write out to a JSON file, and you'll see that a dict-of-dicts and a JSON object are practically the same idea — which is a large part of why this model is worth adopting today.

---

## Segment 6 · Q&A + container-choice discussion (1:38–1:50)

### Slide 34 — Wrap-up, Q&A

Right — let's stop and take your questions, because this is a session where the questions are usually better than anything else I'd fill the time with.

Let me start us off with the one that comes up every single time I teach this, which is: "if dicts are so fast, and they keep their order, why would I ever use a list?" It's a good question. Here's the answer. A list keeps duplicates, and sometimes duplicates are the data — three log lines saying the same thing are three real events, and a set would silently destroy two of them. A list has real positions, so "the third thing" is a meaningful idea, and in a dict it isn't. And a list is ordered by *what you did*, in sequence, which is exactly right for a queue of work or a history. So no — dicts don't replace lists. They answer a different question. The question a dict answers is "give me the one with this id", and the question a list answers is "what happened, in order".

Here's a second one that's worth pre-empting: "should I use a set or a dict for tags?" And the answer is really nice, because it's both, at different levels. On a *single task*, the tags are a small set — you want each tag once, and order doesn't matter. But across the *whole app*, when you're asking "which tasks have the urgent tag", that's a lookup by key, and that's a dict — which is exactly the defaultdict grouping we just built. Same data, two levels, two containers, each doing what it's good at.

And one more you might be chewing on: "is it a problem that my sets print in a different order to yours?" No — and it's important that it isn't. Your set has exactly the same contents as mine. The display order is arbitrary, it's random per run by design, and it means nothing. If it *looks* like it matters in your output, that's your signal to reach for sorted. So if your lab output has tags in a different order to mine, don't debug it. Sort it.

So — what have you got for me? What's confusing, what looks wrong, and what have you already tried that didn't do what you expected?

---

## Segment 7 · Wrap + Lab 7 brief (1:50–2:00)

### Slide 35 — Lab 7, the keyed task model

Let me walk you through this week's lab, because it's a satisfying one — you're going to take something you already built and make it genuinely better.

Step one: re-model. Take Lab 6's list of task tuples and turn it into a dict keyed by id, where each value is a dict with a title, a done flag, and a list of tags. That's the shape I demoed.

Step two: keep the same operations working — add, remove, mark-done, and list-by-title. And here's the point of the whole exercise: I want you to *notice* what happens to your code as you do this. The functions that had to loop through the list hunting for an id don't need to loop any more. They just reach in. Watch those loops disappear. That's the lesson, and it's better felt than told.

Step three: use a set to dedupe your tags, the way I did in Demo 13, so a task can't end up carrying "urgent" twice.

Step four: add the two summaries. A tag-frequency report with Counter, and a group-by-tag with defaultdict — exactly the two things I built in Demo 14.

Then submit tasks.py and your sample output to the course channel on Teams, as always.

Two bits of advice before you go. First: when you display anything that came out of a set, sort it. If you don't, your output will shuffle between runs and you'll think you've broken something when you haven't. Second: mirror the demos. Demo 13 is your sets, Demo 14 is your dict model and your collections — they're both in today's recording, and the lab guide tells you exactly where to find each one.

### Slide 36 — Next week

Next week is Week 8, Comprehensions and Looping Idioms, and it pairs beautifully with today. All those loops you're about to write in the lab — the one that flattens tags, the one that builds the group — collapse into single, elegant lines. We'll also meet enumerate for numbering your task list, zip for walking two collections together, and sorted with a key function, which is what finally lets you sort your tasks by any field you like. Today you got the containers. Next week you get the beautiful way to move through them.

### Slide 37 — See you in the lab

That's Week 7. You've got the two containers that carry the rest of this course — the set for uniqueness, and the dict for everything else. Your task app has a real data model now. Go and do the lab while today is still fresh; the recording's there if you need it. Thanks, everybody — brilliant questions today — and I'll see you next week for comprehensions.
