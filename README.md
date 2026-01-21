# SkillMash - Smart Job Candidate Ranking System

**SkillMash** is a comprehensive software solution designed to automate and optimize the process of shortlisting candidates for job openings. By leveraging efficient Data Structures and Algorithms (DSA), it addresses the challenges of parsing, searching, ranking, and selecting candidates effectively.

## 🚀 Key Features & Algorithms

This project implements a suite of algorithms to solve specific recruitment problems:

*   **Merge Sort & Quick Sort**: Efficiently orders candidates based on criteria like Expected Salary or Experience Years.
*   **Knuth-Morris-Pratt (KMP)**: Performs high-speed keyword searching within candidate resumes/bios.
*   **Dynamic Programming (LCS)**: Calculates a "Fit Score" by comparing candidate skills with job requirements using Longest Common Subsequence logic.
*   **Graph Construction**: Models relationships between skills (e.g., C# ↔ .NET) to suggest related technologies.
*   **Max Heap (Priority Queue)**: efficiently manages and retrieves top candidates.
*   **Trie (Prefix Tree)**: Enables fast autocomplete and prefix-based searching for skills.
*   **AVL Tree**: Maintains a balanced binary search tree for optimized candidate lookups.
*   **Greedy Selection**: Optimizes team selection to maximize quality within a fixed hiring budget.
*   **Hash Indexing**: Provides instant O(1) lookups for candidate details.

## 🛠️ Technology Stack

*   **Framework**: ASP.NET Core (MVC / Web API)
*   **Language**: C#
*   **Frontend**: HTML, CSS, JavaScript (Razor Views)
*   **Data**: In-memory data structures (custom implementations of Trees, Graphs, Heaps, etc.)

## 📖 Documentation

For a deep dive into the algorithmic paradigms, complexity analysis, and implementation details, please refer to the [Project Report](Project_Report.md).

## ⚡ Getting Started

### Prerequisites

*   [.NET SDK](https://dotnet.microsoft.com/download) (Version 6.0 or later recommended)
*   Visual Studio or VS Code

### Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/asad594/Job-Ranking-System.git
    cd Job-Ranking-System
    ```

2.  **Build the project**
    ```bash
    dotnet build
    ```

3.  **Run the application**
    ```bash
    dotnet run
    ```

4.  Open your browser and navigate to `http://localhost:5000` (or the port indicated in the terminal).

## 📄 License

This project is available for educational and demonstration purposes.
