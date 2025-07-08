# Calculator Diagnostic Development Notes

## 📋 Main TODO List

- [x] Reorder tests - Put keypad test at 1st, display at 2nd, and wifi at 3rd and make the keypad screen unstuck while running
- [x] Modify the display test
    - Show the color and ask user to press enter key if correct color is shown or press wrong button if correct color is not shown
    - Color screens will be displayed randomly and user selects whether right color has shown or not
    - Tag colors with numbers internally and send data to server for future debugging
- [x] Modify the wifi test
    - Provide more information to the user for failure causes (wrong credentials, router too far away)
- [x] Modify rtc test
    - Check from kernel interrupt whether coin cell is attached (get help from Shanmukh)
    - Check time from server and validate RTC time
- [x] Modify audio and SD card test
    - After playing sound and timeout expires, prompt user whether they heard the sound
    - Check if speaker sound is loud enough
- [x] Modify the efuse test
    - Check internet connectivity before making API call
    - Prompt user if internet is unavailable
- [x] Add automatic retries (3 times) for every test
- [x] Add numbers in the summary screen before test names
- [x] Add retries in scanning of wifi networks
- [x] Add serial number test

## 🐞 Current Issues

1. [x] In custom screen if one test fails when adding 2+ tests, every test fails (partially fixed)
2. [x] In custom screen, result screen only appears for 1 sec
3. [x] In all summary screen, last test name not visible
4. [x] Keypad test running screen gets stuck
5. [x] In display test, red color sometimes gets disfigured
6. [x] Segmentation faults after 3rd retry
7. [x] Efuse test audio not added, need to add serial number on display
8. [x] Keypad test plays "passed" audio even when it fails

## 🔍 Observations

- [x] In custom screen, there is no result screen (it just comes and goes), needs same functionality as "run all tests"
- [x] Last test not visible in summary screen
- [x] Segmentation faults after last BLE test
- [x] RTC test sometimes fails (negative time)
- [x] When any test reruns and then goes to last test, segmentation fault happens (crash or white screen)
- [x] During run all tests, after last test it waits 10 seconds to show summary screen if wifi is not there (GSM issue)

## 🖥️ Test Screens UI

1. **Main Menu Screen**
    
    - Two buttons: "Run All Tests" and "Run Custom Tests"
    - Need to add green highlight on "Run All Tests" button when program starts
2. **Running Screen**
    
    - Test name with "Running" text
    - Spinner below the test name
    - Square box with test progress count (e.g., "1/9", "2/9")
    - Progress bar at top showing passed/failed tests (green/red)
3. **Result Screen** (two types)
    
    - **Pass Screen**:
        - 70% green background with "Passed" text and LV right image (white)
        - 30% white background with test name in black
        - Need to add "Press Enter to continue" in footer
    - **Fail Screen**:
        - 50% red background with "Failed" text and LV wrong image (white)
        - 50% white background with test name at 70% of screen
        - "Continue" and "Retry" buttons at bottom corners
        - Need to add simplified "Press Enter to continue or choose Retry" footer
4. **Summary Screen**
    
    - White background
    - Progress bar at top showing passed/failed tests (green/red)
    - Square box showing passed tests count (e.g., "2/9", "7/9")
    - Scrollable view below with test names and pass/fail status

## 📝 Work Progress

### Jan 13

- Added custom screen and modified main screen code
- Testing screen changes made in screens/running, result, summary, and main menu
- Observation: RTC test returns negative time

### Completed Items

- ✅ Custom screen issue fixed
- ✅ Reordering tests completed

## 🛠️ Build & Deploy Commands

```bash
devtool build calculator-diagnostic && devtool deploy-target calculator-diagnostic root@192.168.76.172

source ~/PythonEnv/kas/bin/activate
kas build debug.yml -- -k
```

## 🔴 Error Logs

```
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Custom test execution failed
[Thread 0xb4bff300 (LWP 8404) exited]
Showing summary for SUBSET of tests (8)...

Thread 2 "calculator_diag" received signal SIGSEGV, Segmentation fault.
[Switching to Thread 0xb5fb9300 (LWP 8372)]
_lv_event_mark_deleted (obj=obj@entry=0x563fc4 <work_mem_int+30176>) at /usr/src/debug/calculator-diagnostic/0.1-r0/calculator-diagnostic-0.1/_deps/lvgl-src/src/core/lv_event.c:157
157     /usr/src/debug/calculator-diagnostic/0.1-r0/calculator-diagnostic-0.1/_deps/lvgl-src/src/core/lv_event.c: No such file or directory.
```

## 🧐 Observed Issue Behavior

