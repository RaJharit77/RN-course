

# Windows setup (PowerShell)

## create android home sdk

```powershell
New-Item -ItemType Directory -Force -Path "$Env:USERPROFILE\\Android","$Env:USERPROFILE\\Android\\Sdk"
```

## download cmd tools

Download `cmdline-tools` on https://developer.android.com/studio#command-line-tools-only  
Place the unpacked folder in `%USERPROFILE%\Android\Sdk`

## set up env

Run in PowerShell (sets user-level variables) then open a new shell:

```powershell
[Environment]::SetEnvironmentVariable("ANDROID_HOME", "$Env:USERPROFILE\\Android\\Sdk", "User")
[Environment]::SetEnvironmentVariable("ANDROID_SDK_ROOT", "$Env:USERPROFILE\\Android\\Sdk", "User")
[Environment]::SetEnvironmentVariable("Path", $Env:Path + ";$Env:USERPROFILE\\Android\\Sdk\\cmdline-tools\\bin", "User")
```

## Install utilities

```powershell
sdkmanager.bat --sdk_root=$Env:ANDROID_SDK_ROOT "platform-tools" "emulator" "build-tools;36.0.0"
```

## update env

Update the path and open a new shell:

```powershell
[Environment]::SetEnvironmentVariable("Path", $Env:Path + ";$Env:ANDROID_HOME\\cmdline-tools\\bin;$Env:ANDROID_HOME\\emulator;$Env:ANDROID_HOME\\platform-tools", "User")
```

## Download android image

```powershell
sdkmanager.bat --sdk_root=$Env:ANDROID_SDK_ROOT "system-images;android-35;google_apis;x86_64"
```

## Create AVD

```powershell
avdmanager.bat create avd -n Pixel35 -k "system-images;android-35;google_apis;x86_64"
```

## Update Pixel35 config

Edit `%USERPROFILE%\.android\avd\Pixel35.avd\config.ini`

```
hw.keyboard = yes
hw.lcd.density = 120
hw.lcd.vsync = 30
hw.ramSize=8G
image.sysdir.1=system-images/android-35/google_apis/x86_64/
vm.heapSize = 128M
```

## open emulator

```powershell
emulator -avd Pixel35
```

## update logcat size

```powershell
adb.exe logcat -G 10M
```

## update logcat timeout

```powershell
adb.exe shell settings put system screen_off_timeout 600000
```
