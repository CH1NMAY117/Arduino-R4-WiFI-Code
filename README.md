# Arogyavitals Firmware (Arduino Uno R4 WiFi)

**Firmware for Arogyavitals, a low-cost, modular maternal-health monitor for rural India.** It reads pulse, body temperature and blood pressure, then serves the latest values as JSON over Wi-Fi.

Built by Team Clinovators for the *Product Design & Prototyping Laboratory* course at the Jyoti and Bhupat Mehta School of Health Sciences and Technology (JBMSHST), IIT Guwahati, Jan–Apr 2025. The companion mobile app is [MitraLink](https://github.com/CH1NMAY117/mitralink).

> **Status: coursework prototype. Not a medical device and not clinically validated.**

## Hardware

| Part | Connection |
| --- | --- |
| Arduino Uno R4 WiFi | Main board, Wi-Fi via `WiFiS3` |
| Analog pulse sensor | `A0` |
| Dallas/OneWire temperature sensor | digital pin `2` |
| Blood-pressure module | `Serial1` at 9600 baud |
| Built-in LED matrix | Shows a heart on new readings and a cloud while sending |

## How it works

1. Connects to a Wi-Fi network or phone hotspot, then prints its IP address to the serial monitor.
2. Samples the three sensors and builds one JSON record.
3. Runs a small HTTP server on port 80:
   - `GET /discover` returns `{"device":"arduino","type":"sensor","name":"BP Monitor"}`
   - `GET /` returns the latest reading:

```json
{"bpm": 72, "temp": 36.6000, "systolic": 118, "diastolic": 76, "bp_pulse": 71}
```

## Setup

1. Install the Arduino IDE and the libraries `OneWire` and `DallasTemperature` (the `WiFiS3` and `Arduino_LED_Matrix` libraries come with the Uno R4 board package).
2. Open `arogyavitals_firmware/arogyavitals_firmware.ino`.
3. Replace `YOUR_WIFI_SSID` and `YOUR_WIFI_PASSWORD` with your own network details. **Never commit real credentials.**
4. Upload to the board and open the serial monitor at 9600 baud to read the device IP.

## Limitations

- The live connection from a phone app to this firmware was demonstrated with a separate companion app that is not published here. The [MitraLink](https://github.com/CH1NMAY117/mitralink) interface does not yet include it.
- Heart rate (`bpm`) is a rough estimate: the raw analog pulse value is mapped linearly to 40–120 and smoothed with a moving average. It is not a validated beat-detection algorithm.
- Blood pressure comes from an external module; the firmware only parses its serial output (expected as comma-separated values, with systolic, diastolic and pulse in fields 2 to 4).
- The server is unauthenticated and sends data in plain HTTP, so it is suitable only for local prototyping.

## Team Clinovators

- **Chinmay Arora** (Pharmacy): MitraLink app
- **Sanjoy Kumar Basu** (Pharmacy): 3D-printed enclosure, designed in Fusion 360
- **Parijat Roychowdhury** (Biotechnology): project integration and clinical alignment
- **Rudrasish Mukherjee** (Electronics and Communication Engineering): circuit simulation in KiCad and Arduino integration

Thanks to Dr. Erwin Fuhrer, Dr. Subrata Pramanik, Prof. Subramani Kanagaraj and teaching assistants Sheetal Das and Vishal Tande for their mentorship.

## License

[MIT](LICENSE)
