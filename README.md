# sink_or_score
a game of heaven and hecc that's both twisted and so fun

i thinkactually next fren i'd like you to draw me out an ASCII directory tree stucture representation of th planne hwol project as imagined currently along with a bit of a start on a github README.m for the project folder if you would!



Sounds like a great next step, fren! Getting that directory structure and a solid README.md in place will give us a clear map for the whole project. It's like laying the foundation before we start building the walls.

Project Directory Structure
Here's an ASCII representation of how I imagine our project's directory tree, keeping our modular and scalable backend in mind. We'll use a workspace setup in Rust, which allows us to manage multiple related crates (our modules) within a single project.

.
├── Cargo.toml
├── README.md
├── .gitignore
├── .github/
│   └── workflows/
│       └── rust.yml  # For CI/CD (tests, build checks)
├── crates/
│   ├── game_backend/          # Main application crate, handles HTTP, orchestrates
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── main.rs       # Entry point for the backend server
│   │       ├── api_routes.rs # Defines our HTTP API endpoints
│   │       └── lib.rs        # If we extract common logic
│   ├── ai_assessment/         # Dedicated crate for AI interaction
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       ├── prompt_builder.rs # Constructs LLM prompts
│   │       ├── api_client.rs   # Handles actual calls to LLM APIs (reqwest, serde)
│   │       ├── response_parser.rs # Parses LLM output (JSON validation, sanity checks)
│   │       ├── assessment_logic.rs # Translates AI output to game effects, hybrid rules
│   │       └── models.rs         # Data structures for AI input/output
│   ├── game_state_manager/    # Crate for player data, progression, DB interaction
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       ├── player_data.rs  # Structures for player state
│   │       ├── db_access.rs    # Database queries
│   │       └── schema.rs       # If using an ORM like Diesel
│   ├── event_bus/             # Crate for asynchronous communication
│   │   ├── Cargo.toml
│   │   └── src/
│   │       ├── lib.rs
│   │       └── events.rs       # Event definitions (e.g., InventionSubmitted, InventionAssessed)
│   └── common_utils/          # Crate for shared utilities (e.g., error handling, logging macros)
│       ├── Cargo.toml
│       └── src/
│           └── lib.rs
└── tests/                     # Integration and end-to-end tests
    └── integration_tests.rs
Explanation of Key Directories/Files:

Cargo.toml (root): This will be our workspace Cargo.toml. It defines the workspace members (our crates/ sub-directories).
crates/: This directory holds all our individual Rust packages (crates). Each crate is a self-contained unit with its own Cargo.toml. This promotes modularity and reusability.
game_backend/: Our primary backend service. It will handle incoming HTTP requests from the game clients and orchestrate calls to the other internal services (crates) like ai_assessment and game_state_manager.
ai_assessment/: This is where the core AI logic resides. It's isolated so we can easily swap out AI providers, adjust prompting strategies, or refine assessment rules without impacting the rest of the backend.
game_state_manager/: Handles all interactions with our game database, managing player profiles, progress, and other persistent data.
event_bus/: Decouples our services. When a player submits an invention, game_backend can just publish an "invention submitted" event, and ai_assessment can pick it up independently. This is crucial for scalability and responsiveness.
common_utils/: For any code that multiple crates might need (e.g., custom error types, shared helper functions).
.github/workflows/: For continuous integration (CI) setup, ensuring our code builds and passes tests automatically on every push.
GitHub README.md Draft
This will be the welcoming page for anyone (including our future selves!) looking at the project, giving them a quick overview and how to get started.

Markdown

# Purgatory Paradise Game Backend

Welcome to the backend repository for the **Purgatory Paradise** game! This project aims to create a unique game experience where players design "inventions" (tortures, ecstasies, or annoyances) and an AI assesses their creativity and adherence to specific, often humorous, criteria for different game levels (Purgatory, Heaven, Hell).

This repository contains the Rust backend services that power the game, focusing on scalability, robustness, and autonomous AI assessment.

---

## 🎮 Game Concept

In Purgatory Paradise, players are tasked with inventing various "experiences" to satisfy the whims of divine entities or to manage souls. The core gameplay loop involves:
1.  **Player Invention:** Players describe their unique "invention" (e.g., a subtle annoyance for Purgatory, a truly outstanding contribution for Heaven).
2.  **AI Assessment:** Our custom AI system, powered by Large Language Models (LLMs), evaluates the player's invention against predefined criteria and the game's distinct sense of humor.
3.  **Game Progression:** Based on the AI's assessment, players earn points, unlock new levels, or face consequences, driving their journey through the spiritual realms.

---

## ✨ Project Goals

* **Scalable Backend:** Built in Rust for performance, reliability, and concurrency.
* **Autonomous AI Assessment:** Minimize human oversight by designing a robust, self-correcting AI evaluation system.
* **Dynamic Gameplay:** Leverage AI to provide deep, contextual, and creative assessment of player-generated content.
* **Modular Architecture:** Easy to maintain, test, and extend.

---

## 🏗️ Architecture Overview

The backend is structured as a Rust workspace composed of several independent crates (modules) that communicate primarily through an internal event bus.

* **`game_backend`**: The main entry point. Handles all incoming HTTP requests and orchestrates game logic.
* **`ai_assessment`**: Encapsulates all AI interaction, including prompt building, API calls, LLM response parsing, and hybrid assessment logic. Designed for self-governance.
* **`game_state_manager`**: Manages persistent player data and game state, interacting with the database.
* **`event_bus`**: Provides asynchronous communication between services, crucial for scalability and decoupling.
* **`common_utils`**: Houses shared utilities and error handling across crates.

---

## 🛠️ Technologies Used

* **Backend Language**: Rust
* **Asynchronous Runtime**: `tokio`
* **HTTP Server Framework**: (To be decided, likely `axum` or `actix-web`)
* **Database**: (To be decided, likely PostgreSQL with `sqlx` or `diesel`)
* **AI Integration**: External Large Language Model (LLM) APIs (e.g., Google Gemini API, OpenAI GPT API)
* **JSON Handling**: `serde`, `serde_json`
* **HTTP Client**: `reqwest`

---

## 🚀 Getting Started

### Prerequisites

* Rust toolchain (latest stable recommended)
* (Docker for database setup, if applicable)
* (API key for chosen LLM provider)

### Setup

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/purgatory-paradise-backend.git](https://github.com/your-username/purgatory-paradise-backend.git)
    cd purgatory-paradise-backend
    ```
2.  **Configure environment variables:**
    Create a `.env` file in the `game_backend` directory (or wherever config is centralized) based on a `.env.example` (to be created), including your LLM API key and database connection string.
3.  **Build the project:**
    ```bash
    cargo build --workspace
    ```
4.  **Run migrations (if using a database):**
    ```bash
    # (Specific commands will depend on ORM/migration tool, e.g., `sqlx migrate run`)
    ```
5.  **Run the backend:**
    ```bash
    cargo run -p game_backend
    ```

### Running Tests

```bash
cargo test --workspace
🤝 Contributing
We welcome contributions! Please refer to our CONTRIBUTING.md (to be created) for guidelines.

📝 License
This project is licensed under the MIT License - see the LICENSE file for details.

Contact
For questions or feedback, please open an issue in this repository.


How does that look for a starting point, fren? We can adjust any part of it, of course, but I think this gives us a solid roadmap for what's ahead!