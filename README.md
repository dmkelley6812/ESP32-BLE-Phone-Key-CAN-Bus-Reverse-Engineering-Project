# ESP32-BLE-Phone-Key-CAN-Bus-Reverse-Engineering-Project
A DIY Tesla-style phone key built for a 2017 GMC Sierra 1500, using an ESP32 and CAN transceiver wired into the vehicle's OBD-II port.


# The Plan
- Build a CAN bus sniffer using an ESP32 board with a CAN transciever. Sniff CAN messages and attempt to isolate the LOCK and UNLOCK signals in a software (possibly SavvyCAN)
- Create a CAN injection device (again on ESP32) along with BLE RSSI detection to automatically unlock doors when a paired phone approaches and lock doors when phone leaves

# Considerations
- This involves live CAN injection. How do we ensure risks are mitigated?
  -  Gear/Ignition state gating, rate limiting of lock/unlock commands, RSSI threshold/Hysteresis
-  Accessing relevant CAN frames
  - It is possible that the relevant CAN frames may be blocked by a CAN Gateway. In this case, it will be necessary to tap CAN signals behind Gateway close to BCM. This is more invasive than simple OBD-II dongle.
- Security: Pairing Phone, Ensuring only correctly paired phones have access
  - Physical button on the device to enter pairing mode
  - PIN display on the device for Passkey entry
  - MAC address whitelisting once phone is paired
  - Companion phone app with shared secret challenge/nonce to prevent message replay/spoofing
 
  
# CAN Signals Needed
- Ignition Status
- Gear (P)
- Lock
- Unlock

# Hardware
- ESP32 Board
- CAN Transceiver (sn65hvd230)
- OBDII Dongle/Pigtail

# Resources
- CABANBA: https://github.com/commaai/openpilot/tree/master/tools/cabana
- GMLAN Bible: https://docs.google.com/spreadsheets/d/1qEwOXSr3bWoc2VUhpuIam236OOZxPc2hxpLUsV0xkn0/edit?gid=12#gid=12
- ESP32 TWAI/SLCAN firmware: https://github.com/mintynet/esp32-slcan



# Current Status

As part of the CAN signal reverse engineering process, I realized I didn't really like using SavvyCAN. It seems so old school and clunky, so I built a new browser based CAN anlysis tool called browserCAN. You can find it at browsercan.netlify.app
<img width="1893" height="983" alt="image" src="https://github.com/user-attachments/assets/53c843e5-644f-40b6-a08b-5573cfc17e49" />

Browser can is 100% client side using the web serial API to interface with an SLCAN hardware device. In its current iteration, it offers SLCAN formatted CAN file upload and analysis as well as live CAN data streaming via the Web Serial API with frame analysis, byte and bit graphing, and SignalScout reverse engineering tools. 

I've also built the CAN bus sniffer using a generic ESP32 Dev Module and a CAN transciever wired up to an ODBII pigtail. 


# Up Next:
- Continued iterations of browserCAN web app
- Perform the actual reverse engineering of my truck's CAN signals to try to isolate the relevant signals needed to send lock/unlock commands and check vehicle status.
- Write firmware for another ESP32 to handle the actual phone key logic and CAN injection
