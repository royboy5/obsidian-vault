---
created: 2026-01-13 23:17
status: todo
tags:
  - project
  - brief
  - rust
---

## Project Brief: Guessing Game

*This is the high-level "what, why, and who" of the project. It's the guiding star to keep you focused.*

## 🎯 Summary / Mission Statement
Build a classic "Guess the Number" game. The program will generate a random secret number between 1 and 100, and the player will attempt to guess it. The game will provide feedback (Too high, Too low) until the user wins.

## 🧠 Learning Goals (The "Why")
This project is designed to introduce core Rust concepts before diving into Ownership:
- **Cargo Basics:** Adding the `rand` crate to `Cargo.toml`.
- **Standard I/O:** Using `std::io` to handle user input.
- **Variables & Mutability:** Understanding `let` vs `let mut`.
- **Result Type:** Basic error handling with `.expect()`.
- **Enums & Pattern Matching:** Using the `Ordering` enum with the `match` keyword.
- **Loops:** Using `loop` and `break` to handle repeated guesses.

## 🛠️ Tech Stack
- **Language:** Rust
- **Build System:** Cargo
- **Key Crates:** `rand` (external crate for random number generation)

## ✅ Scope & Features (MVP)
1. **Input:** Capture a number from the command line.
2. **Logic:** Generate a secret number and compare it to user input.
3. **Feedback:** Tell the user if their guess was too high, too low, or correct.
4. **Validation:** Ensure the game doesn't crash if the user enters a non-number (Chapter 2's "Handling Invalid Input" section).
5. **Win Condition:** Exit the game when the user guesses correctly.

## 🔗 Related Resources (MOCs)
- [[3 Resources/Rust/Rust MOC]]
- [The Rust Book: Chapter 2](https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html)