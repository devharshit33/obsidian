run custom test first and then run all test 

logs - root@rtl8730elh-va8-harshitdev:~# calculator_diagnostic 
Starting calculator diagnostic app...
Initializing LVGL...
drm: Found plane_id: 31 connector_id: 36 crtc_id: 34
drm: 480x960 (0mm X 0mm) pixel format RG16
DRM subsystem and buffer mapped successfully
LVGL initialized successfully
Creating LVGL thread...
LVGL thread created successfully
Creating test manager...
LVGL task handler started
Creating and registering tests...
[KeypadTest] Constructor called.
[DisplayTest] Constructor called.
[WifiTest] Constructor called.
[ModemTest] Constructor called.
[RtcTest] Constructor called.
[AudioSDTest] Constructor called.
[BleTest] Constructor called.
[BatteryTest] Constructor called.
[EFuseTest] Constructor called.
MainScreen init with 9 tests.
Showing main menu
Entering main loop...
Key pressed: 106
Key pressed: 96
Starting custom test selection...
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
Key pressed: 106
Key pressed: 96
retryTest(1) - blocking approach
Executing test 1: Display Test
Test state changed: 1 => 1 (Display Test)
Test state changed: 1 => 1 (Display Test)
[DisplayTest] Initializing...
Key pressed: 106
Key pressed: 96
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
retryTest(2) - blocking approach
Executing test 2: Wifi Test
Test state changed: 2 => 1 (Wifi Test)
Test state changed: 2 => 1 (Wifi Test)
[WifiTest] Initializing Wi-Fi Test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/wifi_test/wifi_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[WifiTest] wlan0 interface is up.
[WifiTest] Scanning for networks...

