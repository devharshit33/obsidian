# Multi-SIM Support Implementation - Conversation Summary

## Initial Requirement

Add support for multiple SIM operators (Vi and Jio) to a calculator OS that currently only supports Airtel. The system should automatically detect which SIM is inserted and start the appropriate PPP service.

## System Context

- **Platform**: Calculator OS built with Yocto/OpenEmbedded
- **Current Setup**: Only Airtel connectivity supported
- **Modem Device**: `/dev/ttyRTK1` (Realtek RTK1)
- **Build System**: Using multiconfig builds (debug, recovery)
- **Current File Structure**:
    
    ```
    meta-tohands/recipes-connectivity/tohands-connectivity/├── files/│   ├── 78-mm-lynq-rtk1-enable.rules│   └── ppp/│       └── airtel/│           ├── airtel│           ├── airtel-connect│           └── airtel-disconnect└── tohands-connectivity.bb
    ```
    

## Solution Overview

Implemented automatic SIM detection that:

1. Detects inserted SIM operator using AT commands and IMSI
2. Starts the appropriate `ppp@<operator>.service`
3. Supports Airtel, Vi (Vodafone-Idea), and Jio

## Files Created/Modified

### 1. PPP Configuration Files for Vi

**files/ppp/vi/vi**

```
/dev/ttyRTK1
460800
nocrtscts
modem
debug
nodetach
usepeerdns
defaultroute
user ""
0.0.0.0:0.0.0.0
persist
holdoff 5
connect '/usr/sbin/chat -s -v -f /etc/chatscripts/vi-connect'
disconnect '/usr/sbin/chat -s -v -f /etc/chatscripts/vi-disconnect'
```

**files/ppp/vi/vi-connect**

```
ABORT "BUSY"
ABORT "NO CARRIER"
ABORT "ERROR"
TIMEOUT 5
"" AT
OK AT+CGDCONT=1,"IP","www"
OK ATD*99#
CONNECT ""
```

**files/ppp/vi/vi-disconnect**

```
ABORT "ERROR"
ABORT "NO DIALTONE"
SAY "\nDisconnecting Modem\n"
"" +++
"" +++
"" +++
SAY "\nGood Bye\n"
```

### 2. PPP Configuration Files for Jio

**files/ppp/jio/jio**

```
/dev/ttyRTK1
460800
nocrtscts
modem
debug
nodetach
usepeerdns
defaultroute
user ""
0.0.0.0:0.0.0.0
persist
holdoff 5
connect '/usr/sbin/chat -s -v -f /etc/chatscripts/jio-connect'
disconnect '/usr/sbin/chat -s -v -f /etc/chatscripts/jio-disconnect'
```

**files/ppp/jio/jio-connect**

```
ABORT "BUSY"
ABORT "NO CARRIER"
ABORT "ERROR"
TIMEOUT 5
"" AT
OK AT+CGDCONT=1,"IP","jionet"
OK ATD*99#
CONNECT ""
```

**files/ppp/jio/jio-disconnect**

```
ABORT "ERROR"
ABORT "NO DIALTONE"
SAY "\nDisconnecting Modem\n"
"" +++
"" +++
"" +++
SAY "\nGood Bye\n"
```

### 3. SIM Detection Script

**files/sim-detect.sh**

