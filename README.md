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

- `path` é relativo a `%USERPROFILE%\\GoGamesData` e deve começar com `Assets/`, `Configs/` ou `FakeRoms/`.
- `url` deve apontar para um arquivo deste repositório, no GitHub Releases ou em raw.githubusercontent.com.
- `sha256` é o SHA-256 real do arquivo e `size` seu tamanho exato em bytes.
- O GG valida tamanho e hash, limita cada arquivo a 50 MiB e evita sobrescrever arquivos alterados pelo usuário.
- Um arquivo existente só é atualizado se tiver sido instalado pelo próprio sincronizador e ainda corresponder ao hash anterior registrado.
- O GG salva o histórico e o último resultado em `GoGamesData/Diagnostics/`.

**Primeira versão:** SNES, PS1 e PS2. Este sincronizador não atualiza executáveis de emuladores, BIOS, saves nem ROMs comerciais. O suporte a pacotes de emuladores exige fluxo próprio com verificação e instalação segura. Não publique material sem direitos de redistribuição.

Nenhum GitHub Actions é necessário.
