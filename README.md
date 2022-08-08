# STM32-Voice-Recorder
This Project main objective is creating a digital voice recorder that can record voice, playback a voice, and delete data from EEPROM. The modules that are going to be used in this Project are Timer, PWM, ADC and External Interrupts. According to the numbers pressed from the keypad, stats must be realized. These stats are named record, playback and clear.

## Components Used in The Project

| 220uF, 22uF, 47nF, capacitors | 100kΩ, 1kΩ, 10Ω, resistors    |
|-------------------------------|-------------------------------|
| 20k potentiometer             | LM386 Opamp                   |
| Speaker                       | Max4466 Microphone            |
| 4x4 Keypad                    | Seven Segment Display 4 Digit |
| 24LC256 EEPROM                | STM32G031K8T6                 |

## Tasks

### Building a Digital Voice Recorder Circuit

The materials to be used for this are determined. First the required amplifier circuit for the speaker was built then microphone was added to the circuit. EEPROM has been added to the circuit so that analog data from the microphone can be recorded and playback.

### Understanding the EEPROM's Working Mechanism

To do this, the datasheet of the EEPROM has been examined. Looked at the pin function table and understood where the pins of the EEPROM should be connected.

![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/img%201.jpg)
![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/img%202.jpg)

A0, A1, A2, VSS and WP are grounded. SDA and SCL are connected to PB-7 and PB6 VCC connected to 3.3 volts for EEPROM to work.

### Defining EEPROM Address

Since we connect a0, a1 and a2 in ground, the binary value of the given address is 1010000. By converting it to hexadecimal, the address value is obtained.

![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/img%203.jpg)


### Creating Write and Read Functions for EEPROM

It is necessary to write an algorithm according to the 32kbyte size.

![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/img%204.jpg)
![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/img%205.jpg)


## Diagrams

![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/diagram%201.jpg)
![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/diagram%202.jpg)

