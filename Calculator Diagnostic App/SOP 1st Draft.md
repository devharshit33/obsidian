# Standard Operating Procedure (SOP)

# Calculator Diagnostic Testing

## Document Information

- **Document ID:** CALC-DIAG-SOP-001
- **Version:** 1.0
- **Last Updated:** May 08, 2025
- **Department:** Quality Assurance
- **Device Model:** Calculator RTL8730ELH
- **Firmware Version:** 1.0.0

## Table of Contents

1. [Introduction](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#introduction)
2. [Required Equipment](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#required-equipment)
3. [Safety Precautions](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#safety-precautions)
4. [Phase 0: Preparation](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#phase-0-preparation)
5. [Phase 1: Initial Setup and Diagnostics](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#phase-1-initial-setup-and-diagnostics)
6. [Phase 2: Specific Tests and Transition](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#phase-2-specific-tests-and-transition)
7. [Troubleshooting](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#troubleshooting)
8. [References](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#references)
9. [Revision History](https://claude.ai/chat/ccc63440-ab8c-4685-a934-e0204b5c8b44#revision-history)

## Introduction

This Standard Operating Procedure (SOP) defines the process for performing diagnostic testing on the Calculator RTL8730ELH device. The procedure is divided into three phases:

- **Phase 0:** Preparation and flashing of the diagnostic application
- **Phase 1:** Initial setup and comprehensive diagnostic testing
- **Phase 2:** Specific critical tests and transition to the Smart Calculator application

Following this procedure ensures consistent and thorough testing of all device components before final deployment.

## Required Equipment

- Calculator device (RTL8730ELH)
- USB Type-C cable
- PC/Laptop with terminal application (e.g., PuTTY, TeraTerm)
- Flashing tool
- SD Card (minimum 4GB, formatted as FAT32)
- Calculator Diagnostic Application package
- WiFi access point for connectivity testing
- Test SIM card for modem testing

[IMAGE: Equipment setup showing calculator device connected to laptop with all required tools]

## Safety Precautions

- Ensure all equipment is powered off before making connections
- Use only approved power adapters and cables
- Handle the PCB with ESD protection measures
- Ensure proper ventilation during testing
- Do not leave devices unattended during flashing operations

## Phase 0: Preparation

### 0.1 Prepare Flashing Environment

1. Connect the calculator device to your computer using the USB Type-C cable
2. Power on the calculator device by pressing the power button
3. Launch the flashing tool on your computer
4. Verify the device is detected by the flashing tool

[IMAGE: Screenshot of flashing tool with device connected and detected]

### 0.2 Flash Diagnostic Application

1. Load the Calculator Diagnostic Application package into the flashing tool
2. Select the appropriate partition for flashing
3. Start the flashing process
4. Wait for the flashing to complete (approximately 2-3 minutes)
5. Some xModem transfer errors may occur but can be ignored if flashing completes successfully

[IMAGE: Screenshot of successful flashing completion]

### 0.3 Initial Boot

1. After flashing, the device will automatically reboot
2. Observe the boot sequence on the terminal
3. Verify the following boot messages appear:
    - `ROM:[V1.1]`
    - `FLASHRATE:1`
    - `BOOT FROM NAND`
    - `IMG1(OTA1) VALID, ret: 0`

[IMAGE: Terminal showing successful boot sequence]

### 0.4 Terminal Connection

1. Configure your terminal application with the following settings:
    - Baud Rate: 1500000
    - Data Bits: 8
    - Parity: None
    - Stop Bits: 1
    - Flow Control: None
2. Connect to the device's serial port
3. Press Enter to access the login prompt
4. Log in using the credentials:
    - Username: `root`
    - No password required (press Enter)

[IMAGE: Terminal login screen with root access]

## Phase 1: Initial Setup and Diagnostics

### 1.1 First-Time Setup

#### 1.1.1 Partition Configuration

1. After login, the diagnostic application will automatically start
2. The application will check and configure the required partitions
3. Observe the following partition operations:
    - Creation of partition 1 (update, 256MB)
    - Creation of partition 2 (database, 128MB)
    - Creation of partition 3 (files, remaining space)
4. Wait for "Partition success" audio confirmation

[IMAGE: Terminal showing successful partition creation]

#### 1.1.2 Modem Baud Rate Configuration

1. After partition setup, the application will configure the modem
2. The application will attempt to communicate with the modem at 460800 baud
3. Observe the AT command response on the terminal
4. Wait for "Baud rate success" audio confirmation

[IMAGE: Terminal showing successful modem baud rate configuration]

> **NOTE:** If the baud rate configuration fails on first attempt, restart the device by pressing the power button and begin Phase 1 again. This is a known issue that typically resolves after restart.

#### 1.1.3 Audio Files Setup

1. After modem configuration, the application will set up the required audio files
2. The application will unzip audio files to the storage partition
3. Observe the extraction progress on the terminal
4. Wait for "Copy audio success" audio confirmation

[IMAGE: Terminal showing successful audio files extraction]

#### 1.1.4 Main Menu

1. After successful setup, the main menu will appear on the device display
2. The menu will show two options:
    - Run All Tests
    - Custom Tests
3. Use the FWD/BWD keys to navigate between options
4. Press ENTER to select an option

[IMAGE: Device display showing main menu with two options]

### 1.2 Run All Tests

#### 1.2.1 Select "Run All Tests"

1. Navigate to "Run All Tests" option using FWD/BWD keys
2. Press ENTER to select
3. The device will begin executing all diagnostic tests in sequence

[IMAGE: Device display showing "Run All Tests" selected]

#### 1.2.2 WiFi Test

1. The WiFi test will attempt to scan for available networks
2. The test checks for proper WiFi hardware functionality
3. The display will show scanning progress
4. If hardware is functioning correctly, the test will pass regardless of connection success
5. Press ENTER to continue to the next test

[IMAGE: Device display showing WiFi test progress and pass result]

#### 1.2.3 Display Test

1. The display test will show a series of patterns and colors
2. Verify:
    - Full screen illumination
    - No dead pixels
    - Even brightness
    - Proper color reproduction
3. Press ENTER to continue to the next test

[IMAGE: Device display showing display test pattern]

#### 1.2.4 Keypad Test

1. The keypad test will prompt you to press each key on the device
2. Follow the on-screen instructions to press specific keys
3. Each successful keypress will be highlighted on-screen
4. Test will pass when all keys have been verified
5. Press ENTER to continue to the next test

[IMAGE: Device display showing keypad test with some keys already verified]

#### 1.2.5 Modem Test

1. The modem test will check communication with the modem
2. The test will send AT commands and verify responses
3. No SIM card is required for this basic functionality test
4. Press ENTER to continue to the next test

[IMAGE: Device display showing modem test progress and pass result]

#### 1.2.6 RTC Test

1. The Real-Time Clock test will check the RTC functionality
2. The test verifies:
    - Clock running properly
    - Time can be set
    - Time increments correctly
3. Press ENTER to continue to the next test

[IMAGE: Device display showing RTC test progress and pass result]

#### 1.2.7 Audio & SD Test

1. The Audio & SD test checks:
    - Audio playback functionality
    - SD card detection and access
    - File read/write operations
2. Listen for audio playback during the test
3. Press ENTER to continue to the next test

[IMAGE: Device display showing Audio & SD test progress and pass result]

#### 1.2.8 BLE Test

1. The Bluetooth Low Energy test checks the BLE hardware
2. The test verifies:
    - BLE controller initialization
    - Scan functionality
    - Advertising capability
3. Press ENTER to continue to the next test

[IMAGE: Device display showing BLE test progress and pass result]

#### 1.2.9 Battery Test

1. The Battery test checks:
    - Battery presence
    - Voltage level
    - Charging capability
    - Battery percentage reporting
2. Press ENTER to continue to the next test

[IMAGE: Device display showing Battery test progress and pass result]

#### 1.2.10 eFuse Test

1. The eFuse test will program and verify critical device parameters
2. This test configures:
    - Device MAC address
    - Security keys
    - Hardware identifiers
3. Press ENTER to continue to the next test

[IMAGE: Device display showing eFuse test progress and pass result]

#### 1.2.11 Summary Screen

1. After all tests are completed, a summary screen will appear
2. The summary shows:
    - Total number of tests performed
    - Number of tests passed
    - Number of tests failed
    - List of any failed tests
3. Press ENTER to proceed or MENU to return to the main menu

[IMAGE: Device display showing summary screen with all tests passed]

#### 1.2.12 Shutdown

1. After pressing ENTER on the summary screen, the shutdown confirmation will appear
2. Select "Yes" to shutdown the device
3. The device will power off automatically

[IMAGE: Device display showing shutdown confirmation screen]

> **IMPORTANT:** After Phase 1 completes successfully, proceed to Phase 2 by powering on the device again.

## Phase 2: Specific Tests and Transition

### 2.1 Power On and Login

1. Press the power button to turn on the device
2. Wait for the boot sequence to complete
3. Log in as root (no password required)
4. The Calculator Diagnostic application will start automatically
5. The main menu will appear with "Run All Tests" and "Custom Tests" options

[IMAGE: Device display showing main menu after Phase 2 boot]

### 2.2 Run Specific Tests

#### 2.2.1 Select "Custom Tests"

1. Navigate to "Custom Tests" option using FWD/BWD keys
2. Press ENTER to select
3. A list of available tests will appear

[IMAGE: Device display showing "Custom Tests" option selected]

#### 2.2.2 WiFi Test

1. Navigate to "WiFi Test" using FWD/BWD keys
2. Press ENTER to select
3. This test verifies:
    - WiFi hardware functionality
    - Network scanning capability
    - Connection to configured access point
4. A "Pass" result indicates successful WiFi functionality
5. Press ENTER to return to the test selection screen

[IMAGE: Device display showing WiFi test selected in Custom Tests menu]

#### 2.2.3 Modem Test

1. Navigate to "Modem Test" using FWD/BWD keys
2. Press ENTER to select
3. This test verifies:
    - Modem hardware functionality
    - AT command response
    - SIM card detection (if inserted)
4. A "Pass" result indicates successful modem functionality
5. Press ENTER to return to the test selection screen

[IMAGE: Device display showing Modem test selected in Custom Tests menu]

#### 2.2.4 eFuse Test

1. Navigate to "eFuse Test" using FWD/BWD keys
2. Press ENTER to select
3. This test is critical as it:
    - Ensures proper MAC address configuration
    - Programs unique device identifiers
    - Sets security parameters
4. A "Pass" result indicates successful eFuse programming
5. Press ENTER to return to the test selection screen

[IMAGE: Device display showing eFuse test selected in Custom Tests menu]

> **CRITICAL:** The eFuse test must be run and pass in Phase 2 to ensure proper MAC address configuration. Skipping this test may result in MAC address issues.

### 2.3 Return to Main Menu

1. Press the MENU button to return to the main menu
2. The main menu with "Run All Tests" and "Custom Tests" options will appear

[IMAGE: Device display showing return to main menu]

### 2.4 Exit Diagnostic and Transition

1. From the main menu, navigate to "Run All Tests"
2. Press ENTER to select
3. When the summary screen appears, press ENTER
4. Select "Yes" on the shutdown confirmation screen
5. The device will initiate the transition to the Smart Calculator application
6. Wait for the device to complete the transition (may take 30-60 seconds)

[IMAGE: Device display showing shutdown confirmation with "Yes" selected]

### 2.5 Verify Transition

1. After the transition completes, the Smart Calculator application will start
2. Verify:
    - Smart Calculator application loads correctly
    - Display shows calculator interface
    - Keypad is responsive
    - Functionality works as expected

[IMAGE: Device display showing Smart Calculator application after successful transition]

## Troubleshooting

### Common Issues and Solutions

#### Baud Rate Configuration Failure

**Symptoms:**

- "Baud rate failed" audio message
- Error message: "Failed to configure modem baud rate"
- White screen or application not starting

**Solutions:**

1. Restart the device by pressing the power button
2. Begin Phase 1 procedure again
3. If the issue persists after 3 attempts, check modem hardware connection

#### WiFi Test Failure

**Symptoms:**

- "WiFi test failed" message
- No networks found

**Solutions:**

1. Ensure WiFi is not blocked by hardware switch
2. Verify WiFi access point is available
3. Check antenna connections
4. Retry the test

#### eFuse Test Failure

**Symptoms:**

- "eFuse test failed" message
- MAC address issues

**Solutions:**

1. Retry the eFuse test
2. Check for proper device initialization
3. If the issue persists, the device may require service

#### Transition Failure

**Symptoms:**

- Device does not transition to Smart Calculator
- Stuck at loading or shutdown screen

**Solutions:**

1. Complete both Phase 1 and Phase 2 testing
2. Ensure eFuse test passes in Phase 2
3. Restart the device and try the transition again
4. If persistent, reflash the device

## References

### Diagnostic Flowchart

```
digraph CalculatorDiagnostic {
    // Graph settings
    rankdir=TB;
    splines=ortho;
    node [shape=box, style=rounded, fontname="Arial"];
    edge [fontname="Arial", fontsize=10];

    // Node definitions
    start [label="Calculator Diagnostic", shape=oval, style=filled, fillcolor="#98FB98"];
    menu [label="Main Menu", style=filled, fillcolor="#d4edda"];
    choice [label="Choose Option\n(FWD/BWD to select)", shape=diamond, style=filled, fillcolor="#e2c8df"];
    
    runall [label="Run All Tests", style=filled, fillcolor="#cce5ff"];
    custom [label="Custom Tests", style=filled, fillcolor="#cce5ff"];

    // Test sequence in a subgraph
    subgraph cluster_tests {
        label="Test Sequence";
        style=filled;
        color=lightgrey;
        node [style=filled, fillcolor="#cce5ff"];

        tests [label="Available Tests:\n1. WiFi Test\n2. Display Test\n3. Keypad Test\n4. Modem Test\n5. RTC Test\n6. Audio & SD Test\n7. BLE Test\n8. Battery Test\n9. eFuse Test"];
        result [label="Test Result Screen"];
        next [label="Next Test"];
    }

    summary [label="Summary Screen", style=filled, fillcolor="#fff3cd"];
    
    // Controls note
    controls [label="Key Controls:\n• FWD/BWD - Navigate Options\n• ENTER - Select/Continue\n• MENU - Return to Menu", 
              shape=note, style="filled,dashed", fillcolor="#f8f9fa"];

    // Edge definitions
    start -> menu;
    menu -> choice;
    choice -> runall [label="ENTER"];
    choice -> custom [label="ENTER"];
    
    {runall custom} -> tests;
    tests -> result;
    result -> next [label="ENTER"];
    next -> result;
    
    result -> summary [label="All Tests\nComplete"];
    summary -> menu [label="MENU"];
}
```

### Test Results Reference

| Test       | Critical for Phase | Expected Result                             |
| ---------- | ------------------ | ------------------------------------------- |
| WiFi       | Phase 1 & 2        | Hardware functional, networks detected      |
| Display    | Phase 1            | All pixels functional, colors accurate      |
| Keypad     | Phase 1            | All keys responsive                         |
| Modem      | Phase 1 & 2        | AT commands responded, baud rate configured |
| RTC        | Phase 1            | Clock functional, time can be set           |
| Audio & SD | Phase 1            | Audio playback works, SD card accessible    |
| BLE        | Phase 1            | BLE controller initializes and scans        |
| Battery    | Phase 1            | Battery detected, charging functional       |
| eFuse      | Phase 1 & 2        | MAC address and identifiers programmed      |

### Key Controls Reference

- **FWD/BWD**: Navigate through options
- **ENTER**: Select an option or continue
- **MENU**: Return to the main menu or previous screen
- **Power Button**: Force shutdown if needed (hold for 5 seconds)

## Revision History

| Version | Date       | Author      | Changes         |
| ------- | ---------- | ----------- | --------------- |
| 1.0     | 05/08/2025 | [Your Name] | Initial release |
|         |            |             |                 |