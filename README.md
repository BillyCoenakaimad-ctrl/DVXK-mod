**THE "GHOST" IN THE ENGINE
The stuttering you're seeing isn't because your PC is weak. It’s a memory bottleneck. Every time RE6 or Rev 2 needs to load a texture for a floor, a door, or a blood splatter, the engine pulls the <mark>emergency brake</mark> on your CPU to wait for the integrated GPU. On Intel chips, this "handshake" is broken. The game isn't lagging; it's literally stopping to ask for permission to continue. We fixed this by moving the game to Vulkan using DXVK, which lets us <mark>rewrite the engine's logic</mark>.

KILLING THE 1-SECOND FREEZE
The first thing we did was kill the memory lock. By disabling allowDirectBufferMapping, we told the engine to stop trying to access the GPU's memory in a way that <mark>freezes the system</mark>. Instead of the CPU and GPU constantly interrupting each other, they now use a "staging" method. This is why you can finally walk through doors and over new floor textures without the game <mark>tripping over its own feet</mark>.

RESTORING THE SMOOTHNESS
Earlier tweaks made the game stable but "heavy" or slow. We fixed that by finding the sweet spot with maxFrameLatency = 2. At a value of 1, the game was too strict and felt like a slideshow. By bumping it to 2, we gave the CPU just enough room to start working on the next frame while the current one is still on screen. It <mark>restores that "liquid" flow</mark> to the movement while keeping the lag dead.

FIXING GUNSHOTS AND EXPLOSIONS
If your game hitched every time you fired a gun, it’s because of an ancient tech called R2VB that Intel HD chips don't understand. The engine <mark>panics</mark> trying to calculate sparks and blood. We used floatEmulation = 0 to bypass that trash and deferredSurfaceCreation = False to force the game to have those effects ready before you pull the trigger. <mark>No more "hiccups" mid-combat</mark>.

THE REV 2 ENGINE RESET
Revelations 2 is a disaster under the hood. It has a bugged internal clock that gets out of sync with your hardware. That’s why you have to do the <mark>ritual</mark>: launch the game, switch to 30 FPS for a few seconds, then flip it back to Variable. It’s like a <mark>jump-start for the engine’s heart</mark> to make sure all our technical fixes actually align with your monitor's refresh rate.**