> Issue observed: When choosing "Run All Tests" button first, everything runs perfectly. But when later choosing "Run Custom Tests" button with 3-5 tests selected, after the first test running, all other tests fail without running and the summary screen appears immediately. Sometimes segmentation faults occur.
> 
> However, when restarting device and choosing "Run Custom Test" button first with 4-7 random tests, everything works fine - all tests run sequentially showing the summary screen at the end (though it automatically moves to next test without user input after a passed test).
> 
> When later selecting "Run All Tests", it starts well but segmentation faults occur in some tests (display, audio SD) and after the last test when summary screen should appear.

## 🔄 Tofupilot Integration Issues

- [x] Make background check after 20 seconds if network exists, then send local files
- [x] JSON coming but not with all test info (solved)

## 🔧 Final UI Tweaks

- [x] Add "Press Enter key" label footer in result screen
- [x] Add green highlight on "Run All" button when program starts
- [x] Fix issue where modem test passes even after one component fails
- [x] Fix double button press to move from "Run All Test" and "Custom Test"

## 🧪 Additional Test Changes

- [x] RTC: Check time from server, validate RTC time (in progress)

# April 22 - New To-Do List

## 📋 New Tasks

- [x] Send Sim card info to API (Get API from Mayank)
- [x] don't include timestamp for generating hmac
- [x] Send test result data only for "Run All Tests" data
    - [x] Select procedure name based on if `/etc/factory_mode` is 1 or not
- [x] Add a reboot screen option on summary screen
    - [x] All tests should pass
    - [x] User presses Enter, show screen "Reboot to Main System" with Yes/No buttons
    - [x] Default focus on No button
    - [x] If user presses No button, go back to summary screen
    - [x] If user presses Yes button, send data to Mayank's API and run factory reset commands
    

Strategy for test sim and actual sim insertion and how the flow will be
- [x] when we'll  switch to smart- calculator app ( at what state and stage )
- [x] how this will be triggered ( transition from calculator-diagnostic to smart-calculator )
      
      
## 🔄 Phase Implementation Workflow

This section outlines the complete workflow across Phase 1 and Phase 2 of the calculator diagnostic process.

### Phase 1 Flow

1. Run diagnostic tests in "Run All Tests" mode
2. When all tests are complete:
    - [x] User presses Enter on summary screen
    - [x] Show shutdown screen with Yes/No buttons
    - [x] Default focus on No button
    - [x] If user presses No button → go back to summary screen
    - [x] If user presses Yes button:
        - Set `/etc/factorymode` value to 2
        - Send data to tofupilot API (Phase 1 marker)
        - Do NOT call device status API
        - Shutdown the device
    - This completes Phase 1

### Transition to Phase 2

- [x] Phase 2 begins when device restarts or next boot happens
- [x] App setup does not run (already completed in Phase 1)

### Phase 2 Flow

- [x] Only run modem and WiFi tests (from custom test selection)
- [x] Send results to both tofupilot API and device status API
- [x] After tests have run and summary screen is shown:
    - User presses Enter on summary screen
    - Show shutdown screen with Yes/No buttons
    - Default focus on No button
    - If user presses No button → go back to summary screen
    - If user presses Yes button → run factory reset commands:
        
        ```bash
        rm -rf /rom/mnt/overlay/*mount -o remount /reboot
        ```
        
    - This completes Phase 2

### Transition to Smart Calculator

- Device transitions to smart calculator app on next boot after factory reset

## 📊 Process Diagram

```
┌──────────────┐     ┌───────────────┐     ┌───────────────┐
│   Phase 1    │     │  Transition   │     │    Phase 2    │
│ (Diagnostics)│────▶│  (Reboot)     │────▶│  (Modem/WiFi) │
└──────────────┘     └───────────────┘     └───────────────┘
        │                                          │
        ▼                                          ▼
┌──────────────┐                         ┌───────────────┐
│ Set factory  │                         │ Factory Reset │
│ mode to 2    │                         │ Commands      │
└──────────────┘                         └───────────────┘
        │                                          │
        ▼                                          ▼
┌──────────────┐                         ┌───────────────┐
│ Tofupilot API│                         │ Transition to │
│ (no DB API)  │                         │ Smart Calc App│
└──────────────┘                         └───────────────┘
```

## 🔍 Key Implementation Details

### Phase 1 Specifics

- All tests must pass to proceed to shutdown screen
- Only send data to tofupilot API (marked as Phase 1)
- Do not call device status API in Phase 1
- Set `/etc/factorymode` to 2 before shutdown

### Phase 2 Specifics

- Only run modem and WiFi tests
- Send data to both tofupilot API and device status API
- Factory reset commands triggered on "Yes" at shutdown screen
- Automatic transition to smart calculator on next boot

## ❓ Questions to Discuss

