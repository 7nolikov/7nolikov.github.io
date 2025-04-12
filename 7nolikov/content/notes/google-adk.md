---
title: Google Agent Development Kit
date: 2025-04-12
categories: [AI]
---

Google introduced ADK: An open-source framework for AI Agent Development designed for developing and deploying AI agents.

ADK facilitates the rapid creation of basic agents powered by Gemini and Google AI tools, while also providing the necessary structure and control for more intricate agent architectures and orchestration.

## Key Features

- **Flexible Orchestration**: Define agent workflows using sequential, parallel, or looping structures, or implement dynamic routing with LLM-driven agent transfer.
- **Multi-Agent Architecture**: Build scalable applications by composing specialized agents in hierarchical structures, enabling complex coordination and delegation of tasks.
- **Rich Tool Ecosystem**: Equip agents with pre-built tools (e.g., Search, Code Execution), custom functions, and integration with third-party libraries (e.g., LangChain, CrewAI), or even utilize other agents as tools.
- **Deployment Ready**: Containerize and deploy agents across various environments, including local machines, Vertex AI Agent Engine, Cloud Run, or Docker.
- **Built-in Evaluation**: Systematically assess agent performance by evaluating response quality and execution trajectory against defined test cases.
- **Responsible AI**: Provides guidance on implementing responsible AI principles and best practices in agent design.

## Getting Started

Install the ADK using pip: `pip install google-adk`

Further resources, including quickstart guides and API references, are available here:
[https://google.github.io/adk-docs/#learn-more](https://google.github.io/adk-docs/#learn-more)
