# Radar Saúde — Documentação

Documentação de produto e engenharia do Radar Saúde, o tutorial de uso e o site público
[saudemonitor.dev](https://saudemonitor.dev).

| Pasta | Conteúdo |
|---|---|
| [`Documentos/`](Documentos/00-INDICE.md) | Negócio, arquitetura, contratos da API, backlog, ADRs, histórico de melhorias, git-flow e registros de homologação. Comece pelo [índice](Documentos/00-INDICE.md). |
| [`doc/tutorial-usuario/`](doc/tutorial-usuario/) | Tutorial de primeiros passos no app, com imagens |
| [`site/`](site/README.md) | Site público (Cloudflare Workers): política de privacidade, termos e exclusão de conta |

## Repositórios do projeto

| Repositório | Conteúdo |
|---|---|
| [radar-saude-mobile](https://github.com/devpointinnovations/radar-saude-mobile) | App Expo / React Native |
| [painel-admin-frontend](https://github.com/devpointinnovations/painel-admin-frontend) | Painel Admin Angular |
| [radar-saude-backend](https://github.com/devpointinnovations/radar-saude-backend) | API Spring Boot, deploy no Cloud Run, testes de desempenho |
| **radar-saude-docs** (este) | Documentação, tutorial e site |

Até 29/10/2026 tudo ficava no monorepo
[GabrielVogado/saude-monitor](https://github.com/GabrielVogado/saude-monitor). O histórico foi
preservado em cada repositório; PRs `#N` citados nos documentos são daquele repositório, e
caminhos como `frontend/`, `admin/` e `backend/` correspondem à raiz do repositório da área.
Detalhes em [`Documentos/10-git-flow/Migracao-Repositorios-devpointinnovations-2026-10-29.md`](Documentos/10-git-flow/Migracao-Repositorios-devpointinnovations-2026-10-29.md).
