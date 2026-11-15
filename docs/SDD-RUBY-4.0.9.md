# SDD — Padronizar todos os projetos Ruby em 4.0.9

**Data:** 2026-11-01 · **Status:** Executado (branches `chore/ruby-4.0.9` por repo, sem PR)

## Contexto
Os repositórios estavam em Ruby 3.3.0, 3.3.6, 3.4.9, 4.0.2, 4.0.7 e 4.0.8. Ruby 4.0.9 existe
(`ruby:4.0.9-slim` no Docker Hub; tarball em cache.ruby-lang.org).

## Decisões
- Alvo único: **4.0.9** (inclui o salto major dos apps em 3.x).
- Escopo: todos os repos Ruby, inclusive os com alterações locais.
- Cada repo ganha a branch `chore/ruby-4.0.9` a partir de `origin/<default>`, criada em
  worktree isolado, para não tocar no checkout nem no trabalho em andamento.
- Sem PR, sem merge; só commit convencional + push da branch.

## O que muda (por repo, em qualquer subpasta)
`.ruby-version`, `.tool-versions` (linha `ruby`), `Gemfile` (`ruby "..."`), `Gemfile.lock`
(`RUBY VERSION`), `Dockerfile*` (`ARG RUBY_VERSION`, `FROM ruby:`), workflows
(`ruby-version`, `RUBY_VERSION`, `image: ruby:`).

## Fora de escopo
Código em `legacy/` e `origem/`, `Dockerfile.backup`, `*.gemspec`, `.ruby-lsp/`,
worktrees auxiliares e o repo-pai `conexus-systems` (mesmo remoto do `conexus`).

## Critérios de aceite
- [ ] Nenhuma declaração de Ruby diferente de 4.0.9 nos arquivos acima.
- [ ] `bundle install` e testes passam com 4.0.9 (**não verificado**: 4.0.9 não está instalado no asdf local).
- [ ] Gems incompatíveis com o salto 3.x → 4.0 tratadas por repo (aucta_nexus, cv_ai_nexus, gov_nexus, workflow_nexus, horus_nexus).

## Riscos
- Salto major em apps 3.3/3.4: gems nativas e sintaxe podem quebrar.
- Imagens Docker/CI dependem de `ruby:4.0.9` publicado (confirmado `-slim`; conferir `-alpine`).
- `ruby "~> 3.3.0"` no Gemfile passa a `ruby "4.0.9"` exato.
