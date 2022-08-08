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

### Project Setup

An amplifier circuit has been established that has been researched and studied on the internet. Only 3 rows and 3 columns are selected on the keypad because there are not enough pins in the microprocessor.

## Methodology for any Numerical Work

To start writing and reading from EEPROM, the EEPROM has an area of 64 Kbyte. half of these 64 kbps are 32 kbps. The function “write_I2C” was written to send data from this 32 kbyte analog to digital. The function has 4 input variables for sending this data. These entries are the address of the EEPROM, the address to be written, the data, and the data size, respectively. To save the data, it is necessary to press the 1 key on the keypad. pressing 1 will make recordFlag 1, and this function can be run on “ADC_COMP_IRQHandler”. The operation returned in ADC_COMP_IRQHANDLER is as follows: The values converted from analog to digital are stored in a variable called "voltageValue", and this is taken according to "voltageValue" mode 256, so that the data to be stored can be 8 bits.
The modified "voltage Value" is stored in an array called the "voltage Buffer". To transfer data to each part of this array, the index j is constantly increased in the handler, but when j = 32, this means that 32 bytes are received. After the write operation is performed, the “Writtenaddress” is increased to 32. j is equal to zero, and the variable i is incremented by 1. In total, this process must occur a thousand times before we can write 32 kbyte of data. if i = 1000, we know that 32 kbyte of data is written to the “voltageBuffer”. when i = 1000, recordFlag = 0, and i is set to zero at the beginning of the typed addresses. In this way, the writing process is completed.
When writing, there is reading. The same method is used for reading. The values written to the EEPROM should be read after they are written. The same method is used for reading. When pressing 2 from the keypad, the read function in the timer3 handler works. a looping mechanism has been set up to read 32 kbyte of data into a variable called “voiceRecorded”. Thanks to this loop, the values written in VOICERECORDED are thrown into a variable called freq, which is sent to “TIM2-CCR2”. Each time the “CCR2” changes, a different sound should come out. Since a total of 32 kbyte has been read, the recorded audio must be heard.

## Challenges

It was difficult to adjust the sample frequency and the pwm frequency so that the sound from the microphone can be heard from the speaker. The pwm frequency was chosen much higher than the sample frequency, resulting in a less loud sound.
Another challenge is the process of writing and reading EEPROM, almost the part that covers the project. A methodology has been developed accordingly to use half the EEPROM, but when the code was run, the desired was not fulfilled.

![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/img%206.jpg)

## Diagrams

![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/diagram%201.jpg)
![alt text](https://raw.githubusercontent.com/voghbum/STM32-Voice-Recorder/main/diagram%202.jpg)

