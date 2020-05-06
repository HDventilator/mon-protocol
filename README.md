# Control⇔Monitoring data-exchange protocol

## Design considerations
* Easy to implement in C/C++ (control) and python (monitoring)
* Human-readable (to ease debugging)
* Unidirectional (to protect control unit)
* Value-identification strings (to ease adding values and prevent mixups in update scenarios)
* Error-detection (values displayed are medically-critical) — Discuss: which checksum (e.g., fletcher-32 or crc-32?)
* No error-correction (simplicity, short signal-path)
* No reordering (straight signal-path)
* Discuss: Support for datagram-identification (aka sequence-numbers) to re-send and identify repeated messages

## Protocol-specification
In short: Ascii-encoded, <CR><LF>-delimited, field-wise protocol with checksum. Loosely inspired by ![NMEA0183](https://en.wikipedia.org/wiki/NMEA_0183) …

### Reserved Characters
ASCII | Meaning
------|--------
<CR><LF> | End of message
$ | Start of message
, | Field delimiter
* | Checksum delimiter

### Message Structure
| `$` | value identifier (string, max ?? chars) | `,` | value (`%f`-style float32) | `*` | checksum | `<CR><LF>` |
|-----|-----------------------------------------|----------------------------|-----|----------|------------|

### Example Messages
```
$Pressure,1084.345356*f4fae471<CR><LF>
$Something (staged),1234.240000*f29d99c8<CR><LF>
```
