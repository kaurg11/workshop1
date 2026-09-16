# LLM Code Evaluation Report — Assignment 0: Setup

## Student Information
- **Name**: Gurpreet Kaur
- **Date**: September 16, 2026
- **LLM Used**: Claude (Opus 5, via Claude Code in VS Code)

## Prompt Used

The prompt from `LLM-Evaluation-prompt.md`, with my actual `Greeting.java` and terminal output
pasted in place of the markers:

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

## What the LLM said

Because I ran the prompt through Claude Code inside the project, it was able to read the actual
files and build reports rather than reasoning only from the pasted text. Its answers:

**1. Does `message()` include my name?**

Yes. `Greeting.java:15` holds `"Gurpreet Kaur"` — the `"YOUR NAME HERE"` placeholder is gone. The
exact returned string is:

```text
Hello, Java! — Gurpreet Kaur
```

The character after `Java!` is an em dash (U+2014). Claude checked the file's raw bytes and found
`e2 80 94`, confirming the source file really is UTF-8 and the dash is intact.

**2. Will the tests pass?**

Both pass — and they already did:

- `messageNamesTheStudent` — the message is not blank and does not contain `"YOUR NAME HERE"`. ✅
- `messageHasGreetingPrefix` — the string still starts with `"Hello, Java!"`, because only the name
  constant changed, not the prefix. ✅

Rather than just reasoning about it, Claude confirmed this in the recorded results:
`build/test-results/test/TEST-edu.northeastern.setup.GreetingTest.xml` reports
`tests="2" skipped="0" failures="0" errors="0"`, with both test names listed.

**3. What `STUDENT_NAME` and `message()` do**

`STUDENT_NAME` is a private constant holding my name in one place, so the name lives in exactly one
spot. `message()` is a static method that builds and returns the greeting by gluing the fixed prefix
`"Hello, Java! — "` onto that constant — nothing else in the class needs to know my name.

The class is `final` with a private constructor because it is a utility holder: you call
`Greeting.message()` directly and never create a `Greeting` object.

**4. Was the run clean?**

Yes, clean. All four artifacts exist on disk:

| Artifact | Path |
|---|---|
| Test report | `build/reports/tests/test/index.html` |
| Coverage (HTML) | `build/reports/jacoco/test/html/index.html` |
| Coverage (XML) | `build/reports/jacoco/test/jacocoTestReport.xml` |
| Checkstyle | `build/reports/checkstyle/main.html` + `main.xml` |

Two things worth understanding, neither of which is a problem:

- **"5 up-to-date" is fine.** The second command re-ran nothing because Gradle saw that no inputs
  had changed since the earlier run. Up-to-date means "already built successfully," not "skipped."
  The reports are timestamped and real.
- **The `∩┐╜` in `./gradlew run` is a console display artifact, not a bug.** The em dash was garbled
  on its way through Gradle's output capture into the PowerShell window, because the terminal is
  using an OEM code page that cannot render U+2014. The string inside the JVM is correct, which is
  why the prefix test passes. Running `chcp 65001` first (or using Windows Terminal) makes it render
  properly. The code should not be changed for it.

**Nothing to re-check.** The JDK is correct — `build.gradle:12-16` pins a Java 21 toolchain, and a
mismatched JDK would have failed the build outright instead of succeeding. `checkstyleMain` produced
zero violations; Claude verified this by reading `main.xml` directly, because `build.gradle:57` sets
`ignoreFailures = true`, which means a passing build alone would not prove the style check was clean.

## Your checklist
- **`./gradlew run` prints the greeting with my name?**: Yes
- **`./gradlew test` is green?**: Yes (2 tests, 0 failures)
- **Coverage + checkstyle reports generated?**: Yes
- **I can explain what `Greeting.message()` returns and why the test passes?**: Yes

## Reflection

The only thing that tripped me up was seeing `Hello, Java! ∩┐╜ Gurpreet Kaur` in the terminal — I
assumed my edit had corrupted the file, when in fact the source is valid UTF-8 and only the
PowerShell console's code page could not render the em dash. Checking the JUnit XML and the raw file
bytes instead of trusting the console output is the habit I want to carry into the graded weeks; I
also learned that Gradle's "up-to-date" means a previous run already succeeded, and that a green
`checkstyleMain` does not by itself mean zero violations when `ignoreFailures = true` is set.
