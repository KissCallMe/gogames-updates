# GoGames — atualizações públicas

Canal público de recursos do GoGames. O código-fonte principal permanece privado.

## Canal stable

O aplicativo consulta [manifest.json](manifest.json) em segundo plano ao iniciar. Sem itens publicados, não baixa nem modifica nada. Falhas de rede não impedem a abertura do GoGames.

## Publicação de arquivos

Adicione uma entrada a `resources` para cada arquivo permitido:

```json
{
  "path": "Assets/exemplo.txt",
  "url": "https://github.com/KissCallMe/gogames-updates/releases/download/resources-v1/exemplo.txt",
  "sha256": "COLOQUE_AQUI_O_SHA256_HEXADECIMAL_DE_64_CARACTERES",
  "size": 123
}
```

- `path` é relativo à pasta GoGamesData configurada pelo aplicativo. São aceitos `Assets/`, `Configs/`, `FakeRoms/` e os caminhos restritos de catálogo abaixo.
- `url` deve apontar para um arquivo deste repositório, no GitHub Releases ou em raw.githubusercontent.com.
- `sha256` é o SHA-256 real do arquivo e `size` seu tamanho exato em bytes.
- O GG valida tamanho e hash, limita cada arquivo a 50 MiB e evita sobrescrever arquivos alterados pelo usuário.
- Um arquivo existente só é atualizado se tiver sido instalado pelo próprio sincronizador e ainda corresponder ao hash anterior registrado.
- O GG salva o histórico e o último resultado em `GoGamesData/Diagnostics/`.

**Primeira versão:** SNES, PS1 e PS2. Este sincronizador não atualiza executáveis de emuladores, BIOS, saves nem ROMs comerciais. O suporte a pacotes de emuladores exige fluxo próprio com verificação e instalação segura. Não publique material sem direitos de redistribuição.

Nenhum GitHub Actions é necessário.

## Catálogo de jogos (novo)

O GoGames aceita, no mesmo `manifest.json` (`schema_version: 1`), arquivos adicionais do catálogo:

- `Games/<CONSOLE>/<JOGO>.zip.torrent` e `Games/<CONSOLE>/<JOGO>.utorrent`: FakeROMs de catálogo.
- `Games/<CONSOLE>/Torrent/<ARQUIVO>.torrent`: metadados de torrents verdadeiros.

O conteúdo de cada FakeROM permanece independente da seleção dos arquivos dentro do torrent real. O sincronizador **não baixa ROMs** nem modifica emuladores, saves ou BIOS. Ele verifica SHA-256 e tamanho, rejeita caminhos perigosos e preserva arquivos locais modificados. Arquivos com mais de 50 MiB são recusados.

Exemplo de entrada (valores fictícios, **não adicionar sem publicar o arquivo e calcular hash/tamanho reais**):

```json
{
  "path": "Games/WIIU/3Souls (USA).zip.torrent",
  "url": "https://raw.githubusercontent.com/KissCallMe/gogames-updates/main/catalog/WIIU/3Souls%20(USA).zip.torrent",
  "sha256": "<SHA-256 real de 64 caracteres>",
  "size": 123
}
```

### Procedimento de publicação

1. Hospedar os arquivos permitidos neste repositório ou nos Releases.
2. Calcular o SHA-256 e o tamanho em bytes de cada arquivo.
3. Adicionar os itens ao `resources` do manifesto; manter `schema_version: 1`.
4. Testar em instalação separada e solicitar nova varredura do catálogo para visualizar os jogos novos.

**Atualização de aplicativo:** já existe Tauri Updater no GoGames, com verificação manual em Configurações. Releases precisam conter instaladores e artefatos assinados corretamente para serem instaláveis.

**Atualização de emuladores:** ainda não está habilitada. Requer fluxo separado com confirmação, backup e recuperação, sem sobrescrever configurações do usuário.

Não publicar jogos ou arquivos de terceiros sem autorização de redistribuição.
