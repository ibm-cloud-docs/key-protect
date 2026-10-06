---

copyright:
  years: 2026

lastupdated: "2026-10-06"

keywords: smart card troubleshooting, PPD troubleshooting, PIN pad, smartcard reader not detected

subcollection: key-protect

content-type: troubleshoot

---



{{site.data.keyword.attribute-definition-list}}

# Troubleshooting smart card issues for {{site.data.keyword.keymanagementserviceshort}} Dedicated
{: #kp-smartcard-troubleshooting}

Use this topic to diagnose and resolve common issues when using smart cards with {{site.data.keyword.keymanagementserviceshort}} Dedicated.
{: shortdesc}

## Why is my smart card reader not detected?
{: #kp-smartcard-troubleshooting-reader-not-detected}
{: troubleshoot}

A command reports that the smart card reader is not connected even though the reader is physically attached to the system.
{: tsSymptoms}

The USB reader might not be properly recognized by the operating system, the operating system version might not be supported, or on Windows, the device driver might not be installed.
{: tsCauses}

1. Disconnect the USB cable from the smart card reader, reconnect it, and retry the command.
2. Verify that your operating system and architecture are listed in the [Supported platforms for smart card operations](/docs/key-protect?topic=key-protect-smartcard#kp-ppd-supported-platforms).

   For example, macOS is supported up to macOS Tahoe, and Windows ARM64 is not supported.
   {: note}

3. On Windows systems, verify that the CyberJack One driver is installed. If the driver is not installed, see [Installing the CyberJack One driver on Windows](/docs/key-protect?topic=key-protect-smartcard#smartcard-windows-driver).
{: tsResolve}

## Why does PPD fail to start with a port conflict?
{: #kp-smartcard-troubleshooting-ppd-port}
{: troubleshoot}

PPD fails to start or returns an error when you try to run it.
{: tsSymptoms}

The configured port is already in use — either by a previously running PPD process or another application.
{: tsCauses}

Complete the following steps to identify and stop the process that is occupying the port, and then restart PPD.
{: tsResolve}

1. Check which process is occupying the PPD port (default `6070`):

   For [Linux]{: tag-linux} and [macOS]{: tag-macos}:
   ```sh
   lsof -i :<PORT>
   ```
   {: pre}

   For [Windows]{: tag-windows}:
   ```sh
   netstat -ano | findstr :<PORT>
   ```
   {: pre}

   Where `<PORT>` is the PPD port number (default `6070`).

2. Kill the process occupying the port:

   For [Linux]{: tag-linux} and [macOS]{: tag-macos}:
   ```sh
   kill <PID>
   ```
   {: pre}

   For [Windows]{: tag-windows}:
   ```sh
   taskkill /PID <PID> /F
   ```
   {: pre}

   Where `<PID>` is the process ID returned in the previous step.

3. Retry starting PPD.

4. If PPD still fails to start, change the port number in `ppd.cfg` to a free port, then rerun PPD. Update the `port` field in your smart card JSON config file to match the new port.

## Why can't I connect to a remote PPD?
{: #kp-smartcard-troubleshooting-ppd-connect}
{: troubleshoot}

A command cannot reach a remote PPD instance.
{: tsSymptoms}

The PPD port might not be reachable from your system due to PPD not running, SSH not being enabled, or a firewall blocking the port.
{: tsCauses}

Verify that the PPD port is reachable from your system before running smart card commands.
{: tsResolve}

For [Linux]{: tag-linux} and [macOS]{: tag-macos}:

```sh
nc -zv <PPD_HOST> <PPD_PORT>
```
{: pre}

Where:

* `<PPD_HOST>` is the IP address or hostname of the system where PPD is running.
* `<PPD_PORT>` is the port PPD is listening on (default `6070`).

A successful response confirms the port is open. If the connection fails, verify that PPD is running on the remote system, that SSH is enabled, and that no firewall is blocking the port.
