# ZMK Raytac USB Dongle Module

This repository adds a ZMK board definition named `raytac_mdbt50q_rx` for the [Raytac MDBT50Q-RX](https://www.raytac.com/product/ins.php?index_id=89) USB stick, with the intention of using it as a [ZMK keyboard dongle](https://zmk.dev/docs/development/hardware-integration/dongle).

![IMG_1775](./docs/images/IMG_1775.jpeg)

## Caveat: Buy One With A UF2 Bootloader Installed

The stock MDBT50Q-RX does not come with the UF2 bootloader installed—that's the bit of code that allows it to mount itself as a USB drive for flashing.

But! Some versions of the dongle do exist with the UF2 bootloader pre-installed, and that's what you want buy: [Adafruit (USA)](https://www.adafruit.com/product/5199) and [Pi Hut (UK)](https://thepihut.com/products/nrf52840-usb-key-with-tinyuf2-bootloader-bluetooth-low-energy-mdbt50q-rx) sell them, and probably others if you search around. I used the one from Adafruit.

### Maybe You Should Update The Bootloader Too?

I upgraded the version of the bootloader on my Raytac dongle. I don't know if this is necessary because I upgraded before flashing ZMK onto it. It was easy and didn't hurt anything?

I used these [instructions](https://learn.adafruit.com/introducing-the-adafruit-nrf52840-feather/update-bootloader-use-uf2) to update the bootloader to version [0.9.2](https://github.com/adafruit/Adafruit_nRF52_Bootloader/releases/tag/0.9.2). Specifically I used [this exact one](https://github.com/adafruit/Adafruit_nRF52_Bootloader/releases/download/0.9.2/update-raytac_mdbt50q_rx_bootloader-0.9.2_nosd.uf2).

## Usage

Add the following entries to `remotes` and `projects` in `config/west.yml`

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: rschenk
      url-base: https://github.com/rschenk
  projects:
    - name: zmk
      remote: zmkfirmware
      import: app/west.yml
    - name: zmk-component-raytac-dongle
      remote: rschenk
      revision: main
  self:
    path: config
```

## Configuring Your Dongle

Follow the setup steps in the [ZMK Keyboard Dongle docs](https://zmk.dev/docs/development/hardware-integration/dongle) to configure your keyboard and dongle. When you get to the [Building the Firmware](https://zmk.dev/docs/development/hardware-integration/dongle#building-the-firmware) step, you will use `board: raytac_mdbt50q_rx` in your `build.yml` file like so:

```yaml
include:
  # Config settings for the dongle
  - board: raytac_mdbt50q_rx
    shield: my_keyboard_dongle
    
  - board: raytac_mdbt50q_rx
    shield: settings_reset

  # Whatever your keyboard uses...
  - board: nice_nano_v2
    shield: my_keyboard
```

## Flashing The Raytac

Entering the bootloader mode is a bit annoying. Unplug the dongle, then hold the button down while plugging it back in. 

For some reason you *can't* double-click the reset button like on many boards.
