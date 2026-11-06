# rdgen - copia privada da Infotec

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
