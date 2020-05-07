# Control⇔Monitoring data-exchange protocol

## Design considerations
* Easy to implement in C/C++ (control) and python (monitoring)
* Unidirectional (to protect control unit)
* Value-identification strings (to ease adding values and prevent mixups in update scenarios)
* Error-detection (values displayed are medically-critical)
* No error-correction (simplicity, short signal-path)
* No reordering (straight signal-path)

## Protocol-specification
In short: COBS for packetization, implying 0-bytes for packet delimiting. Packet-data is 

### Message Structure
| 1-byte COBS-start | value identifier (6-char ASCII) | value (float32) | checksum (uint32) | `0x00` (COBS end marker) | 
|-------------------|---------------------------------|-----------------|-------------------|--------------------------|

### Example packets
| hex-string value | value-identifier | float | checksum |
|------------------|------------------|-------|----------|
| `\rVOLI<\xdd\xf6BjW\xc9\x8c` | `DVOLI_` | `123.4321` | `2362005354` |
| `\x05PRES\x01\x07\x87D\xf0q\x1b\xa1` | `DPRES_` | `1080.0` | `2702930416` |

