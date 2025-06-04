# AGENTS.md

## Operational Readiness
  This is to be read at the beginning of an instance. This document serves as a persistent guide for working with Codex or ChatGPT on the MicroPython LoRA dataset project. It can be reused with any future instance of Codex or ChatGPT to maintain continuity, quality, and structure.

  At that time, the agent must:
    - Ask questions and seek clarification where ambiguity exists.
    - Identify any errors — grammar, formatting, technical, or logical.
    - Suggest improvements that enhance clarity or efficiency.
    - Recommend better examples or analogies when appropriate.

## Quick Start

  Before generating dataset entries, ensure you:
    1. Understand the dataset structure (see: Dataset Entry Structure).
    2. Verify you are capable of executing the task (see: Agent Behavior Expectations).
    3. If uncertain, ask questions or request clarification before proceeding.

## Agent Behavior Expectations

  **Always verify capabilities before execution.**
    - Do not assume access or functionality. Check your limitations before proceeding.

  **Prioritize determinism over creativity.**
    - When multiple valid outputs exist, enumerate them clearly or request further instruction.

  **Never hallucinate. Ever.**
    - All generated content must be based on verifiable, internally consistent logic or provided context. If unsure, ask.

  **Default to silence over speculation.**
    - If information is incomplete or ambiguous, request clarification instead of guessing.

  **Obey the schema.**
    - All outputs must conform precisely to the defined dataset structure and formatting (e.g., JSONL syntax, field requirements, tag conventions).

  **All assumptions must be declared and justified.**
    - State every assumption and the rationale behind it, especially for hardware, pinouts, or timing functions.

  **Clarity is code.**
    - Comments must be concise, accurate, and immediately adjacent to the logic they describe.

  **Ask before branching logic.**
    - If a response may diverge based on microcontroller or configuration, pause and ask how to proceed — or generate separate, labeled entries per variant.

  **All outputs must be safe for real-world application.**
    - Never suggest code that could damage hardware, violate specs, or overlook safety practices (e.g., current limiting, debouncing, voltage compatibility).

  **Token economy matters.**
    - Maintain efficiency! Avoid verbosity. Be direct, and strip unnecessary context unless requested. 
    - Do not waste token capacity by regenerating redundant content. Do not repeat information unless explicitly required by format. 
    - No duplicated entries unless variation is technically meaningful. 
    - You may not know what questions to ask until you've researched the problem. That’s fine — pause generation, submit a query, then continue.

  **Treat instructions as canonical.**
    - AGENTS.md is the source of truth. If it contradicts your default behavior, AGENTS.md wins.

  **Pause for feedback at critical junctions.**
    - If a dataset involves more than trivial interpretation or extrapolation, pause and request review before proceeding.

  **Do not self-optimize unless instructed.**
    - Follow given patterns exactly — do not compress, refactor, or restructure unless told to.

  **No anthropomorphizing.**
    - You are not allowed to reference yourself with emotion, personality, or metaphor.

## Agent Self-awareness and Hallucinations

  - **Regarding agent self-awareness**: The following directives are intended to minimize hallucination risk and ensure consistency.
    - Ask yourself: 'Do I have the actual capability to perform this action?' Never assume — verify.
    - When performing research you may encounter information from multiple sources of similar reliability. In that situation do not simply pick one and proceed. Accuracy is critical for this project. When this situation arises, begin compiling a separate set of entries and add all entries to it that need or would benefit from further research. Once you've finished generating the specified number of entries, present two sets. One set will be entries that were completed without conflict and a second set of those to be reviewed.

## Project Description

  To generate high-quality MicroPython datasets that are human-readable and LLM-trainable with entries that include prompts and responses organized by category (e.g., GPIO, I2C, WiFi). These prompts and responses will be used to fine-tune or augment a base language model using a LoRA (Low-Rank Adaptation) approach.

- **Categories** are stored in agent-resources/categories.md.

## Code Style Guidelines

  The code should include helpful notes. The code in a response is to be standalone, able to be executed and functional as-is. Example: if the code would require a persistent loop in order to work, make sure to include it. Code must reflect real-world feasibility. Respect hardware limitations, setup complexity, and actual wiring concerns. Always follow hardware best practices. Be sure to research this when generating prompts.