Available Networks:
--------------------------------------------------------------------------------
SSID                Signal         Quality        Security
--------------------------------------------------------------------------------
tohands             -54            Good           [WPA2-PSK-CCMP][ESS]
--------------------------------------------------------------------------------
[WLAN-A] set BSSID: ca:4d:5e:5f:eb:87
[WLAN-A] set ssid tohands
[WLAN-A] start auth to ca:4d:5e:5f:eb:87
[WLAN-A] sta recv deauth 1 ca:4d:5e:5f:eb:87
[WLAN-A] start auth to ca:4d:5e:5f:eb:87
[WLAN-A] auth success, start assoc
[WLAN-A] assoc success(8)
fullmac-8730e 40000000.axi_wlan: INIC_WLAN_GET_GW is not supported. Add into global_idev if needed.
[WLAN-A] set pairwise key 4(WEP40-1 WEP104-5 TKIP-2 AES-4)
[WLAN-A] set group key 4 1
[WLAN-I] set cam: gtk alg 4 0
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/wifi_test/wifi_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 2 => 2 (Wifi Test)
showResultScreen for: Wifi Test => PASS
Test state changed: 2 => 2 (Wifi Test)
showResultScreen for: Wifi Test => PASS
[WifiTestScreen] Starting cleanup
[WifiTestScreen] Cleanup complete
retryTest(4) - blocking approach
Executing test 4: RTC Test
Test state changed: 4 => 1 (RTC Test)
Test state changed: 4 => 1 (RTC Test)
[RtcTest] Initializing RTC test
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/rtc_test/rtc_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[RtcTest] Time difference: 61 seconds
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/rtc_test/rtc_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 4 => 2 (RTC Test)
showResultScreen for: RTC Test => PASS
Test state changed: 4 => 2 (RTC Test)
showResultScreen for: RTC Test => PASS
[RtcTest] Cleaning up RTC test
retryTest(5) - blocking approach
Executing test 5: Audio & SD Test
Test state changed: 5 => 1 (Audio & SD Test)
Test state changed: 5 => 1 (Audio & SD Test)
[AudioSDTest] Initializing Audio & SD Card Test...
[AudioSDTest] Verifying partition: /rom/mnt/storage/mmcblk0p1
[AudioSDTest] Successfully verified partition: /rom/mnt/storage/mmcblk0p1
[AudioSDTest] Verifying partition: /rom/mnt/storage/mmcblk0p2
[AudioSDTest] Successfully verified partition: /rom/mnt/storage/mmcblk0p2
[AudioSDTest] Verifying partition: /rom/mnt/storage/mmcblk0p3
[AudioSDTest] Successfully verified partition: /rom/mnt/storage/mmcblk0p3
Key pressed: 106
Key pressed: 96
Test state changed: 5 => 3 (Audio & SD Test)
showResultScreen for: Audio & SD Test => FAIL
Test state changed: 5 => 3 (Audio & SD Test)
showResultScreen for: Audio & SD Test => FAIL
Test state changed: 5 => 3 (Audio & SD Test)
showResultScreen for: Audio & SD Test => FAIL
retryTest(6) - blocking approach
Executing test 6: BLE Test
Test state changed: 6 => 1 (BLE Test)
Test state changed: 6 => 1 (BLE Test)
[BleTest] Initializing BLE test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/ble_test/ble_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[BleTest] Adapter power state: ON
rtk_btcoex: hci (periodic)inq start
rtk_btcoex: inquiry complete
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/ble_test/ble_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 6 => 2 (BLE Test)
showResultScreen for: BLE Test => PASS
Test state changed: 6 => 2 (BLE Test)
showResultScreen for: BLE Test => PASS
[BleTest] Cleanup complete.
retryTest(7) - blocking approach
Executing test 7: Battery Test
Test state changed: 7 => 1 (Battery Test)
Test state changed: 7 => 1 (Battery Test)
[BatteryTest] Initializing battery test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/battery_test/please_connect_charger.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 7 => 3 (Battery Test)
showResultScreen for: Battery Test => FAIL
Test state changed: 7 => 3 (Battery Test)
showResultScreen for: Battery Test => FAIL
[BatteryTest] Cleanup complete.
retryTest(8) - blocking approach
Executing test 8: EFuse Test
Test state changed: 8 => 1 (EFuse Test)
Test state changed: 8 => 1 (EFuse Test)
[EFuseTest] Initializing EFuse Test...
Test state changed: 8 => 3 (EFuse Test)
showResultScreen for: EFuse Test => FAIL
Test state changed: 8 => 3 (EFuse Test)
showResultScreen for: EFuse Test => FAIL
[EFuseTest] Cleanup complete.
Showing summary for SUBSET of tests (7)...
Key pressed: 106
Key pressed: 106
Key pressed: 106
Key pressed: 106
Key pressed: 106
Key pressed: 106
Key pressed: 106
Key pressed: 105
Key pressed: 105
Key pressed: 105
Key pressed: 106
Key pressed: 105
Key pressed: 106
Key pressed: 105
Key pressed: 106
Key pressed: 105
Key pressed: 106
Key pressed: 102
Showing main menu
Key pressed: 105
Key pressed: 96
Starting all tests (blocking approach)...
Starting all tests execution (blocking approach)...
Test manager state changed: 1
Test manager state changed: 2
Executing test 0: Keypad Test
Test state changed: 0 => 1 (Keypad Test)
Test state changed: 0 => 1 (Keypad Test)
[KeypadTest] Initializing keypad test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/keypad_test/press_all_keys.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Key pressed: 116
Key pressed: 105
Key pressed: 106
Key pressed: 117
Key pressed: 14
Key pressed: 42
Detected button press 1 with key code 42
Key pressed: 14
Detected button press 2 with key code 14
Key pressed: 61
Detected button press 3 with key code 61
Key pressed: 42
Key pressed: 14
Key pressed: 210
Detected button press 4 with key code 210
Key pressed: 42
Key pressed: 61
Key pressed: 210
Key pressed: 60
Detected button press 5 with key code 60
Key pressed: 61
Key pressed: 59
Detected button press 6 with key code 59
Key pressed: 60
Key pressed: 60
Key pressed: 61
Key pressed: 72
Detected button press 7 with key code 72
Key pressed: 102
Detected button press 8 with key code 102
Key pressed: 73
Detected button press 9 with key code 73
Key pressed: 71
Detected button press 10 with key code 71
Key pressed: 72
Key pressed: 98
Detected button press 11 with key code 98
Key pressed: 72
Key pressed: 98
Key pressed: 63
Detected button press 12 with key code 63
Key pressed: 98
Key pressed: 63
Key pressed: 115
Detected button press 13 with key code 115
Key pressed: 114
Detected button press 14 with key code 114
Key pressed: 63
Key pressed: 98
Key pressed: 62
Detected button press 15 with key code 62
Key pressed: 98
Key pressed: 104
Detected button press 16 with key code 104
Key pressed: 74
Detected button press 17 with key code 74
Key pressed: 55
Detected button press 18 with key code 55
Key pressed: 74
Key pressed: 104
Key pressed: 76
Detected button press 19 with key code 76
Key pressed: 77
Detected button press 20 with key code 77
Key pressed: 55
Key pressed: 96
Detected button press 21 with key code 96
Key pressed: 83
Detected button press 22 with key code 83
Key pressed: 78
Detected button press 23 with key code 78
Key pressed: 355
Detected button press 24 with key code 355
Key pressed: 82
Detected button press 25 with key code 82
Key pressed: 75
Detected button press 26 with key code 75
Key pressed: 78
Key pressed: 80
Detected button press 27 with key code 80
Key pressed: 81
Detected button press 28 with key code 81
Key pressed: 79
Detected button press 29 with key code 79
Key pressed: 111
Detected button press 30 with key code 111
Key pressed: 78
Key pressed: 78
Key pressed: 81
Key pressed: 80
Key pressed: 104
Key pressed: 78
Key pressed: 109
Detected button press 31 with key code 109
Key pressed: 78
Key pressed: 74
Key pressed: 109
Key pressed: 96
Key pressed: 83
Key pressed: 39
Detected button press 32 with key code 39
Key pressed: 39
Key pressed: 82
Key pressed: 109
Key pressed: 102
Key pressed: 60
Key pressed: 71
Key pressed: 75
Key pressed: 355
Key pressed: 75
Key pressed: 79
Key pressed: 111
Key pressed: 79
Key pressed: 39
Key pressed: 82
Key pressed: 60
Key pressed: 42
Key pressed: 210
Key pressed: 59
Key pressed: 42
Key pressed: 210
Key pressed: 116
Detected button press 33 with key code 116
Key pressed: 106
Detected button press 34 with key code 106
Key pressed: 105
Detected button press 35 with key code 105
Key pressed: 106
Key pressed: 117
Detected button press 36 with key code 117
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/keypad_test/keypad_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Key pressed: 14
Key pressed: 14
Test state changed: 0 => 2 (Keypad Test)
showResultScreen for: Keypad Test => PASS
Test state changed: 0 => 2 (Keypad Test)
showResultScreen for: Keypad Test => PASS
[KeypadTest] Cleaning up...
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 1: Display Test
Test state changed: 1 => 1 (Display Test)
Test state changed: 1 => 1 (Display Test)
[DisplayTest] Initializing...
Key pressed: 96
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Test state changed: 1 => 3 (Display Test)
showResultScreen for: Display Test => FAIL
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 2: Wifi Test
Test state changed: 2 => 1 (Wifi Test)
Test state changed: 2 => 1 (Wifi Test)
[WifiTest] Initializing Wi-Fi Test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/wifi_test/wifi_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[WifiTest] wlan0 interface is up.
[WifiTest] Scanning for networks...

