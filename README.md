This is a fully fledged out Canny edge detector simulation with timing delays hard coded in. These values were found by running a previous Canny assignment on a smaller image
and measuring the time each module took. Then, to account for the larger image size of Keck, the wait times were multiplied by a value relative to the frame size of the new image.
It was then ran on the server's RISC-V modeled processor. Sample image included but simulation was meant for multiple frames at a time to simulate pipeline. 
