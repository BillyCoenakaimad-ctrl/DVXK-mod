.# DVXK-mod
DXVK modifications for old resident evil 6 and revelations 2 engines (fixing stuttering)
##Contains specific DXVK configuration files to fix the 1-second freezes and constant micro-stutters in Resident Evil 6 and Revelations 2.

These fixes are specifically tuned for Intel HD/UHD graphics (like the Intel HD 630) but work for anyone whose hardware should be running these games smoothly but isn't.
​The stuttering happens because the MT Framework engine has a logic bottleneck where the CPU and GPU share memory. These config files bypass those "handshakes" and force the engine to process data without locking the CPU.
​Files Included
​Resident Evil 6 folder: dxvk.conf
​RE Revelations 2 folder: dxvk.conf
​Required Binaries: You must download DXVK 1.10.3 manually (see instructions below)
##​Installation Instructions**
​Download DXVK version 1.10.3 from the official releases. Do not use newer versions as they may not be compatible with integrated graphics drivers.
​From the x32 folder of the DXVK download, copy d3d9.dll and dxgi.dll into your game's main folder (where the .exe is).
​Copy the dxvk.conf from this repository into the same game folder.
​For Revelations 2: You must also edit your game's internal config.ini (usually in AppData/Local/CAPCOM) with the settings listed below.
​Important Ritual for Revelations 2
​Revelations 2 has a broken internal clock. Every time you launch the game, go to Graphics Settings, set the frame rate to 30 for a few seconds, move around, and then set it back to Variable. This aligns the engine with your hardware and kills any remaining rhythmic stutter.
​Resident Evil 6 Config (dxvk.conf)
