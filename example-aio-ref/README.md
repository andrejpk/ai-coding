# Architecture Diagram Image -> Markdown: Azure IOT Reference Architcture

Guidance from [AIO reference architecture](https://learn.microsoft.com/en-us/azure/iot-operations/overview-iot-operations) used as a test for models and prompting:

![Reference arch source image](aio-reference-arch.png)

## Prompt

```text
Look at this architecture and write up a document with the components, structure and relationships between the components in the diagram sufficient to serve as detailed design document for an engineering team building a solution on the plan. Create a markdown file structured by layer of architecture, starting at the top level for the separate layered tiers/subsystems. Create mermaid diagrams but layer them so we don't have one huge diagram, but maybe 2 or 3 layers with links between them.

Create a single Markdown file. Use only the info from the image. Only include what's in the image. Include everything in the image and nothing else. (Don't make any engineering decisions to fix anything that is wrong, just document.)
```

## Output


- [Gemini 2.0 Flash - Low](aio-ref-gemini-2-flash-v1.md)
- [Claude Sonnet 3.7 Reasoning - Low](aio-ref-claude-37-reasoning-low-v1.md)
- [Claude Sonnet 3.7 Reasoning - High](aio-ref-claude-37-reasoning-high-v1.md)

all run in [T3 Chat Pro](https://t3.chat/) on Mar-06-2025
