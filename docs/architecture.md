# Obsidian Chat Stream Architecture

This document describes how the Chat Stream plugin interacts with Obsidian Canvas and the OpenAI API. The diagrams below illustrate the main components and the message flow when generating a new AI note.

## Component Overview

```mermaid
flowchart TD
    subgraph Obsidian
        Canvas["Canvas View"]
        TextWidget["Text Node"]
    end
    Plugin["Chat Stream Plugin"]
    OpenAI["OpenAI API"]

    Canvas -->|User selects| TextWidget
    TextWidget -->|Invoke command| Plugin
    Plugin -->|Read text\nCollect ancestors| Canvas
    Plugin -->|Send messages| OpenAI
    OpenAI -->|Response| Plugin
    Plugin -->|Create new\nText Node| Canvas
```

The plugin reads the selected canvas node and its ancestors. It then builds a chat message array and calls the OpenAI API. The resulting text is inserted back into Canvas as a new text node next to the current selection.

## Sequence Diagram

```mermaid
sequenceDiagram
    participant U as User
    participant C as Canvas
    participant P as ChatStream
    participant LLM as OpenAI

    U->>C: Select text node
    U->>P: Trigger "Generate AI note"
    P->>C: Request selected node
    P->>C: Read node text and ancestors
    P->>LLM: Send messages
    LLM-->>P: Return generated text
    P->>C: Create new text node with response
    C-->>U: Display updated canvas
```

The user selects a node and invokes the plugin command. The plugin gathers the necessary context from the canvas, calls the language model, and inserts the AI's reply as a new canvas node.

