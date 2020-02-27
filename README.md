## **This is a brief summary. See [`READMORE`](READMORE.md) for the complete overview.**

# ft232h
##### Go module for [FT232H](https://www.ftdichip.com/Products/ICs/FT232H.htm) USB to GPIO/SPI/I²C/JTAG/UART protocol converter

_This is a work-in-progress and not at all stable_

## Features
- [x] `GPIO` - read/write
   - 8 dedicated pins available in any mode
   - 8-bit parallel, and 1-bit serial read/write operations
- [x] `SPI` - read/write 
   - SPI `Mode0` and `Mode2` only, i.e. `CPHA=1`
   - configurable clock rate up to 30 MHz
   - chip/slave-select `CS` on both ports, pins `D3—D7`, `C0—C7`, including:
     - automatic assert-on-write/read, configurable polarity
     - multi-slave support with independent clocks `SCLK`, SPI modes, `CPOL`, etc.
   - unlimited effective transfer time/size
     - USB uses 64 KiB packets internally
- [ ] `I2C` - _not yet implementented_
- [ ] `JTAG` - _not yet implementented_
- [ ] `UART` - _not yet implementented_
- [x] **TBD** (WIP)

## Installation
Installation is conventional, just use the Go built-in package manager:
```sh
go get -v github.com/ardnew/ft232h
```
No other libraries or configuration is required. 

###### Common issues
If you have trouble finding/opening your device in Linux, you probably have the incompatible module `ftdi_sio` loaded. See the Linux `Installation` section in [`READMORE`](READMORE.md) for details.

## Supported platforms
Internally, `ft232h` depends on some proprietary software from FTDI that is only available for a handful of platforms (binary-only). This would therefore be the only platforms supported by the `ft232h` Go module:
#### Linux 
- [x] x86 (32-bit) `[386]`
- [x] x86_64 (64-bit) `[amd64]`
- [x] ARMv7 (32-bit) `[arm]` - includes Raspberry Pi models 3 and 4
- [x] ARMv8 (64-bit) `[arm64]` - includes Raspberry Pi model 4
#### macOS
- [x] x86_64 (64-bit) `[amd64]`
#### Windows
- [ ] x86 (32-bit) `[386]`
- [ ] x86_64 (64-bit) `[amd64]`
###### Windows compatibility
Windows support is possible – and in fact appears to be FTDI's preferred target – but drivers for this `ft232h` Go module have not been compiled or tested. The modifications made to `libMPSSE` to support static linkage would need to be verified or merged in for Windows. See the `Drivers` section in [`READMORE`](READMORE.md) for info.

## Usage
Simply import the module and open the device:
```go
import (
	"log"
	"github.com/ardnew/ft232h"
)


	// open the fist MPSSr-capable USB device found
	ft, err := ft232h.NewFT232H()
	if nil != err {
		log.Fatalf("NewFT232H(): %s", err)
	}
	defer ft.Close() // be sure to close device

	// do stuff
	log.Printf("ᵈᵒⁱⁿᵍ ˢᵗᵘᶠᶠ ᴅᴏɪɴɢ sᴛᴜғғ DOING STUFF: %s", ft)
}
```

## Peripheral devices
Of course the FT232H isn't that useful without a device to interact with. You are encouraged to create drivers or adapters based on the `ft232h` Go platform for your own devices—the hard work of binding a Go runtime to the physcial GPIO/SPI/I²C/JTAG/UART interfaces on the FT232H has been done for you! 

[An basic driver for the ILI9341 TFT LCD with SPI+GPIO](drv/ili9341), along with [a fun demo application that uses it](examples/spi/ili9341/boing), has been created to serve as a reference example.

For more details, be sure to read the `Peripheral devices` section  in [`READMORE`](READMORE.md); and, of course, the godoc for this `ft232h` module.