1. **API Integration Question**: Do we need to send data to Mayank's API every time it makes the tofupilot API call, or just in "Run All Tests" summary screen? Currently, whenever it's making the API call for tofupilot, it's also sending data to Mayank's API. ( discussed and resolved )
    
2. **Security Concern**: Where should we put the secret key for HMAC encryption? Currently, it's in the calculator-diagnostic.service file. { ok for now }
    
3. **Deployment Strategy**: What is our plan for 5000 devices?
    
    - Run calculator diagnostic "Run All Tests" on all devices?
    - Or run "Run All Tests" for ~200 devices and run custom tests (WiFi, Modem, BLE, Efuse, Battery) for the rest?
4. **QR Code Implementation**: Should we implement QR code for getting the serial number in smart-calculator app or calculator-diagnostic app?
    
    - Current plan is to implement in smart-calculator app so we'll know it moved to smart-calculator app.
5. **Tofupilot Subscription**: Need to discuss new plans they have announced.
6. sending data to tofupilot take around 5 to 8 seconds , so when to show the shutdown screen ( whats the strategy to follow here )
    




## 🔄 Phase 2 Implementation Issues

### Workflow Issues

- [x] Phase 2: Show loading and shutdown screen only when both WiFi and modem tests have passed
- [x] Phase 1: Verify it's hitting device status API (and remove if so)
- [x] Add `ifconfig wlan0 down` to factory reset commands
- [x] Show loading and shutdown screen only when all tests are passed in "Run All Tests" in Phase 1

### API Integration Issues

- [x] Fix multiple API calls (3-4 tofupilot API calls occurring)
- [x] Fix issue where local files aren't being deleted after API call
- [x] Fix duplicate API calls (currently making 2 API calls)
- [x] Run efuse test to get serial number when sending device status API call
    - Serial number needed instead of IMEI for device status API
    - Even if efuse test hasn't run in Phase 2, read serial number from existing address (rraw)

### UI/UX Flow Issues

- [x] Fix Phase 2 custom tests UI flow - when choosing "No" on shutdown screen, it shows "Run All Tests" summary screen instead of custom tests screen

## 🧪 Test Cases

### Validation Tests

- [x] Test behavior when all tests don't pass (don't proceed)
- [ ] Verify signal quality issues (3a appearing sometimes)
- [x] Test behaviour when device status API returns error (can proceed if not 200)
- [x] 3 partitions 
- [x] baud rate change successfull or not 
- [x] Main menu screen  ( tests , summary , loading , shutdown , error screens , confirmation screen ( display and sd&audio test)
- [x] Keypad , display , wifi , rtc ( coin ) , audio and sd test, ble test
- [x] modem test ( sim info , signal quality )
- [x] battery test ( audio is correct)
- [x] serial number , mac address 
- [x] validate efuse calibration values on the rmap 
- [x] Tofupilot Data ( phase 1 and phase 2)
- [x] Backend db API data sending in phase 2 
- [x] Transition to smart calculator



## 🚀 Pre-Deployment Tasks

- [x] Configure production environment for testing (dev and stage reads rmap only but tofupilot reads rraw)
- [x] Send serial number to Sentry
- [x] Rebase branch
- [x] update erase factory settings ticket
- [x] remove the one sound from audio and sd test 
- [x] API Call (Tofupilot and Device) - Increase timeout from 30 to 600 seconds  (try every 5 seconds not every 1 second )
- [x] have to check if i dont run efuse test in phase 2 , do i get the shutdown screen or not also does it sends the imei number or serial num
- [x] Phase 2: Test offline behavior (don't store locally, don't proceed)
- [x] phase 2 :  it stores the data locally and shows timeout after 20 seconds
- [x] i believe it doesn't check the attachment to check if it uploaded , it only check results only
- [x] mac address validation
- [x] running phase 2 without efuse test
- [x] dont write efuse address again if it already written
- [x] verify on pressing boot or power button it restarts  -restart 
	jour
	
	
	
	
	
	
## Future implementations :
	1. by default after failing the test , it should be on retry button not on  continue 
	2. add machine id if serial number and imei is not there
	3. add a check if they put the test sim again 
	4. remove to show the shutdown screen after 10 mins if data is not sent
	5. segregate phase 1 or phase 2 ( on tofupilot and factory mode )
	6. signal is 0 after activated sim also in production  (data and action plan needed )
	7. hardware version 1.0.0 ( investgate, verify and action)
	8. office testing and production testing ( daig 1 and diag 2)
	9. write wifi mac address in efuse ( reboot before wifi and bluetooth testing)
	10. in phase 2 ( remove the selecting of 3 tests  , it should be done automaticallly )
	11. led issue in hardware ( collect data from nithish)
	12. 