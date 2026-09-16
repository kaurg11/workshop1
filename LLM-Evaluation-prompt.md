# LLM Evaluation Prompt — Assignment 0

**How to use.** Open your LLM (Claude, ChatGPT, Copilot Chat, …), paste **everything inside the box
below**, and replace each `<PASTE …>` marker with your actual files/output. Then copy the LLM's
answer into `LLM-Evaluation.md` (structure it with
[LLM-Evaluation-Template.md](LLM-Evaluation-Template.md)) and add your reflection.

> This week the code is trivial — the point is to **practice using an LLM as a reviewer** and to
> confirm your setup works. The LLM should *check* your work, not do it for you.

---

```text
You are a friendly but precise teaching assistant for a graduate Java course. This is a SETUP
assignment (Assignment 0) — the only code change is that the student put their name into a Greeting
class so a test passes. Help me confirm my environment and workflow are correct. Do not rewrite my
code; just verify and explain.

Check the following and answer each briefly:
1. In my Greeting class, does message() return a greeting that includes MY name (not the placeholder
   "YOUR NAME HERE")? Quote the exact string it would return.
2. Given that, will the JUnit test messageNamesTheStudent PASS (it asserts the message is not blank
   and no longer contains "YOUR NAME HERE")? Will messageHasGreetingPrefix still pass?
3. In one or two plain-English sentences, explain what STUDENT_NAME and message() do, so I can say it
   in my own words during my code walk.
4. Did my tool output below indicate a clean run (tests green, coverage + checkstyle reports
   generated)? If something looks wrong, tell me what to re-check (JDK version, ./gradlew, etc.).

Here is my Greeting.java:
package edu.northeastern.setup;

/**
 * The one thing you edit this week: put your name in {@link #STUDENT_NAME} so the
 * app greets you by name. That is the whole assignment — the point is to get the
 * build, tests, LLM evaluation, and submission workflow working before Assignment 01.
 *
 * <p>This package is deliberately its own small world. It belongs to neither of the
 * two codebases you will actually build this term, and nothing here carries forward:
 * after this week you can forget it entirely. What carries forward is the layout it
 * sits in, and the commands you ran against it.
 */
public final class Greeting {

  private static final String STUDENT_NAME = "Gurpreet Kaur";

  private Greeting() {
    // utility class: no instances
  }

  /**
   * @return a greeting naming the student, e.g. {@code "Hello, Java! — Ada Lovelace"}.
   */
  public static String message() {
    return "Hello, Java! — " + STUDENT_NAME;
  }
}

Here is the output of my commands (paste what you saw):
PS C:\Users\gifty\OneDrive\Desktop\VS\workshop1> ./gradlew run
> Task :run
Hello, Java! ∩┐╜ Gurpreet Kaur

BUILD SUCCESSFUL in 2s
2 actionable tasks: 1 executed, 1 up-to-date
PS C:\Users\gifty\OneDrive\Desktop\VS\workshop1> ./gradlew test jacocoTestReport checkstyleMain

BUILD SUCCESSFUL in 1s
5 actionable tasks: 5 up-to-date
```
