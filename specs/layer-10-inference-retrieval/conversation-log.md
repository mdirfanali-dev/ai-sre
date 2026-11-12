# Layer 10 · Inference and Retrieval — Working Log

Owner: Irfan. Built from: Artifact 2 (Scope and Constraints) and Artifact 9 (Agent Design). Both are not written yet, so every answer here is provisional.

## Conversation Log

| Questions | Agent | Conversation link | Decision reached | Open unknowns and owner | Artifact section |
|---|---|---|---|---|---|
| Q10.1 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |

### Decisions A

- **Q10.1 Model that generates tokens:** A transformer-based, decoder-only, autoregressive LLM generates the tokens. It computes a probability for the next token, selects one, appends it to the input and repeats until a stop token or the max_tokens limit. The exact model and provider are decided in Q10.2 onward and in A2.

### Open unknowns / assumptions A (all provisional)

| # | Item | Type | Owner | Validate when |
|---|---|---|---|---|
| 10A-1 | Every model the platform uses is a decoder-only transformer LLM | Assumption | Irfan | A2 fixes the allowed models |
| 10A-2 | V1 uses ready-made models. The platform does not train a model | Assumption | Irfan | A2 scope and constraints |
| 10A-3 | Which exact model and provider the platform will use | Unknown | Team | A2 and Q10.2 onward |

### Where Claude and ChatGPT disagreed
_(none yet — only Claude used so far; ChatGPT comparison and team agreement are still open)_

## Artifact 10 draft sections
_(fill after all sections A–E are done)_
