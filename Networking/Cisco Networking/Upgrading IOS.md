# Upgrade IOS version

1. Add the new IOS image to a TFTP server. You can run a free TFTP server from SolarWinds on any Windows device https://www.solarwinds.com/free-tools/free-tftp-server
2. Copy IOS image from server to router/switch. Input IP address of server and file name:

```ios
copy tftp: flash:
```

3. Verify file in flash storage:

```ios
#show flash
```

4. Specify the new image as the boot image and write config:

```ios
#(config)boot system flash:<file-name>.bin
#(config)exit
#write memory
#reload
```

5. Once device boots up, verify it booted from the new image. If so, delete the old image:

```ios
#delete flash:<old-image>.bin
```
