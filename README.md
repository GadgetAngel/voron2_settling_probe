## Voron 2 Settling Probe which I plan to use on my Ender 3 printer and Voron 2.4 printers

This work was mostly done by @voidtrance, He has a GitHub Repo and calls his version ``settling_probe``.  I recommend you check his out his Repo. His GitHub Repo can be found at [@voidtrance GitHub Repo](https://github.com/voidtrance/voron-klipper-extensions)

A Klipper Extension for the Probe Obj that allows the first probe sample to be discarded from the following commands:

```BASH
PROBE
PROBE SETTLING_SAMPLE=0
PROBE SETTLING_SAMPLE=1

PROBE_ACCURACY
PROBE_ACCURACY SETTLING_SAMPLE=0
PROBE_ACCURACY SETTLING_SAMPLE=1

PROBE_CALIBRATE
PROBE_CALIBRATE SETTLING_SAMPLE=0
PROBE_CALIBRATE SETTLING_SAMPLE=1

QUAD_GANTRY_LEVEL
QUAD_GANTRY_LEVEL SETTLING_SAMPLE=0
QUAD_GANTRY_LEVEL SETTLING_SAMPLE=1

BED_MESH_CALIBRATE
BED_MESH_CALIBRATE SETTLING_SAMPLE=0
BED_MESH_CALIBRATE SETTLING_SAMPLE=1

PROBE_Z_ACCURACY
PROBE_Z_ACCURACY SETTLING_SAMPLE=0
PROBE_Z_ACCURACY SETTLING_SAMPLE=1

CALIBRATE_Z
CALIBRATE_Z SETTLING_SAMPLE=0
CALIBRATE_Z SETTLING_SAMPLE=1
```
---

I developed my own version of @voidtrance code because I own and Ender 3 and Voron 2.4 printer. I especially did it for my Ender 3 which uses a BLTouch.  Furthermore, I see a lot of first samples that are outliers from the rest of the sample set.

Besides, I am looking forward to adding this to my Klipperized Ender 3 printer!

---

### Settling Probe, GadgetAngel's version; This version of the settling probe is called ``z_v2settling_probe``

I quote from @voidtrance GitHub page:
```
For some currently unknown reason, Voron printers seem to suffer from an issue
where the first probing sample is off by some margin. Subsequent samples are
much closer (or the same) to each other. The current theory is that there is
some toolhead/axis settling on the first sample.

In order to avoid polluting the probe samples, the first sample should be
thrown away.

This extension adds support for performing a single, throw-away, settling
probe sample that is not part of the sample set used for calculating Z
positions.

The extension replaces the default `probe` Klipper object with the modified
one in order to allow all commands/operations that perform Z probing to
benefit from this.
```

I would add that the Ender 3 and other printers that use a BLTouch also experience this
issue with the first probe sample.

Especially when it comes to performing BED_MESH_CALIBRATE!

---
## BEFORE you install ``z_v2settling_probe`` on your Klipper 3D printer, do the following:

This extension does not add the ability to attach your Klicky probe/ Euclid probe to your tool head.  It also is not responsible for docking your Klicky probe/ Euclid probe.  You need to ensure that your probe is working, I mean you can do a BED_MESH_CALIBRATE, your homing procedure `G28` and your leveling procedure (QGL or BED_TILT_ADJUST).

This extension adds a sample to the very beginning of the probing sequence.  If you need to have the probe attached then you are responsible for writing the Macros that will allow the probe to attach automatically and docked automatically for each of the following commands.

This extension does not do any checking to ensure that when you probe the bed you are over the bed surface.  So ensure you place the tool head in the appropriate location.

The `PROBE_Z_ACCURACY` and `CALIBRATE_Z` will move the tool head to the correct locations because the (`Auto-Z-calibate` plugin)[https://github.com/protoloft/klipper_z_calibration] does that as part of its Klipper extension.

---
## How to enable ``z_v2settling_probe`` on your Klipper 3D printer:

To enable the module, add the following to your `printer.cfg` file:

```BASH
[z_v2settling_probe]
#settling_sample:
#   Globally enable the throw-away settling sample. Default is 'False'.
#   Setting this to 'True' will enable the throw-away sample for all
#   commands/operations that perform Z probing (QUAD_GANTRY_LEVEL, Z_TILT_ADJUST,
#   BED_MESH_CALIBRATE, PROBE, PROBE_ACCURACY, etc.)
```

>:point_up: NOTE: The default for the option `settling_sample` is `False`.  So if you forget to assign `True` or `False` for the option `settling_sample` then the extension will load `False` automatically.  But this only applies if you have a `z_v2settling_probe]` section defined in your printer.cfg file.

The module also augments the `PROBE`, `PROBE_ACCURACY`, and `PROBE_CALIBRATE`, `QUAD_GANTRY_LEVEL`, `BED_MESH_CALIBRATE`,`PROBE_Z_ACCURACY`, and `CALIBRATE_Z` commands with an extra parameter - `SETTLING_SAMPLE` - which can be used to
control whether the commands perform a settling sample independently of the`settling_sample` setting in the configuration section `[z_v2settling_probe]`.  If your printer does not use `QUAD_GANTRY_LEVEL` this extension will still work.

The extension will only augment the commands that apply for your type of printer.  If you do not use (`Auto-Z-Calbration` plugin)[https://github.com/protoloft/klipper_z_calibration] for your Voron Printer, the extension will not load that part of the extension that deals with the `PROBE_Z_ACCURACY`command and the `CALIBRATE_Z` command.

All you will see is a message in your klippy.log file saying that the `PROBE_Z_ACCURACY`command and the `CALIBRATE_Z` commands will not be available for your printer.

---

### **Installation:**

You have two ways you can install this `z_v2settling_probe` extension (manually or via an installation script)

Here is the manual install instruction:

1. Write down the location of the following directories:
    * Klipper Extras: Usually located at `/home/USER/klipper/klippy/extras`.
2. Download `z_v2settling_probe.py` file.
3. Download the `Z_V2SETTLING_PROBE.cfg` and include it into your printer.cfg file.
3. Move `z_v2settling_probe.py` to your Klipper Extras folder (home/pi/klipper/klippy/extras).
4. Restart Klipper service and do a `FIRMWARE RESTART` command if needed

>:bulb: **INFO:** Please restart the Klipper service by using the Mainsail/Fluidd UI (upper right-hand corner Power
>button Symbol) or perform the following command at your Raspberry Pi command prompt (on the Pi that runs Klipper
>for your 3D printer):

```BASH
cd ~
sudo systemctl restart klipper
```

>:point_down: Run the following command:
```BASH
cd ~
```
```BASH
sudo systemctl restart klipper
```

### Here is the installation script method:

Perform the following commands at your Raspberry Pi command prompt:

1. Download the file `Z_V2SETTLING_PROBE.cfg` and place it in the same folder as you `printer.cfg` file.  Now add
a `[include Z_V2SETTLING_PROBE.cfg]` to your `printer.cfg` file.

You could also just type in the following straight into your `printer.cfg` file, but typos can cause errors.

```BASH
[z_v2settling_probe]
settling_sample = True
```

>:bulb: NOTE:
> If you install the extension before you create the`[z_v2settling_probe]` section with the option `settling_sample: True`
> Klipper will NOT load the extension when you restart Klipper.

2. Install the ``z_v2settling_probe` extension to Klipper, please run the following commands:
```BASH
cd /home/pi
```
```BASH
git clone https://github.com/GadgetAngel/voron2_settling_probe.git
```
```BASH
./voron2_settling_probe/install-v2settling_probe.sh
```

---
### **Example Usage: An Example of using `[z_v2settling_probe]` section**

First you must create the `[z_v2settling_probe]` section in your printer.cfg file so Klipper knows to load the `z_v2settling_probe.py` file and include it into the Klipper code running on your Raspberry Pi.

Here are the contents of `Z_V2SETTLING_PROBE.cfg` file:

```python
#-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
# Z_V2SETTLING_PROBE Setup
#-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
#Z_V2SETTLING_PROBE
[z_v2settling_probe]
# z_v2settling_probe has code and the code can be found in the file 'z_v2settling_probe.py'
# z_v2settling_probe also has one additional option parameter called 'settling_sample'
settling_sample = True
```

Now restart the Klipper service by going to the upper right-hand side corner of Mainsail UI and click on the Power Button symbol.  Take your mouse and click on the curved arrow next to the word `Klipper` to restart the Klipper service.

---

### Additional Files:

In this GitHub Repo I have added two additional files, so you can see what my setting where for my Euclid Probe (PROBE_EUCLID(GM).cfg) and my `Auto-Z-Calibration` plugin (Z_CALIBRATION.cfg).

>:point_up: NOTE: These are for reference only. DO NOT load them onto your 3D printer!

---

Now, you are free to try out the following commands:

```BASH
PROBE
```

```BASH
PROBE SETTLING_SAMPLE=0
```

```BASH
PROBE SETTLING_SAMPLE=1
```

```BASH
PROBE_ACCURACY
```

```BASH
PROBE_ACCURACY SETTLING_SAMPLE=0
```

```BASH
PROBE_ACCURACY SETTLING_SAMPLE=1
```

```BASH
PROBE_CALIBRATE
```

```BASH
PROBE_CALIBRATE SETTLING_SAMPLE=0
```

```BASH
PROBE_CALIBRATE SETTLING_SAMPLE=1
```

```BASH
QUAD_GANTRY_LEVEL
```

```BASH
QUAD_GANTRY_LEVEL SETTLING_SAMPLE=0
```

```BASH
QUAD_GANTRY_LEVEL SETTLING_SAMPLE=1
```

```BASH
BED_MESH_CALIBRATE
```

```BASH
BED_MESH_CALIBRATE SETTLING_SAMPLE=0
```

```BASH
BED_MESH_CALIBRATE SETTLING_SAMPLE=1
```

```BASH
PROBE_Z_ACCURACY
```

```BASH
PROBE_Z_ACCURACY SETTLING_SAMPLE=0
```

```BASH
PROBE_Z_ACCURACY SETTLING_SAMPLE=1
```

```BASH
CALIBRATE_Z
```

```BASH
CALIBRATE_Z SETTLING_SAMPLE=0
```

```BASH
CALIBRATE_Z SETTLING_SAMPLE=1
```

Happy 3D printing!!



























