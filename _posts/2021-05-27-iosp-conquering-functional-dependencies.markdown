---
layout: post
title:  "IOSP: Conquering functional dependencies"
date:   2021-05-27 18:00:00 +0100
categories: architecture
---

There are lots of well-known programming principles which you have probably already heard about… the SOLID principles, “Don’t Repeat Yourself” (DRY), “Keep it Simple, Stupid” (KISS), “You ain’t gonna need it” (YAGNI) and so on. In this post I’ll introduce you to a principle not commonly found in literature: the “Integration Operation Segregation Principle” (IOSP).

I’ve been introduced to this principle by clean code coach Ralf Westphal, who was the first to write about it in 2013. If you happen to speak German and want to explore the principle’s origins you should head over to [Software fraktal – Funktionale Abhängigkeit entschärfen](https://blog.ralfw.de/2013/04/software-fraktal-funktionale.html).

> Functions shall either only contain logic or they shall only call other functions. -- Ralf Westphal, [Integration Operation Segregation Principle](https://programming-with-ease.circle.so/c/articles/integration-operation-segregation-principle)

Let me put this somewhat more verbose: Functions shall either be an operation or an integration. An operation is a function containing only logic, where logic refers to control flow structures (e.g. if-else and while) and API calls to the standard or third-party libraries (e.g. `System.out.println`). An integration is a function containing only calls to other source code functions, i.e. operations and other integrations.

# Exploring IOSP

Let’s explore this principle with an example: A customer wants you to process a text file to display the contained words. The words shall be displayed in uppercase and alphabetical order. For example, the file content “C b a c b A” should be displayed as “A B C”. A problem of that scale may be implemented in a single function and still be readable, but for the example’s sake let’s impose a **three-layered architecture**; presentation, business and data access. For now, let’s forget about IOSP and see what happens!

```java
public class LayeredApp
{
    public static void main(String[] args)
    {
        UI ui = new UI(new Business(new DataAccess()));
        ui.displayWords(args[0]);
    }
    static class UI
    {
        private final Business business;
        public void displayWords(String fileName)
        {
            Set<String> words = business.extractUniqueWords(fileName);
            Set<String> sorted = new TreeSet<>(words);
            System.out.println(sorted);
        }
    }
    static class Business
    {
        private final DataAccess dataAccess;
        public Set<String> extractUniqueWords(String fileName)
        {
            String text = dataAccess.readText(fileName);
            String[] words = text.split(" ");
            Set<String> uppercaseWords = new HashSet<>();
            for (String w : words) {
                uppercaseWords.add(w.toUpperCase());
            }
            return uppercaseWords;
        }
    }
    static class DataAccess
    {
        public String readText(String fileName)
        {
            try {
                return Files.readString(Path.of(fileName));
            } catch (IOException e) {
                return "";
            }
        }
    }
}
```

Okay, nothing spectacular here. Some presentation details, processing logic and file access. Everything is nicely separated in modules/classes and dependencies run according to the three-layered architecture. Now, let’s get back to the IOSP and evaluate our functions.

Two functions already conform to the IOSP; `main`
is an integration, `readText`
is an operation. However, `displayWords`
and `extractUniqueWords`
both violate the principle. They contain mostly logic, but also call another source code function. These hybrid functions are shown again in the following snippet with the offending lines highlighted.

```java
public void displayWords(String fileName)
{
    Set<String> words = business.extractUniqueWords(fileName); // <--
    Set<String> sorted = new TreeSet<>(words);
    System.out.println(sorted);
}
public Set<String> extractUniqueWords(String fileName)
{
    String text = dataAccess.readText(fileName);  // <--
    String[] words = text.split(" ");
    // ... for-loop skipped for brevity
    return uppercaseWords;
}
```

Now that we’ve identified the offending functions, let’s **refactor staying true to the IOSP**. In the next snippet you will find that the main function has grown a little bit – it’s now the only integration. The hybrid functions turned into operations, which allowed the use of more specific method parameters.

```java
public class IOSPApp
{
    public static void main(String[] args)
    {
        UI ui = new UI();
        Business business = new Business();
        DataAccess dataAccess = new DataAccess();
        String text = dataAccess.readText(args[0]);
        Set<String> words = business.extractUniqueWords(text);
        ui.displayWords(words);
    }
    static class UI
    {
        public void displayWords(Set<String> words)
        {
            Set<String> sorted = new TreeSet<>(words);
            System.out.println(sorted);
        }
    }
    static class Business
    {
        public Set<String> extractUniqueWords(String text)
        {
            String[] words = text.split(" ");
            Set<String> uppercaseWords = new HashSet<>();
            for (String w : words) {
                uppercaseWords.add(w.toUpperCase());
            }
            return uppercaseWords;
        }
    }
    static class DataAccess
    {
        public String readText(String fileName)
        {
            try {
                return Files.readString(Path.of(fileName));
            } catch (IOException e) {
                return "";
            }
        }
    }
}
```

You’ll find the two snippets in my [github gists](https://gist.github.com/joshuaju/f1949f8087b72227cdf9421a34525943).

# Zooming out

So what has happened here? Consider the dependency tree of the first implementation. `main` depends on `displayWords`, which depends on `extractUniqueWords`, which depends on `readText`. The dependencies are nested, going deeper and deeper.

![tree-after-iosp](/assets/boc_iosp_layered_app.png)

Now compare that to the dependency tree of the refactored code. `main`
is the only function that depends on other functions. `displayWords`, `extractUniqueWords` and `readText` are operations, so by definition they are independent of other (source code) functions.

![tree-after-iosp](/assets/boc_iosp_iosp_app.png)

On the surface the two solutions may look alike. After all, method names and logic haven’t changed too much. However, looking at the dependency trees it is clear that something has changed indeed. Code that was previously nested and inter-dependent is now shallow and decoupled.

# Why should we care about IOSP?

Thanks to applying the IOSP, the refactored solution has a few favorable qualities compared to the initial solution.

**Testability**: It shouldn’t come as a surprise that decoupled code is easier to test compared to tightly coupled code. As seen in the dependency trees respecting the IOSP has decoupled presentation, business and data access layers from one another. Before the refactoring, testing the business logic required you to either (a) actually prepare text files to be read during a test, or (b) mock the data access layer. But now you just pass in the String text
, because the business logic doesn’t care about the text’s origin. It just knows how to extract those words.

**Readability**: The IOSP has pulled up the relevant aspects of the program: read the text, extract the words, display them – it’s all in the very first function. Gathering that information from the initial solution would require you to read through three functions and filter out details and by then you involuntarily have read most of the code already.
// excerpt from refactored main
var text = dataAccess.readText(args[0]);
var words = business.extractUniqueWords(text);
ui.displayWords(words);

**Flexibility**: The initial solution is geared towards reading from a file – just take a look at the method signatures (see excerpt below). The requirement that words shall be read from a text file has proliferated into every layer. The design decision has run along with the dependencies from top to bottom. What if we’d want to read text from a database? Which places would you need to update? The refactored solution is much more welcoming to such change, as just one method needs to be updated or swapped.

```java
// initial solution
public void UI.displayWords(String fileName) { ... }
public Set<String> Business.extractUniqueWords(String fileName) { ... }
public String DataAccess.readText(String fileName) { ... }
// refactored solution
public void UI.displayWords(Set<String> words) { ... }
public Set<String> Business.extractUniqueWords(String text) { ... }
public String DataAcess.readText(String fileName) { ... }
```

**Distributability**: Decoupled code is not just easier to test and reason about, it’s also easier to distribute work when implementing it in the first place. In a design respecting the IOSP the heavy lifting is done in the operations, which are dependency-free. Consequently, if you know your operations, you know how many developers could potentially work together. Add one additional developer who writes an integration test in the meantime.

# Closing

When I first learned about the IOSP I couldn’t foresee the effect it would have on me. After using it for more than a year I can say that it has fundamentally transformed the way I work! To the extent that it has become my daily companion; present throughout designing, implementing, refactoring and reviewing code. It’s usually the IOSP guiding my decisions and leading me to better solutions.

> If there is one principle in clean code development that’s my north star, then that’s the Integration Operation Segregation Principle, the IOSP. It guides my code design, it guides my code refactoring, it guides my code reviews. -- Ralf Westphal, [Integration Operation Segregation Principle](https://programming-with-ease.circle.so/c/articles/integration-operation-segregation-principle)

I’ve experienced that respecting the IOSP when designing code lead to higher collaboration. Features that would otherwise be implemented by 1-2 developers, could now be worked on by 4-5 without stepping on each other’s toes. This allowed features to be delivered quicker and to better distribute know-how between developers.

I’ve experienced that applying the IOSP motivated developers to write more and better tests. In a way, the test code mirrors the source code. So IOSP code encourages to write many, highly focused unit test (for operations) and few, more general integration tests (for integrations). These tests tend to require less setup and mocking code compared to tests of source code not following the IOSP.

I’ve experienced that coming back to IOSP code after several months is relatively painless. Locating the place to make a specific change is easy: Skimming through integrations quickly reveals where you should take a closer look. At this point, you’ll have to read through some operations, which tend to stay small and focused, therefore still easy to understand.

![iosp-tree](/assets/boc_iosp_tree-4.png)

Finally, I want to encourage you to try the IOSP yourself! It’s a fundamental principle that should be part of every programmer’s tool kit and it’s unfortunate, that the principle isn’t commonly known and applied. However, remember that following a principle should never defeat the principle’s purpose! There are situations where following the IOSP may not be practical, which is perfectly fine. But most of the time I’d say the results speak for themselves (pun intended).
