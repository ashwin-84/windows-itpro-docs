---
title: Resources for Microsoft Defender ATP for Mac
description: Resources for Microsoft Defender ATP for Mac, including how to uninstall it, how to collect diagnostic logs, CLI commands, and known issues with the product.
keywords: microsoft, defender, atp, mac, installation, deploy, uninstallation, intune, jamf, macos, catalina, mojave, high sierra
search.product: eADQiWindows 10XVcnh
search.appverid: met150
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
ms.author: dansimp
author: dansimp
ms.localizationpriority: medium
manager: dansimp
audience: ITPro
ms.collection: M365-security-compliance
ms.topic: conceptual
---

# Resources for Microsoft Defender ATP for Mac

**Applies to:**

- [Microsoft Defender Advanced Threat Protection (Microsoft Defender ATP) for Mac](microsoft-defender-atp-mac.md)

## Collecting diagnostic information

If you can reproduce a problem, increase the logging level, run the system for some time, and restore the logging level to the default.

1. Increase logging level:

   ```bash
   mdatp --log-level verbose
   ```

   ```Output
   Creating connection to daemon
   Connection established
   Operation succeeded
   ```

2. Reproduce the problem

3. Run `sudo mdatp --diagnostic --create` to back up Microsoft Defender ATP's logs. The files will be stored inside a .zip archive. This command will also print out the file path to the backup after the operation succeeds.

   ```bash
   sudo mdatp --diagnostic --create
   ```
   ```Output
   Creating connection to daemon
   Connection established
   ```

4. Restore logging level:

   ```bash
   mdatp --log-level info
   ```
   ```Output
   Creating connection to daemon
   Connection established
   Operation succeeded
   ```

## Logging installation issues

If an error occurs during installation, the installer will only report a general failure.

The detailed log will be saved to `/Library/Logs/Microsoft/mdatp/install.log`. If you experience issues during installation, send us this file so we can help diagnose the cause.

## Uninstalling

There are several ways to uninstall Microsoft Defender ATP for Mac. Note that while centrally managed uninstall is available on JAMF, it is not yet available for Microsoft Intune.

### Interactive uninstallation

- Open **Finder > Applications**. Right click on **Microsoft Defender ATP > Move to Trash**.

### From the command line

- ```sudo rm -rf '/Applications/Microsoft Defender ATP.app'```
- ```sudo rm -rf '/Library/Application Support/Microsoft/Defender/'```

## Configuring from the command line

Important tasks, such as controlling product settings and triggering on-demand scans, can be done from the command line:

|Group        |Scenario                                   |Command                                                                |
|-------------|-------------------------------------------|-----------------------------------------------------------------------|
|Configuration|Turn on/off real-time protection           |`mdatp --config realTimeProtectionEnabled [true/false]`                |
|Configuration|Turn on/off cloud protection               |`mdatp --config cloudEnabled [true/false]`                             |
|Configuration|Turn on/off product diagnostics            |`mdatp --config cloudDiagnosticEnabled [true/false]`                   |
|Configuration|Turn on/off automatic sample submission    |`mdatp --config cloudAutomaticSampleSubmission [true/false]`           |
|Configuration|Add a threat name to the allowed list      |`mdatp threat allowed add --name [threat-name]`                        |
|Configuration|Remove a threat name from the allowed list |`mdatp threat allowed remove --name [threat-name]`                     |
|Configuration|List all allowed threat names              |`mdatp threat allowed list`                                            |
|Configuration|Turn on PUA protection                     |`mdatp --threat --type-handling potentially_unwanted_application block`|
|Configuration|Turn off PUA protection                    |`mdatp --threat --type-handling potentially_unwanted_application off`  |
|Configuration|Turn on audit mode for PUA protection      |`mdatp --threat --type-handling potentially_unwanted_application audit`|
|Configuration|Turn on/off passiveMode                    |`mdatp --config passiveMode [on/off]`                                  |
|Diagnostics  |Change the log level                       |`mdatp --log-level [error/warning/info/verbose]`                       |
|Diagnostics  |Generate diagnostic logs                   |`mdatp --diagnostic --create`                                          |
|Health       |Check the product's health                 |`mdatp --health`                                                       |
|Protection   |Scan a path                                |`mdatp --scan --path [path]`                                           |
|Protection   |Do a quick scan                            |`mdatp --scan --quick`                                                 |
|Protection   |Do a full scan                             |`mdatp --scan --full`                                                  |
|Protection   |Cancel an ongoing on-demand scan           |`mdatp --scan --cancel`                                                |
|Protection   |Request a security intelligence update     |`mdatp --definition-update`                                            |
|EDR          |Turn on/off EDR preview for Mac            |`mdatp --edr --early-preview [true/false]` OR `mdatp --edr --earlyPreview [true/false]` for versions earlier than 100.78.0                                |
|EDR          |Add group tag to device. EDR tags are used for managing device groups. For more information, please visit https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/machine-groups |`mdatp --edr --set-tag GROUP [name]` |
|EDR          |Remove group tag from device              |`mdatp --edr --remove-tag [name]`                                            |

### How to enable autocompletion

To enable autocompletion in `Bash`, run the following command and restart the Terminal session:

```bash
echo "source /Applications/Microsoft\ Defender\ ATP.app/Contents/Resources/Tools/mdatp_completion.bash" >> ~/.bash_profile
```

To enable autocompletion in `zsh`:

- Check whether autocompletion is enabled on your device:

   ```zsh
   cat ~/.zshrc | grep autoload
   ```

- If the above command does not produce any output, you can enable autocompletion using the following command:

   ```zsh
   echo "autoload -Uz compinit && compinit" >> ~/.zshrc
   ```

- Run the following commands to enable autocompletion for Microsoft Defender ATP for Mac and restart the Terminal session:

   ```zsh
   sudo mkdir -p /usr/local/share/zsh/site-functions
   ```
   ```zsh
   sudo ln -svf "/Applications/Microsoft Defender ATP.app/Contents/Resources/Tools/mdatp_completion.zsh" /usr/local/share/zsh/site-functions/_mdatp
   ```

## Client Microsoft Defender ATP quarantine directory

`/Library/Application Support/Microsoft/Defender/quarantine/` contains the files quarantined by `mdatp`. The files are named after the threat trackingId. The current trackingIds is shown with `mdatp --threat --list --pretty`.

## Microsoft Defender ATP portal information

[This blog](https://techcommunity.microsoft.com/t5/microsoft-defender-atp/edr-capabilities-for-macos-have-now-arrived/ba-p/1047801) provides detailed guidance on what to expect in Microsoft Defender ATP Security Center.
