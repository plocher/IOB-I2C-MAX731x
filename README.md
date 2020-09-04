# IOB-I2C-MAX731x
## License: MIT License
## License: CERN Open Hardware Licence v1.2
## License: Creative Commons Attribution-NonCommercial-ShareAlike

IOB I2C MAX7311/7318 based IO Expander

The MAX 731x series costs twice that of the MCP23017/PCA9555 expanders; for that price one gets support for  up to 64x devices on a single I2C chain.

### Specifications

This board is based on the MAX7311/7318 16 bit IO Expander, which supports up to 64x instances.

Each board provides a latched set of 16x IO points, with each point
being software selectable to be either an input or an output. The
individual points are reasonably protected from the environment, and can
sink 10 mA each (they are NOT current sources!)
By using active low inputs and outputs, the effects of environmental noise are
reduced - remote sensors only need to ground an I/O point to register
activity.

### Communication Protocol

``` {.cpp}
Simple I2C 16-bit Reads and Writes:
uint16_t I2Cexpander::read7311() {
    uint16_t data = 0;
    Wire.beginTransmission(_i2c_address);
    Wire.write(REGISTER_7311INPUT);
    Wire.endTransmission();
    // Wire.beginTransmission(_i2c_address);
    Wire.requestFrom(_i2c_address, (uint8_t)2);
    if(Wire.available()) {
        data = Wire.read();
    }
    if(Wire.available()) {
        data |= (Wire.read() << 8);
    }
    // Wire.endTransmission();
    return data;
}

void I2Cexpander::write7311(uint16_t data) {
    data = data | _config;
    Wire.beginTransmission(_i2c_address);
    Wire.write(REGISTER_7311OUTPUT);
    Wire.write(0xff & data);  //  low byte
    Wire.write(data >> 8);    //  high byte
    Wire.endTransmission();
}
```

See [I2Cexpander-lib](/pages/I2Cexpander "wikilink") for a more complete interface
library.


### Addressing
The addres selection for the MAX731x is complex, as it uses 3x address pins, but adds SDA and SCL to the traditional "Vcc and Gnd" used elsewhere.

**AD2**|**AD1**|**AD0**|**A6**|**A5**|**A4**|**A3**|**A2**|**A1**|**A0**|**ADDRESS (HEX)**
:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:
GND|SCL|GND|0|0|1|0|0|0|0|0x20
GND|SCL|V+|0|0|1|0|0|0|1|0x22
GND|SDA|GND|0|0|1|0|0|1|0|0x24
GND|SDA|V+|0|0|1|0|0|1|1|0x26
V+|SCL|GND|0|0|1|0|1|0|0|0x28
V+|SCL|V+|0|0|1|0|1|0|1|0x2A
V+|SDA|GND|0|0|1|0|1|1|0|0x2C
V+|SDA|V+|0|0|1|0|1|1|1|0x2E
GND|SCL|SCL|0|0|1|1|0|0|0|0x30
GND|SCL|SDA|0|0|1|1|0|0|1|0x32
GND|SDA|SCL|0|0|1|1|0|1|0|0x34
GND|SDA|SDA|0|0|1|1|0|1|1|0x36
V+|SCL|SCL|0|0|1|1|1|0|0|0x38
V+|SCL|SDA|0|0|1|1|1|0|1|0x3A
V+|SDA|SCL|0|0|1|1|1|1|0|0x3C
V+|SDA|SDA|0|0|1|1|1|1|1|0x3E
GND|GND|GND|0|1|0|0|0|0|0|0x40
GND|GND|V+|0|1|0|0|0|0|1|0x42
GND|V+|GND|0|1|0|0|0|1|0|0x44
GND|V+|V+|0|1|0|0|0|1|1|0x46
V+|GND|GND|0|1|0|0|1|0|0|0x48
V+|GND|V+|0|1|0|0|1|0|1|0x4A
V+|V+|GND|0|1|0|0|1|1|0|0x4C
V+|V+|V+|0|1|0|0|1|1|1|0x4E
GND|GND|SCL|0|1|0|1|0|0|0|0x50
GND|GND|SDA|0|1|0|1|0|0|1|0x52
GND|V+|SCL|0|1|0|1|0|1|0|0x54
GND|V+|SDA|0|1|0|1|0|1|1|0x56
V+|GND|SCL|0|1|0|1|1|0|0|0x58
V+|GND|SDA|0|1|0|1|1|0|1|0x5A
V+|V+|SCL|0|1|0|1|1|1|0|0x5C
V+|V+|SDA|0|1|0|1|1|1|1|0x5E
SCL|SCL|GND|1|0|1|0|0|0|0|0xA0
SCL|SCL|V+|1|0|1|0|0|0|1|0xA2
SCL|SDA|GND|1|0|1|0|0|1|0|0xA4
SCL|SDA|V+|1|0|1|0|0|1|1|0xA6
SDA|SCL|GND|1|0|1|0|1|0|0|0xA8
SDA|SCL|V+|1|0|1|0|1|0|1|0xAA
SDA|SDA|GND|1|0|1|0|1|1|0|0xAC
SDA|SDA|V+|1|0|1|0|1|1|1|0xAE
SCL|SCL|SCL|1|0|1|1|0|0|0|0xB0
SCL|SCL|SDA|1|0|1|1|0|0|1|0xB2
SCL|SDA|SCL|1|0|1|1|0|1|0|0xB4
SCL|SDA|SDA|1|0|1|1|0|1|1|0xB6
SDA|SCL|SCL|1|0|1|1|1|0|0|0xB8
SDA|SCL|SDA|1|0|1|1|1|0|1|0xBA
SDA|SDA|SCL|1|0|1|1|1|1|0|0xBC
SDA|SDA|SDA|1|0|1|1|1|1|1|0xBE
SCL|GND|GND|1|1|0|0|0|0|0|0xC0
SCL|GND|V+|1|1|0|0|0|0|1|0xC2
SCL|V+|GND|1|1|0|0|0|1|0|0xC4
SCL|V+|V+|1|1|0|0|0|1|1|0xC6
SDA|GND|GND|1|1|0|0|1|0|0|0xC8
SDA|GND|V+|1|1|0|0|1|0|1|0xCA
SDA|V+|GND|1|1|0|0|1|1|0|0xCC
SDA|V+|V+|1|1|0|0|1|1|1|0xCE
SCL|GND|SCL|1|1|0|1|0|0|0|0xD0
SCL|GND|SDA|1|1|0|1|0|0|1|0xD2
SCL|V+|SCL|1|1|0|1|0|1|0|0xD4
SCL|V+|SDA|1|1|0|1|0|1|1|0xD6
SDA|GND|SCL|1|1|0|1|1|0|0|0xD8
SDA|GND|SDA|1|1|0|1|1|0|1|0xDA
SDA|V+|SCL|1|1|0|1|1|1|0|0xDC
SDA|V+|SDA|1|1|0|1|1|1|1|0xDE


