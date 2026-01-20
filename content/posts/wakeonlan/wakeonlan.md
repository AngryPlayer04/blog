+++
title = 'Ativando o Wake On Lan via systemctl'
date = 2026-01-19T23:03:03-03:00
draft = false
+++

No Linux, nem sempre o wake on lan fica ativado após um reboot, e para contornar esse problema, eu descobri que criando um serviço no systemctl, eu consigo deixar o Wake on Lan sempre ativado.
Para isso criei um arquivo chamado `wakeonlan.service` na pasta `/usr/lib/systemd/system/` contendo o seguinte:
```bash
[Unit]
Description=Enable Wake On Lan




[Service]
Type=oneshot
ExecStart=/usr/bin/nmcli connection modify 'nome-da-minha-conexao' 802-3-ethernet.wake-on-lan magic




[Install]
WantedBy=basic.target
```

Usei o nmcli, já que o ethool não é mais tanto recomendando para gerenciar as conexões de rede, e também porque só consegui fazer funcionar pelo nmcli.
Após salvar o arquivo, usei: 
```
sudo systemctl daemon-reload 
sudo systemctl enable wol.service
``` 
E então iniciei o serviço com 
```
sudo systemctl start wol
``` 
Para conferir se tudo está certo e depois reiniciei.
Depois de reiniciar, verifiquei com 
```
nmcli c show "nome-da-minha-conexao" | grep 802-3-ethernet.wake-on-lan
``` 
Embora logo depois que fiz login, apareceu uma notificação avisando que a rede havia sido desconectada, um sinal que funcionou, já que o nmcli desativa primeiro a conexão e depois ativa novamente.
Pronto, agora o Wake On Lan está ativado.
