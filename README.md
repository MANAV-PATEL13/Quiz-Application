# Quiz Application (Java Swing)

A desktop quiz application built with Java Swing. It presents a 10-question multiple-choice quiz on Java topics, with a 15-second timer per question, a 50/50-style lifeline, and a final score screen.

## Tech Stack

- **Language:** Java
- **UI:** Java Swing (AWT/Swing components, JFrame)
- **Build/IDE:** IntelliJ IDEA module (`QuizApp.iml`), standard `javac`/`java`
- **Resources:** PNG icons under `src/icons/` loaded via the classpath

## Project Structure

```
Quiz application/
└── QuizApp/
    ├── src/
    │   ├── quiz/app/          # Java source package
    │   │   ├── Login.java     # Entry screen: enter name, start/exit
    │   │   ├── Rules.java     # Rules screen before quiz
    │   │   ├── Quiz.java      # Main quiz screen (questions, timer, lifeline, scoring)
    │   │   └── Score.java     # Final score screen
    │   └── icons/             # UI images (login.png, quiz.png, back.png, score.png)
    ├── out/                   # Compiled classes (quiz/app/*.class)
    └── QuizApp.iml            # IntelliJ module descriptor
```

## Component Toolkit (Swing vs AWT)

| Component | Package | Type | Used In |
|---|---|---|---|
| `JFrame` | `javax.swing` | Swing | All screens (top-level window) |
| `JLabel` | `javax.swing` | Swing | Headings, questions, images |
| `JTextField` | `javax.swing` | Swing | Name input on Login |
| `JButton` | `javax.swing` | Swing | Next, Back, Start, Submit, Help (50/50) |
| `JRadioButton` | `javax.swing` | Swing | Answer options (opt1–opt4) |
| `ButtonGroup` | `javax.swing` | Swing | Groups radio buttons so only one option is selectable |
| `ImageIcon` | `javax.swing` | Swing | Loads PNG icons |
| `Graphics` / `g.drawString` | `java.awt` | AWT | Draws the countdown timer text on the quiz screen |
| `Font` | `java.awt` | AWT | Sets fonts for all components |
| `Color` | `java.awt` | AWT | Background/foreground colors |
| `Image` | `java.awt` | AWT | Scales icons before display |
| `ActionListener` / `ActionEvent` | `java.awt.event` | AWT (event) | Handles button clicks |
| `Thread.sleep` + `repaint()` | `java.lang` / AWT | AWT painting | Drives the 15-second countdown |

> In short: **Swing** provides the widgets (`J*`), while **AWT** provides the underlying graphics, fonts, colors, images, and the event model.

## Screen Flow

```
Login (enter name) --> Rules (show rules) --> Quiz (10 questions, timer, lifeline) --> Score (final score) --> back to Login
   |                                                                                              ^
   +-- Back exits app                                                                             +-- EXIT
```

## How It Works (Step by Step)

1. **Login.java** — Creates an undecorated `JFrame`. Reads the name from a `JTextField`. "Next" opens `Rules(name)`; "Back" calls `System.exit(50)`.
2. **Rules.java** — Receives the player name, shows rules in a `JLabel` with HTML. "Start" opens `Quiz(name)`; "Back" returns to `Login`.
3. **Quiz.java** — Holds question/answer arrays. `start(count)` fills labels and radio buttons. A countdown is driven by overriding `paint()` → `Thread.sleep(1000)` → `repaint()`. Selecting an option stores it in `useranswers`. "Next" advances; "Help" (50/50) disables two wrong options; "Submit" (enabled on last question) scores and opens `Score`.
4. **Score.java** — Receives name and score, displays them, and "EXIT" returns to `Login`.

Entry point: `quiz.app.Login` (`main`).

## OOP Concepts Used

- **Encapsulation**: Each screen is a class (`Login`, `Rules`, `Quiz`, `Score`) bundling its own state and behavior; fields like `text`, `Next`, `back` are instance fields.
- **Inheritance**: Every screen `extends JFrame` and inherits window behavior; event handling via `implements ActionListener`.
- **Polymorphism**: `actionPerformed(ActionEvent e)` overrides the interface method; branching on `e.getSource()` selects behavior.
- **Abstraction**: UI details hidden behind constructors; callers just do `new Quiz(name)`.
- **Composition**: `Quiz` composes `JLabel`, `JRadioButton`, `ButtonGroup`, `JButton` objects; `Login` composes an `ImageIcon`-backed `JLabel`.
- **Static members**: `timer`, `count`, `score`, `ans_given` are `static`, shared across instances/repaints (class-level state).

