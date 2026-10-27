# Proposta do lucerna-notes

O `lucerna-notes` é um bloco de notas em Rust. Funciona sozinho, com uma TUI, e ganha um painel no Lucerna.

Segue a estrutura de apps do [ecossistema](https://github.com/lucerna-desktop/lucerna-sdk/blob/main/docs/ecossistema.md): núcleo, serviço, TUI e painel em QML.

## Serviço

O serviço só sobe quando um cliente (o painel, a TUI ou a linha de comando) abre, e sai sozinho depois de um tempo sem clientes. Todos são clientes dele, então nenhum mexe nas notas direto.

Como ele sobe, no molde do ecossistema (spec do protocolo do `lucerna-sdk`):

1. **Com systemd:** o cliente conecta ao `notes.sock`, e a própria conexão sobe o serviço (ativação de socket do systemd do usuário). Se o serviço cair, o systemd o reinicia.
2. **Sem systemd** (ou sem a unidade instalada, ou em desenvolvimento): sem ninguém ouvindo no socket, o cliente roda `lucerna-notes serve` solto (num grupo de sessão próprio, com a saída num log em `$XDG_STATE_HOME`) e tenta conectar por até 2 s, com espera crescente. Se dois clientes fizerem isso juntos, o segundo `serve` não consegue ouvir e sai, e os dois conectam no primeiro. Depois de uma queda, quem sobe o serviço de novo é o próximo cliente.
3. **Sem conseguir em 2 s:** a linha de comando sai com o código 2 ("sem serviço") e diz o porquê; a TUI mostra a causa. As notas continuam sendo arquivos Markdown, abertos por qualquer editor.

Sem o shell, nada muda além de não haver painel: a TUI e a linha de comando fazem tudo.

## Painel e TUI

O painel serve para olhar e agir rápido. O trabalho completo fica na TUI.

**No painel:**

- captura rápida: digita, Enter, salvo;
- fixadas e recentes, com prévia de 2 linhas;
- busca no texto;
- abrir uma nota numa leitura simples, com as caixas de seleção marcáveis;
- um botão "editar" que abre a nota na TUI ou no editor.

**Só na TUI:**

- editar texto longo;
- organizar em pastas ou etiquetas;
- ligar notas entre si;
- importar e exportar.

## Armazenamento

Os dados ficam só neste computador, por enquanto. As pastas seguem as regras de "Pastas e dados" no [ecossistema](https://github.com/lucerna-desktop/lucerna-sdk/blob/main/docs/ecossistema.md).

- **Arquivos Markdown:** cada nota é um arquivo, numa pasta visível. O padrão é `~/Documentos/Notas`, configurável. As subpastas são a organização.
- **Metadados:** a nota fixada, as etiquetas e a data de criação ficam num bloco YAML pequeno no topo do arquivo (front matter).
- **Links e tarefas:** os links entre notas usam `[[nome da nota]]`. As caixas de seleção usam `- [ ]` e `- [x]`, que é o que o painel marca.
- **Índice de busca:** um SQLite em `~/.cache/lucerna/notes/`, só para buscar. Os arquivos são sempre a verdade: se o índice sumir ou corromper, o serviço o refaz.
- **Edição por fora:** o serviço observa a pasta, então uma edição feita fora (Neovim, VS Code) aparece na hora no painel e na TUI.

**Motivos:**

- sem o Lucerna, as notas continuam sendo arquivos que qualquer programa abre;
- dá para fazer backup com git ou cópia;
- uma sincronização futura (Syncthing ou git) não exige mudar o app.

## Futuro

- A sincronização entre aparelhos.

## Em aberto

Nada por enquanto.
