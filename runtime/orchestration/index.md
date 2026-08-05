# runtime/orchestration/ — Index

Control plane of the DevOS runtime.

| File | Purpose | Status |
|------|---------|--------|
| [README.md](README.md) | Domain overview & design notes | Active |
| [pipeline-driver.md](pipeline-driver.md) | Invocation loop, state detection, single-stage contract | Active |
| [agent-loader.md](agent-loader.md) | State → agent mapping and load protocol | Active |
| [transition-enforcer.md](transition-enforcer.md) | Precondition & blocking-rule enforcement | Active |
| [human-gates.md](human-gates.md) | Checkpoint presentation and clearance protocol | Active |

**Load order for any new session:**  
pipeline-driver.md → agent-loader.md → transition-enforcer.md → human-gates.md.
