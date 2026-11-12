# Layer 10 · Inference and Retrieval — Working Log

Owner: Irfan. Built from: Artifact 2 (Scope and Constraints) and Artifact 9 (Agent Design). Both are not written yet, so every answer here is provisional.

## Conversation Log

| Questions | Agent | Conversation link | Decision reached | Open unknowns and owner | Artifact section |
|---|---|---|---|---|---|
| Q10.1 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |
| Q10.2 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |
| Q10.3 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |

### Decisions A

- **Q10.1 Model that generates tokens:** A transformer-based, decoder-only, autoregressive LLM generates the tokens. It computes a probability for the next token, selects one, appends it to the input and repeats until a stop token or the max_tokens limit. The exact model and provider are decided in Q10.2 onward and in A2.
- **Q10.2 Model vs provider:** Treat the model (family, version, model ID) and the provider (who hosts and serves it, and its interface) as two separate things. The platform records both as separate fields, with the exact pinned model version, on every request and result. The same model can be served by different providers, and the same provider offers many models. The application uses its own internal model name that maps to the provider's model ID.
- **Q10.3 Delivery approach without GPU operations:** Primary choice: managed (hosted) inference. A provider runs the model on its own GPUs, and the platform calls it through an API and pays per token. The team does not operate GPUs, drivers or serving software. Fallback, only if a hosted API is not suitable (for example, if incident data may not leave our network): rent GPU servers from a provider and run the model-serving software ourselves. This fallback does not meet the "no GPU operations" condition, because the team operates the serving software. CPU-only inference is not considered for V1 because of speed and scale. The trade-offs (less control, data leaves our network, dependence on provider limits and outages) are handled in Q10.6 and Q10.7.

### Open unknowns / assumptions A (all provisional)

| # | Question | Item | Type | Owner | Validate when |
|---|---|---|---|---|---|
| 10A-1 | Q10.1 | Every model the platform uses is a decoder-only transformer LLM | Assumption | Irfan | A2 fixes the allowed models |
| 10A-2 | Q10.1 | V1 uses ready-made models. The platform does not train a model | Assumption | Irfan | A2 scope and constraints |
| 10A-3 | Q10.1, Q10.2 | Which exact model and provider the platform will use | Unknown | Team | A2 and Q10.2 onward |
| 10A-4 | Q10.2 | Providers let us pin an exact model version, not only a moving alias | Assumption | Irfan | Q10.24 and A2 provider list |
| 10A-5 | Q10.2 | The same model has different ID formats on different providers | Unknown | Irfan | Adapter design, Q10.3 and Q10.4 |
| 10A-6 | Q10.3 | CPU-only inference cannot meet V1 speed and volume needs | Assumption | Irfan | A2 sets latency and volume limits |
| 10A-7 | Q10.3 | Sending incident data (logs, config) to an outside provider is allowed. If not, the GPU-rental fallback applies | Unknown | Team | A2 data and security constraints |

### Where Claude and ChatGPT disagreed
_(none yet — only Claude used so far; ChatGPT comparison and team agreement are still open)_

## Artifact 10 draft sections
_(fill after all sections A–E are done)_