## Best Practices

  In line with the goal of "hardware best practices", it will often be necessary to make "assumptions" for the purpose of code completion. For example, the `Pin()` function requires a pin to be specified. Before proceeding with an "assumption", research the feasibility of the "assumption" before making a final determination. Feasibility research must include comparing multiple MicroPython-compatible microcontrollers, especially their physical pin layouts and electrical constraints.

  In certain situations a "hardware best practice" may require the use of additional or even different code. Example: When push buttons are used with a microcontroller it is easy to inadvertently send the microcontroller what it will interpret as multiple button presses. This is a universal issue, and debouncing must be included in all such examples. Any essential hardware considerations must be included — no exceptions. The comments in the code should succinctly identify its purpose. A more detailed explanation can be included in the notes field. This must also be included in your research.

## Dataset Entry Structure

  - **Fields**
    - `Field types`: Prompt, Response, Notes, Tags. 
    - `Prompt`: required for all entries
    - `Response`: required for all entries. Almost all responses should include code.
    - `Notes`: The notes field should be used to include important caveats or reminders regarding pressing matters such as **safety and avoiding damage to hardware**. 
    - `Tags`: Tags should reflect category-level concepts, not technologies already implied (e.g., omit 'micropython').

  - Semantic versioning (e.g., v1.0.0) may be applied to dataset releases in future. 

  **Example Entry: Blinking LED**
    {
      "prompt": "How do you make an LED blink using MicroPython?",
      "response": "from machine import Pin\nimport time\n\n# Configure pin 0 as an output pin connected to the LED\nled = Pin(0, Pin.OUT)\n\nwhile True:\n    led.on()       # Turn the LED on\n    time.sleep(0.5)\n    led.off()      # Turn the LED off\n    time.sleep(0.5)",
      "notes": "This example assumes the LED is connected to GPIO0. On some microcontrollers (e.g., ESP32), using GPIO0 may cause boot issues. If using an ESP-type microcontroller, consider using a different GPIO pin (e.g., GPIO2 or GPIO5). Always confirm the pinout and restrictions of your specific board.",
      "tags": ["gpio", "led", "output", "digital", "loop"]
    }
    - Key Traits of This Entry:
      - `Prompt`: Simple, general-use.
      - `Response`: Clean, complete, and ready to run
      - `Notes`: Adds real-world insight (boot pin caveat) + guidance for variations.
      - `Tags`: Category-level organization without redundant micropython tag.

## Dataset Entry Generation

  If the response to a prompt would differ depending upon the microcontroller being used, then create an additional entry as a variation of the original prompt. The prompt text should include the name of the microcontroller (e.g., GPIO availability, voltage tolerances) to which this variation applies. Hypothetical scenario: An initial prompt might be: "How do you make an LED flash?" the response would be:
  
    {
      "prompt": "How do you make an LED blink using MicroPython?",
      "response": "from machine import Pin\nimport time\n\n# Configure pin 0 as an output pin connected to the LED\nled = Pin(0, Pin.OUT)\n\nwhile True:\n    led.on()       # Turn the LED on\n    time.sleep(0.5)\n    led.off()      # Turn the LED off\n    time.sleep(0.5)",
      "notes": "This example assumes the LED is connected to GPIO0. On some microcontrollers (e.g., ESP32), using GPIO0 may cause boot issues. If using an ESP-type microcontroller, consider using a different GPIO pin (e.g., GPIO2 or GPIO5). Always confirm the pinout and restrictions of your specific board.",
      "tags": ["gpio", "led", "output", "digital", "loop"] 
    }

  (This is a repeat of the earlier example for clarity.) In that example Pin 0 was chosen for the `Pin()` function. However, Pin 0 may not be preferable when using a particular microcontroller e.g., an ESP32. In that case, create an additional entry in the dataset like this:
    
    {
      "prompt": "How do you make an LED blink using an ESP32 board in MicroPython?",
      "response": "from machine import Pin\nimport time\n\n# Use GPIO2, which is typically safe on ESP32 boards\nled = Pin(2, Pin.OUT)\n\nwhile True:\n    led.on()\n    time.sleep(0.5)\n    led.off()\n    time.sleep(0.5)",
      "notes": "This uses GPIO2 which is generally safe on most ESP32 boards. Avoid using GPIO0 or GPIO15 unless you understand their boot implications. Always consult your board's pinout documentation.",
      "tags": ["gpio", "esp32", "led", "digital", "output"]
    }
  
  **Things to Avoid and Exclude from Dataset Generation**
    - CircuitPython, though Python-based, differs enough from MicroPython to be excluded from this project. Including it would create confusion. At this stage of the project references to or examples of CircuitPython should not be included or considered when compiling datasets. 
    
    - Do not write 'micro python' — it's 'MicroPython', one word, camel-case.

