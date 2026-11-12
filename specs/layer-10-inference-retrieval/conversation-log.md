# Layer 10 · Inference and Retrieval — Working Log

Owner: Irfan. Built from: Artifact 2 (Scope and Constraints) and Artifact 9 (Agent Design). Both are not written yet, so every answer here is provisional.

## Conversation Log

| Questions | Agent | Conversation link | Decision reached | Open unknowns and owner | Artifact section |
|---|---|---|---|---|---|
| Q10.1 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |
| Q10.2 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |
| Q10.3 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |
| Q10.4 (Section A) | C | (this session) | See "Decisions A" below | See "Open unknowns / assumptions A" below | 10.1 |

### Decisions A

- **Q10.1 Model that generates tokens:** A transformer-based, decoder-only, autoregressive LLM generates the tokens. It computes a probability for the next token, selects one, appends it to the input and repeats until a stop token or the max_tokens limit. The exact model and provider are decided in Q10.2 onward and in A2.
- **Q10.2 Model vs provider:** Treat the model (family, version, model ID) and the provider (who hosts and serves it, and its interface) as two separate things. The platform records both as separate fields, with the exact pinned model version, on every request and result. The same model can be served by different providers, and the same provider offers many models. The application uses its own internal model name that maps to the provider's model ID.
- **Q10.3 Delivery approach without GPU operations:** Primary choice: managed (hosted) inference. A provider runs the model on its own GPUs, and the platform calls it through an API and pays per token. The team does not operate GPUs, drivers or serving software. Fallback, only if a hosted API is not suitable (for example, if incident data may not leave our network): rent GPU servers from a provider and run the model-serving software ourselves. This fallback does not meet the "no GPU operations" condition, because the team operates the serving software. CPU-only inference is not considered for V1 because of speed and scale. The trade-offs (less control, data leaves our network, dependence on provider limits and outages) are handled in Q10.6 and Q10.7.
- **Q10.4 Self-hosted inference vs external provider:** With self-hosted inference, the organization runs the GPUs, the serving software, scaling, uptime and security patches itself. In return it gets full control: data stays inside its own network (data residency), it chooses and pins the exact model version, and it is not bound by a provider's rate limits or deprecation schedule. Costs are fixed and GPU-based, so self-hosting pays off only at high, steady volume; at low or uneven volume the GPUs sit idle and cost more. Only open-weight models can be self-hosted, so closed models such as Claude cannot be used, and quality may be lower. Small models for narrow tasks are easier and cheaper to self-host. With an external provider, the provider handles GPUs, scaling and uptime. We pay per token and start fast, but data leaves our network and we depend on the provider's limits, pricing and outages. The choice between the two is made in A2 and in Q10.6 and Q10.7.

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
| 10A-8 | Q10.4 | Self-hosting is considered only for open-weight models. Closed models cannot be self-hosted | Assumption | Irfan | A2 fixes the allowed models |
| 10A-9 | Q10.4 | Self-hosting costs less than an external provider when request volume is high and steady | Assumption | Irfan | A2 volume estimate and real GPU and provider price quotes |
| 10A-10 | Q10.4 | The team has, or can get, people to operate GPU serving (scaling, uptime, security patches) | Unknown | Team | A2 team and skills constraints |
| 10A-11 | Q10.4 | Open-weight models are good enough for incident analysis in V1 | Unknown | Team | Layer 12 evaluation results |

### Where Claude and ChatGPT disagreed
_(none yet — only Claude used so far; ChatGPT comparison and team agreement are still open)_

## Artifact 10 draft sections
_(fill after all sections A–E are done)_
