FROM debian:bullseye-20211011-slim
MAINTAINER ROBERTA SOLIMAN
USER root

RUN apt-get update && apt-get install -y \ 
    lynis \
    libpam-tmpdir \
    apt-listbugs \
    apt-listchanges \
    needrestart \
    debsecan \
    debsums \
    fail2ban \
    net-tools \
    busybox-syslogd \
    nano \
    rkhunter \
    procps \
    efibootmgr \
    ssh \
    acct \
    apt-transport-https \
    host \
    gpg \
    linux-image-5.10.0-8-amd64 \
    libc6-dev \
    gcc \
    puppet \
    aide \
    apparmor \
    apparmor-profiles \
    apparmor-utils \
    auditd 

RUN chmod o-rx /usr/bin/gcc
RUN chmod o-rx /usr/bin/as

RUN rm -rf /etc/ssh/sshd_config
RUN rm -rf /etc/profile 
RUN rm -rf /etc/lynis/default.prf

ADD sshd_config /etc/ssh/
ADD profile.prf /etc/lynis
ADD profile etc/

RUN cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
RUN /etc/init.d/ssh restart
RUN sysctl -a > /tmp/sysctl-defaults.conf 
RUN wget https://cisofy.com/files/cisofy-software.pub 
RUN gpg --import cisofy-software.pub 
RUN gpg --list-keys --fingerprint 
RUN host -t txt cisofy-software-key.cisofy.com
RUN mkdir /root/.ssh
RUN chmod 644 /var/run/sshd \
    /etc/crontab  \                                      
    /etc/group \                                    
    /etc/group- \                                      
    /etc/hosts.allow \                                    
    /etc/hosts.deny \                                    
    /etc/issue \                                          
    /etc/issue.net \                                      
    /etc/motd \                                          
    /etc/passwd \                                        
    /etc/passwd- \                                       
    /etc/ssh/sshd_config \                              
    /root/.ssh \                            
    /etc/cron.d \                             
    /etc/cron.daily \                           
    /etc/cron.hourly \                       
    /etc/cron.weekly \                          
    /etc/cron.monthly

RUN cp -v /usr/share/systemd/tmp.mount /etc/systemd/system/ 
RUN systemctl enable tmp.mount
RUN wget -O - https://packages.cisofy.com/keys/cisofy-software-public.key | apt-key add - \
    echo "deb https://packages.cisofy.com/community/lynis/deb/ stable main" | tee /etc/apt/sources.list.d/cisofy-lynis.list

RUN echo "deb http://deb.debian.org/debian buster-backports main" |  tee -a /etc/apt/sources.list 
