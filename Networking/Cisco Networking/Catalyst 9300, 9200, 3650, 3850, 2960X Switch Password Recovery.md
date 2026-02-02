# Password‑recovery notes for Cisco Catalyst 9300/9200/3650/3850/2960X switches

This guide summarizes the procedure to reset a lost password on Cisco Catalyst 9k/3k/2960X switches.  The video shows how to interrupt the boot process, tell the switch to ignore its startup configuration, then create a new privileged account and restore the configuration.  These steps apply to Catalyst 9300 and 9200 models and also work for Catalyst 3650, 3850 and 2960X switches running current software. This process can also be used to factory reset a switch when the password isn't known, for instance when a used item has been purchased.

## High‑level overview

1. **Connect to the switch** – Attach a console cable to the switch and open a terminal session【21499060033818†screenshot】.
2. **Power cycle and interrupt boot** – Power off the switch, power it on and immediately press and hold the *mode* button.  This interrupts the boot process and drops you to the boot loader prompt (`switch:`)【884741778652360†screenshot】.  You must accept a brief outage to perform this procedure.
3. **Tell the boot loader to ignore the startup configuration** – At the `switch:` prompt, set the environment variable `SWITCH_IGNORE_STARTUP_CFG=1`.  This command (referred to in the video as “switch ignore startup CFG = 1”) instructs the switch to ignore its saved configuration when it boots.
4. **Boot the switch** – Type `boot` to continue the boot process.  The switch will start without loading its startup configuration.
5. **Enter privileged EXEC mode** – When the switch finishes booting, enter `enable` to reach privileged EXEC mode.  You can now examine the running configuration (for example, `show run | include username`) and verify that no working username/password combination exists.
6. **Restore the startup configuration** – Copy the startup configuration into the running configuration with `copy start run`.  This brings back all features and configuration settings.
7. **Create a new admin user** – Configure a new username and password so you can log in normally.  For example, create an admin account with full privileges and a secret password: `username admin privilege 15 secret <PASSWORD>.
8. **Stop ignoring the startup configuration** – After setting the new credentials, disable the ignore flag with `no system ignore startup config` (the video refers to this as “no system ignore”).  This reverses the earlier environment change so future boots load the configuration normally.
9. **Save and reboot** – Save the configuration (`write memory` or `copy run start`) and reload the switch.  The narrator recommends saving and then rebooting.  When the switch returns, you can log in with the username and password you created.

## Commands and their purpose

| Command (at prompt) | Purpose |
|---|---|
| `switch: SWITCH_IGNORE_STARTUP_CFG=1` | Tells the boot loader to ignore the existing startup configuration so you can recover access【154033567937894†screenshot】. |
| `switch: boot` | Boots the switch after setting the ignore flag【539755215015443†screenshot】. |
| `enable` | Enters privileged EXEC mode after the switch boots【539755215015443†screenshot】. |
| `copy start run` | Restores the startup configuration into RAM; this brings back all settings【909440100128804†screenshot】. |
| `username admin privilege 15 secret <password>` | Creates a new privileged administrative user with a known password【875141889904261†screenshot】. |
| `no system ignore startup config` | Removes the ignore flag so the switch will load its configuration normally on the next boot【533864028674331†screenshot】. |
| `write memory` or `copy run start` | Saves your changes to the startup configuration【709573300726252†screenshot】. |
| `reload` | Reboots the switch; afterwards you can log in using the credentials you created【709573300726252†screenshot】. |

## Tips

- Keeping a backup of your configuration prevents data loss when performing password recovery.
- Hold the mode button until you see the `switch:` prompt; releasing too early may allow the normal boot to continue.
- After logging in with the new credentials, consider setting up additional local users or configuring AAA (RADIUS/TACACS+) for authentication.
- The procedure applies to Catalyst 9300/9200 models and also works on Catalyst 3650, 3850 and 2960X switches running recent code.