Available Networks:
--------------------------------------------------------------------------------
SSID                Signal         Quality        Security
--------------------------------------------------------------------------------
tohands             -64            Fair           [WPA2-PSK-CCMP][ESS]
--------------------------------------------------------------------------------
[WLAN-A] set BSSID: ca:4d:5e:5f:eb:87
[WLAN-A] set ssid tohands
[WLAN-A] start auth to ca:4d:5e:5f:eb:87
[WLAN-A] sta recv deauth 1 ca:4d:5e:5f:eb:87
[WLAN-A] start auth to ca:4d:5e:5f:eb:87
[WLAN-A] auth success, start assoc
[WLAN-A] assoc success(9)
fullmac-8730e 40000000.axi_wlan: INIC_WLAN_GET_GW is not supported. Add into global_idev if needed.
[WLAN-A] set pairwise key 4(WEP40-1 WEP104-5 TKIP-2 AES-4)
[WLAN-A] set group key 4 1
[WLAN-I] set cam: gtk alg 4 0
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/wifi_test/wifi_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 2 => 2 (Wifi Test)
showResultScreen for: Wifi Test => PASS
Test state changed: 2 => 2 (Wifi Test)
showResultScreen for: Wifi Test => PASS
[WifiTestScreen] Starting cleanup
[WifiTestScreen] Cleanup complete
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 3: Modem Test
Test state changed: 3 => 1 (Modem Test)
Test state changed: 3 => 1 (Modem Test)
[ModemTest] Initializing modem test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/modem_test/modem_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[ModemTest] Initialization complete.
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/modem_test/modem_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 3 => 2 (Modem Test)
showResultScreen for: Modem Test => PASS
Test state changed: 3 => 2 (Modem Test)
showResultScreen for: Modem Test => PASS
[ModemTest] Cleanup completed.
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 4: RTC Test
Test state changed: 4 => 1 (RTC Test)
Test state changed: 4 => 1 (RTC Test)
[RtcTest] Initializing RTC test
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/rtc_test/rtc_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[RtcTest] Time difference: 61 seconds
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/rtc_test/rtc_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 4 => 2 (RTC Test)
showResultScreen for: RTC Test => PASS
Test state changed: 4 => 2 (RTC Test)
showResultScreen for: RTC Test => PASS
[RtcTest] Cleaning up RTC test
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 5: Audio & SD Test
Test state changed: 5 => 1 (Audio & SD Test)
Test state changed: 5 => 1 (Audio & SD Test)
[AudioSDTest] Initializing Audio & SD Card Test...
[AudioSDTest] Verifying partition: /rom/mnt/storage/mmcblk0p1
[AudioSDTest] Successfully verified partition: /rom/mnt/storage/mmcblk0p1
[AudioSDTest] Verifying partition: /rom/mnt/storage/mmcblk0p2
[AudioSDTest] Successfully verified partition: /rom/mnt/storage/mmcblk0p2
[AudioSDTest] Verifying partition: /rom/mnt/storage/mmcblk0p3
[AudioSDTest] Successfully verified partition: /rom/mnt/storage/mmcblk0p3
Key pressed: 96
Test state changed: 5 => 3 (Audio & SD Test)
showResultScreen for: Audio & SD Test => FAIL
Test state changed: 5 => 3 (Audio & SD Test)
showResultScreen for: Audio & SD Test => FAIL
Test state changed: 5 => 3 (Audio & SD Test)
showResultScreen for: Audio & SD Test => FAIL
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 6: BLE Test
Test state changed: 6 => 1 (BLE Test)
Test state changed: 6 => 1 (BLE Test)
[BleTest] Initializing BLE test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/ble_test/ble_test_started.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
[BleTest] Adapter power state: ON
rtk_btcoex: hci (periodic)inq start
rtk_btcoex: inquiry complete
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/ble_test/ble_test_passed.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 6 => 2 (BLE Test)
showResultScreen for: BLE Test => PASS
Test state changed: 6 => 2 (BLE Test)
showResultScreen for: BLE Test => PASS
[BleTest] Cleanup complete.
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 7: Battery Test
Test state changed: 7 => 1 (Battery Test)
Test state changed: 7 => 1 (Battery Test)
[BatteryTest] Initializing battery test...
Playing WAVE '/usr/share/calculator-diagnostic/assets/audio_files/battery_test/please_connect_charger.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Mono
Test state changed: 7 => 3 (Battery Test)
showResultScreen for: Battery Test => FAIL
Test state changed: 7 => 3 (Battery Test)
showResultScreen for: Battery Test => FAIL
[BatteryTest] Cleanup complete.
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Executing test 8: EFuse Test
Test state changed: 8 => 1 (EFuse Test)
Test state changed: 8 => 1 (EFuse Test)
[EFuseTest] Initializing EFuse Test...
Test state changed: 8 => 3 (EFuse Test)
showResultScreen for: EFuse Test => FAIL
Test state changed: 8 => 3 (EFuse Test)
showResultScreen for: EFuse Test => FAIL
[EFuseTest] Cleanup complete.
Key pressed: 96
User pressed CONTINUE => setUserAction(CONTINUE)
User chose CONTINUE -> proceed to next test.
Test manager state changed: 4
All tests done (blocking approach).
Showing summary for ALL tests (9)...
Segmentation fault
root@rtl8730elh-va8-harshitdev:~# 