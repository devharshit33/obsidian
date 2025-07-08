

# Rejected Units Tracking Log

## Overview

This document tracks units rejected from production, including verification results and resolution actions.

## Tracking Table

| Serial Number | Rejection Date | Labeled Issue        | Verification Findings                   | Actions Taken                              | Result   | Resolution Details                                   |
| ------------- | -------------- | -------------------- | --------------------------------------- | ------------------------------------------ | -------- | ---------------------------------------------------- |
| 00001         | 2025-05-15     | Display failure      | Confirmed - Display not powering on     | Replaced display cable, reflashed firmware | Resolved | Cable was loose, not properly seated during assembly |
| 00002         | 2025-05-16     | Battery issue        | Different - Power management IC failure | Replaced PMIC, retested                    | Resolved | Root cause: Faulty PMIC causing battery drain        |
| 00003         | 2025-05-18     | Touch not responding | Confirmed - Touch calibration error     | Reflashed touch firmware, recalibrated     | Resolved | Calibration issue during production                  |
|               |                |                      |                                         |                                            |          |                                                      |
|               |                |                      |                                         |                                            |          |                                                      |

## Field Descriptions

1. **Serial Number**: Unique identifier of the rejected unit
2. **Rejection Date**: Date when the unit was rejected/received
3. **Labeled Issue**: Original reason for rejection as labeled on the device
4. **Verification Findings**: Results after verification (Confirmed/Different - with details)
5. **Actions Taken**: Steps performed to diagnose/fix (flashing, testing, part replacement)
6. **Result**: Current status (Resolved/Unresolved)
7. **Resolution Details**: Explanation of root cause and how it was fixed (if resolved)

## Monthly Summary Section

### May 2025

- Total units rejected: 3
- Issues confirmed correct: 2
- Issues misidentified: 1
- Resolution rate: 100%
- Most common issue: N/A (varied issues)

### June 2025

- Total units rejected:
- Issues confirmed correct:
- Issues misidentified:
- Resolution rate:
- Most common issue:

## Common Issues Reference

| Issue Type      | Common Symptoms                   | Standard Resolution Path                               | Average Resolution Time |
| --------------- | --------------------------------- | ------------------------------------------------------ | ----------------------- |
| Display Failure | No display, flickering, lines     | Check cable, replace display if needed, reflash        | 1-2 hours               |
| Battery Issues  | No power, short life, overheating | Test battery, check PMIC, replace if needed            | 2-3 hours               |
| Connectivity    | WiFi/BT/Cell signal issues        | Test antennas, reflash radio firmware                  | 1-2 hours               |
| Audio Issues    | No sound, distortion              | Test speakers/mic, check audio IC                      | 1 hour                  |