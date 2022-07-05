# Android

## Add ADB to path

```shell
# Linux
export PATH=${PATH}:~/Android/Sdk/platform-tools/

# macOS
export PATH=${PATH}:~/Library/Android/sdk/platform-tools/
```

## Check ADB public key fingerprint

```shell
adb pubkey ~/.android/adbkey | awk '{ print $1 }' | openssl base64 -A -a -d | openssl md5 -c | tr a-z A-Z
```

## List connected devices

```shell
# List devices
adb devices
```

## Interract with device

```shell
# Push file to device
adb push <FILE> /data/local/tmp

# Access to device shell
adb shell

# Run command remotely
adb shell <COMMAND>
```
