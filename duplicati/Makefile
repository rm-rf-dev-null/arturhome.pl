# https://forum.arturhome.pl/t/kopia-zapasowa-proxmox/7607

DUPLICATI_VERSION=2.0.6.3-1


define banner
        @figlet -w 150 -f smmono9 $(1)
endef

.PHONY : install
install:
        @apt update -y
        @apt install -y figlet

        $(call banner,"Updating system")
        @apt full-upgrade -y

        $(call banner,"Installing base packages")
        @apt install -y gnupg curl debian-keyring debian-archive-keyring apt-transport-https

        $(call banner,"Installing caddy")
        @curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --batch --yes --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
        @curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' > /etc/apt/sources.list.d/caddy-stable.list
        @apt update -y
        @apt install -y caddy
        @echo -e ":80 {\n    reverse_proxy 127.0.0.1:8200\n}" > /etc/caddy/Caddyfile

        $(call banner,"Installing mono")
        @apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
        @echo "deb http://download.mono-project.com/repo/debian buster main" > /etc/apt/sources.list.d/mono-official.list
        @apt update -y
        @apt install -y mono-devel

        $(call banner,"Installing duplicati")
        @curl https://updates.duplicati.com/beta/duplicati_${DUPLICATI_VERSION}_all.deb --output duplicati.deb
        @apt install -y ./duplicati.deb
        @systemctl enable duplicati
        @echo 'DAEMON_OPTS="--webservice-interface=loopback --webservice-port=8200 --portable-mode"' > /etc/default/duplicati

        $(call banner,"Finished preparation")

        $(call banner,"Rebooting")
        @reboot
