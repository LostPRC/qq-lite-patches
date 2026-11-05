# QQ Lite Patches

Morphe patches that turn the stock **QQ 9.3.75** (`com.tencent.mobileqq`, versionCode 16580, arm64-v8a)
into a slim build matching the `QQ_9.3.75_LITE.apk` reference build.

## ❓ About

The patch comes from diffing the original APK against the lite APK:

| Part | Result |
|---|---|
| `classes*.dex` | identical, so there are **no bytecode changes** |
| `resources.arsc`, `res/` | identical |
| assets + native libraries | **113 entries removed** (~88 MB) |
| `AndroidManifest.xml` | **41 `<application>` children removed** |

### How to use these patches

Add this repo to Morphe Manager: https://github.com/LostPRC/qq-lite-patches

<a href="https://morphe.software/add-source?github=LostPRC/qq-lite-patches">
  <img src=".github/assets/add-to-morphe.svg" alt="Add to Morphe" height="48">
</a>

## 🩹 Patches list

<!-- PATCHES_START EXPANDED -->

### Slim QQ (`resourcePatch`, on by default)

Each category can be turned off with a patch option:

| Option | Removes |
|---|---|
| `removeGameCenter` | Game center, YunGame cloud games, mini games (`assets/mini`, `assets/minigame`, WXA package), game widgets, WADL download services, game rooms |
| `removeZPlan` | Super QQ Show / ZPlan / Zootopia / CMShow (Apollo): UE4 + Filament assets, Wwise `libAk*.so` audio plugins, `libzplan*`, `libQQShow2D*`, GCloudVoice, UE4 manifest meta-data, `ApolloProvider` |
| `removeTelemetry` | Bugly, Matrix, RMonitor, Beacon, QIMEI, WeChat crash/backtrace and `librtc_node_report` native libraries |
| `removeMisc` | `UserLeadingActivity` (VIP ad guide), `KingkongPatch.apk`, `MidasPay.zip`, `live_shopping_manager.apk`, spring-festival red-packet assets, `libMMANDKSignature.so`, `com.android.vending.splits` meta-data |

The full list of removed entries and components is in
[`Removals.kt`](patches/src/main/kotlin/app/lostprc/patches/qq/lite/Removals.kt).

<!-- PATCHES_END -->

### 🛠️ Building locally

- Add a GitHub PAT with `read:packages` to `~/.gradle/gradle.properties` (`gpr.user` / `gpr.key`).
- Run `./gradlew buildAndroid`
- The built patches .mpp file is in `patches/build/libs/patches-*.mpp`
- Patch with [Morphe Desktop](https://github.com/MorpheApp/morphe-desktop):
  `java -jar morphe-desktop-all.jar patch --patches patches-*.mpp QQ_9.3.75.apk`

## ⚠️ Notes

- Removed features (games, Super QQ Show and so on) will fail or show errors when opened. That is expected.
- The telemetry native libraries are loaded by Java code that stays in the APK. The lite reference build
  ships without them as well, but if a specific device crashes at startup, re-patch with `removeTelemetry=false`.

## 📜 License

Licensed under the [GNU General Public License v3.0](LICENSE)
