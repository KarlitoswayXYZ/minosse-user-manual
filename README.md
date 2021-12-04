                       
                       
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




INTRODUCTION
============

Minosse DSP is a Digital Signal Processor plugin for [Volumio 3](https://volumio.org/) based on [Brutefir](https://torger.se/anders/brutefir.html), a popular Open Source convolution engine by Anders Torger. The priority of Minosse DSP is to deliver the best possible sound quality along with a pleasant user experience. Generally speaking, the computer audio industry is more oriented towards versatility rather than audio quality. In the digital world, often happens that the audio signal path becomes a jungle of unidentified elaborations which unique purpose is to deliver audio output at all costs. In this context the advantages of digital signal processing might be lost along the way because of inadequate, misconfigured or poorly implemented algorithms that the user is completely unaware of.

Minosse DSP is intended to be a rock solid, no-frills, easy-to-use Digital Signal Processor. It provides a 10 band graphic equalizer and [Digital Room Correction](https://www.drc.one/), along with multi channel options for building digital x-overs. Integration with [Volumio 3](https://volumio.org/) is profound: volume cursor and resampler configurations are overridden to become part of Minosse itself. The advantages are:
- audio signal path is minimal, as a single elaboration (the convolution engine) performs several tasks
- attenuation is implicit; no need to configure filters' attenuation separately to avoid saturation (unless you use to play music at maximum volume!)
- volume control for multi channel mode (up to 8 channels, the standard 5.1 and 7.1 Surround cards are well supported) is solved at the root and you don't need to buy or configure any other dedicated tool
- resampling is only used in two cases:
  - audio source sampling rate is not supported by the output device
  * audio source is not in [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) format (e.g. [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital) audio)
- all the above is done automatically, so that the steps to achieve the best audio quality are made depending on the underlying hardware and are completely transparent to the user



Sorry, still work in progress...
--------------------------------



