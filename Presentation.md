# Seminar Presentation: Quiz Application (Java Swing)

---

## Slide 1: Title Slide

- **Title:** Quiz Application using Java Swing
- **Course:** 5th Semester — Java
- **Presenter:** [Your Name]
- **Guide:** [Guide Name]
- **Institution:** [Institution Name]

---

## Slide 2: Introduction

- A desktop-based quiz application built with Java Swing.
- Provides an interactive multiple-choice quiz on Java topics.
- Features a timer, lifeline, and score tracking.
- Demonstrates GUI programming, event handling, and OOP in Java.

---

## Slide 3: Problem Statement

- Traditional quizzes are often paper-based or web-dependent.
- Need a lightweight, offline, desktop quiz tool.
- Should be engaging with time pressure and scoring.
- Should be simple to run on any machine with a JDK.

---

## Slide 4: Objectives

- Build a desktop quiz application with a graphical interface.
- Implement multiple-choice questions with 4 options each.
- Add a per-question countdown timer (15 seconds).
- Provide a 50-50 lifeline to remove two wrong options.
- Track and display the final score.
- Allow replay by returning to the login screen.

---

## Slide 5: Methodology

- **Approach:** Object-oriented design with separate JFrame classes per screen.
- **Flow:** Login → Rules → Quiz → Score → Login.
- **Data:** Questions, options, and correct answers stored in 2D arrays.
- **UI:** Absolute layout (null layout) with custom bounds for components.
- **Events:** ActionListener for button clicks; Timer for countdown.
- **Resources:** PNG icons loaded via classpath (`ClassLoader.getSystemResource`).

---

## Slide 6: Technology Used

- **Language:** Java (JDK 8+)
- **GUI Framework:** Java Swing / AWT
- **IDE:** IntelliJ IDEA
- **Build:** Standard `javac` / `java`
- **Paradigm:** Object-Oriented Programming
- **Libraries:** `javax.swing`, `java.awt`, `java.awt.event`

---

## Slide 7: Project Structure

```
QuizApp/
├── src/
│   ├── quiz/app/
│   │   ├── Login.java   (entry screen)
│   │   ├── Rules.java   (rules screen)
│   │   ├── Quiz.java    (main quiz logic)
│   │   └── Score.java   (result screen)
│   └── icons/           (UI images)
├── out/                 (compiled classes)
└── QuizApp.iml          (IDE module)
```

- Package: `quiz.app`
- Four classes, each a JFrame screen.

---

## Slide 8: Implementation — Login & Rules

- **Login.java:** Text field for name; "Next" opens Rules; "Back" exits.
- **Rules.java:** Displays quiz rules; "Start" launches Quiz; "Back" returns to Login.
- Undecorated windows with custom background color (green theme).
- Icons loaded from `icons/` resource folder.

---

## Slide 9: Implementation — Quiz Logic

- **Quiz.java** contains:
  - 10 questions with 4 options each (2D arrays).
  - Correct answers stored separately.
  - `JRadioButton` options grouped with `ButtonGroup`.
  - 15-second `Timer` per question; auto-moves on timeout.
  - "Next" / "Submit" / "50-50 Lifeline" buttons.
  - Score incremented on correct answer.
- Static fields track timer, count, score, and answers given.

---

## Slide 10: Implementation — Score Screen

- **Score.java** displays:
  - Thank-you message with player name.
  - Final score.
  - Score icon and background image.
  - "EXIT" button returns to Login screen.
- Allows replay without restarting the application.

---

## Slide 11: Results

- Application runs successfully on JDK 8+.
- All four screens transition correctly.
- Timer counts down and auto-advances.
- Lifeline removes two wrong options.
- Score is calculated and displayed accurately.
- Lightweight, offline, and easy to deploy.

---

## Slide 12: Conclusion & Future Scope

- **Conclusion:** Successfully built a functional desktop quiz app demonstrating Java Swing, OOP, and event-driven programming.
- **Future Scope:**
  - Add more questions / categories.
  - Persist high scores to a file or database.
  - Add sound effects and animations.
  - Multiplayer / networked mode.
  - Convert to a web or mobile version.

---

*Thank You — Questions?*