```bash
#!/bin/bash

# Simple SIM detection script
LOG_FILE="/var/log/sim-detect.log"
MODEM_DEVICE="/dev/ttyRTK1"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Function to send AT command and get response
send_at_command() {
    local cmd="$1"
    local response
    
    # Send command and wait for response
    stty -F "$MODEM_DEVICE" 460800 raw -echo
    echo -e "${cmd}\r" > "$MODEM_DEVICE"
    sleep 1
    response=$(timeout 2 cat "$MODEM_DEVICE" 2>/dev/null | tr -d '\r' | grep -v "^$")
    echo "$response"
}

detect_sim_operator() {
    log "Starting SIM detection..."
    
    # Wait for modem to initialize
    sleep 5
    
    # Check if modem is responding
    local at_response=$(send_at_command "AT")
    if [[ ! "$at_response" =~ "OK" ]]; then
        log "Modem not responding to AT commands"
        return 1
    fi
    
    # Get operator information
    local cops_response=$(send_at_command "AT+COPS?")
    log "COPS response: $cops_response"
    
    # Get IMSI for operator detection
    local imsi_response=$(send_at_command "AT+CIMI")
    log "IMSI response: $imsi_response"
    
    # Extract IMSI number
    local imsi=$(echo "$imsi_response" | grep -E '^[0-9]{15}$' | head -1)
    
    if [ -n "$imsi" ]; then
        local mcc_mnc="${imsi:0:5}"
        log "MCC-MNC: $mcc_mnc"
        
        # Detect operator based on MCC-MNC
        case "$mcc_mnc" in
            # Airtel codes
            40410|40440|40470|40490|40492|40493|40494|40495|40496|40497|40498)
                echo "airtel"
                return 0
                ;;
            # Vi (Vodafone-Idea) codes
            40401|40405|40411|40413|40415|40420|40427|40430|40443|40446|40460|40484|40486|40488)
                echo "vi"
                return 0
                ;;
            # Jio codes - Note: Jio uses 6-digit MNC
            40585|40586|40587|40588|40589|40590|40591|40592|40593|40594|40595|40596|40597)
                echo "jio"
                return 0
                ;;
        esac
    fi
    
    # Fallback: Check operator name in COPS response
    if echo "$cops_response" | grep -qi "airtel\|bharti"; then
        echo "airtel"
        return 0
    elif echo "$cops_response" | grep -qi "vodafone\|idea\|vi"; then
        echo "vi"
        return 0
    elif echo "$cops_response" | grep -qi "jio\|reliance"; then
        echo "jio"
        return 0
    fi
    
    log "Could not detect operator"
    return 1
}

# Main execution
log "SIM detection service started"

# Stop all PPP services
systemctl stop ppp@airtel.service 2>/dev/null
systemctl stop ppp@vi.service 2>/dev/null
systemctl stop ppp@jio.service 2>/dev/null

# Detect operator
OPERATOR=$(detect_sim_operator)

if [ -n "$OPERATOR" ]; then
    log "Detected operator: $OPERATOR"
    
    # Start the appropriate service
    log "Starting ppp@${OPERATOR}.service"
    systemctl start "ppp@${OPERATOR}.service"
    
    # Verify service started
    sleep 3
    if systemctl is-active --quiet "ppp@${OPERATOR}.service"; then
        log "Successfully started ppp@${OPERATOR}.service"
        exit 0
    else
        log "Failed to start ppp@${OPERATOR}.service"
        exit 1
    fi
else
    log "No SIM detected or unsupported operator"
    exit 1
fi
```

### 4. Systemd Service

**files/sim-detect.service**

```ini
[Unit]
Description=SIM Card Detection and PPP Service Starter
After=multi-user.target
Wants=network.target

[Service]
Type=oneshot
ExecStart=/usr/bin/sim-detect.sh
RemainAfterExit=yes
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

### 5. Updated Yocto Recipe

**tohands-connectivity.bb**

```bash
SUMMARY = "Tohands Connectivity Settings"
LICENSE = "CLOSED"
LIC_FILES_CHKSUM = ""

SRC_URI = "file://ppp/ \
    file://78-mm-lynq-rtk1-enable.rules \
    file://sim-detect.sh \
    file://sim-detect.service"

# Use standard paths instead of /usr/local
FILES:${PN} += "${sysconfdir}/udev/rules.d/"
FILES:${PN} += "${sysconfdir}/ppp/"
FILES:${PN} += "${sysconfdir}/chatscripts/"
FILES:${PN} += "${bindir}/"
FILES:${PN} += "${systemd_system_unitdir}/"
FILES:${PN} += "${sysconfdir}/systemd/system/multi-user.target.wants/"

MULTIUSER_TARGETS_DIR = "${sysconfdir}/systemd/system/multi-user.target.wants"

