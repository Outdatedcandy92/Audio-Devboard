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

---
## PCB Time!!

Currently writing this journal at 1am, I had basically the most productive day today and genuinely entered flow state while designing the PCB.

I first started off with just completing up the schematics and assigning footprints.

*Schematics*
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/1e0f8e1de0d40cf0_A0_jl2vLX5n1AAAAAElFTkSuQmCC)


You can probably see I added a bit more stuff, such as the mounting holes, the pin header and some general purpose switches.

After doing that I assigned net classes to different sections like power, SPI signals and USB signals so I could visualize them better in the PCB editor.

*PCB*
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/a7f59afcc087a500_PGDyTL7zBrRj11NVgMAAAAAAAAAgLtl7rGPxcWnJrpP4Org8AMfiZf_6kFNUHeJNaIfJqsBAAAAAAAAAIyAUZ6sBowGk9UAAAAAAAAAAAAYuP8fNHanwrwyilYAAAAASUVORK5CYII_)


Assigning different net colors made it a whole lot easier for me to layout components as I could easily visualize how connections would look without being overwhelmed by a bajillion rastnets. 
Component layout took a *long time*, I went with a 56x36mm board size and added a 2.5mm fillet to the edges. 
I actually spent quite a significant bit of time just laying out components, as I wanted it to be functional and look like a work of art. You can sort of see the layout progress down below.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/18267a804118e686_39DRN5VKErpOwAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/2a53f6e3605514d6_akzzCnCNaV4WNZUV5c6RLJNsRFBrTPPyqcY0L4sa5t4sL59qTPOyqGHuzfLyqcY0L4sa5t4sL59qTPOyqGHuzfLyqcY0L4sa5t4sL59qTPOyqGHuzfLyqcY0L4sam7mX5XVM_VFjmpdPNaZ5WdQw92Z5_VRjmpdFDXNvlpdPNaZ5WdT0du4zVFzW6T0JAAAAAAAAAEhtCVf_AgAAAAAAAABSE81fAAAAAAAAAAghmr8AAAAAAAAAEEI0fwEAAAAAAAAghGj_AgAAAAAAAEAI0fwFAAAAAAAAgBCi_QsAAAAAAAAAIUTzFwAAAAAAAABC6P8HBAJ7fmdM3zcAAAAASUVORK5CYII_)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/c4d26786684b540a_ayxZQAwAAAChxBAAAAAAAwIkjAAAAAAAAShwBAAAAAABw4ggAAAAAAIASRwAAAAAAAJw4AgAAAAAAoMQRAAAAAAAAJ44AAAAAAAAocQQAAAAAAMCJIwAAAAAAAEocAQAAAAAAcOIIAAAAAACAEkcAAAAAAACcOAIAAAAAAKDEEQAAAAAAACeOAAAAAAAAKHEEAAAAAADAiSMAAAAAAABKHAEAAAAAAHDiCAAAAAAAgBJHAAAAAAAAnDgCAAAAAACgxBEAAAAAAAAnjgAAAAAAAChxBAAAAAAAwIkjAAAAAAAAShwBAAAAAABwA5ijW6Of0WKqAAAAAElFTkSuQmCC)


This is how it all looked out in the end!

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/6250318b511a3f72_wm3_K8ImwxYAAAAASUVORK5CYII_)


I was pretty happy with this layout and moved onto routing the board. Routing this board was a teeny bit problematic for me, mainly because of how crowded some areas of the board were. But here's an in progress picture of routing.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/0eb6c7a5f9a981db_wMYVFnFXAjgcQAAAABJRU5ErkJggg__)

Right around this time I noticed something, I accidentally put the MicroSD card reader in the wrong orientation, the pins are actually suppose to be facing the edge of the board like in the image shown down below. Now this came with it's own set of problems, mainly being that there was now not enough space between the reader and the STM32, and I can't move the STM32 to the left.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/11c64415b0d2c7b1_P7tn6y7E3r0GAAAAAElFTkSuQmCC)


To solve the space issue, I decided to switch out my SD card reader from a push-push to a push-pull type which takes up less space.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/415783d52692f3b8_RLePIPL8p3pyQ1w0r0EtXikajzEhERERHFm5phZXgSFR4SdV4iIiIiongaMEuYEgwrwRIVHrhOCxERERGlu4gDC8NKaIkKLVynhYiIiIjSWUSBhWFlcIkKLYk6LxERERHRSFMcWBhWlElUNy2GFiIiIiJKR4oCC8NKZBLVTYuhhYiIiIjSzZCBhWElOokKD4k6LxERERHRSBg0sDCsDE_iwkOizktEREREFGthAwvDSmwkKjwk6rxERERERLEUMrAwrMRWosJDos5LRERERBQrAwILw8rISFR4SNR5iYiIiIhi4f8DSNf4PU9QrIcAAAAASUVORK5CYII_)


