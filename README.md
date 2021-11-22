# Lynis Debian com Dockerfile

Foi necessário instalar as seguintes ferramentas:

> apt-listbugs: Ferramenta que lista bugs críticos antes de cada instalação apt.

>libpam-tmpdir: Diretórios temporários por usuários automáticos.

> apt-listchanges: Ferramenta de notificação do histórico de mudanças em pacotes.

>needrestart: Verifica quais daemons precisam ser reiniciados após as atualizações da biblioteca.

>debsecan: Analisador de segurança do debian.

>debsums:  Ferramenta para verificação de arquivos instalados por pacotes contra somas de verificação md5.

> fail2ban: Proíbe o acesso de máquinas que causam diversos erros no momento da autenticação.

> net-tools: Conjunto de ferramentas para rede net-3

>busybox-syslogd: Oferece syslogd usando busybox.

>rkhunter: Rootkit, backdoor, sniffer e escaneador de vulnerabilidades.

>procps: Utilitários para o sistema de arquivos /proc.

>efibootmgr: Interage com o gerenciador de inicialização.

> ssh: Servidor e cliente "secure shell".

>acct: Utilitários de contabilidade gnu para contabilidade de processo e login.

>linux-image-5.10.0-8-amd64 (kernel): Linux 5.10 para pcs de 64 bits (assinado).

>libc6-dev: Biblioteca c gnu (arquivos de desenvolvimento).

## Personalização de perfis de digitalização do Lynis:
Para agrupar testes, permitindo que você habilite ou desabilite os testes, defina o escopo dos testes e mais, o Lynis tem perfis de verificação armazenados no diretório / etc/lynis. Os perfis de verificação têm várias configurações, fornecendo uma maneira prática de gerenciar testes semelhantes em massa, em vez de se preocupar em gerenciá-los um de cada vez. Fiz a criação do arquivo profile.prf e fiz as seguintes mudanças:
>	skip-test=KRNL-5830
Reboot of system is most likely needed [KRNL-5830]
Não é necessário dar reboot.

>skip-test=LOGG-2138
klogd is not running, which could lead to missing kernel messages in log files [LOGG-2138]
O Metalog vem com seu próprio logger de kernel, portanto, não há necessidade de executar o klogd.

## Umask padrão em /etc/profile pode ser mais restrito:
O padrão o umask é 002 em /etc /profile, então atualizei para 027.

## Endurecer a configuração SSH:
Adicionei as mudanças no arquivo etc/ssh/sshd_config
> PermitRootLogin without-password
X11Forwarding no
AllowAgentForwarding no
UseDNS yes

## Como tem o GnuPG instalado, foi necessário baixar a chave pública da Lynis.
$ wget https://cisofy.com/files/cisofy-software.pub 
$ gpg --import cisofy-software.pub 
$ gpg --list-keys --fingerprint
/home/user/.gnupg/pubring.gpg

## Ativar o tmp.mount.
Quando habilitado, esse armazenamento temporário aparece como um sistema de arquivos montado, mas armazena seu conteúdo na memória volátil em vez de em um dispositivo de armazenamento persistente. ... E ao usar isso, nenhum arquivo em /tmp é armazenado no disco rígido, exceto quando a memória está baixa, caso em que o espaço de troca é usado.
