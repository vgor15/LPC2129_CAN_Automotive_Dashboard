🚗 LPC21xx CAN-Bus Dashboard: Distributed Control System
This project is a two-node embedded system prototype demonstrating core principles of Automotive CAN communication and real-time data processing using the NXP LPC21xx ARM7 microcontroller.

1. System Architecture and Roles
The system is split into two distinct, communicating nodes:

Node A (Transmitter/Sensor Node): (Files in the ./Node A/ folder)

Primary Role: Sensor reading and transmission of control commands.

Inputs: On-chip ADC (Speed and Temperature) and Driver Switches (Headlight, Left/Right Indicators).

Node B (Receiver/Dashboard UI): (Files in the ./Node B/ folder)

Primary Role: Displaying data and controlling vehicle status indicators.

Outputs: 16x2 LCD (Speed and Temperature) and LEDs (Indicator/Headlight status).

2. Key Technical Focus Areas
This section highlights the specific engineering techniques used in the project.

2.1. CAN Communication and Data Integrity
Protocol: Implemented using CAN 2.0B Standard on CAN Controller 2.

Message ID: Uses the Standard 11-bit ID 0x500 for all transmissions.

Data Structure: Speed (from ADC) is packed into byteA and Temperature is packed into byteB for efficient data transfer.

Command Signaling: Control commands (e.g., 0x05, 0x07) are sent by temporarily placing specific hex codes in byteA.

2.2. Real-Time Processing and Interrupts
Periodic Transmission (Node A): A Timer1 Interrupt is configured to ensure Speed and Temperature data are sampled and transmitted at a fixed, reliable rate.

Asynchronous Reception (Node B): Incoming CAN messages are handled immediately via a dedicated CAN Receive Interrupt Handler. This is critical for minimizing latency.

Non-Blocking I/O: The flashing logic for the turn indicators is managed using separate Timer variables toggled within an ISR. This prevents the use of blocking delay() functions, ensuring the dashboard remains fully responsive.

2.3. Signal Processing
Sensor Input: Utilizes the LPC21xx's internal 10-bit ADC.

Conversion: Raw ADC values are processed and converted:

Temperature is calculated using the standard formula for an LM35 sensor.

Speed is scaled to a visible range for the display.

3. Development Environment
Microcontroller Family: NXP LPC21xx (ARM7TDMI-S Core)

Development IDE: Keil µVision
