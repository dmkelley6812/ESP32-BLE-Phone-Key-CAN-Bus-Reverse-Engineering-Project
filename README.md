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
 
  

  
