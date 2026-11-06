# rdgen - copia privada da Infotec

Origem: https://github.com/bryangerlach/rdgen (commit d26ae25, versao 1.5.1), licenca AGPL-3.0.

Alteracao propria:
- `.github/workflows/generator-windows.yml`: passo "Infotec - copia do exe no GitHub",
  que guarda o .exe compilado como artefato por 3 dias. O servidor compara esse arquivo
  com o que recebeu pelo `/save_custom_client` antes de publicar.

O rdgen roda em Docker no servidor `rmm` (/opt/rdgen). Detalhes em /opt/rdgen/LEIA-ME.txt.
