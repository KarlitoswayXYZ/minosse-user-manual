                       
                       
                       __
               ___ __ /\_\     __     ___     ___     ___     ___
             /´ __` _`\/\ \  /´ _`\  / __`\  / __`\  / __`\  / __`\
            /\ \/\ \/\ \ \ \/\ \/\ \/\ \/\ \/\ \/_/ /\ \/_/ /\ \L\ \
            \ \ \ \ \ \ \ \ \ \ \ \ \ \ \_\ \ `___ `\ `___ `\ \ \__/_
             \ \_\ \_\ \_\ \_\ \_\ \_\ \____/`/\____/`/\____/\ \____/
              \/_/\/_/\/_/\/_/\/_/\/_/\/___/  \/___/  \/___/  \/___/
      
    Minimal sIgnal path, Non-OverSampling, cuStom pipEline Digital Signal Processor

                  (Graphic Equalizer and Digital Room Correction)

                             a plugin for Volumio 3

                             © 2021 Carlo Raffaelli


TABLE OF CONTENTS
-----------------

  * [INTRODUCTION](#INTRODUCTION)
  * [HARDWARE REQUIREMENTS](#HARDWARE-REQUIREMENTS)
  * [TROUBLESHOOTING AND KNOWN ISSUES](#TROUBLESHOOTING-AND-KNOWN-ISSUES)
  * [CREDITS](#CREDITS)
  * [DONATIONS](#DONATIONS)
  * [APPENDIX 1](#APPENDIX-1)
  * [APPENDIX 2](#APPENDIX-2)



INTRODUCTION
------------

Minosse DSP is a Digital Signal Processor plugin for [Volumio 3](https://volumio.org/) based on [Brutefir](https://torger.se/anders/brutefir.html), a popular Open Source convolution engine by Anders Torger. The priority of Minosse DSP is to deliver the best possible sound quality along with a pleasant user experience. Generally speaking, the computer audio industry is more oriented towards versatility rather than audio quality. In the digital world, often happens that the audio signal path becomes a jungle of unidentified elaborations which unique purpose is to deliver audio output at all costs. In this context the advantages of digital signal processing might be lost along the way because of inadequate, misconfigured or poorly implemented algorithms that the user is completely unaware of.

Minosse DSP is intended to be a rock solid, no-frills, easy-to-use Digital Signal Processor. It provides a 10 band graphic equalizer and [Digital Room Correction](https://www.drc.one/), along with multi channel options for building digital x-overs. Integration with [Volumio 3](https://volumio.org/) is profound: volume cursor and resampler configurations are overridden to become part of Minosse itself. The advantages are:
- audio signal path is minimal, as a single elaboration (the convolution engine) performs several tasks
- attenuation is implicit; no need to configure filters' attenuation separately to avoid saturation (unless you use to play music at maximum volume!)
- volume control for multi channel mode (up to 8 channels, the standard 5.1 and 7.1 Surround cards are well supported; see [APPENDIX 2](#APPENDIX-2)) is solved at the root and you don't need to buy or configure any other dedicated tool
- resampling is only used in two cases:
  - audio source sampling rate is not supported by the output device
  * audio source is not in [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) format (e.g. [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital) audio)
- all the above is done automatically, so that the steps to achieve the best audio quality are made depending on the underlying hardware and are completely transparent to the user

HARDWARE REQUIREMENTS
----------------------

**CPU and mother board:**

64 bit support is mandatory. CPU support for [SSE2](https://en.wikipedia.org/wiki/SSE2) instruction set is strongly advised. If [SSE2](https://en.wikipedia.org/wiki/SSE2) is present, Minosse can configure the system for maximum performance (best audio quality for both convolution engine and resampler, lower latency, etc.). For x86-64 systems, any CPU from Intel I3 or AMD Athlon 64 on should be fine. Minosse has also been successfully tested on a Raspberry Pi 3B+.

**DAC:**

The best option is a DAC that supports all standard sampling rates in the range from 44100 to 192000 Hz, so that the resampler is never activated. Minosse has been limited to 192kHz to avoid excessive straining on the CPU. Hence, DAC support for higher sampling rates is irrelevant. Bit depth should be 32 bit, so that the dynamic range reduction due to the volume control is well compensated. For this reason, as a minimum requirement your DAC should support 24 bit audio at the very least. In case of DACs only supporting 16 bit format, dither is applied automatically.

[Brutefir](https://torger.se/anders/brutefir.html) only works with [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) formats and Minosse automatically configures [Volumio 3](https://volumio.org/) to convert [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital) streams to [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) audio, even though your DAC does support [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital).

TROUBLESHOOTING AND KNOWN ISSUES
--------------------------------

- **Stuttering sound:** This is very rare and may happen because audio sources are slow (for example when you stream audio from a remote server). In general, it's enough to press stop and then play again. Should it persist, check your network and/or streaming service throughput.
- **Unresponsive to commands:** if Minosse seems unresponsive to new configurations and commands, it is likely that [Brutefir](https://torger.se/anders/brutefir.html) needs to be reloaded. This happens for example when you add new coefficient files to the filter folders. Their coefficient ID will appear as an option in Minosse configuration panel, but the new coefficients will be inaccessible to [Brutefir](https://torger.se/anders/brutefir.html) until it is reloaded. To reload [Brutefir](https://torger.se/anders/brutefir.html), simply press stop and then play again.
- **Latency time:** Minosse responsiveness is fair enough, but don't expect it to be blazing fast, even though you are using the latest cutting-edge hardware. The price to pay for [FIR](https://en.wikipedia.org/wiki/Finite_impulse_response) filtering and automatic configuration is a certain amount of latency time. The user is always informed of what is going on through [Volumio](https://volumio.org/) standard messages. As a rule of thumb, while messages from Minosse are shown on the screen don't do anything until they disappear.
- **Languages:** At the moment, only English and Italian translations are available.


To be continued...
==================


CREDITS
-------

- Minosse is a plugin for [Volumio 3](https://volumio.org/). As such, it has been created using the plugin framework devised by the [Volumio team](https://volumio.org/about-us/)

- At the heart of Minosse is [Brutefir](https://torger.se/anders/brutefir.html), a very fast convolution engine by [Anders Torger](https://torger.se/anders/)

- [Rephase](https://rephase.org/) is a [Finite Impulse Response](https://en.wikipedia.org/wiki/Finite_impulse_response) (FIR) generation tool by Thomas Drugeon (runs on Windows only)

DONATIONS
---------

If you appreciate my work, please make a donation to support the development of Minosse DSP plugin. Thanks.

To donate, click or scan the QR code:

<a href="https://www.paypal.com/donate/?hosted_button_id=3X4ESPGKFDATU">
  <img src="https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/QR_Code.png" width="130" height="130"/>
</a>

APPENDIX 1
----------

Rephase suggested configuration for each sampling rate
------------------------------------------------------

**44100 Hz:**

![Rephase configuration for 44100 Hz](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/rephase-44100.png "Rephase configuration for 44100 Hz")

----------------------------------------------------------------

**48000 Hz:**

![Rephase configuration for 48000 Hz](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/rephase-48000.png "Rephase configuration for 48000 Hz")

----------------------------------------------------------------

**88200 Hz:**

![Rephase configuration for 88200 Hz](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/rephase-88200.png "Rephase configuration for 88200 Hz")

----------------------------------------------------------------

**96000 Hz:**

![Rephase configuration for 96000 Hz](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/rephase-96000.png "Rephase configuration for 96000 Hz")

----------------------------------------------------------------

**176400 Hz:**

![Rephase configuration for 176400 Hz](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/rephase-176400.png "Rephase configuration for 176400 Hz")

----------------------------------------------------------------

**192000 Hz:**

![Rephase configuration for 192000 Hz](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/rephase-192000.png "Rephase configuration for 192000 Hz")

APPENDIX 2
----------

Minosse output configurations for standard Surround audio cards
---------------------------------------------------------------

![Multi channel table](https://github.com/KarlitoswayXYZ/minosse-user-manual/blob/master/img/multichannel_table.png "Multi channel table")

Please, note that although Minosse can exploit multi channel output from Surround cards, it does not support any Surround format and only works with stereo PCM audio sources. For more info about jack colors and their function, visit [this site](https://www.phase4.org/pdfs/The%20Ins%20and%20Outs%20of%20PC%20Audio%20Jack%20Colors.pdf). All "Sub" outputs work in mono, as they have both left and right channels as input.

