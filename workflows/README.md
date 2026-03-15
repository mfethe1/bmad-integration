# Step-File Architecture

This directory contains the templates for breaking monolithic workflows into sequential micro-files.

## Critical Rules for Agents
1. **READ COMPLETELY**: Read the entire step file before acting.
2. **FOLLOW SEQUENCE**: Execute sections in order.
3. **WAIT FOR INPUT**: Halt at checkpoints and wait for human input before loading the next step.
4. **LOAD NEXT**: Only when directed, read fully and follow the next step file.
5. **NEVER** load multiple step files simultaneously.