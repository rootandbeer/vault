---
categories:
  - "[[Security]]"
type: Technique
domain: Offensive
topics:
  - "[[Wireshark]]"
status: Complete
platform: Network
severity:
cwe:
owasp:
tools:
  - Wireshark
created:
updated: 2025-07-16
---


## Setting SSLKEYLOGFILE Environment Variable

To decrypt HTTPS traffic in Wireshark, SSL keys need to be written to a file on the target machine that the attacker has access to:

### Windows

> [!example] Option 1: Command Prompt
> In the command prompt:
> ```shell
> setx SSLKEYLOGFILE filename /m
>  ```

> [!tldr] Option 2: GUI
> 1. Open **Advanced System Properties**:
>     - Press `Win + R`, type `sysdm.cpl`, and press `Enter`.
> 2. Go to **Advanced** Tab
> 3. Click **Environment Variables**.
> 4.  Under **User variables**, click **New**.
> 5. Set:
>     - **Variable name**: `SSLKEYLOGFILE`  
>     - **Variable value**: Full path to the log file you want to create, e.g. `C:\Users\Public\sslkeys.log`.
> 6. Click **OK** on all windows to apply.

To confirm the environmental variable has been set, open command prompt:
```shell
set
```

### macOS & Linux
Set the environment variable in your current shell:
```shell
echo "export SSLKEYLOGFILE=<filename>" >> ~/.bashrc
```
**OR**
```shell
echo "export SSLKEYLOGFILE=<filename>" >> ~/.bash_profile
```

Reload the profile:
```shell
source ~/.bashrc # or ~/.bash_profile
```

Verify the changes:
```shell
echo $SSLKEYLOGILE
```

## Configuring Wireshark
1. go to `edit` --> `preferences` --> `protocol` --> `TLS`
2. Select the file you created above for the `(Pre)-Master-Secret log filename`
3. Click `Apply` then `OK`
