# 🧠 Agentic Study - Flashcard App

> **Note:** This is a showcase repository for my portfolio. The complete source code is maintained in a private repository.

> [**Download from the Play Store**](#) *(Link coming soon)* | [**Download Android APK**](https://github.com/<your-username>/agentic-study-showcase/releases) | [**Privacy Policy**](https://<your-username>.github.io/agentic-study-showcase/privacy-policy)

Agentic Study is a mobile application (Android/iOS) for learning through flashcards. It uses Artificial Intelligence (Google Gemini) to automatically generate card decks from text and implements a **Leitner-based Spaced Repetition system**.

---

## ✨ Key Features

* **🤖 AI Generation (Data Extractor)**: Paste a text and let Gemini extract key concepts, transforming them into categorized atomic nodes (flashcards).

* **🔄 Remediation Loop**: If a card is answered incorrectly repeatedly, the AI intervenes to simplify and regenerate it.

* **📚 5-Box Leitner Algorithm**: Adaptive spaced repetition with intervals of 1, 2, 4, 8, and 16 days. Mastered cards are permanently promoted.

* **🔒 Privacy-First (No DB)**: All decks and user progress live exclusively on the local device (`AsyncStorage`), with no cloud database.

---

## 🛠️ Technology Stack

The architecture uses pnpm workspaces and is divided into Mobile and API Proxy components.

* **Frontend Mobile**: React Native, Expo, Expo Router

* **Storage**: `@react-native-async-storage/async-storage`

* **Backend (API Proxy)**: Express 5, Node.js 24

* **Artificial Intelligence**: Google Gemini (`@google/genai`)

* **Validation & Contracts**: OpenAPI, Zod (`zod/v4`), Orval (API hooks code generation)

* **Language**: TypeScript 5.9 (Strict Typechecking)

---

## 🏗️ App Architecture & Structure

### 1. AI Pipeline & Remediation Loop (Data Extractor)

Deck creation is driven by an agent based on Google Gemini. The user provides unstructured text (notes, articles), and the AI extracts and summarizes the key concepts, returning a strongly typed and validated JSON output.

The architecture implements an innovative **Remediation Loop**: if the user repeatedly answers the same card incorrectly (exceeding a predefined failure threshold), the system flags the node and queries the AI again. The agent then regenerates that specific card by breaking it down into simpler and more atomic concepts, with a maximum retry limit to prevent infinite loops.

### 2. Spaced Repetition Engine (Leitner System)

The core of the app is a deterministic spaced repetition algorithm based on 5 Boxes with increasing time intervals (1, 2, 4, 8, and 16 days).

* **Promotion & Penalty**: A correct answer promotes the card to the next box; an incorrect answer immediately demotes it to Box 1.

* **Graduation**: A card that reaches Box 5 must pass a final review after 30 days to be removed from the active deck and promoted to an isolated pool of "mastered cards" (saved in a permanent history).

### 3. Local-First Architecture & Zero Database

The application is designed to maximize privacy and offline performance. All complex domain models (`Deck`, `Card`, `MasteredCard`, `UserProgress`) are stored and managed entirely on the local device through `AsyncStorage` from React Native.

There is no relational database in the cloud; user data never leaves the phone, eliminating latency during study sessions.

### 4. Stateless Thin Proxy for Security & Contracts

The Node.js backend (Express 5) does not store any state or user data. It acts solely as an intermediate **Proxy** between the mobile application and Google's Gemini APIs. This design pattern provides:

* Secure API key protection by keeping keys off the client.

* Resilient handling of LLM rate limits and errors.

* Strict validation of exchanged payloads using **OpenAPI**, **Zod**, and **Orval** for automated generation of React Query hooks on the client side.

### 5. Dynamic Time Calculation (Engagement)

Instead of implementing complex background tasks that consume battery to degrade user statistics over time, the system uses a "lazy" approach.

Engagement metrics are recalculated dynamically whenever the application starts or gains focus: the system reads the timestamp of the last activity (`last_active_at`), calculates the actual elapsed time, and applies the decay formula immediately.

---

## 📸 Screenshots & Demo

*(Add some images or a GIF demonstrating the app here. Example:)*

<p align="center">

  <img src="./assets/screenshot-mainframe.png" width="250" />

  <img src="./assets/screenshot-review.png" width="250" />

  <img src="./assets/screenshot-deck.png" width="250" />

</p>

---

## 🚀 How to Try the App

1. **Android (Google Play Store)**: *(Link coming soon once the review process is complete)*.

2. **Android (APK Sideloading)**: Go to the [Releases](https://github.com/<your-username>/agentic-study-showcase/releases) section and download the `app-release.apk` file. Transfer it to your phone and install it. You may need to enable installation from unknown sources.

3. **Privacy Policy**: Hosted publicly through GitHub Pages [here](https://<your-username>.github.io/agentic-study-showcase/privacy-policy).

---

## 📄 License

The complete source code of this application is proprietary. This repository and the included code snippets are provided exclusively for demonstration and portfolio purposes (All rights reserved).
