# SNEdge - An Opensource SNES Edge Sharpener
**This has only been tested on a few SNES revisions | Proceed with caution**

SNEdge V2 is to a state where it can be considered stable. As SNES revisions are tested, instructions will be added to the wiki.
For now, only the full bypass is supported. The sidecar install will get a dedicated design at some point.

The SNEdge corrects much of the smearing that's present on the 2 chip SNES. It taps the console's RGB, cleanly buffers it, and applies a tuned amount of sharpening to bring back the crisp edges the stock 2 chip output loses.

This is the latest revision of the SNEdge. Over the original it refines the input biasing, properly impedance-matches the outputs to the 75 ohm video standard, and re-tunes the sharpening — see [How It Works](#how-it-works) for the details. It still uses 0805 components, so it's relatively approachable to solder by hand.

The SNEdge pairs with the **Multiout QSB**, which makes for a clean install right at the multiout. There are two ways to connect the SNEdge to the QSB:

- **U.FL Coax (recommended)** — connect the two boards with U.FL-terminated 1.13mm coax (the same cable sold as WiFi antenna pigtails). This is the preferred method for new installs: it's a higher-quality, shielded connection, the U.FL connectors let you position and route the cable freely, and the cable is cheaper and easier to source than FFC. Around 10cm length works well.
- **FFC cable** — a single ribbon cable between the two boards. Still supported, but coax is the better choice going forward.

> Where to get the boards: **[SNEdge + Multiout QSB — add link]**. The coax is standard U.FL 1.13mm WiFi antenna pigtail cable (roughly 10cm), widely available online.

The SNEdge

<img src="/SNEdge%20PCB/SNEdge_V2_Render.png" width="640">

The Multiout U.FL QSB

<img src="/Multi%20AV%20QSB/U.FL%20QSB/Multiout_UFL_render.png" width="240">

The Multiout FFC QSB

<img src="/Multi%20AV%20QSB/FFC%20QSB/MultiOut_FFC_PCB_render.png" width="240">

(I'll be adding screenshots of the video output soon)

## How It Works

The SNEdge taps the RGB video signal at the PPU2 output (the Q3/Q5/Q7 points) and runs each channel through a THS7376 video amplifier. The 2 chip SNES PPU has slow color transitions that show up on screen as smearing; cleanly buffering the signal and applying a small amount of high-frequency peaking sharpens those transitions without changing the rest of the picture.

A few design choices worth knowing about:

- **Ground reference and input attenuation.** Each RGB input has a 7.5k pull-down resistor. It does two jobs. First, it gives the amplifier input a proper, defined ground/DC reference so the signal sits where it should. Second, it attenuates the input down to around 2.20Vptp, which keeps the signal within the THS7376's 2.35Vptp linear input voltage spec and so avoids clipping the amplifier's input.
- **Sharpening.** A small feedback capacitor on each channel provides the edge sharpening. It's tuned for the most sharpening you can get before overshoot starts to become visible.
- **75 ohm output.** Each output uses a 240R / 110R network. In parallel that's about 75 ohms, which is the standard video source impedance, so the SNEdge drives a cable correctly.
- **Optional noise caps.** Adding a 10uF cap to each of C90-C94 on the SNES motherboard helps suppress video noise. These are optional — you can leave them out and only add them if you notice any noticeable noise in the output.

## Installation
I don't have specific, picture instructions on how to install the SNEdge yet (remember, alpha release).

However, looking at the [SNES schematic (PDF)](https://wiki.superfamicom.org/uploads/snes_schematic_color.pdf) from [wiki.superfamicom.org](wiki.superfamicom.org) will be critical for understanding how to install this.

This revision installs using the **RGB Bypass** method, which gives the best sharpness. The older **Sidecar** method (which didn't require desoldering any components) is no longer supported on this revision — see [A Note on the Sidecar Install](#a-note-on-the-sidecar-install) below if that's what you're after.

Optionally, you can solder an additional 10uF capacitor onto C90, C91, C92, C93, and C94 on the SNES motherboard (for PAL systems, it's C91, C92, C93, C94, and C95). These help reduce video noise. They aren't required, but if you notice any noticeable noise in the output, adding them should clean it up.

Below is an SOT-23 pinout I will be referencing later.
<img src="https://github.com/ThunderTechnologies/SNEdge/blob/main/SNEdge%20PCB/SOT-23.png" width="640">

### A Note on the Sidecar Install
The Sidecar method is **not supported on this revision.** If you want a sidecar-style install, the original SNEdge can still be used for that. A dedicated sidecar revision may be released later by ThunderTechnologies.

This revision has not been tested in a sidecar configuration. It may work fine, but it's untested, so it isn't documented as a supported install here. For this revision, use the Full RGB Bypass install below.

### Full RGB Bypass Install
(Please note that PAL system installation is a bit different, I have info on this, and will be adding it eventually.)

Mount the SNEdge, keeping one priority above everything else: **keep the input wires as short as possible.**

The R, G, and B input wires are very sensitive to parasitic capacitance, so position the SNEdge so those wires reach their mainboard points (Q3, Q5, Q7) over the shortest distance you can manage. Keeping the input wires as close to the mainboard points as possible is the single most important part of placement. It's also preferred to keep the three input wires at **similar lengths** to each other.

Orientation depends on how you're connecting to the QSB:

- **Coax (recommended):** the U.FL coax gives you a lot of freedom. Orient and place the SNEdge however best keeps the input wires short, and route the coax wherever is convenient.
- **FFC:** point the FFC connector toward the multiout of the SNES.

Remove Q3, Q5, and Q7. If you only care about RGB working, remove R8, R9, R13, R14, R18, and R19. If you also care about Composite video and S-Video, also remove C6, C7, and C8.

Q3 is Red, Q5 is Green, and Q7 is Blue. Looking at the SOT-23 pinout above, pin 3 of each is ground. Pin 1 of each is the R, G, and B signals that will be soldered to the SNEdge.

Solder leads from Q3 to R_IN and GND.

Solder leads from Q5 to G_IN and GND.

Solder leads from Q7 to B_IN and GND.

The SNEdge needs to output video to U7 (S-ENC) on the SNES in order for composite video and S-video.

The SNEdge R_E, G_E, and B_E outputs go to U7's pins 20, 21, 22 respectively. (you can ignore the GND pin next to these if you'd like. If you care, connect it to pin 2 of U7.

Solder the multiout QSB on to the bottom of the SNES where the multiout pins are. If you're looking at the bottom of the SNES board, the "TOP" label on the QSB needs to be facing you. (If you're using FFC, also orient it so the FFC connector points toward the SNEdge; with coax, routing is flexible so this doesn't matter.)

The SNEdge needs 5V. Solder 5V to the + and ground to the -

Connect the SNEdge to the QSB. Coax is the recommended method:

- **Coax (recommended):** connect the SNEdge to the QSB with U.FL-terminated 1.13mm coax (the cable commonly sold as WiFi antenna pigtails). Around 10cm length works well. It's the better-quality connection, easy to route thanks to the U.FL connectors, and cheap to source.
- **FFC cable:** alternatively, connect an FFC cable between the SNEdge and the QSB. The contacts of the FFC face down, with the blue tabs facing up.

Congrats! You have completed the full RGB bypass install. Enjoy some crispy RGB.

### A Note on PAL Systems
**PAL has not been tested yet.** The SNEdge should work on PAL consoles since they share the same PPU, but please treat PAL installs as experimental for now.

The main difference to be aware of is the cap designators: if you add the optional noise-reduction caps, on PAL systems they go on **C91, C92, C93, C94, and C95** instead of C90-C94. PAL boards also differ around the encoder, so the Full RGB Bypass encoder connections (the U7 pin references above) may not match a PAL console one-to-one. Until PAL-specific details are confirmed and documented, treat a PAL install as experimental and proceed carefully. More complete PAL instructions will be added as they're verified.

### Credits

The SNEdge is based on the work of YoshiYukiBlade over on the [shmups.system11.org](https://shmups.system11.org/viewtopic.php?t=70196) forums.
