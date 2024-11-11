# LM3914 LED Bar Graph Driver Library

[![Arduino Library](https://img.shields.io/badge/Arduino-Library-blue.svg)](https://www.arduino.cc/reference/en/libraries/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: Arduino](https://img.shields.io/badge/Platform-Arduino-green.svg)](https://www.arduino.cc/)

A powerful and flexible Arduino library for controlling LM3914 LED dot/bar display drivers. Features include cascading multiple displays, I2C expansion support via MCP23017, VU meter functionality, sound reactivity, and customizable thresholds.

<img src="docs/images/basic_demo.jpg" width="400" alt="Basic Demo">

## 🌟 Features

- **Multiple Operation Modes**
  - Dot or bar display
  - Individual segment control
  - Range and pattern display
  - Adjustable brightness

- **Advanced Functions**
  - VU meter with peak hold
  - Sound reactive animations
  - Customizable thresholds
  - Multiple display cascading

- **Flexible Hardware Support**
  - Direct pin control
  - I2C expansion via MCP23017
  - Multiple display support
  - PWM brightness control

- **Robust Implementation**
  - Comprehensive error handling
  - Modular design
  - Optional feature compilation
  - Memory efficient

## 📋 Requirements

### Hardware
- Arduino board (or compatible)
- LM3914 LED bar graph driver
- LED bar graph display
- Optional: MCP23017 I2C port expander

### Software
- Arduino IDE 1.5.0 or higher
- If using I2C: Adafruit_MCP23X17 library

## 🚀 Installation

### Arduino IDE
1. Open Arduino IDE
2. Go to Sketch -> Include Library -> Manage Libraries
3. Search for "LM3914"
4. Click Install

### Manual Installation
1. Download this repository
2. Move the folder to your Arduino libraries directory
3. Restart Arduino IDE

### PlatformIO
Add to your `platformio.ini`:
```ini
lib_deps =
    LM3914
    adafruit/Adafruit_MCP23X17 @ ^2.0.0  # If using I2C features
```

## 📝 Basic Usage

### Simple Display
```cpp
#include <LM3914.h>

LM3914Controller::Options opts;
LM3914Controller display(opts);

void setup() {
    // Initialize with direct pins (clock, data, latch, mode)
    display.begin(2, 3, 4, 5);
    
    // Set to bar mode
    display.setMode(true);
}

void loop() {
    // Light up segments 0-5
    display.setRange(0, 5);
    delay(1000);
    
    // Light up alternate segments
    int segments[] = {0, 2, 4, 6, 8};
    display.setSegments(segments, 5);
    delay(1000);
}
```

### I2C Expansion
```cpp
#include <LM3914.h>

LM3914Controller::Options opts;
opts.useI2C = true;
LM3914Controller display(opts);

void setup() {
    // Create configuration with MCP23017 pins
    auto config = LM3914Controller::DisplayConfig{
        display.createMCPPin(0, 0x20),  // clock
        display.createMCPPin(1, 0x20),  // data
        display.createMCPPin(2, 0x20),  // latch
        display.createMCPPin(3, 0x20),  // mode
        display.createDirectPin(9),      // ref
        true
    };
    
    display.begin(config);
}
```

See [examples](examples/) for more usage scenarios.

## 📚 Documentation

- [Getting Started Guide](docs/getting_started.md)
- [Hardware Setup Guide](docs/hardware_setup.md)
- [API Reference](docs/api_reference.md)
- [Advanced Features](docs/advanced_features.md)
- [Troubleshooting Guide](docs/troubleshooting.md)

## 🔧 Hardware Configuration

### Basic Connection
```
Arduino         LM3914
--------       --------
Digital 2  -->  Clock (Pin 3)
Digital 3  -->  Data  (Pin 5)
Digital 4  -->  Latch (Pin 9)
Digital 5  -->  Mode  (Pin 9)
5V        -->  V+ (Pin 3)
GND       -->  GND (Pin 2)
```

### With MCP23017
```
Arduino         MCP23017          LM3914
--------       ---------         --------
SDA       -->  SDA      
SCL       -->  SCL      
           |   GPB0     -->     Clock
           |   GPB1     -->     Data
           |   GPB2     -->     Latch
           |   GPB3     -->     Mode
```

See [hardware setup guide](docs/hardware_setup.md) for detailed wiring diagrams.

## 🎓 Examples

- [Basic Usage](examples/BasicUsage/BasicUsage.ino)
- [I2C Expansion](examples/I2CExpander/I2CExpander.ino)
- [VU Meter](examples/VUMeter/VUMeter.ino)
- [Sound Reactive](examples/SoundReactive/SoundReactive.ino)
- [Multiple Displays](examples/MultipleDisplays/MultipleDisplays.ino)
- [Threshold Control](examples/ThresholdControl/ThresholdControl.ino)
- [Animation Patterns](examples/AnimationPatterns/AnimationPatterns.ino)

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## ✨ Acknowledgments

- National Semiconductor (now TI) for the LM3914 IC
- Adafruit for the MCP23017 library
- All contributors and users of this library

## 📞 Support

- Create an [Issue](https://github.com/FRCIndustries/LM3914Controller/issues)
- Read our [Troubleshooting Guide](docs/troubleshooting.md)
- Check the [FAQ](docs/faq.md)

## 🗺️ Roadmap

- [ ] Add FFT-based spectrum analyzer
- [ ] Support for other I2C expanders
