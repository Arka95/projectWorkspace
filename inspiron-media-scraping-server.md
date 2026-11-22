# Dell Inspiron Media Center and Scraping Server

## Goal

Repurpose the Dell Inspiron laptop as:

1. An always-on, low-power web-scraping and storage server.
2. A TV-connected media center with a remote-friendly interface and full browser access.
3. A source for a separate DIY side monitor using the original LCD panel.

## Recommended Hardware

- Existing Dell motherboard, CPU, cooling system, charger, and chassis
- 8 GB DDR3/DDR3L-1600 SODIMM RAM
- Existing 128 GB NVMe SSD, if the motherboard has a documented NVMe slot, or in a USB 3.x NVMe enclosure
- 240-500 GB 2.5-inch SATA SSD in the main drive bay if the NVMe SSD cannot be used as the internal system drive
- Existing healthy HDD in a 9.5 mm optical-drive caddy
- Wired Ethernet
- Bluetooth remote, air mouse, or FLIRC USB IR receiver
- Optional USB Bluetooth and Gigabit Ethernet adapters
- External backup drive
- Optional UPS and power-monitoring smart plug

## Software Architecture

- Minimal Debian or Ubuntu installation
- Kodi launched automatically in full-screen mode
- Chromium for normal browser access
- Python with Scrapy/BeautifulSoup for static websites
- Playwright for JavaScript-rendered websites
- Scraping jobs managed with systemd services and timers
- SSH for administration
- Tailscale for private remote access
- Samba or NFS for network storage
- `smartmontools` for disk-health monitoring
- Automatic security updates

## Operating Model

Keep the laptop running continuously and turn only the TV off. This keeps scraping jobs and storage available while eliminating TV-interface boot delays. When the TV selects the laptop's HDMI input, Kodi should already be available.

Do not power the laptop through a TV-controlled outlet. Abrupt power loss can interrupt scraping and corrupt storage.

## TV Control

- Pair a compatible Xiaomi Bluetooth remote directly with Linux.
- Use Kodi's Keymap Editor for unsupported buttons.
- Use FLIRC for an infrared TV remote.
- Use a USB HDMI-CEC adapter if TV power/input integration is required.
- Use Wake-on-LAN and Home Assistant if the laptop should sleep when unused.

## LCD Reuse

The original LCD can become a separate HDMI monitor:

1. Remove the panel and record the exact model number from its rear label.
2. Buy a controller board, display cable, backlight driver, button board, and power adapter specifically compatible with that panel.
3. Build or buy an enclosure and stand.
4. Relocate the lid-mounted Wi-Fi/Bluetooth antennas or replace them with external adapters.

Estimated conversion cost in India: approximately INR 2,500-4,500. Compare this with the price of a used monitor before purchasing components.

## M.2 SSD Compatibility

Do not buy an M.2 SSD until the laptop's exact Inspiron model or service tag and the motherboard connectors have been checked. M.2 describes the physical format; it does not guarantee that an SSD is electrically compatible.

### Recommended Option for This Build

For an older DDR3-era Inspiron without a documented storage M.2 slot:

1. Buy an **M.2 SATA** SSD, not an NVMe SSD.
2. Install it in a **2.5-inch, 7 mm M.2 SATA-to-SATA enclosure or adapter**.
3. Fit the adapter in the laptop's main 2.5-inch SATA drive bay.
4. Confirm that the adapter supports the SSD length, normally M.2 2280, and its B-key or B+M-key connector.

This arrangement behaves like a normal 2.5-inch SATA SSD. It will be limited to the motherboard's SATA speed and offers no performance advantage over a regular 2.5-inch SATA SSD, but it allows the M.2 drive to be reused in newer hardware later.

### Native M.2 or NVMe Option

Use a bare M.2 SSD directly on the motherboard only if Dell's service manual confirms that the connector is a storage slot and identifies its supported interface and drive size.

- A socket marked **M.2 SATA** accepts only SATA-compatible M.2 SSDs.
- A socket marked **PCIe/NVMe** accepts NVMe SSDs, subject to BIOS support.
- A Wi-Fi M.2 or mini-PCIe slot is not a practical substitute for a storage slot. It may lack the required PCIe lanes, physical space, BIOS boot support, or stable mounting.
- An NVMe-to-SATA passive adapter cannot work because NVMe uses PCIe while the drive bay uses SATA.

If a native NVMe slot is documented, update the BIOS before installation, confirm that NVMe boot is supported, secure the drive with the correct M.2 screw and standoff, and add a thin thermal pad only when clearance and the service manual permit it.

An NVMe SSD can alternatively be used in a USB 3.x NVMe enclosure for external storage. This is less suitable for the operating-system drive and is worthwhile only if the laptop has USB 3.x ports.

### Reusing the Existing 128 GB NVMe SSD

The existing 128 GB NVMe SSD cannot be connected to the laptop's SATA drive bay with a passive adapter. Use it in one of these configurations:

1. **Native motherboard NVMe slot:** Use it as the Linux system drive only if Dell's service manual confirms PCIe/NVMe storage support and the correct M.2 length. This is the preferred configuration.
2. **USB 3.x NVMe enclosure:** Use it for scraping workspace, downloads, caches, or temporary data. Choose an enclosure that explicitly supports **NVMe/PCIe**, not one that supports only M.2 SATA.
3. **USB boot drive:** This may work if the BIOS supports USB boot, but it is less reliable for an always-on server because the enclosure, cable, or USB connection can be disturbed.

Keep retained scraped data and media on the internal SATA SSD or HDD, and back it up separately. A 128 GB system drive is adequate for a minimal Linux installation, Kodi, scraping tools, and application files if large media, browser downloads, container images, and collected datasets are stored elsewhere. Enable periodic cleanup of Playwright browser caches, package caches, logs, and temporary scraping files.

## Important Constraints

- The motherboard does not support DDR4.
- Native M.2 or NVMe support cannot be assumed from the current information.
- The existing NVMe SSD is not compatible with an M.2 SATA-to-2.5-inch SATA adapter.
- The CPU is normally soldered and not practically upgradeable.
- Modern AAA gaming is outside the machine's useful performance range.
- Limit Playwright to one or two concurrent browser workers.
- Verify HDD SMART health before using it for retained data.
- Remove a swollen or unsafe battery.
- Preserve the fan, heatsink, heat pipe, ventilation covers, and power-button board.
- Back up all collected data to a separate device.
- Respect website terms, authentication controls, robots guidance, and rate limits.

## Suggested Build Order

1. Record the exact Inspiron model and service tag, inspect the motherboard, and check Dell's service manual for a storage M.2 connector.
2. Inspect battery, fan, RAM, HDD health, and exact CPU/GPU.
3. If a native NVMe slot is documented, install the existing 128 GB NVMe SSD as the Linux system drive; otherwise install a 2.5-inch SATA SSD internally and place the NVMe SSD in a USB 3.x NVMe enclosure.
4. Install Linux and configure remote SSH access.
5. Configure storage, backups, and scraping services.
6. Add Kodi, Chromium, HDMI audio, and remote control.
7. Move the HDD into the optical-drive caddy.
8. Confirm reliable HDMI operation before removing the LCD.
9. Convert the LCD into a separate monitor.
