# expo-ws-tunnel-repro

Minimal repro for the SDK 58 ws tunnel issue (re-filing of expo/expo#52352).
Fresh `create-expo-app` (blank-typescript, SDK 58), no changes.

## Steps

1. `npm install`
2. `npx expo start --tunnel --go`
3. Scan the QR code with Expo Go 58 on iOS → "Something went wrong" / "Gateway Timeout"
4. Open `exps://<sub>.on.expo.app` manually instead → the app loads

## Cause

The ws tunnel URL is `https://`, but the CLI builds the Expo Go runtime URL with `exp://` (plain HTTP):

```
devserver:url {"url":"https://<sub>.on.expo.app/","runtimeUrl":"exp://<sub>.on.expo.app","hostType":"tunnel"}
```

## See it in the debug logs

PowerShell:

```powershell
$env:DEBUG="expo:*"; npx expo start --tunnel --go 2>&1 | Select-String "devserver:url|tunnel:url|tunnel:done|loading_url"
```

macOS / Linux:

```bash
DEBUG="expo:*" npx expo start --tunnel --go 2>&1 | grep -E "devserver:url|tunnel:url|tunnel:done|loading_url"
```

Note: piping disables the interactive CLI (no QR code or shortcuts), so open the URL on the device manually.

## Environment

- Windows 11, @expo/cli 58.1.15, expo ^58.0.0, react-native 0.88.0
- Expo Go 58 on iOS
