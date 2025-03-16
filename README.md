# Catan Board Generator

A digital Catan board generator for ESP32 with physical LED visualization and web interface. This project allows you to generate randomized Catan game boards with configurable rules, display them on a physical LED board, and control everything through a web interface.

![Catan Board Generator](https://github.com/yourusername/catan-board-generator/raw/main/images/board-example.jpg)

## Features

- Generates random Catan boards with customizable rules
- Supports both classic (19 hexes) and extension (30 hexes) boards
- Physical board visualization using WS2812B addressable LEDs
- Web interface for easy control from any device
- Configurable board generation rules:
  - Whether 6 & 8 tokens can be adjacent
  - Whether 2 & 12 tokens can be adjacent
  - Whether same resource types can be adjacent
  - Whether same number tokens can be adjacent
- Dice rolling simulation with LED animation
- Optional Home Assistant integration

## Hardware Requirements

- ESP32 development board (ESP32-WROOM or similar)
- WS2812B addressable LED strip:
  - 19 LEDs for classic Catan board
  - 30 LEDs for extension board
- Power supply for the LEDs (5V)
- USB connection for programming

## Software Requirements

- [PlatformIO](https://platformio.org/) (recommended) or Arduino IDE
- Web browser for accessing the interface

## Getting Started

### Setting Up PlatformIO

1. Install [Visual Studio Code](https://code.visualstudio.com/)
2. Install the [PlatformIO Extension](https://platformio.org/install/ide?install=vscode)
3. Clone this repository:
   ```
   git clone https://github.com/yourusername/catan-board-generator.git
   ```
4. Open the project folder in VS Code

### WiFi Configuration

There are two ways to set your WiFi credentials:

#### Option 1: Edit the placeholders in main.cpp (Recommended for users)

Look for the following section in `src/main.cpp` and simply replace the placeholder values with your actual WiFi credentials:

```cpp
// ------------- WIFI Credentials -------------
#ifndef PASSWORD_H
#define PASSWORD_H
const char *WIFI_SSID = "PlaceholderSSID";  // Change to your WiFi name
const char *WIFI_PASS = "PlaceholderPassword";  // Change to your WiFi password
#endif
```

#### Option 2: Create a separate password.h file (Recommended for contributors)

If you plan to contribute to this project or make multiple commits, creating a separate `include/password.h` file is better practice:

1. Create a file named `include/password.h` with the following content:
   ```cpp
   #ifndef PASSWORD_H
   #define PASSWORD_H
   
   const char *WIFI_SSID = "YourWifiName";
   const char *WIFI_PASS = "YourWifiPassword";
   
   #endif
   ```

2. This file is already listed in `.gitignore` to prevent accidentally committing your credentials.

Using option 2 is better for development as it keeps your personal credentials separate from the code you might commit.

### Building and Uploading the Firmware

1. Connect your ESP32 via USB
2. In VS Code with PlatformIO extension installed, click the PlatformIO icon in the sidebar
3. Under "PROJECT TASKS" > "esp32dev", click:
   - "Build" to compile the firmware
   - "Upload" to flash the firmware to the ESP32
   - "Upload Filesystem Image" to upload the web interface files to SPIFFS

Alternatively, you can use these commands from the terminal:

```bash
# Build the project
pio run

# Upload firmware to ESP32
pio run --target upload

# Upload filesystem image (web interface)
pio run --target uploadfs
```

### Wiring

Connect the WS2812B LED strip to the ESP32:
- Connect the LED data line to GPIO 4 (default, can be changed in `main.cpp`)
- Connect the LED power (5V) and ground to appropriate power supply
- Make sure your power supply can handle the current required by the LEDs

### Using the Web Interface

1. After uploading and restarting, the ESP32 will connect to your WiFi
2. Check the Serial Monitor for the assigned IP address
3. Open a web browser and go to that IP address
4. Use the interface to:
   - Choose between classic or extension board
   - Configure board generation rules
   - Start/end games
   - Roll dice or manually select numbers

## Project Structure

- `src/main.cpp` - Main application code
- `data/` - Web interface files (HTML, CSS, JS)
- `lib/` - Project libraries:
  - `BoardGenerator/` - Board generation algorithms
  - `LedController/` - LED control and animations
  - `WebPage/` - Web server setup
  - `HomeAssistant/` - Optional Home Assistant integration

## Optional: Home Assistant Integration

The project includes optional integration with Home Assistant. To enable it:

1. Uncomment the `#define ENABLE_HOME_ASSISTANT` line in your code
2. Set up a webhook automation in your Home Assistant instance
3. Update the Home Assistant configuration in your code with your server details

This allows the board to trigger Home Assistant automations when dice are rolled or numbers are selected.

## Customization

- To change the GPIO pin for the LED strip, modify `LED_STRIP_PIN` in `main.cpp`
- To modify the board generation rules, update the default settings in `main.cpp`
- To customize the web interface, edit the files in the `data/` directory

## Troubleshooting

### Common Issues

- **Can't upload firmware**: Make sure the ESP32 is connected and the correct port is selected, and you have the correct drivers.
- **LEDs not lighting up**: Check power supply and data connection. Check all the connections with a voltimeter.
- **Web interface not showing**: Make sure SPIFFS filesystem was uploaded
- **Generating boards takes too long**: Adjust the board generation options, particularly avoid disabling too many adjacency rules at once

### Serial Monitor

You can monitor debug output by connecting to the ESP32's serial port at 115200 baud.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Based on the game Catan by Klaus Teuber
- Uses the Adafruit NeoPixel library for LED control
