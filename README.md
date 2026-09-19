# LSLED Controller

A browser-based controller for legacy LSLED LED name badges using the FEE0/FEE1 Bluetooth Low Energy protocol.

The application allows you to draw images, create text messages, configure animation effects, and upload up to eight image slots directly from a modern Chromium-based browser using Web Bluetooth.

## Features

- Connect to LSLED LED badges using Bluetooth Low Energy
- No installation required
- Runs entirely in the browser
- Draw directly on a 44 × 11 pixel canvas
- Store up to 8 independent image slots
- Enable or disable individual slots
- Configure animation speed and display effects
- Adjustable brightness
- Built-in bitmap font for text rendering
- Support for:
  - Uppercase and lowercase Latin letters
  - Digits
  - German characters (Ä Ö Ü ä ö ü ß ẞ)
  - Nordic characters (Å å Æ æ Ø ø)
  - Icelandic characters (Þ þ Ð ð Ý ý)
  - Sámi and Greenlandic characters (Ŋ ŋ)
  - Polish characters (Ł ł)
  - Common accented Latin characters (Á á É é Ó ó and others)
- Automatic reconnect support
- Local storage of images and settings
- No cloud services
- No telemetry
- No tracking

## Requirements

- A Chromium-based browser with Web Bluetooth support:
  - Google Chrome
  - Microsoft Edge
  - Chromium
- HTTPS or localhost
- An LSLED badge supporting the legacy FEE0/FEE1 protocol, e.g. from [OpenElab](https://openelab.io/): [Wireless Bluetooth LED Name Badge 11x44 DIY Reusable](https://openelab.io/products/wireless-bluetooth-led-name-badge).

## Usage

### Connect

1. Power on the badge.
2. Open the controller page.
3. Click **Scan and connect**.
4. Select the LSLED device from the Bluetooth chooser.

### Draw Images

1. Select a slot.
2. Choose **Draw** or **Erase**.
3. Click or drag on the pixel matrix.
4. Configure speed and effect if desired.

### Render Text

1. Change the slot content type to **Text**.
2. Select a font.
3. Enter the desired text.
4. The text is rendered automatically into the slot image.

### Upload

1. Configure one or more slots.
2. Click **Send Images**.
3. The controller converts all enabled slots into the legacy LSLED packet format and uploads them to the badge.

### Running Locally

Because Web Bluetooth requires a secure context, the controller should be served from `localhost` or HTTPS.

A simple way to run it locally is:

```bash
python -m http.server -b 127.0.0.1 8080
```

Then open:

```text
http://127.0.0.1:8080
```

in a Chromium-based browser such as Google Chrome or Microsoft Edge.

## Hosted Version

A live version of the controller is available at:

[LSLED Controller](https://5inf.github.io/lsled_controller/)

Because the site is served over HTTPS, it can use Web Bluetooth directly without requiring a local web server.

## Storage

The controller stores the following information locally in the browser:

| Key | Purpose |
|-------|----------|
| `lsled-slot-images-v1` | Image data |
| `lsled-slot-settings-v1` | Slot settings |
| `lsled-brightness-v1` | Brightness setting |

No information is transmitted anywhere except to the selected LSLED device via Bluetooth.

## Privacy

This application runs entirely inside the browser.

- No analytics
- No tracking
- No telemetry
- No external servers
- No external libraries

Images, text, and settings are stored only in browser local storage and are transmitted only to the selected LSLED device using Bluetooth.

## Technical Details

Display size:

- 44 × 11 pixels

Communication:

- BLE Service: `0000fee0-0000-1000-8000-00805f9b34fb`
- BLE Characteristic: `0000fee1-0000-1000-8000-00805f9b34fb`

Protocol:

- Legacy LSLED image upload protocol
- Packetised transfer in 16-byte BLE writes
- Multi-image frame generation

## License

This project is licensed under the MIT License.

SPDX identifier:

```text
SPDX-License-Identifier: MIT
```

See the LICENSE file for the full license text.

## Special Thanks

Special thanks to the contributors of the Badgemagic firmware project for documenting details of the legacy LSLED protocol.

In particular, the implementation of brightness control in this controller was based on information shared in:

- [Brightness control via legacy format · Issue #97 · fossasia/badgemagic-firmware](https://github.com/fossasia/badgemagic-firmware/issues/97)

The discussion in that issue helped identify the brightness byte location used by compatible LSLED badges operating with the legacy FEE0/FEE1 protocol.
