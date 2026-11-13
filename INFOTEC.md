# rdgen - copia da Infotec

Origem: https://github.com/bryangerlach/rdgen (commit d26ae25, versao 1.5.1), licenca AGPL-3.0.

Alteracoes proprias:
- `.github/workflows/generator-windows.yml`: passo "Infotec - copia do exe no GitHub",
  que guarda o .exe compilado como artefato por 3 dias. O servidor compara esse arquivo
  com o que recebeu pelo `/save_custom_client` antes de publicar.
- `rdgenerator/api_views.py`: corrige bug do 1.5.1 que quebrava o `/api/generate`
  (tupla da versao com 3 valores: "too many values to unpack").

O rdgen roda em Docker no servidor `rmm` (/opt/rdgen). Detalhes em /opt/rdgen/LEIA-ME.txt.
- `rdgenerator/api_views.py`: `'version'` tinha uma tupla de 3 itens e a API /api/generate dava erro 500.
- `.github/workflows/generator-windows.yml`, passo removeNewVersionNotif: o sed original apagava
  do `let (request, url) =` ate o proximo `Ok(())` do arquivo; no RustDesk 1.5.1 isso levava
  1118 linhas do `src/common.rs` e a compilacao quebrava. Agora so o corpo de
  `do_check_software_update()` vira `Ok(())`.
- Mesmo workflow: `allowCustom.py`, `removeSetupServerTip.diff`, `xoffline.diff`,
  `allow_custom_update.py` e o `printer-adapter` (vira a DLL que vai junto com o cliente)
  eram baixados da master do bryangerlach/rdgen na hora da compilacao. Agora vem deste
  commit (checkout em ./temp-repo), entao so entra no .exe o que esta revisado aqui.

macOS (`.github/workflows/generator-macos.yml`, 13/11/2026). Conferido antes de compilar com uma
simulacao dos comandos sobre o codigo do RustDesk 1.5.1:
- Configuracao embutida (`custom_.txt`): o fluxo original gravava em `Contents/MacOS`, mas no macOS
  o RustDesk le de `Contents/Resources` (`load_custom_client` em `src/common.rs`). O app saia com
  nome e servidor certos, mas SEM o modo so-acesso e sem os ajustes travados. Agora vai em Resources.
- Identificador do app: no 1.5.1 o `PRODUCT_BUNDLE_IDENTIFIER` do projeto esta sem aspas e o sed
  original nao pegava; o app saia com o identificador do RustDesk oficial (`com.carriez.rustdesk`)
  e as permissoes do macOS dos dois se confundiam. Agora sai `com.<appname>.app`.
- Logo: era gravado numa pasta solta (`./build/.../RustDesk.app`) em vez do app compilado.
- removeNewVersionNotif e patches: as mesmas correcoes do Windows (perl no lugar do sed; patches
  deste commit, nao da master do original).
- Apple Silicon compila em `macos-15` (era `macos-14`, imagem em aposentadoria); limite de 150 min.
- Passos novos: "Infotec - conferencia do app antes de empacotar" (identificador, arquivo de
  configuracao, assinatura ad-hoc) e "Infotec - copia do dmg no GitHub" (artefato por 3 dias; o
  servidor compara com o que recebeu antes de publicar).
- Sem conta de desenvolvedor da Apple o app sai com assinatura ad-hoc (sem notarizacao): o macOS
  bloqueia se ele chegar baixado pelo navegador. A instalacao e feita pelo instalador da pagina
  de suporte (Terminal), que baixa o .dmg sem a marca de quarentena.
