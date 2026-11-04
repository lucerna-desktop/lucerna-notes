<p align="center"><img src="docs/assets/logo.svg" alt="Lucerna" height="120"></p>

# lucerna-notes

Bloco de notas em Rust, com TUI e painel no Lucerna.

**Situação:** em documentação; ainda sem código.

## Ligação com o Lucerna

Faz parte do ecossistema do [Lucerna](https://github.com/lucerna-desktop/lucerna-shell), o shell de desktop para o Hyprland. O shell cuida só da interface; a lógica fica em Rust, neste e nos outros repositórios do ecossistema (veja [`docs/ecossistema.md`](https://github.com/lucerna-desktop/lucerna-sdk/blob/main/docs/ecossistema.md) no `lucerna-sdk`).

## Documentação

Veja a pasta [`docs/`](docs/):

- [`docs/proposta.md`](docs/proposta.md)

O que falta fazer está em [`lucerna-notes — Pendências.md`](<lucerna-notes — Pendências.md>).

## Repositório

A branch `main` é protegida por um ruleset igual ao do `lucerna-shell`: PR obrigatório, conversas resolvidas, só merge commit, sem deleção nem force push. Os checks obrigatórios entram quando houver CI.

## Licença

[GPL-3.0](LICENSE).
