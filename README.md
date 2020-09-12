# IOB-I2C-MAX731x
## License: CERN Open Hardware Licence v1.2

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

