# SNEdge - An Opensource SNES Edge Sharpener
**THIS IS AN ALPHA RELEASE | PROCEED WITH CAUTION**
Right now the SNEdge works really well. The only reason I'm considering it in an alpha state is because I haven't personally tested it on all versions of the SNES. That being said, it _should_ work on all revisions of the SNES since they all use the same major version of the PPU.

The SNEdge aims to correct much of the smearing that's present on the 2 chip SNES. Right now the SNEdge is a simple board that can be soldered to a SNES.
This alpha version is designed to be installable on all revisions, as well as being relatively easy to solder since it uses 0805 components.
QSB versions of the SNEdge are in the works to make a clean install very easy.

The SNEdge

<img src="https://github.com/ThunderTechnologies/SNEdge/blob/main/SNEdge%20PCB/SNEdge_PCB_Render.png" width="640">

The Multiout QSB

<img src="https://github.com/ThunderTechnologies/SNEdge/blob/main/Multi%20AV%20QSB/MultiOut_PCB_render.png" width="640">

(I'll be adding screenshots of the video output soon)

## Installation
I don't have specific, picture instructions on how to install the SNEdge yet (remember, alpha release).

However, looking at the [SNES schematic (PDF)](https://wiki.superfamicom.org/uploads/snes_schematic_color.pdf) from [wiki.superfamicom.org](wiki.superfamicom.org) will be critical for understanding how to install this.

There are two methods of installation; Sidecar and RGB Bypass. The Sidecar install doesn't require any desoldering of components, but its sharpness isn't quite as good. The RGB Bypass has better sharpness, but is a more involved install.

For **_BOTH_** of these methods, it is absolutely critical so solder an additional 10uF capacitor onto C90, C91, C92, C93, and C94 on the SNES motherboard. If these caps aren't installed, the SNES's video output will be terribly noisy.

### Sidecar Install
To start, solder JP1, JP2, and JP3 closed on the SNEdge. 

Mount the SNEdge somewhere convenient (You'll want the RGB inputs to be as close as possible to Q3, Q5, and Q7.