### Examples in this project

- **Encapsulation (button)**: In `Login`, `JButton Next, back;` are fields; their text, colors, and listeners are set inside the constructor, hiding setup details from other classes.
- **Inheritance (screen)**: `public class Quiz extends JFrame implements ActionListener` — `Quiz` IS-A `JFrame` and IS-A `ActionListener`.
- **Polymorphism (button click)**: `public void actionPerformed(ActionEvent e)` handles every button; `if (e.getSource() == next) ... else if (lifeline) ... else if (submit)` picks the right action.
- **Abstraction (screen creation)**: `new Rules(name)` / `new Quiz(name)` / `new Score(name, score)` — callers don't care how the UI is built.
- **Composition (options)**: `Quiz` has `JRadioButton opt1..opt4` and a `ButtonGroup groupoptions`; options are composed into the quiz screen and grouped for single selection.
- **Static (timer/score)**: `public static int timer = 15;` and `public static int score = 0;` keep values alive while `paint()` repaints and across questions.
## Key Concepts to Learn (Java + Swing)

- **JFrame & layout**: `setLayout(null)` + `setBounds(...)` = absolute positioning.
- **Event handling**: implement `ActionListener`, register with `addActionListener(this)`, branch on `e.getSource()`.
- **Radio buttons**: `JRadioButton` + `ButtonGroup` for single selection; `getActionCommand()` reads the chosen value.
- **Images on classpath**: `ClassLoader.getSystemResource("icons/...")` — requires `src`/`out` on classpath.
- **Custom painting + timer**: override `paint(Graphics g)`, draw with `g.drawString`, sleep 1s, call `repaint()`.
- **Passing data between screens**: constructor parameters (`name`, `score`); `setVisible(false)` then `new NextScreen(...)`.
- **Static state**: `timer`, `count`, `score`, `ans_given` are static so they persist across repaints.

## Code Walkthrough

- `Login` — UI construction in constructor; `actionPerformed` routes button clicks.
- `Rules` — HTML text in `JLabel`; two buttons route to Quiz/Login.
- `Quiz` — Data arrays, `start(int)`, `actionPerformed` (Next/Help/Submit), `paint` (timer + auto-advance), scoring loop comparing `useranswers` to `answers`.
- `Score` — Display result + exit.

## Learning Exercises

1. Add a "Previous" button to go back one question (guard `count > 0`).
2. Replace the `paint()`-based timer with `javax.swing.Timer` (cleaner, no blocking the EDT).
3. Load questions from a text/JSON file instead of hard-coded arrays.
4. Add a high-score file (read/write with `java.io`).
5. Convert absolute layout to a layout manager (`BorderLayout`/`GridBagLayout`).
6. Add difficulty levels and shuffle questions with `Collections.shuffle`.

## How to Run

### Prerequisites

- JDK 8 or later installed (`java` and `javac` on PATH).


### Option 1: Visual Studio Code

1. Install the **Extension Pack for Java** (Microsoft) in VS Code.
2. Open the `QuizApp` folder.
3. Make sure `src` is treated as the source root (VS Code usually detects it; if not, add `"java.project.sourcePaths": ["src"]` in `.vscode/settings.json`).
4. Open `src/quiz/app/Login.java` and click **Run** above `main`, or use the Run/Debug panel.

> Ensure `src` is on the classpath so `icons/` resources resolve. The Java extension handles this when `src` is a source root.

### Option 2: Command line

From the `QuizApp` directory:

```sh
# Compile
javac -d out src/quiz/app/*.java

# Run (ensure icons are on the classpath via src)
java -cp "out;src" quiz.app.Login
```

On Linux/macOS use `:` instead of `;` in the classpath:

```sh
java -cp "out:src" quiz.app.Login
```

Note: icons are loaded with `ClassLoader.getSystemResource("icons/...")`, so the `src` (or `out`) directory must be on the classpath for images to load.

## Notes

- The application is undecorated (no window title bar) and uses absolute positioning.
- Compiled output is in `QuizApp/out/`. Rebuild after source changes.
