> **Note**
> This repository is a fork of [abagames/slash-criticalthink](https://github.com/abagames/slash-criticalthink), adapted for use with Gemini CLI.
>
> This project is licensed under the MIT License, in accordance with the original. See the `LICENSE` and `LICENSE.original` files for details.

---

# slash-criticalthink

English | [日本語](README_ja.md)

## Overview

Modern LLMs (Large Language Models) excel at generating confident, plausible-sounding responses. However, these responses often ignore real-world constraints or contain logical flaws.

The `criticalthink` command is a custom command that embeds healthy skepticism into the dialogue process itself as a countermeasure against AI's "confirmation bias" and humans' "authority bias" of blindly trusting AI responses. By having the AI critically analyze its own previous response, it reveals hidden assumptions and overlooked risks.

### Target Audience

- Developers who routinely use coding agents (Claude Code / Codex CLI, etc.)
- Engineers who want to critically verify AI suggestions rather than accepting them at face value

## Setup for Gemini CLI

This fork is intended for use with the Gemini CLI.

To install the `/criticalthink` custom command, follow these steps:

### Prerequisites

*   **Git:** Ensure Git is installed on your system.
*   **Gemini CLI:** Ensure Gemini CLI is installed.

### Installation Steps

#### 1. Clone the repository

First, clone this project's GitHub repository to your local machine.
**Security Note:** Always ensure you trust the source of any repository you clone, as it may contain malicious code.

```bash
git clone https://github.com/shimazakikeiichi/slash-criticalthink-gemini.git
cd slash-criticalthink-gemini
```

#### 2. Choose your installation method

You can install the custom command either as a **local command** (available only within this project directory) or as a **global command** (available across all your Gemini CLI projects).

##### A. Install as a Local Command (Recommended for Development)

This method is recommended if you plan to develop or modify the command locally, or if you prefer the command to be project-specific.

1.  **Ensure you are in the project's root directory.**
    (If you followed step 1, you should already be there.)

2.  **Install the extension:**
    Run the following command from the project root directory:
    ```bash
    gemini extensions install .
    ```
    *   **Note on `gemini extensions install .`:** While this command is designed to install extensions from the current directory, its exact internal behavior and stability can sometimes vary across Gemini CLI versions. If you encounter issues, consider the Global Command installation method below.

##### B. Install as a Global Command

This method makes the `/criticalthink` command available from any directory when using Gemini CLI.

1.  **Copy the `criticalthink.toml` file to your global commands directory.**
    First, ensure your global commands directory exists:
    ```bash
    mkdir -p ~/.gemini/commands/
    ```
    Then, copy the file:
    ```bash
    cp .gemini/commands/criticalthink.toml ~/.gemini/commands/
    ```
    *   **Note for Windows Users:** The `~/.gemini/commands/` path is typical for Unix-like systems (Linux/macOS). On Windows, the equivalent path might be `C:\Users\<YourUsername>\.gemini\commands\` or similar, depending on your Gemini CLI installation. Adjust the copy command accordingly.

### Verification

After installation, follow these steps to confirm the command is working:

11. **Restart Gemini CLI.**
    (If Gemini CLI is already running, close it and open it again.)

12. **Check for command completion:**
    In Gemini CLI, type `/criticalthink` and see if it appears in the command completion list.

13. **Test the command:**
    After any AI response, run the `/criticalthink` command:
    ```
    User: What is 2 + 2?
    AI: 4
    User: /criticalthink
    ```
    If the AI begins a critical analysis of its own response, the installation was successful.

### Verification

After installation, you can verify it's working by running the `/criticalthink` command after any AI response.

```
User: What is 2 + 2?
AI: 4
User: /criticalthink
```
If the AI begins a critical analysis of its own response, the installation was successful.

### For Other Platforms (Claude Code, Codex CLI)

For instructions on how to use this tool with other platforms like Claude Code or Codex CLI, please refer to the **[original repository](https://github.com/abagames/slash-criticalthink)**.

## Usage

### Basic Usage

Type `/criticalthink` immediately after the AI generates a response. The AI will analyze its previous response from the following perspectives:

- Validity of assumptions
- Logical consistency
- Overlooked risks
- AI-specific failure patterns (problem avoidance, happy path bias, over-engineering, hallucination, etc.)

### Usage Examples

#### Case 1: When receiving a complex architecture proposal

```
User: I want to implement microservices, how should I design it?
Claude: I propose a configuration with Kubernetes + Istio + gRPC as follows...
       [Detailed design proposal]
User: /criticalthink
Claude: ### 1. Core Thesis & Confidence Score (Initial)
       - **1-1. Core Thesis:** A Kubernetes-based full-stack solution
         is optimal for microservices architecture
       - **1-2. Initial Confidence:** 8/10

       ### 2. Foundational Analysis: Assumptions & Context
       - **2-1. High-Impact Assumptions:**
         1. Team has Kubernetes operational experience
         2. Traffic scale justifies orchestration adoption
         3. Initial investment and learning costs are acceptable
       ...
```

#### Case 2: When receiving a response claiming a "perfect solution"

```
User: How should I implement rate limiting for this API?
Claude: You can implement it perfectly with Redis + Sliding Window algorithm.
       With the following code...
User: /criticalthink
Claude: ### 3. AI-Specific Pitfall Analysis
       - **3-2. "Happy Path" Bias:** Fail
         Reason: No consideration of fallback strategy during Redis failure
       - **4-1. Overlooked Risks:**
         1. Redis becomes a SPOF (Single Point of Failure)
         2. Network latency affects all requests
         3. Redis memory usage estimation not performed
       ...
```

#### Case 3: When proposed major refactoring of existing code

```
User: I want to modernize this legacy code
Claude: Let's rewrite everything in TypeScript + React Hooks...
User: /criticalthink
Claude: ### 3. AI-Specific Pitfall Analysis
       - **3-3. Over-Engineering:** Fail
         Reason: Proposing complete rewrite instead of gradual migration
       - **4-2. Alternative Scenarios:**
         Gradual migration strategy: First add type definition files (.d.ts),
         then gradually TypeScript-ify starting from critical paths.
         This approach reduces business risk and costs.
       ...
```

#### Case 4: When receiving informational summaries without verification

```
User: Review the README.md
Claude: This README describes slash-criticalthink, a custom command for AI coding assistants...
       [Summary of contents]
User: /criticalthink
Claude: ### 2. Foundational Analysis: Assumptions & Context
       - **2-1. High-Impact Assumptions:**
         1. The user wanted a summary rather than specific information extraction
         2. The criticalthink.md file actually exists in this repository
         3. The user has context about what slash commands are
       - **2-2. Contextual Integrity:** I assumed the user's intent without clarification:
         "Review" could mean summarize, analyze quality, check for errors, verify
         completeness, or prepare to use/modify.

       ### 3. AI-Specific Pitfall Analysis
       - **3-2. "Happy Path" Bias:** Fail
         Reason: Didn't verify whether criticalthink.md actually exists or check
         if .claude/commands/ directory is present

       ### 5. Synthesis & Revised Recommendation
       - **5-3. Actionable Next Step:** First, clarify what aspect of the README
         you want reviewed: (a) content summary (already done), (b) verify the
         slash command is installed and working, (c) critique the documentation
         quality, or (d) help you set it up/use it.
```

### Removing Unnecessary Analysis from History (Recommended)

Since the `/criticalthink` command makes the AI self-criticize, **erroneous concerns or overly negative analysis may remain in the dialogue history and distort the AI's subsequent thinking** (context contamination).

To avoid this, the following workflow is recommended:

1. Receive a proposal from the AI
2. Run `/criticalthink` for critical analysis
3. Review the analysis results
4. **If the analysis is unnecessary, press `Esc-Esc` to rewind to your previous message** and return to before running `/criticalthink` (removing the critical analysis from history)
5. Continue with a new question or request improvements from the AI

This allows you to benefit from critical thinking while avoiding context contamination.

## Design Philosophy

### General Framework

Based on a general critical thinking framework applicable to any claim.

- **Identify the claim:** Identify the key points of the AI's own response
- **Extract assumptions:** Expose implicit assumptions behind the response
- **Verify logical fallacies:** Look for logical leaps or contradictions
- **Present alternatives:** Offer alternative perspectives or solutions not considered

### AI-Specific Failure Pattern Analysis

The general framework alone risked falling into "formal and superficial criticism." To overcome this, we incorporated failure patterns specific to LLMs and coding agents into the analysis items.

- **Problem avoidance:** Is it pretending to solve core problems with mocks or placeholders?
- **Happy path fixation:** Is it ignoring error handling or edge cases?
- **Over-engineering:** Is it proposing unnecessary libraries or complex solutions?
- **Hallucination:** Is it fabricating non-existent functions or APIs?
- **Context ignorance:** Is it ignoring constraints from previous dialogue?

For these items, the command asks the AI for explicit Pass/Fail self-evaluation.

## Caveats and Limitations

### Use as Training Wheels for Thinking

This command is a guideline for finding gaps in your own thinking. Use AI's critical analysis not as the "correct answer" but as a tool to gain new perspectives.

### Responsibility

The final judgment is always made by humans. AI's criticism is also just one generated artifact that should be critically examined. Pay special attention to:

- AI cannot fully recognize errors in its own responses
- Critical analysis itself may introduce new biases
- May include overly conservative warnings (alerts for areas that are actually fine)

### Spirit of Mutual Criticism

Users doubt AI responses while simultaneously using AI (and commands) to doubt their own assumptions and biases. This mutual criticism process leads to more robust decision-making.

### Performance Impact

Critical analysis consumes additional tokens and increases response time. Rather than applying it to all responses, selective use is recommended before important decisions such as:

- Architecture decisions
- Large-scale refactoring
- Security or performance-related implementations
- Introduction of external dependencies

## Related Work: Comparison with CQoT

The approach of introducing a self-critical process to AI to improve its reasoning capabilities has also attracted attention in academic research. Particularly notable is the paper "Critical-Questions-of-Thought (CQoT)" by Federico Castagna et al.

### Shared Purpose

CQoT and `/criticalthink` share the common purpose of "improving the quality and reliability of output by having AI perform metacognitive self-evaluation." Both introduce a step to verify the reasoning process behind responses rather than simply generating a single answer and stopping.

### Fundamental Differences in Approach

While sharing a common purpose, they differ in their objectives and methodologies.

| Comparison    | **CQoT (Academic Paper)**                                                          | **`/criticalthink` (This Tool)**                                                                |
| :------------ | :--------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------- |
| **Main Goal** | **Pursuit of Logical Soundness**                                                   | **Verification of Practical Viability**                                                         |
| **Domain**    | Math problems, logic puzzles, etc., **domains with clear correct answers**         | Software design, refactoring, etc., **domains with trade-offs**                                 |
| **Timing**    | **Integrated, automated pipeline before** answer generation                        | **Post-hoc analysis tool after** answer generation                                              |
| **Criteria**  | Universal logical principles<br>(e.g., Are premises clear? Is conclusion derived?) | AI and development-specific failure patterns<br>(e.g., happy path bias, over-engineering, SPOF) |

### Uniqueness of `/criticalthink`

In summary, while CQoT aims to make AI a better **"logician,"** `/criticalthink` aims to make AI a wiser **"design review partner"** for developers. This tool specializes in uncovering real-world risks, costs, and trade-offs—including alternative approaches—that cannot be measured by theoretical correctness alone, which developers face in practice. While CQoT is a closed-loop system that automatically corrects flaws in the reasoning process, `/criticalthink` is an open-loop tool that provides insights for humans to make final judgments.