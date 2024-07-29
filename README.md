
# SmartKey Bridge: Bluetooth Text Transfer System

## Project Overview

The SmartKey Bridge project aims to create a seamless system that allows users to send text files from their smartphones to an Arduino Leonardo via Bluetooth. The Arduino then acts as a keyboard to input the received text directly into a connected computer. This project is useful for various applications, including automated text entry, script running, and more.

## Components

- Arduino micro Leonardo
- Bluetooth module (e.g., HC-05 or HC-06)
- Smartphone with Bluetooth capability
- Computer with USB port
- Connecting wires
- Breadboard (optional for prototyping)

## Features

- **Wireless Text Transfer**: Send text files from a smartphone to the Arduino via Bluetooth.
- **Automated Text Entry**: The Arduino inputs the received text to a connected computer as if typed by a keyboard.
- **Buffer Management**: Handles large text files with buffer management to prevent overflow.

## Getting Started

### Prerequisites

- Arduino IDE installed on your computer
- Basic knowledge of Arduino programming and electronics
- Bluetooth terminal app on your smartphone (e.g., Bluetooth Terminal HC-05)

### Hardware Setup

1. **Connect the Bluetooth Module**:
   - Connect the VCC and GND pins of the Bluetooth module to the 5V and GND pins of the Arduino.
   - Connect the TX pin of the Bluetooth module to the RX pin (pin 0) of the Arduino.
   - Connect the RX pin of the Bluetooth module to the TX pin (pin 1) of the Arduino.

2. **Connect the Arduino to the Computer**:
   - Use a USB cable to connect the Arduino Leonardo to your computer.

### Software Setup

1. **Arduino Code**:
   - Open the Arduino IDE and upload the following code to your Arduino Leonardo:

    ```cpp
    #include <Keyboard.h>

    const int BUFFER_SIZE = 1024;
    char buffer[BUFFER_SIZE];
    int bufferIndex = 0;
    bool fileReceived = false;

    void setup() {
      Serial.begin(9600);   // Initialize the serial monitor
      Serial1.begin(9600);  // Initialize the serial communication with the Bluetooth module
      Keyboard.begin();     // Initialize keyboard
    }

    void loop() {
      if (Serial1.available() && !fileReceived) {
        char c = Serial1.read();

        // Check for buffer overflow
        if (bufferIndex < BUFFER_SIZE - 1) {
          buffer[bufferIndex++] = c;
        }

        // Check for end of file (using '\n' as delimiter)
        if (c == '\n') {
          buffer[bufferIndex] = '\0'; // Null-terminate the string
          fileReceived = true; // Indicate that the file has been fully received

          // Optional: Print the buffer content to Serial Monitor for debugging
          Serial.println(buffer);
        }
      }

      if (fileReceived) {
        // Send the whole buffer content to the keyboard
        for (int i = 0; i < bufferIndex; i++) {
          Keyboard.write(buffer[i]);
        }

        // Reset the buffer and flags for the next file
        bufferIndex = 0;
        fileReceived = false;
      }
    }
    ```

2. **Bluetooth Terminal App**:
   - Install a Bluetooth terminal app on your smartphone.
   - Pair your smartphone with the Bluetooth module.
   - Open the terminal app and connect to the Bluetooth module.

### Usage

1. **Send Text File**:
   - Open the Bluetooth terminal app on your smartphone.
   - Connect to the Bluetooth module and send the text file.
   - The Arduino will receive the text file and send it as keyboard input to the connected computer.

2. **Monitor Output**:
   - Open a text editor or any application on your computer where you want the text to be entered.
   - The received text will be automatically typed into the application.

## Troubleshooting

- **No Text Received**:
  - Check the connections between the Bluetooth module and the Arduino.
  - Ensure that the Bluetooth terminal app is connected to the correct Bluetooth module.

- **Garbage Text or Incorrect Characters**:
  - Ensure that the baud rate set in the Arduino code matches the baud rate of the Bluetooth module.
  - Check for buffer overflows and adjust the buffer size if necessary.

## Contributing

Feel free to contribute to this project by submitting issues or pull requests. Any contributions are welcome!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by various Arduino and Bluetooth integration projects.
- Special thanks to the Arduino community for their valuable resources and support.
