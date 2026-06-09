# Chapter 5 - Transforming Programming

## Key Ideas

- Think of your program not as a series of instructions, but as a pipeline that transforms data
- Programs are data transformers whose job is to transform data input into data output
- A pipeline example where each step takes data in, transforms it, and passes it on:
  ```bash
  cat log.txt | grep "ERROR" | sort | uniq -c
  ```
- **Pipelines over nested logic:** chain small, focused transformations instead of writing deep nested logic or long methods — each step does one thing and passes the result forward
- **Transformations make state explicit:** data goes in and comes out, it does not hide state inside objects
- In a pipeline, each step is independently testable
- **Error handling fits naturally:** if one step fails, the failure travels through the rest of the pipeline without intermediate steps needing to handle it explicitly
- **It's a mindset, not a framework:** you can apply this in Java, TypeScript, C#, or any language you already use