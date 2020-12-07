# README

This folder contains the FPGA design of AUTD3.

The code is written in SystemVerilog with Vivado 2020.2.

# Connection

* [IN] [16:0] CPU_ADDR,
* [IN/OUT] [15:0] CPU_DATA
* [OUT] [252:1] XDCR_OUT
* [IN] CPU_CKIO
* [IN] CPU_CS1_N
* [IN] RESET_N
* [IN] CPU_WE0_N
* [IN] CPU_WE1_N
* [IN] CPU_RD_N
* [IN] CPU_RDWR
* [IN] MRCC_25P6M
* [IN] CAT_SYNC0
* [OUT] FORCE_FAN
* [IN] THERMO
* [IN] [3:0] GPIO_IN
* [OUT] [3:0] GPIO_OUT

## Address map

### Properties

| BRAM_SELECT | BRAM_ADDR (8bit) | DATA (16 bit)                    | R/W |
|-------------|------------------|----------------------------------|-----|
| 0x0         | 0x00             | Control flags and Clock property | R/W |
| 　          | 0x01             | LM Cycle                         | R   |
| 　          | 0x02             | LM Division                      | R   |
| 　          | 0x03             | LM Clock initialize lap          | W   |
| 　          | 0x04             | LM Calibration shift             | R   |
| 　          | 0x05             | LM bram addr offset              | -   |
| 　          | 0x06             | Unused                           | 　  |
| 　          | ︙               | ︙                               | 　  |
| 　          | 0xFE             | Unused                           | 　  |
| 　          | 0xFF             | FPGA version number              | -   |

### Modulation

| BRAM_SELECT | BRAM_ADDR (11bit) | DATA (8bit) | R/W |
|-------------|-------------------|-------------|-----|
| 0x1         | 0x000             | mod[0]      | R   |
| 　          | 0x001             | mod[1]      | R   |
| 　          | ︙                | ︙          | ︙  |
| 　          | 0xF9F             | mod[3999]   | R   |
| 　          | 0xF9F             | Unused      | 　  |
| 　          | ︙                | ︙          | 　  |
| 　          | 0xFFF             | Unused      | 　  |

### Normal operation

| BRAM_SELECT | BRAM_ADDR (11bit) | DATA (16bit)        | R/W |
|-------------|-------------------|---------------------|-----|
| 0x2         | 0x00              | amp[0]/phase[0]     | R   |
| 　          | ︙                | ︙                  | ︙  |
| 　          | 0xF8              | amp[248]/phase[248] | R   |
| 　          | 0xF9              | Unused              | 　  |
| 　          | ︙                | ︙                  | 　  |
| 　          | 0xFF              | Unused              | 　  |

### LM operation

| BRAM_SELECT | BRAM_ADDR (16bit) | DATA (128 bit)                                                                       | R/W |
|-------------|-------------------|--------------------------------------------------------------------------------------|-----|
| 0x3         | 0x0000            | 79:0 = {lm_amp[0], lm_z[0],   lm_y[0], lm_x[0]}      127:80 = Unused                 | R   |
| 　          | 0x0001            | 79:0 = {lm_amp[1], lm_z[1],   lm_y[1], lm_x[1]}      127:80 = Unused                 | R   |
| 　          | ︙                | ︙                                                                                   | ︙  |
| 　          | 0x9C3F            | 79:0 = {lm_amp[39999],   lm_z[39999], lm_y[39999], lm_x[39999]}      127:80 = Unused | R   |

# Author

Shun Suzuki, 2020-
