# Audio Devboard

---

## Initial Planning - 18/12/25

My goal with this project is to create a small handheld development board dedicated just for audio. It should be able to play lossless audio formats like flac, and should also be capable of playing lossy formats like mp3. I also wanted to use a chip that's dedicated for this purpose, I didn't want to just use a powerful MCU and use that to decode it and then use a DAC, I wanted something which had everything baked in.

After a lot and lot of searching on the internet the only available option that I really had were ICs by VLSI. I've used chips from VLSI before but I didn't want to use them again mainly because they  are hard to source, and are mostly just available through weird European distributors which are super expensive plus on top of that there is expensive shipping, getting just one simple IC from them would cost me around $50!!

I tried super hard to search for alternate ICs but had no luck, I eventually found out that the chips from VLSI were in stock on lcsc and were reasonably priced (I swear they were out of stock the last time I saw them so that's why I was trying so hard to find alternatives)

I ended up with using the VS1053B from VLSI
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/d5c6e60ba6ed9850_AfAih9lQ9aDrAAAAAElFTkSuQmCC)

It's a good chip overall, does basically all audio formats, can drive 32 ohm headphones directly plus it's still affordable on lcsc (it was like 15 euro on other websites). The only thing which I hate about this is the fact that it's a LQFP package only. 

I skimmed a bit of it's datasheet just to get a rough idea of what I'm working and then started working on the high level block diagram of how my board was gonna be like.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/7be1d436199d625f_wAAAABJRU5ErkJggg__)

At it's core, its literally just a MCU sending data over to the VS1053 which decodes and plays it for us. I've got USB there to power stuff up and program the MCU, and SD card to store music files (and config!?).

### Time Spent: 2 Hours 

---
## schematic and concept sketches - 21/12/25

Winter break has officially started and it is now time for me to lock in!! I basically spent my entire day today working on this project :D

I firstly started off with component selection for my project. All I really needed to find was a MCU and LDOs.
I did some research and ended up selecting the STM32F042 as my MCU, for 2 main reasons: 
a) It does crystal-less USB FS
b) Its tiny and comes in UFQFPN package
bonus: It's also relatively cheap at only ~$2.5
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/849da8e007aa5dc9_AVv_G5zAVbQFAAAAAElFTkSuQmCC)

For power I just went with LP5907MFX-X.X LDOs for now.

After component selection was done I moved onto schematics, I started off with the VS1053B first, luckily it's datasheet had a reference schematic which I could basically just follow and copy (although it was a bit confusing)

*Reference Schematic*
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/f2642a7fe944f385_wMA3UgT3w8vGwAAAABJRU5ErkJggg__)

I decided to add a stereo line for my design instead of a microphone because I have absolutely no clue what is going on here right now.
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/759c3894a4d5cc98_GOY3Dv7w5yEAAAAASUVORK5CYII_)


For my MCU, I just skimmed through the datasheet to find the SPI and USB pins and then connected those. I did forget how the STM32 boot pins work so I had to google that (I forget how it works every single time I'm working with them).

Anyways I did some little datasheet reading before connecting up the SPI and SCI interface of the VS1053B to the STM32. I also added a MicroSD card reader onto the STM32's SPI bus. 
And finally I added a USB type-c receptacle and added LDOs for the 3v3 and 1v8 rail.

*Final Schematic (Almost Finished)*
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/bd4a15d67c4f8356_bswMBAAAAAEH_1oNcGokxAAAAAAAAFsQYAAAAAAAAC2IMAAAAAACABTEGAAAAAADAghgDAAAAAABgQYwBAAAAAACwIMYAAAAAAABYEGMAAAAAAAAsiDEAAAAAAAAWxBgAAAAAAAALAR8S2nzIXn_JAAAAAElFTkSuQmCC)



After finishing up the schematics, I moved onto concept sketches for the PCB. 
Drawing up concept sketches is something that actually helps a lot I would say, as it gives you a sense of direction and just makes it so much easier than just directly jumping into the PCB with no sense of what you want.


*Revision 1*
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/02a1fbd4978ed172_ALZ1Xa2VuvHVAAAAAElFTkSuQmCC)

For Rev 1, I had a rectangular board with the decoder chip in the middle and SD card and GPIO on opposite sides of the boards, and then some on board buttons to interact with the chip (add pause/play and whatever functionality I wanted later on)

But I quickly realized that this isn't really optimal as I'd have to route SPI traces from the SD card to the MCU and then to the VS1053B. it would be better if the STM32 was in between of the VS1053 and SD card.


*Revision 2*
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/9caec16beaafccfa_w_ySRCDXfwYngAAAABJRU5ErkJggg__)


I was quite happy with revision 2 of the design, so I went back to kicad and just assigned footprints to most of the parts and called it a day.

### Time Spent: 5.5 Hours