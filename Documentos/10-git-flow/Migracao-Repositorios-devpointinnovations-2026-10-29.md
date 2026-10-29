# Migração para repositórios por área — devpointinnovations (29/10/2026)

**Status:** histórico migrado; ajustes de CI/CD em PR em cada repositório; passos de conta do PO pendentes (§5).
**Base:** `GabrielVogado/saude-monitor`, `develop` em `3bf45be` e `master` com as tags `v1.0.0-rc.1` a `v1.0.0-rc.10`.
**Pedido do PO (29/10/2026):** "migrar o projeto para o github devpointinnovations, onde cada setor terá seu repositório próprio, front end mobile, painel admin e back end". Decisões do PO: repositórios públicos, histórico inteiro preservado, commits antigos do Claude reescritos no nome do PO.

---

## 1. Para onde foi cada pasta

| Repositório | Veio de | Também levou |
|---|---|---|
| [`radar-saude-mobile`](https://github.com/devpointinnovations/radar-saude-mobile) | `frontend/` (vira a raiz) e os arquivos do app que ficaram na raiz entre 26/02 e 21/03/2026 | `deploy/android/`, `doc/assets/` (GIFs do README), o `README.md` de apresentação, `.github/` |
| [`painel-admin-frontend`](https://github.com/devpointinnovations/painel-admin-frontend) | `admin/` (vira a raiz) | `.github/` |
| [`radar-saude-backend`](https://github.com/devpointinnovations/radar-saude-backend) | `backend/` (vira a raiz) | `deploy/google/`, `deploy/oracle/` (histórico), `perf/`, os dados geoespaciais de origem (histórico), o `render.yaml` da raiz em `deploy/render/` (histórico), `.github/` |
| [`radar-saude-docs`](https://github.com/devpointinnovations/radar-saude-docs) (este) | `Documentos/` | `doc/tutorial-usuario/`, `site/` (Worker do saudemonitor.dev) |

Cada repositório leva o histórico de `.github/` inteiro; o PR de ajuste de cada um tira os workflows que não são dele.

Ficaram só no repositório antigo (`GabrielVogado/saude-monitor`, intacto, não arquivado):

- as ~200 branches de trabalho já mergeadas (foram levadas só `develop`, `master` e as tags rc);
- issues, PRs, histórico do Actions e as **Releases com os APKs das rc 1 a 10**;
- as pastas de configuração de agentes que entraram no git por engano em agosto e setembro, por regra do PO de 03/09/2026.

PRs citados em mensagens de commit e nos documentos (`#123`) são do repositório antigo.

## 2. Como o histórico foi separado

- `git filter-repo` por repositório, mantendo autor, data e mensagem de cada commit.
- **Mobile:** o app ficou na raiz do repositório até 19/03/2026 e conviveu com a cópia em `frontend/` até 21/03/2026. A regra junta as duas fases sem deixar a limpeza da raiz apagar os arquivos do app.
- **Backend:** o `render.yaml` da raiz (aposentado em 22/10, M-060) colidia com o `backend/render.yaml`; foi para `deploy/render/render.yaml` no histórico.
- **Autoria (decisão do PO):** os 154 commits com autor `Claude <noreply@anthropic.com>` passaram a `GabrielVogado <gabriel.vogado@gmail.com>`, e as linhas `Claude-Session`, `Co-Authored-By: Claude` e os links do claude.ai saíram de 174 mensagens. Datas, conteúdo e ordem não mudaram.
- **APKs em Git LFS (mobile):** três APKs antigos (`saude-monitor-preview.apk` e dois em `android/app/build/outputs/`) estavam no LFS. O ambiente da migração não consegue enviar objetos LFS e o GitHub recusa ponteiro sem objeto, então os três viraram arquivos comuns do git, com o mesmo conteúdo (`git lfs migrate export`). O maior tem 74 MB, abaixo do limite de 100 MB.
- Conferência: para cada commit, a árvore da pasta no monorepo é igual à árvore do repositório novo; `develop`, `master` e as 10 tags conferidas com `git ls-remote` depois do push.

## 3. O que muda no fluxo

- **CI:** cada repositório roda só o job da sua área (`Frontend (Expo Web)`, `Admin (Angular)`, `Backend (Spring Boot)`) e a trava de contagem de testes, agora com uma área só. Os rulesets exigem o check da área.
- **Homologação por repositório.** Antes, um merge na `master` gerava uma rc com o APK e o backend juntos. Agora:
  - **mobile:** `master` → APK HML → tag `vX.Y.Z-rc.N` (X.Y.Z do `app.json`) + Release com o APK;
  - **backend:** `master` → deploy `saude-monitor-backend-hom` → tag `vX.Y.Z-rc.N` (X.Y.Z do `version` do `build.gradle`, que passou a `1.0.0`) + Release sem anexo;
  - as duas numerações começam em `rc.11` e andam separadas;
  - quando o app depende de uma mudança da API, promover o **backend antes** do app. O APK continua apontando para a URL fixa de homologação.
  - o registro em [`homologacoes/`](./homologacoes/) passa a citar o par usado no teste (APK `rc.N` + backend `rc.M`).
- **Produção:** o `release.yml` de cada repositório cria a `release/<versão>` a partir da rc dele; no backend dispara o deploy de produção, no mobile o AAB.
- **SonarQube Cloud:** organização `devpointinnovations`, projetos `devpointinnovations_radar-saude-mobile`, `devpointinnovations_painel-admin-frontend` e `devpointinnovations_radar-saude-backend`. O histórico de análises da organização `gabrielvogado` não migra.
- **Google Cloud (WIF):** a federação aceitava só `GabrielVogado/saude-monitor`. Os dois comandos que liberam o repositório novo estão em `deploy/google/README.md` do radar-saude-backend.

## 4. PRs de ajuste

| Repositório | PR |
|---|---|
| painel-admin-frontend | [#1](https://github.com/devpointinnovations/painel-admin-frontend/pull/1) |
| radar-saude-backend | [#1](https://github.com/devpointinnovations/radar-saude-backend/pull/1) |
| radar-saude-mobile | [#1](https://github.com/devpointinnovations/radar-saude-mobile/pull/1) |
| radar-saude-docs | o PR deste documento |

## 5. Passos de conta do PO

1. Em cada repositório: branch padrão `develop` e apagar a `main` criada junto com ele.
2. Segredos do Actions: `SONAR_TOKEN` nos três repositórios de código; no mobile, `ANDROID_RELEASE_KEYSTORE_BASE64`, `ANDROID_RELEASE_STORE_PASSWORD`, `ANDROID_RELEASE_KEY_ALIAS`, `ANDROID_RELEASE_KEY_PASSWORD`, `ANDROID_KEYSTORE_BASE64`, `EXPO_PUBLIC_MAPBOX_TOKEN`, `EXPO_TOKEN` e a variável `EXPO_PUBLIC_API_BASE_URL`. Os valores não podem ser lidos do repositório antigo; vêm de onde foram guardados.
3. SonarQube Cloud: importar a conta devpointinnovations e criar os três projetos com as chaves do §3.
4. Google Cloud: rodar os dois comandos de `deploy/google/README.md` (radar-saude-backend) antes do primeiro deploy por lá.
5. Importar os rulesets de `.github/rulesets/` em cada repositório e ligar o Code Scanning.
6. Cópias locais: clonar os repositórios novos (o `D:\saude-monitor` continua apontando para o antigo).
7. Primeira homologação pelo fluxo novo (backend `rc.11`, depois APK `rc.11`) valida tudo de ponta a ponta.
