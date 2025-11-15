# Service Registry

## Suite Orchestrator
- **word-hero-agent** (`https://github.com/sanmu2018/word-hero-agent`): top-level product manager and architect that gathers requirements, defines API contracts, and assigns work to downstream services.

## Delivery Services
| Name | Service | Role |
| --- | --- | --- |
| word-hero-api | https://github.com/sanmu2018/word-hero-api | Curates and governs the shared API contracts used by backend and web clients, ensuring every endpoint is documented and versioned consistently. |
| word-hero-web | https://github.com/sanmu2018/word-hero-web | Delivers the web UI tasks provided by the agent and consumes the published APIs. |
| word-hero | https://github.com/sanmu2018/word-hero | Implements backend logic per agent specs and reports status upstream. |
