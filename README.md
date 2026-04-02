# Voice Assistant Ball v2

Custom ESPHome config for the Spotpear Ball v2 voice assistant satellite with Home Assistant.

## Config

The main config is `va-eye.yaml`.

## Hardware

- **Board:** ESP32-S3-N16R8 (16MB flash, 8MB PSRAM)
- **Audio codec:** ES8311 (I2C 0x18, SDA=GPIO15, SCL=GPIO14)
- **I2S bus:** LRCLK=GPIO45, BCLK=GPIO9, MCLK=GPIO16, DIN=GPIO10, DOUT=GPIO8
- **Speaker enable:** GPIO46
- **Display:** GC9A01A 1.28" 240x240 round (SPI: CLK=GPIO4, MOSI=GPIO2, CS=GPIO5, DC=GPIO47, RST=GPIO38)
- **Backlight:** GPIO42 (inverted, LEDC PWM)
- **LED:** WS2812 on GPIO48
- **Battery:** ADC on GPIO1 (x2 voltage divider)
- **Button:** GPIO0 (push-to-talk, 10s hold = factory reset)

## Features

- Animated face UI drawn in code — expressions for idle, listening, thinking, replying, error, muted, no wifi, no HA
- 120ms animation interval with breathing glow, blinking, comets, sonar rings
- STT retry logic (up to 2 retries with TTS feedback)
- Mixer speaker setup with announcement + silence pipelines
- On-device wake word: "okay nabu" (cutoff 0.92, window 15)
- Push-to-talk via boot button
- Mute toggle via button or HA switch

## Flashing

```bash
esphome compile va-eye.yaml
esphome upload va-eye.yaml --device 192.168.1.224
```

Requires `secrets.yaml` next to the YAML:
```yaml
wifi_ssid: "YOUR_SSID"
wifi_password: "YOUR_PASSWORD"
```

## Known issues

- **Speaker pop/click** on TTS start and end. Caused by the ES8311 codec output stage transitioning. Likely needs a hardware fix (capacitor on speaker output).