do_install() {
    # Install udev rules
    install -D -m 600 ${WORKDIR}/78-mm-lynq-rtk1-enable.rules ${D}${sysconfdir}/udev/rules.d/78-mm-lynq-rtk1-enable.rules

    # Install Airtel configs
    install -D -m 600 ${WORKDIR}/ppp/airtel/airtel ${D}${sysconfdir}/ppp/peers/airtel
    install -D -m 600 ${WORKDIR}/ppp/airtel/airtel-connect ${D}${sysconfdir}/chatscripts/airtel-connect
    install -D -m 600 ${WORKDIR}/ppp/airtel/airtel-disconnect ${D}${sysconfdir}/chatscripts/airtel-disconnect
    
    # Install Vi configs
    install -D -m 600 ${WORKDIR}/ppp/vi/vi ${D}${sysconfdir}/ppp/peers/vi
    install -D -m 600 ${WORKDIR}/ppp/vi/vi-connect ${D}${sysconfdir}/chatscripts/vi-connect
    install -D -m 600 ${WORKDIR}/ppp/vi/vi-disconnect ${D}${sysconfdir}/chatscripts/vi-disconnect
    
    # Install Jio configs
    install -D -m 600 ${WORKDIR}/ppp/jio/jio ${D}${sysconfdir}/ppp/peers/jio
    install -D -m 600 ${WORKDIR}/ppp/jio/jio-connect ${D}${sysconfdir}/chatscripts/jio-connect
    install -D -m 600 ${WORKDIR}/ppp/jio/jio-disconnect ${D}${sysconfdir}/chatscripts/jio-disconnect
    
    # Install SIM detection script to standard bin directory
    install -D -m 755 ${WORKDIR}/sim-detect.sh ${D}${bindir}/sim-detect.sh
    
    # Install systemd service
    install -D -m 644 ${WORKDIR}/sim-detect.service ${D}${systemd_system_unitdir}/sim-detect.service

    # Create directory for systemd links
    install -d ${D}${sysconfdir}/systemd/system/multi-user.target.wants
    
    # Enable SIM detection service
    ln -s ${systemd_system_unitdir}/sim-detect.service ${D}${MULTIUSER_TARGETS_DIR}/sim-detect.service
    
    # Keep existing WPA supplicant service
    ln -s ${libdir}/systemd/system/wpa_supplicant@wlan0.service ${D}${MULTIUSER_TARGETS_DIR}/wpa_supplicant@wlan0.service
}

inherit systemd

SYSTEMD_SERVICE:${PN} = "sim-detect.service"
SYSTEMD_AUTO_ENABLE:${PN} = "enable"
```

## Issues Encountered and Solutions

### Issue 1: Missing modem-monitor.sh

**Error**: `install: cannot stat 'modem-monitor.sh': No such file or directory` **Solution**: Removed the modem-monitor functionality from the initial implementation to simplify

### Issue 2: QA Issue - Files not shipped

**Error**: `Files/directories were installed but not shipped in any package` **Solution**:

- Updated to new Yocto syntax (using colons `:` instead of underscores `_`)
- Changed from `/usr/local/bin` to `/usr/bin` (using `${bindir}`)
- Added all installed paths to `FILES:${PN}`

## Key Technical Details

### MCC-MNC Codes Used

- **Airtel**: 40410, 40440, 40470, 40490-40498
- **Vi**: 40401, 40405, 40411, 40413, 40415, 40420, 40427, 40430, 40443, 40446, 40460, 40484, 40486, 40488
- **Jio**: 40585-40597 (5-digit codes)

### APNs Configured

- **Airtel**: "iot.com" (as per existing configuration)
- **Vi**: "www"
- **Jio**: "jionet"

### Detection Method

1. Sends AT commands to modem
2. Gets IMSI (International Mobile Subscriber Identity)
3. Extracts MCC-MNC from IMSI
4. Maps MCC-MNC to operator
5. Falls back to operator name detection if IMSI method fails

## Build and Testing Steps

### Build Commands

```bash
# Create directory structure
cd ~/Projects/calculator-os/layers/meta-tohands/recipes-connectivity/tohands-connectivity/files/ppp
mkdir -p vi jio

# Clean and rebuild
bitbake -c cleansstate tohands-connectivity
bitbake tohands-connectivity

# Full image rebuild
bitbake -c build -k mc:debug:ameba-image-core ameba-image-userdata mc:recovery:ameba-image-recovery ameba-image-factory-userdata
```

### Testing Commands

```bash
# Manual AT command testing
stty -F /dev/ttyRTK1 460800 raw -echo
cat /dev/ttyRTK1 &
echo -e "AT+CIMI\r" > /dev/ttyRTK1  # Get IMSI
echo -e "AT+COPS?\r" > /dev/ttyRTK1  # Get operator
killall cat

# After deployment
systemctl status sim-detect.service
/usr/bin/sim-detect.sh  # Manual run
journalctl -u sim-detect.service -f  # View logs
systemctl status 'ppp@*'  # Check which PPP service is running
ifconfig ppp0  # Check network interface
```

## How It Works

1. At boot, `sim-detect.service` runs once
2. The script detects the SIM operator using AT commands
3. Based on detection, it starts the appropriate `ppp@<operator>.service`
4. Each operator has its own PPP configuration with correct APN

## Additional Notes

- The solution uses direct AT commands instead of ModemManager for simplicity
- Logs are written to `/var/log/sim-detect.log`
- All PPP services are stopped before starting the detected one
- The script waits 5 seconds for modem initialization
- Service remains active after execution (`RemainAfterExit=yes`)

## Potential Improvements

- Add ModemManager support for more robust detection
- Implement hot-swap monitoring
- Add support for more operators
- Configure operator-specific authentication if needed