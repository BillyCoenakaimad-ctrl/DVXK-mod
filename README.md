HOW TO USE: JUST COPY AND PAST THE DLL AND CONF FILES TO YOUR SPECIFIC GAME FOLDER (where the .exe exists)
EXAMPLE: IF THE GAME YOU WANT TO FIX IS REV 2 THEN GO TO THE REV2 FOLDER AND COPY THE DLL AND .CONF FILES TO THE ROOT OF YOUR GAME FOLDER.

HOW THIS FIXES THE RESIDENT EVIL REVELATIONS 2 STUTTERING ISSUE:
. Bypassing the OS "Middleman"
dxgi.tearFree = False

dxgi/d3d9.numBackBuffers = 2 In Windows 7, when you go "Fullscreen," the game takes total control of your monitor. In Windows 10/11, the OS forces a "windowed" layer over everything so you can Alt-Tab faster. This layer adds a tiny delay (stutter). By setting tearFree to False and limiting BackBuffers to 2, you are telling DXVK: "Don't try to sync with the Windows Desktop Manager; just send the frames straight to the GPU as fast as they are ready." It restores that "raw" connection the game had back in 2015.

. Preventing "Memory Churn"
d3d9.memory_pool = "system" #imp: This is arguably the most important line for Capcom's MT Framework engine. Modern Windows likes to move data in and out of your Video RAM (VRAM) constantly to save space. However, Revelations 2 expects its assets to stay exactly where it put them. By forcing the memory pool to system, DXVK creates a "mapped" area where the CPU and GPU can both see the data instantly without the OS "paging" (swapping) it out. This stops those micro-stutters that happen when you turn a corner or open a door.

. Tightening the "Handshake"
d3d9.maxFrameLatency = 1 By default, modern Windows tries to "queue up" 3 or 4 frames ahead of time to make things look smooth. But in a game with an unoptimized CPU engine like this one, the CPU often falls behind the GPU. This "latency" causes a desync where the game hitches while the CPU tries to catch up. Setting this to 1 forces the CPU and GPU to stay in a tight "1-to-1" lockstep. It's more demanding on the hardware, but it eliminates the "rubbery" feeling and the erratic frame delivery.

. Simplifying Visual Handshakes
d3d9.forceSwapchainMSAA = 0 The "Flashlight Stutter" in this game happens because the lighting effects use a specific transparency technique that modern Anti-Aliasing (MSAA) struggles to interpret. By forcing the SwapchainMSAA to 0, you ensure that DXVK isn't trying to apply modern smoothing techniques to legacy lighting effects, which keeps the frame timing flat and consistent instead of "spiking" every time you aim your light.

HOW THIS FIXES THE RESIDENT EVIL 6 STUTTERING ISSUE
. d3d9.allowDirectBufferMapping = False
This is the "Silver Bullet" for Resident Evil 6.

The Problem: RE6 (and several other Capcom games) uses a very old method of "talking" to the GPU where it asks to write data directly into the GPU's memory buffers. On modern Windows 10/11 drivers, this often causes a "Sync Lock." The CPU has to stop and wait for the GPU to finish everything it’s doing before it can write that data.

The Fix: By setting this to False, you tell DXVK to use an intermediate "staging" area. The CPU writes its data to a temporary spot and moves on immediately, and DXVK handles the transfer to the GPU in the background. This eliminates the "hiccup" every time the game tries to update character models or lighting.

. d3d9.deferredSurfaceCreation = True
This line targets "loading stutters" and transition hitches.

The Problem: RE6 often tries to create new textures and surfaces (like blood splatters, fire effects, or new room textures) the exact millisecond they are needed. This causes a frame drop because the game pauses to build that surface.

The Fix: This tells DXVK to "defer" or delay the actual creation of these surfaces until the GPU is in a quiet moment or to handle it more efficiently in the background. It prevents the engine from "choking" when a lot of new visual effects appear on screen at once (like during the explosive Leon/Helena campaign).

. dxvk.hud = fps
While this seems like just a counter, it actually serves a hidden "stabilizing" purpose in some environments.

The Science: When the DXVK HUD is active, it forces a very specific, consistent polling rate for the Vulkan overlay. In some weird edge cases with Windows' Desktop Window Manager (DWM), having an active overlay can actually help keep the GPU "awake" and prevent it from aggressive power-saving downclocks that cause micro-stuttering.

Mainly: It’s also just great for you to see that your Frame Times (the line graph) are flat, which confirms the stutters are gone!

UPDATE 1.1 final update: Figured out that for resident evil revelations 2 the mod I created doesn't get rid of stutter, it only improves performance, I deduced that it's an OS compatibility issue especially after reading people's comments how the game used to run perfectly on windows 7, so I tried to work on it taking that into consideration: IT WORKED, HAVE FUN!
