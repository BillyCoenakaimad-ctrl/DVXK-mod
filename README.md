UPDATE: 1.2: I thought there was no room for improvement for this project, figured out there is still, since this is what I might call fighting a ghost machine. From now on this project will only focus on fixing resident evil revelations 2.
HOW TO USE (please read carefully): FIRST, RIGHT CLICK REV2.EXE, GO TO PROPERTIES, COMPATIBILITY AND THEN CHECK "Disable fullscreen optimizations" AND "RUN THIS PROGRAM AS AN ADMINISTRATOR" then click APPLY, AFTER THAT, COPY AND PAST THE DLL AND CONF FILES TO YOUR GAME FOLDER (where the RE REV2.exe exists)

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
