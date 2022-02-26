# opensuse-zabbix-userparams: openSUSE system monitoring

## Usage

- Copy userparams file (```opensuse-userparams.conf```) to a directory included in your Zabbix Agent configuration file, e.g. ```/etc/zabbix/zabbix_agentd.d/``` or ```/etc/zabbix/zabbix_agent2.d/```
- Set userparams file owner and group (```chown root:zabbix opensuse-userparams.conf```)
- Set userparams file mode (```chmod 0440 opensuse-userparams.conf```)
- Copy corresponding sudoers file (```opensuse-userparams-sudoers```) to ```/etc/sudoers.d/``` directory
- Set sudoers file mode (```chmod 0440 /etc/sudoers.d/opensuse-userparams-sudoers```)
- Restart Zabbix Agent (```systemctl restart zabbix-agent.service``` or ```systemctl restart zabbix-agent2.service```)
- Import corresponding template file (```opensuse-userparams-template.yaml```) on Zabbix Server
- Link ```Template opensuse-userparams by Zabbix agent active``` template to selected hosts
- You may need to increase the ```Timeout``` value in your Zabbix Agent configuration file, e.g. ```Timeout=30```

## System requirements

- ```sudo``` package installed (installed by default on Leap 15.3) for some checks
- ```rebootmgr``` package installed and ```rebootmgr.service``` running for some checks

## Tested on

- Zabbix Agent and Zabbix Agent 2 on openSUSE Leap 15.3
- Zabbix Server 5.x

## Keys

- ```opensuse-userparams.userparams-version```

  Return version number of this file. Used internally within the template.

- ```opensuse-userparams.reboot.needed```

  Check if the reboot-needed flag was set by a previous update or install of a core library or service. This flag indicates that reboot is required to ensure that your system benefits from these updates.

  Expected return value: ```1``` - reboot is needed, ```2``` - reboot is not needed.

- ```opensuse-userparams.reboot.requested```

  Check if system reboot is requested from reboot manager.
  Requires rebootmgr and sudo.

  Expected return value: 1 - reboot requested, 2 - reboot not requested.

- ```opensuse-userparams.services.restart```

  Get the number of services which are using meanwhile deleted files (e.g. shared libraries). These services may need to be restarted after an update. Requires sudo.

  Expected return value: number indicating the number of services.

- ```opensuse-userparams.patches.all```

  Get the number of all applicable patches, including security patches. Requires sudo.

  Expected return value: number indicating the number of patches.

- ```opensuse-userparams.patches.security```

  Get the number of applicable security patches. Requires sudo.

  Expected return value: number indicating the number of security patches.

- ```opensuse-userparams.patches.critical```

  Get the number of applicable critical patches. Requires sudo.

  Expected return value: number indicating the number of critical patches.

- ```opensuse-userparams.patches.interactive```

  Get the number of applicable interactive patches. Patches are interactive when they need reboot, contain a message, or update a package whose license needs to be confirmed. Requires sudo.

  Expected return value: number indicating the number of interactive patches.

- ```opensuse-userparams.graceful-shutdown```

  Check if system was shut down gracefully before current boot.

  Expected return value: 1 - ungraceful shutdown, 2 - graceful shutdown.

## Template triggers

- Package patches are available: {ITEM.VALUE} patch[es]

- Security patches pending: {ITEM.VALUE} patch[es]

- Critical patches pending: {ITEM.VALUE} patch[es]

- Interactive patches pending: {ITEM.VALUE} patch[es]

- Some services need to be restarted: {ITEM.VALUE} service[s]

  There are running services which still use files and libraries deleted or updated by recent upgrades. They should be restarted to benefit from the latest updates.

- System reboot is needed

  The reboot-needed flag was set by a previous update or install of a core library or service. This flag indicates that reboot is required to ensure that your system benefits from these updates.

- System reboot is requested

  System reboot is requested from reboot manager.

- Ungraceful shutdown

  System was not shut down gracefully before current boot.
