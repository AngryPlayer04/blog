+++
title = 'Instalando o Makemkv'
date = 2025-05-20T20:24:28-03:00
draft = false
[cover]
  image = "/makemkv.png"  # Caminho corrigido (remova '/public')
  alt = "Logo do MakeMKV"
  width = 55           # Largura máxima em pixels
  height = 55
  responsiveImages = true
  hiddenInSingle = true

tags = ["video", "conversão", "tutorial"]

+++

Hoje resolvi instalar o [MakeMKV](https://www.makemkv.com/) pra extrair os arquivos dos dvds que tenho aqui em casa, lembrando que esse processo é totalmente legal desde que seja para uso pessoal, que é o meu caso.

Para isso segui um tutorial em inglês do [Reddit](https://www.reddit.com/r/truenas/comments/1hlrtvp/guide_for_makemkv_installation_on_truenas_scale/) de instalação no TrueNas.

Como já tenho certo conhecimento em docker, não tive dificuldade em seguir o passo a passo.

A minha configuração ficou assim(ocultei o caminho das minhas pastas), removi os dispositivos que não irei precisar, pois o meu leitor se encontra em `/dev/sr0` e `/dev/sg1`.

```yaml

version: '3'
services:
  makemkv:
    image: jlesage/makemkv
    ports:
      - "5800:5800"
    volumes:
      - "/mnt/XXXXXXX/makemkv:/config:rw"
      - "/mnt/XXXXXXX/makemkv/storage:/storage:ro"
      - "/mnt/XXXXXXX/makemkv:/output:rw"
    devices:
      - "/dev/sr0:/dev/sr0"
      - "/dev/sg1:/dev/sg1"

    environment:
      - "MAKEMKV_KEY=BETA"
      - "DARK_MODE=1"
      - "USER_ID=568"
      - "GROUP_ID=568"

```

![](/1.png)

Após iniciar o container e ir para o endereço no qual o makemkv está exposto, recebo esta tela:

![](/2.png)

Ao que parece, está tudo ok!

Agora é inserir um disco no leitor e ver se será reconhecido.

Inseri um disco da segunda temporada de Doctor Who, uma das minhas séries favoritas.

![](/3.png)

O disco foi reconhecido, agora é só clicar no ícone do leitor e esperar o programa ler os dados.

![](/4.png)

Após isso, o programa vai mostrar quais arquivos foram encontrados, e me dar a opção de escolher quais deles eu quero extrair e qual opção de áudio e legenda.

![](/5.png)

Como já está tudo do jeito que quero, é só clicar no botão `Make MKV` e esperar. Isso pode demorar alguns minutos (ou mais  se for um bluray).
![](/6.png)

13 minutos depois:
![](/7.png)

Os arquivos foram diretamente para a pasta compartilhada.
![](/8.png)

Agora é só renomear e mover para a pasta de séries, e depois assistir no jellyfin.
