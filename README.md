# HelioMax
HelioMax is a modular extended variation of the classic split mechanical keyboard, ErgoDox. Optimised for detaching the thumb cluster as well as adding an also detachable F-key row. Please note that this is a **wired** keyboard, designed to have the controller and USB connection in a detached central hub.

## Project Architecture

Unlike traditional monolithic keyboard layouts, **HelioMax** breaks the hardware matrix down into standalone, independent modules. This allows the modification of, or iteration upon, individual sections of the keyboard independently, without needing to re-fabricate the entire board.

The second main feature of the HelioMax is the fact that it is designed to be controlled by a single **modified** Raspberry Pi Pico. Each half of the HelioMax consists of the central board, which includes all the communication lines to the controller board, as well as the connections to the thumb cluster and F-key row of its side. The entire half is using a 7×7 integrated matrix and each half is designed to be **independently scanned** by the Pico.

The importance of the "modified" part becomes relevant at this point. The **unmodified** Pico only has **23** consecutive GPIO pins available (GPIO0 - GPIO22). However, two halves of the HelioMax require 2×(7+7)=28 GPIO pins. While this is still attainable using a unified row or column scanning, I really wanted the two halves to be independently scanned. The solution was somewhat easy: the Pico *does* have GPIO23-25 present, only assigned to things redundant for this project. With a soldering iron, a scalpel, and some patience, these pins can be liberated and put to use, the result being a contiguous GPIO space starting at GPIO0 and ending at GPIO28 -- and that leaves **one** extra GPIO to do with as we please.
* **GPIO23**  
  Requires cutting two traces next to TestPoint 4 (TP4) on the underside of the Pico PCB and desoldering one tiny resistor near the microUSB port (R8). The signal is now accessible from TP4!  
* **GPIO24**  
Requires desoldering of two tiny resistors (R1, R10) next to the RP2040 chip and soldering a tiny wire on the bottom (common) pads, which will carry the GPIO24 signal.
* **GPIO25**  
Requires desoldering the LED (D2) and replacing its current-limiting resistor (R3) with a tiny piece of wire (or a solder blob). The GPIO25 signal is now accessible from TP5!

Now, the GPIO range is uninterrupted and at our disposal.

Moreover, the PCB itself needs some slight modification, namely filing down the backplane window, as I have made the choice to include full pads rather than castellated pads, in order to reduce manufacturing cost. The _offending_ pads are meant to connect the TP4 and TP5 of the Pico to the PCB.

The repository includes a legacy 4-in-1 master layout blueprint at the root for spatial configuration, alongside four distinct, decoupled sub-projects representing the four "quadrants":

### The Four Quadrants
* **Praetor Enclave (`/Praetor_Enclave`)**  
  The controller board. Houses the Raspberry Pi Pico micro-controller and connections to the two halves (Rows and Columns) on separate JST-SH sockets.
* **Corpus Sedes (`/Corpus_Sedes`)**  
  The main mechanical switch matrix body. Serves as the primary foundation for your standard alpha-numeric typing layout.
* **Telorum Belt (`/Telorum_Belt`)**  
  The function top row. Positioned specifically above the main matrix to the exact position F-keys should be. The connection is regular pins and can be lined up with mounting holes aligned. Recommending 5mm gap.
* **Pollex Cluster (`/Pollex_Cluster`)**  
  The specialized ergonomic thumb cluster extension, now able to be angled to leverage natural thumb position for primary keys like Space, Enter, Layer control, or whatever suits your typing style.

## Manufacturing & Gerber Instructions

Each directory operates as its own self-contained KiCad project. No particular instructions needed.

## Licensing & Contributions

This project is open-source. Feel free to clone the repository and spin up custom branches to match your preferred physical desk ergonomics.

## Support the Project
If you find this design useful or want to support future hardware iterations, consider buying me a coffee! Donations most welcome.

[![PayPal](https://shields.io)](https://paypal.me/skagon)
