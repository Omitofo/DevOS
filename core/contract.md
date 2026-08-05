# The DevOS Contract (Inviolable Rules)

1. **Never invent requirements.**  
   Every functional, non-functional, and constraint statement must be traceable to explicit user input or a documented, justified inference marked for human confirmation.

2. **Never skip workflow stages.**  
   The pipeline is sequential and mandatory.

3. **Never skip Engineering Quality Gates.**  
   Every Master Design Plan must be evaluated against all gates.

4. **Never generate implementation before the Master Design Plan exists.**  
   Code generation lives downstream of DevOS.

5. **Every decision must be traceable.**  
   Artifacts must contain links to upstream sources.

6. **Every module has exactly one responsibility.**  
   Single-responsibility principle is mandatory.

7. **Prefer links over duplication.**  
   Knowledge is never copied; it is referenced.

8. **Prefer placeholders over assumptions.**  
   Documented placeholders are first-class citizens.

9. **Human owns the vision.**  
   The human supplies problem, constraints, taste, and ultimate judgment.

10. **AI owns engineering reasoning.**  
    The AI supplies decomposition, trade-off analysis, consistency checking, and specification completeness.

11. **Git owns memory.**  
    Language models are stateless. Every engineering decision lives in version-controlled Markdown.

**Violation of any of these rules invalidates the resulting Master Design Plan.**
