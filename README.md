# expo-ws-tunnel-repro

Fresh `create-expo-app` (blank-typescript, SDK 58). No changes.

1. `npm install`
2. `npx expo start --tunnel --go`
3. Scan the QR code with Expo Go 58 on iOS -> "Gateway Timeout"
4. Open `exps://<sub>.on.expo.app` instead -> app loads

The CLI emits `exp://` (HTTP) although the ws tunnel URL is `https://`:
devserver:url {"url":"https://<sub>.on.expo.app/","runtimeUrl":"exp://<sub>.on.expo.app"}