# Whiteboard Diagram -> Markdown/Mermaid

Goal: Take a diagram like we'd create in an ADS or other design session and create a Markdown document that includes all of the important details in that diagram, in a way that is readable by humans (for validation) and to AI (for context on coding and assitance).


## Prompts


```text
Look at #file:leak-detection-proposed-architecture-2.png   and write up a document with the structrue and relationships between the components in the diagram. Create a markdown file broken down by layer for the separate deployment locations, then subsystems. Create mermaid diagrams but layer them so we don't have one huge diagram, but maybe 2 or 3 layers with links between them.

Create a markdown file in /work 

Use only the info from the image. Ignore everything else in the repo for now, We are just documenting what's in the image. Only include what's in the image. Include everything in the image and nothing else.
```

[Test Results with Azure Docs sample](./example-aio-ref/README.md)

Challenges:
- Inconsistent file create/edit output -- often rendered markdown 


## Learning

- Copilot Chat Agent/Editor support images but only with the 4o model at the moment. It will let you hash-tag any file in the repo and even highlight it, but it will ignore the file silently (and give you gibberish)
- One-shotting an entire diagram isn't realistic for anything slightly complicated -- expect to iterate with the tool and be hands-on with the process by breaking the system down into pieces. If possible, split the whiteboard into sparate pieces if there are clean boundaries between the parts and then stitch them together.
- Giving Copilot an example as a model for what it should generate saves some prompting and produces more predictable outputs
- Asking Copilot to summarize the architecture it sees in the diagram and correcting that through a few steps and then having it produce the diagram improves the quality of the first draft
- Clean up and simplify the source image where possible -- Copilot can over-index on nuance and unimportant pieces
- This approach is questionably helpful with "Marketecture" diagrams like the sample used above -- internal testing was done with real customer assets but not shared here -- for these it gave more concrete technical docs.
