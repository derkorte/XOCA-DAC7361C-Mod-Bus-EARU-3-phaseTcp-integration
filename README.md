# XOCA-DAC7361C-Mod-Bus-EARU-3-phaseTcp-integration
Integrate XOCA 3Phase Smart Meter with ESP32 via ESP Home to Home Assistant 


What you need:

ESP32 (may 8266 work to), TTL to RS485 Modul like: 

https://www.amazon.de/Heemol-Converter-Automatische-Durchflusskontrolle-F%C3%BCrUart-Serie/dp/B0CFF9MWFR/ref=sr_1_11?crid=10JP1A8RAR502&dib=eyJ2IjoiMSJ9.yu6vA7tBtzwzYtxe1dfgYZdUDTXoE5gyiKPc49-ir6Aj4Y3R7gEX-GDzdg1lTowrLKEEvVHgJLbtCW4oVnxnAho3cvonjZWhxDsV2PmbZbADN4WaAb2O6kGgCfXhvYWzqf7BlLn6T6Syzuf7GH0Zkon37r5ahZrzhyGg9ZXOZhQvghgM-RwJXbfKFI9HmCOvFNatpqAep53BJ38l4XA4mjNQkj7ETXvoT7vbhQOr8nU.7WgOQiH8C1P6Zcf70GBbV1gBVG0beZA52PrelEMPSeQ&dib_tag=se&keywords=ttl+rs485&qid=1722629589&sprefix=ttl+r%2Caps%2C99&sr=8-11 

Wire the TTL to RS485 module to the ESP:

VCC -> VCC
2times GRD -> GRD
RXD-> RXD
TXD-> TXD
A-> A
B-> B

from the TTL Module:

A -> A of Smart Meter
B -> B of Smart Meter

easy^^ 

add the ESP Home integration (AddON) to Home Assistant

Navigate: Settings->AddOns->ESP-Home-> open user interface

plug in the esp via usb and create a new device. Prepare for first use.

After succesfull installation your device should be noticed online, hit edit.

Copy and paste (in code mode) following to you file right after the ESP section:

uart:
  id: mod_uart
  tx_pin: GPIO3
  rx_pin: GPIO1
  baud_rate: 9600

modbus:
  id: modbus1
  uart_id: mod_uart

modbus_controller:
- id: modbus_stromzaehler #if you change the id you have to change every modbus_controller_id in every sensor!
  address: 1
  update_interval: 5s

Scroll down to the end of file and add:


sensor:

  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Line to Neutral Volts"
    register_type: holding
    address: 0x00
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Line to Neutral Volts"
    register_type: holding
    address: 0x02
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Line to Neutral Volts"
    register_type: holding
    address: 0x04
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Current"
    register_type: holding
    address: 0x06
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Current"
    register_type: holding
    address: 0x08
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Current"
    register_type: holding
    address: 0x0A
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Active Power"
    register_type: holding
    address: 0x0C
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Active Power"
    register_type: holding
    address: 0x0E
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Active Power"
    register_type: holding
    address: 0x10
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Reactive Power"
    register_type: holding
    address: 0x12
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Reactive Power"
    register_type: holding
    address: 0x14
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Reactive Power"
    register_type: holding
    address: 0x16
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Apparent Power"
    register_type: holding
    address: 0x18
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Apparent Power"
    register_type: holding
    address: 0x1A
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Apparent Power"
    register_type: holding
    address: 0x1C
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Power Factor"
    register_type: holding
    address: 0x1E
    value_type: S_WORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Power Factor"
    register_type: holding
    address: 0x1F
    value_type: S_WORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Power Factor"
    register_type: holding
    address: 0x20
    value_type: S_WORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Phase Angle"
    register_type: holding
    address: 0x21
    value_type: S_WORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Phase Angle"
    register_type: holding
    address: 0x22
    value_type: S_WORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Phase Angle"
    register_type: holding
    address: 0x23
    value_type: S_WORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Line 1 to Line 2 Volts"
    register_type: holding
    address: 0x24
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Line 2 to Line 3 Volts"
    register_type: holding
    address: 0x26
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Line 3 to Line 1 Volts"
    register_type: holding
    address: 0x28
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Frequency of Supply Voltages"
    register_type: holding
    address: 0x2A
    value_type: S_WORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Active Power"
    register_type: holding
    address: 0x2C
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Reactive Power"
    register_type: holding
    address: 0x2E
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Apparent Power"
    register_type: holding
    address: 0x30
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Power Factor"
    register_type: holding
    address: 0x32
    value_type: S_WORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Phase Angle"
    register_type: holding
    address: 0x33
    value_type: S_WORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Sum of Line Currents"
    register_type: holding
    address: 0x34
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Average Line to Neutral Volts"
    register_type: holding
    address: 0x36
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Average Line to Line Volts"
    register_type: holding
    address: 0x38
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Average Line Current"
    register_type: holding
    address: 0x3A
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Neutral Current"
    register_type: holding
    address: 0x3C
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Active Power Demand"
    register_type: holding
    address: 0x66
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Reactive Power Demand"
    register_type: holding
    address: 0x68
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total System Apparent Power Demand"
    register_type: holding
    address: 0x6A
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 1 Current Demand"
    register_type: holding
    address: 0x6C
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 2 Current Demand"
    register_type: holding
    address: 0x6E
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Phase 3 Current Demand"
    register_type: holding
    address: 0x70
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Maximum Total System Active Power Demand"
    register_type: holding
    address: 0x7C
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Maximum Total System Reactive Power Demand"
    register_type: holding
    address: 0x7E
    value_type: S_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Maximum Total System Apparent Power Demand"
    register_type: holding
    address: 0x80
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Maximum Phase 1 Current Demand"
    register_type: holding
    address: 0x82
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Maximum Phase 2 Current Demand"
    register_type: holding
    address: 0x84
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Maximum Phase 3 Current Demand"
    register_type: holding
    address: 0x86
    value_type: U_DWORD
    accuracy_decimals: 3
    filters:
      - multiply: 0.001
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total Import Active Energy"
    register_type: holding
    address: 0x4000
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total Export Active Energy"
    register_type: holding
    address: 0x4002
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total Active Energy"
    register_type: holding
    address: 0x4004
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total Import Reactive Energy"
    register_type: holding
    address: 0x4008
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total Export Reactive Energy"
    register_type: holding
    address: 0x400A
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01
  - platform: modbus_controller
    modbus_controller_id: modbus_stromzaehler
    name: "Total Reactive Energy"
    register_type: holding
    address: 0x400C
    value_type: U_DWORD
    accuracy_decimals: 2
    filters:
      - multiply: 0.01


Hit SAVE then INSTALL -> ready to watch the magic!
Navigate to devices and configure the new detected device. Finish


