#  Kaypro

The Kaypro computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.


## Kaypro II (and other 83 models)

Example compile line:

    zcc  +cpm -subtype=kayproii -create-app program.c

A .dsk file in Kaypro format is produced. This can be loaded into an emulator or transferred onto a physical disc as required.

The 83 models support primitive graphics, two graphics modes are available:

* Mode 0: 80x24 solid characters. Though borders are visible around each "pixel"
* Mode 1: 80x48 pseudo graphics. This mode takes advantage of the weight characteristics of the Kaypro font to provide a slightly higher resolution.

The generic console is available on the Kaypro-83 machines.

### Screenshots

![](images/platform/kp-dstar.png)
![](images/platform/kp-globe.png)

## Kaypro 4 (and other 84 models)

These machines contain an MC6845 and are capable of producing low-resolution graphics at a resolution of 160x100.





