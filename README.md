# Zabbix Agent 2 – Debian 13

## Cíl projektu

Cílem projektu je vytvoření virtuálního stroje s operačním systémem Debian 13 pomocí nástroje Vagrant a automatická instalace a konfigurace Zabbix Agent 2 verze 8.0 LTS.

Agent je nastaven pro automatickou registraci na Zabbix serveru `enceladus.pfsense.cz`.

## Použité technologie

- Debian 13 (Trixie)
- Oracle VirtualBox
- Vagrant
- Zabbix Agent 2 – 8.0 LTS
- Bash
- Git a GitHub

## Struktura projektu

```text
├── Images/
├── .gitignore
├── LICENSE
├── README.md
├── Vagrantfile
├── configure-zabbix-agent.sh
└── install-zabbix-agent.sh
```

## Instalace a konfigurace

Instalace a konfigurace probíhá automaticky prostřednictvím provisioning skriptů připojených ve Vagrantfile.

### 1. Instalace Zabbix Agent 2

Skript `install-zabbix-agent.sh` provádí:

- Aktualizaci seznamu balíčků.
- Přidání oficiálního repozitáře Zabbix 8.0 pro Debian 13.
- Instalaci Zabbix Agent 2.
- Povolení služby při startu systému.

### 2. Konfigurace Zabbix Agent 2

Skript `configure-zabbix-agent.sh` nastavuje:

- Server: `enceladus.pfsense.cz`
- ServerActive: `enceladus.pfsense.cz`
- HostMetadata: `SPOS`
- Jedinečný název hosta (Hostname).

Jedinečný název hosta je vytvořen pomocí UUID a uložen tak, aby zůstal stejný i po restartu virtuálního stroje a opakovaném provisioningu.

## Spuštění projektu

V kořenovém adresáři projektu spusťte PowerShell a postupujte následovně:

1. Spuštění virtuálního stroje:

```powershell
vagrant up
```

2. Připojení k virtuálnímu stroji:

```powershell
vagrant ssh
```

3. Opětovné spuštění provisioningu:

```powershell
vagrant provision
```

## Ověření funkčnosti

Po dokončení instalace lze funkčnost ověřit přímo ve virtuálním stroji:

Kontrola verze agenta:

```bash
zabbix_agent2 -V
```

Kontrola stavu služby:

```bash
systemctl status zabbix-agent2
```

Kontrola konfigurace:

```bash
grep -E '^(Hostname|Server|ServerActive|HostMetadata)=' /etc/zabbix/zabbix_agent2.conf
```

Po automatické registraci je potřeba ověřit v rozhraní Zabbix serveru, že se host objevil, má přiřazenou správnou skupinu, štítek a šablonu pro Linux agent 2 a že ze zařízení přicházejí monitorovací data.

## Dokumentace výsledku

Ve složce `Images/` jsou umístěny screenshoty dokládající:

- Registraci virtuálního stroje v Zabbixu.
- Přiřazení monitorovací šablony.
- Příjem monitorovacích dat.

