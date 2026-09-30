# Device tree for the Redmi Pad Pro (ruan), Axion based (Android 16)

Redmi Pad Pro 5G, codename `ruan`, SM7435 ("parrot"). Not for the wifi model (`dizi`).

## Building

1. `repo init -u https://github.com/AxionAOSP/android.git -b lineage-23.2 --git-lfs`
2. search and duck the ruan manifest.
3. `repo sync`
4. Apply the platform patches in `patches/<project path>/` with `git am` in each project.
5. Build:
   ```
   source build/envsetup.sh && lunch ...then ax -b based on ur choice
   ```

## Platform patches

| Project | Patch |
|---|---|
| `system/core` | init: keep `/data/resource-cache` on upgrade |
| `vendor/gms` | CrossDeviceAccessServicePrimary uses-library match |
| `vendor/lineage` | kernel out dir prefix for any relative `OUT_DIR` (release builds) |
| `frameworks/native` | RenderEngine: realtime Vulkan queue priority |
| `build/make` | releasetools: `PartitionMapFromTargetFiles` accepts a ZipFile (signing) |

## Kernel

`device/xiaomi/dizi-kernel` holds the kernel artefacts:
- the GKI Image built from LineageOS `android_kernel_xiaomi_sm7435` (5.10.269);
- the stock OS3.0.303.0 dtb, dtbo and modules, which are CRC-compatible with that Image;
- `msm_drm.ko` built from Xiaomi's `ruan-u-oss` display-driver source, plus a fix for the bootloader's
  60 Hz splash handover.

The switches are in `BoardConfig.mk`:
- `DIZI_SOURCE_KERNEL` (default `true`);
- `DIZI_SOURCE_DISPLAY` (default `splashfix`).

## Build switches

- `DIZI_SELINUX_PERMISSIVE=true`: permissive, on debuggable builds only. The default is enforcing.
- `DIZI_ADB_KEYS=<adb_keys>`: debuggable builds trust this adb key file (a path relative to the source root).
- `WITH_ADB_INSECURE=true`: adb without authorization on debuggable builds.

Thanks for the base tree:
[dizi-bringup](https://github.com/g8row/dizi-bringup).
[garnet-random](https://github.com/garnet-random) — kernel source and base device tree (Redmi Note 13 Pro)
[LineageOS](https://github.com/LineageOS) — Android base