After switching the MicroSD card footprint I spent the next few hours just routing the board. I went with a 4 layer stackup of SIG/GND/PWR/SIG for now. Though I do want to switch it out later for something better where the bottom layers actually have a solid ground reference, right now audio signals are going through Top layer to Bottom layer, and are referencing a power plane on the bottom layer which is not good. 

*PCB Rev 1*

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/a001d6abb664ae2d_y5AAAAAElFTkSuQmCC)



Here you can see the signals on the bottom being routed over power planes with some splits, most of them are just low frequency gpio signals so it doesn't matter
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/30e4699e443a859f_X9tB9iG6SaudQAAAABJRU5ErkJggg__)


### Time Spent:


---

## Finished up PCB

Looking back at my design this morning, I saw a lot of room for improvement and did a whole lotta changes on the boards.

The first major change I did was I deleted a bunch of traces under the VS1053B and thought about how to route them better. One such example would be the DCS and CS traces as shown below' routing them to different pins on the STM32 would allow me to route the traces OUT of the VS1053 instead of IN if you get what I'm saying.


![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/1ed4b82902c2d29f_j84PNW9jKnUqwAAAABJRU5ErkJggg__)


The second thing I did was I made both the inner layers a solid ground pour to server as reference plane. For power I decided to pour it on top and bottom layers as shown below-

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/c683dcb0e3950739_wc6CaCN1yhYywAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/37da45c7388d7c8d_8BPyyuFXxk3XoAAAAASUVORK5CYII_)

Doing this was a bit more harder compared to just a straight up power plane pour for obvious reasons; I had to now manually route traces to pull up resistors and 3v3 pins that were far away from the VS1053B, but it was still overall worth it.
I spent quite a bit of time just cleaning up the board and optimizing the traces. After I was satisfied I moved onto silkscreen!!
Having good silkscreen labels is so vital on a PCB, it saves you so much time if you label all the GPIOs and important stuff so you don't have to refer back to the schematic later.

(oh yeah another thing, I switched from a 2x5 header to a 1x8 header just cause I didn't have enough space to route out 10 signals)


![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/b2dd289122713b5f_wHNbCRM2c7NvwAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/29a8bb6875ae4185_6hlgIAAAAAAANQ4QoAAAAAMJDy6adtmgUAAAAAMAQVrgAAAAAAAxG4AgAAAAAMxKZZAAAAAAADUeEKAAAAADCQ8umnVbgCAAAAAAxBhSsAAAAAwEAErgAAAAAAAymffvrntRQAAAAAABhA_dRTergCAAAAAAxBSwEAAAAAgIEIXAEAAAAABiJwBQAAAAAYiE2zAAAAAAAGUj71lMAVAAAAAGAIfwdhYK6DP4SyDQAAAABJRU5ErkJggg__)


After this I added some more tiny things to the board like Test Points for the power rails and some LEDs connected to the STM32 

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/f6ea6370ea5ea07a_MHh4xwv4W7AAAAAASUVORK5CYII_)


![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/d8da1617ba7c607d_AfMDPZrQlzRDAAAAAElFTkSuQmCC)


I also ended up rerouting my stereo out traces, I added 20R series resistor to them in case I wanted to drive lower impedance earphones (the VS1053 is only capable of driving 30ohm loads). The traces were now routed OUT from the chip and then went to the 3.5mm jack; they originally were routed IN the chip and the whole area was pretty crowded so this was a major improvement I would say.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/191cbc50852b207c_wMLWo7Q01yTxAAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/f87cb0329143f308_F9xIVhdTWl9JSK5BGYzdt2Yfqd06ibvyyukqO1L7iFXb0yE5Mzis_dL9uLVr0hXKsCKRZ_cW0FpTqzYntoses1a6ENGLGsoxVL2xeJxUVquQ2YUUpP6f8HbDYHeq7aXoMAAAAASUVORK5CYII_)


I also spent some time setting up the github repository and making a reddit post on r/PrintedCircuitBoards for a board review for this.
All in all this how my board looks like at the end of the day!

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/4f8097807a9fbefc_H9mUungo6XIWQAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/1a595181ce554880_H4HupGMFF55hAAAAAElFTkSuQmCC)

( I absolutely cooked on the silkscreen )

### Time Spent: 6 Hours