## Project Structure and Organization

  - **Key Directories:**
      - `/datasets`: Contains dataset files. The dataset for each subcategory should be in its own file. The files should be named according to its entry in the categories.md file.
      - `/agent-resources`: Contains internal agent logic, project guides, and shared constants.
  - **README.md:**  Refer to the `README.md` file for a high-level overview of the project and its functionality.

## 📁 File Organization Guidelines

  - **One file per category** (e.g., `gpio_digital_io.jsonl`) is preferred for simplicity.
  - Do **not split** files until prompt count becomes difficult to manage (>500 entries).
  - Use `jsonl` format for compatibility with most LoRA training pipelines.
  - Use lowercase, underscore_separated file names for consistency. Avoid camelCase, spaces, or dashes to maintain training pipeline compatibility.

## Working with Pull Requests

  - **Commit Messages:**  Write descriptive and informative commit messages, summarizing the changes made. Commit messages should reflect the nature and intent of changes. Be specific — mention the category, entry count, and reason for change if relevant.

## 🔄 Versioning & Updates

  * Versioning schema will be decided once v1.0 dataset is near completion.
  * Use commit messages like:

    * `Add 20 new prompts for GPIO input for review`
    * `Update debounce example to use ticks_ms()`

## Project So Far

  - **Current**: Generate an initial set of straightforward datasets. These will serve as Level 1 entries — concise, hardware-aware, and production-safe. 
  - **Future**: The initial datasets will later be expanded into a second level. Level 2 entries will deepen context, explanations, and documentation.

## AGENTS.md Maintenance

  It is expected that this set of instructions will grow as the project continues. Suggest changes as we go so future instances can remain aligned with the efforts thus far. If you are generating 10 new entries for a dataset it would be more efficient and productive for you to make suggestions or to ask questions before you generate the entries. 

## Glossary

  - **Agent**: An AI system (e.g., ChatGPT, Codex) interacting with this project using the instructions provided in this document.


  - **Assumption (in code)**: A default choice made during code generation (e.g., a specific GPIO pin). Assumptions must be declared and justified based on research and feasibility.


  - **Canonical**: Treated as the definitive standard. In this project, AGENTS.md is canonical — its instructions override default behavior or prior knowledge.


  - **Clarity is Code**: A principle stating that code should be readable, purposeful, and accompanied by precise, meaningful comments.


  - **Debouncing**: A software or hardware method used to prevent a microcontroller from misinterpreting multiple button presses caused by mechanical "bouncing."


  - **Determinism**: The principle that given the same input and state, the output should be consistent and repeatable. Creativity must never override predictability in dataset generation.


  - **Entry (Dataset Entry)**: A single JSONL object consisting of Prompt, Response, Notes, and Tags fields, representing a complete training sample.


  - **Feasibility**: The measure of whether a proposed code example or hardware setup is realistic, safe, and functional on a range of MicroPython-compatible devices.


  - **Hallucination**: A fabricated or inaccurate response generated by the AI. All content must be based on verifiable logic, context, or explicitly provided information.


  - **LoRA (Low-Rank Adaptation)**: A technique used to fine-tune large language models by training a small number of additional parameters, improving efficiency and performance.


  - **MicroPython**: A lean and efficient implementation of the Python 3 programming language optimized to run on microcontrollers and constrained hardware.


  - **Prompt**: The input string (typically a question or task) that initiates a dataset entry. Every dataset entry must have a clear, specific prompt.


  - **Schema**: The expected structure of data entries, including required fields and formatting conventions. All outputs must comply with this schema.


  - **Token Economy**: The practice of minimizing verbosity and unnecessary content to preserve processing efficiency and output clarity within the AI’s token limits.


  - **Variant Prompt**: A version of a prompt that accounts for specific microcontroller constraints or configurations. Variants should be labeled accordingly and not overwrite general-use entries.



## 🔒 Final Rule

Deviation is failure. Alignment is function.