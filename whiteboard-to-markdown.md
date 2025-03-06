# Whiteboard Diagram -> Markdown

Goal: Take a diagram like we'd create in an ADS or other design session and create a Markdown document that includes all of the important details in that diagram, in a way that is readable by humans (for validation) and to AI (for context on coding and assitance).


## Prompts


```text
Look at #file:leak-detection-proposed-architecture-2.png   and write up a document with the structrue and relationships between the components in the diagram. Create a markdown file broken down by layer for the separate deployment locations, then subsystems. Create mermaid diagrams but layer them so we don't have one huge diagram, but maybe 2 or 3 layers with links between them.

Create a markdown file in /work 

Use only the info from the image. Ignore everything else in the repo for now, We are just documenting what's in the image. Only include what's in the image. Include everything in the image and nothing else.
```

Challenges:
- Inconsistent file create/edit output -- often rendered markdown 


## Learning

- Copilot Chat Agent/Editor support images but only with the 4o model at the moment. It will let you hash-tag any file in the repo and even highlight it, but it will ignore the file silently (and give you gibberish)

