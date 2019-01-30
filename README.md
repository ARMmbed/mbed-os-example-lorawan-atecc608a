# Example LoRaWAN application with the ATECC608A-MAHTN-T

This is an Mbed OS LoRaWAN example application that uses the [Microchip ATECC608A-MAHTN-T](http://www.microchip.com/ATECC608aLoRaTTI) secure element for all cryptographic operations. The ATECC608A-MAHTN-T contains a secure identity, root keys compatible with LoRaWan 1.0.x and 1.1, and a crypto accelerator.

With the ATECC608A-MAHTN-T, you'll receive a file from Microchip. This file contains the unique identifiers for the ATECC608As and is cryptographically signed. Upload this file to the [The Things Industries Join Server](http://ttn.fyi/joinserver/microchip) to claim the devices.

More information can be found in this blog post: [Introducing hardware crypto for LoRaWAN](https://os.mbed.com/blog/entry/introducing-lorawan-hw-crypto/).

## Requirements

You'll need the following hardware:

* An [Mbed-enabled development board](https://os.mbed.com/platforms/).
* The [CryptoAuthentication™ UDFN Socket Kit](https://www.microchip.com/DevelopmentTools/ProductDetails/AT88CKSCKTUDFN-XPRO) extension board, and enable DIP switches 1, 3 and 6.
* A [pre-provisioned ATECC608A-MAHTN-T with the The Things Industries profile](https://www.microchipdirect.com/product/search/all/atecc608a-mahtn).
    * Insert the ATECC608A to the CryptoAuthentication Socket Kit. When inserting the secure element, make sure the 'T' on the chip is aligned with the 'T' on the Socket Kit. You might need to use a magnifying glass.
* An SX1272 or SX1276 LoRa radio, such as the [SX1272MB2xAS](https://os.mbed.com/components/SX1272MB2xAS/) or [SX1276MB1xAS](https://os.mbed.com/components/SX1276MB1xAS/) shield.

Connect the CryptoAuthentication UDFN Socket Kit to the `I2C_SDA` and `I2C_SCL` pins on your development board. Connect the LoRa radio shield to the main SPI bus. If you use different pins, update `mbed_app.json` with the right configuration.

On the SAML21 Xplained Pro development board, connect the LoRa radio to `EXT1`, and the CryptoAuthentication UDFN Socket Kit to `EXT3`.

![SAML21 Xplained Pro with LoRa radio and secure element](media/secure01.png)

## Getting started

This application can be built using [Mbed CLI](https://os.mbed.com/docs/mbed-os/v5.11/tools/installation-and-setup.html) or the [Mbed Online Compiler](https://os.mbed.com/compiler).

### Importing the project

**Mbed CLI**

1. Connect your development board to your computer.
1. Import the application:

    ```
    $ mbed import https://github.com/armmbed/mbed-os-example-lorawan-atecc608a
    ```

**Online Compiler**

1. Connect your development board to your computer.
1. Open the [Online Compiler](https://os.mbed.com/compiler).
1. Click **Import > Import from URL**.
1. Enter `https://github.com/armmbed/mbed-os-example-lorawan-atecc608a`

### Setting credentials

You need to set your application EUI if you're using LoRaWAN 1.0.x. In addition, you might need to set your device EUI, if your ATECC608A-MAHTN-T does not have it's Device EUI present in Slot 10. If this is the case, the message `Cannot read device EUI in slot 10` will pop up.

To set these EUIs, open `main.cpp`, and look for `LORAWAN_DEV_EUI` and `LORAWAN_APP_EUI`.

### Setting the channel plan

This application is configured to use the `EU868` channel plan. To override this, see [Selecting a PHY](https://github.com/armmbed/mbed-os-example-lorawan#selecting-a-phy) in the official LoRaWAN example application.

Note that if you're using an 8-channel gateway in the US, you also need to set your frequency sub band. For instance, for The Things Network public network in the US, add:

```
            "lora.fsb-mask": "{0xFF00, 0x0000, 0x0000, 0x0000, 0x0002}",
```

### Setting the LoRaWAN version

This application works with LoRaWAN 1.0.2, 1.0.3 and 1.1.1. To set the version, open `mbed_app.json` and override `lora.version`. The values supported are:

* `0` - LoRaWAN 1.0.2
* `1` - LoRaWAN 1.0.3
* `10` - LoRaWAN 1.1.1

### Compiling

You're now ready to compile and flash the application.

**Mbed CLI**

1. Compile and flash the application:

    ```
    $ mbed compile -m auto -t GCC_ARM -f
    ```

**Online Compiler**

1. Click **Compile**.
1. A file downloads, drag-and-drop this file to your development board (it mounts as a USB mass storage device).

### Seeing debug information

Attach a serial monitor on baud rate 115,200 to see debug information. For more information see [Serial communication](https://os.mbed.com/docs/mbed-os/v5.11/tutorials/serial-communication.html) in the Mbed OS documentation.

## Documentation

Full documentation of the application can be found in the [offical Mbed OS LoRaWAN example](https://github.com/armmbed/mbed-os-example-lorawan) and in the Mbed OS documentation, under [LoRaWAN APIs](https://os.mbed.com/docs/mbed-os/v5.11/apis/lorawan.html) and [LoRaWAN architecture](https://os.mbed.com/docs/mbed-os/v5.11/reference/lora-tech.html).
