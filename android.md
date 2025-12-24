# Dockerify-Android Quick Setup & Usage Guide


## 1. Clone the Repository

```bash
git clone https://github.com/Shmayro/dockerify-android.git
cd dockerify-android
```

## 2. (Optional) Customize Settings

Edit `docker-compose.yml` to enable or adjust features **before the first run**:

Key environment variables:
- `ROOT_SETUP=1` → Install Magisk root (done once on first boot)
- `GAPPS_SETUP=1` → Install Open GApps Pico package (done once on first boot)
- `RAM_SIZE=2048` → Set emulator RAM in MB (default is usually 2048–4096)
- `SCREEN_RESOLUTION=600x800` → Custom screen resolution (e.g., 600x800, 1080x1920)
- `SCREEN_DENSITY=240` → Optional DPI for better scaling

> Changes to resolution/density require a full reset (see "Reset Emulator" section).

## 3. Build the Image (Optional – Compose will auto-build if needed)

```bash
docker compose build
```

Verify the image:

```bash
docker images | grep dockerify-android
```

## 4. Run the Containers (First Time)

```bash
docker compose up -d
```

- First boot takes **10–15 minutes** (AVD creation, optimizations, optional GApps/Magisk).
- Subsequent starts are fast (seconds to minutes).
- Web interface (Scrcpy + controls): http://localhost:8000

Monitor progress:

```bash
docker logs -f dockerify-android
```

## 5. Stop and Start Containers

Stop:

```bash
docker compose stop
```

Start again (preserves emulator state):

```bash
docker compose start
```

Or restart quickly:
```bash
docker compose restart
```

## 6. Connect via ADB
The setup exposes the emulator on port 5555.

Connect:
```bash
adb connect localhost:5555
```

Check devices:
```bash
adb devices -l
```

Expected output includes:

- `localhost:5555    device`
- `emulator-5554      device` (same emulator, different connection – normal in this setup)

If "offline" or no devices:

```bash
adb kill-server
adb start-server
adb connect localhost:5555
```

## 7. Install APK
Always specify the target to avoid "more than one device" errors:

Preferred (host connection):
```bash
adb -s localhost:5555 install /path/to/your/app.apk
```

Alternative (internal connection):

```bash
adb -s emulator-5554 install /path/to/your/app.apk
```

Both target the **same emulator**.

## 8. Reset Emulator (Start Fresh)

If you changed resolution, want to redo GApps/Magisk, or clear everything:

```bash
docker compose down -v   # Deletes persisted volume (AVD data)
docker compose up -d     # Rebuilds from scratch (long first boot again)
```

## Tips

- Place APKs in the `extras/` folder before first boot → they auto-copy to `/sdcard/Download/` inside the emulator.
- Use the web interface at http://localhost:8000 for easy control (mouse/keyboard/touch).
- For best performance: Ensure KVM is enabled on your Ubuntu host.

