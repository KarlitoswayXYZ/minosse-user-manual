
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
  * [BEFORE INSTALLATION](#BEFORE-INSTALLATION)
  * [INSTALLATION](#INSTALLATION)
  * [CONFIGURATION](#CONFIGURATION)
  * [KNOWN ISSUES](#KNOWN-ISSUES)
  * [CREDITS](#CREDITS)
  * [APPENDIX 1](#APPENDIX-1)
  * [APPENDIX 2](#APPENDIX-2)

INTRODUCTION
============

Minosse DRC is a [Digital Room Correction](https://www.drc.one/) (DRC) plugin for [Volumio](https://volumio.org/) 3 based on [Brutefir](https://torger.se/anders/brutefir.html), a popular Open Source convolution engine by Anders Torger. The priority of Minosse DRC is to deliver the best possible sound quality along with a pleasant user experience. Generally speaking, the computer audio industry is more oriented towards versatility rather than audio quality. Sure enough, the first thing that most audiophile dedicated operating systems like Volumio do, is to get rid of unnecessary audio layers such as [Pulseaudio](https://www.freedesktop.org/wiki/Software/PulseAudio/). Often happens that in the digital world the audio signal path becomes a jungle of unidentified elaborations which unique purpose is to deliver audio output at all costs. In this context the advantages of [Digital Room Correction](https://www.drc.one/) might be lost along the way because of inadequate, misconfigured or poorly implemented digital signal processing algorithms (resampling or volume control, just to give a couple of examples) that the user is completely unaware of.

![Adaptation to audio source info](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/adaptation_info.png "Adaptation to audio source info")

*Fig. 1 - Minosse adapts its convolution engine to the audio source to avoid resampling*

The first elaboration to bypass is resampling: every time an audio stream starts in [Volumio](https://volumio.org/), Minosse checks its sampling rate and bit depth, and automatically adapts the convolution engine to the audio source. In this way, Minosse prevents [ALSA](https://alsa-project.org/wiki/Main_Page) or [MPD](https://www.musicpd.org/) from using one of their resamplers ([Speex](https://www.speex.org/docs/manual/speex-manual/), [Sox](http://sox.sourceforge.net/SoX/Resampling), etc. depending on your configuration). Even though they are configurable for "very high" quality, they should be avoided for audiophile purposes as much as possible. Resampling is used only when it cannot be averted, that is when the sound card doesn't support the audio source sampling rate or when the audio stream is not in [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) format (for example [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital)), as [Brutefir](https://torger.se/anders/brutefir.html) only works with [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) streams.

![Non PCM warning](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/non_pcm_warning.png "Non PCM warning")   ![Unsupported sampling rate warning](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/sr_warning.png "Unsupported sampling rate warning")

*Fig. 2 - When resampling cannot be eluded, one of these warning messages will pop up*

Another elaboration to consider is volume control: Minosse takes over the volume cursor of [Volumio](https://volumio.org/) so that it acts on [Brutefir](https://torger.se/anders/brutefir.html) internal attenuation directly, instead of the usual [Volumio](https://volumio.org/) Hardware/Software devices. In this way, the very algorithm that performs digital filtering also works as a volume control. The advantages are:
- audio signal path is minimal, as a single elaboration performs several tasks
- attenuation is implicit, so no need to configure filters attenuation separately to avoid their saturation (unless you use to play music at maximum volume!)
- volume control for multi channel mode (up to 8 channels, depending on your audio card) is solved at the root and you don't need to buy or configure any other dedicated tool

Please, note that the advantages of using Minosse volume control are better appreciated if all other downstream volume controls are truly eliminated from the audio chain. For example, if your amplifier has a volume knob, you may temporarily bypass it by setting it to the maximum level. However, even in this way it's still part of the audio chain, and it would be different if it was completely removed instead. In general, the advantages of using Minosse can be better appreciated if the whole audio chain is assembled following the same no-frills principle that inspired Minosse.

![Minosse volume info](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/volume_info.png "Minosse volume info")

*Fig. 3 - Minosse overrides the volume cursor of Volumio; whenever you change its value, an info message will appear reporting the new attenuation in dB*

In addition to this, Minosse takes care of all relevant [Volumio](https://volumio.org/) options automatically, so that the steps to achieve the best audio quality are completely transparent to the user. Sure enough, you may notice that some [Volumio](https://volumio.org/) configuration pages are not available anymore after Minosse installation. This is to avoid the possibility of interfering and unintentionally misconfiguring the system.

Last but not least, Minosse also gets the most out of your hardware by automatically evaluating it. [Brutefir](https://torger.se/anders/brutefir.html) internal details (number of filter partitions, etc.) are configured depending on your CPU, and if your sound card is a 5.1 or 7.1 Surround device (see [APPENDIX 2](#APPENDIX-2)), its hardware controls (volume cursors, switches, etc.) are configured as well, either for stereo or multi channel mode. Please, note that Minosse doesn't work with Surround audio sources, but can use Surround audio cards for multi channel output from conventional stereo input.

Here follow some other features of Minosse:
- Minosse auto configures the underlying system, both hardware and software, to create (if feasible) the shortest audio chain that works exclusively with double (64 bits) floating-point internal precision, from the resample algorithms (in case they cannot be ruled out) to the convolution engine
- filter's coefficients can be changed in real time, so that they can be tested and confronted easily during listening sessions
- available channel modes are 2.0, 2.1, 4.0, 4.1, 6.0, 6.1 and 8.0; with the right hardware, it is possible to configure Minosse as a digital x-over (see [APPENDIX 2](#APPENDIX-2)), including delays for each channel
- [Brutefir](https://torger.se/anders/brutefir.html) (the convolution engine) is compiled from source for maximum synergy with the underlying hardware

Hardware requirements:
----------------------

**CPU and mother board:**

64 bit support is mandatory. CPU support for [SSE2](https://en.wikipedia.org/wiki/SSE2) instruction set is strongly advised. If [SSE2](https://en.wikipedia.org/wiki/SSE2) is present, Minosse can configure the system for maximum performance (best audio quality for both convolution engine and resampler, lower latency, etc.). For x86-64 systems, any CPU from Intel I3 or AMD Athlon on should be all right. Raspberry Pi 3 CPU doesn't support [SSE2](https://en.wikipedia.org/wiki/SSE2).

**DAC:**

The best option is a DAC that supports all standard sampling rates in the range from 44100 to 192000 Hz, so that the resampler is never activated. Support for higher sampling rates is irrelevant because Minosse uses the Loopback device, which is a Linux kernel module that is limited to 192kHz, and also because they are rarely used in commercial formats. Bit depth should be 32 bit, so that the dynamic range reduction due to the volume control is well compensated. For this reason, as a minimum requirement your DAC should support 24 bit audio at the very least.

Support for non [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) formats such as [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital) is useless, because [Brutefir](https://torger.se/anders/brutefir.html) only works with [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) formats and Minosse automatically configures [Volumio](https://volumio.org/) to convert [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital) streams to [PCM](https://en.wikipedia.org/wiki/Pulse-code_modulation) audio. This is the only case where [Sox](http://sox.sourceforge.net/SoX/Resampling) resampler cannot be avoided, even though your DAC does support [DSD](https://en.wikipedia.org/wiki/Direct_Stream_Digital).

If the sample clock used by [Brutefir](https://torger.se/anders/brutefir.html) is different from that used by the DAC, the following issue, as described by the author of [Brutefir](https://torger.se/anders/brutefir.html) himself, may arise: "It is very important that the input and output sample clock use the same clock as reference. Or else, micro-differences between the input and output sample clock will make BruteFIR's IO buffers to slide apart, and eventually make the program stop."

Surely, this is of concern whenever the DAC is an external device. One solution is to choose an asynchronous USB DAC. The asynchronous data transmission eliminates the problem of clock micro-differences between input and output. Moreover, the audio stream can be re-clocked by the DAC with beneficial effects on [jitter](https://en.wikipedia.org/wiki/Jitter#Sampling_jitter). This is just an option, and there are many others to evaluate. My purpose was to make you aware of the problem. So, when the music stops and the following message pops up, don't tell me that I didn't warn you!

![Broken pipe error message](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/brokenpipe_error.png "Broken pipe error message")

*Fig. 4 - If you don't choose your DAC wisely, this error may occasionally occur*

BEFORE INSTALLATION
===================

The purpose of Minosse is to provide a top-notch infrastructure for [Digital Room Correction](https://www.drc.one/), and it comes with demo filters for all audio configurations so that the user can explore and practice its functionalities. But in the real world, [Digital Room Correction](https://www.drc.one/) filters come from measuring audio systems in their environments with test signals. The results are then elaborated using special software to generate coefficients. Finally, the generated coefficients are saved to special files that are used by [Digital Room Correction](https://www.drc.one/) software like Minosse.

If you are not acquainted with this complex procedure, you should refer to external resources, because explaining it is far beyond the purpose of this text. Here, we will only focus on the final part, that can be split in two steps:
1. how to produce coefficient files that are suitable to Minosse
2. how to configure Minosse to use them

For the first step we will use [Rephase](https://rephase.org/), a popular and free tool for equalizing audio and generating coefficient files. Please, note that it runs under Windows only. As I already explained, working on the equalization is not of concern here. Once you have done your desired equalization, a coefficient file has to be generated. Now, since Minosse adapts itself to the audio source sampling rate, you actually need to generate a coefficient file for each sampling rate. This increased complexity is the tradeoff for getting rid of resampling. To simplify this step, in [APPENDIX 1](#APPENDIX-1) you will find [Rephase](https://rephase.org/) screenshots with the advised settings for each sampling rate.

Some of those settings are compulsory, such as the .dbl file format. It represents data with the highest possible precision using the 64 bits [IEEE-754](https://en.wikipedia.org/wiki/IEEE_754) standard. As Minosse uses that same internal precision for its arithmetic, .dbl is the only file format allowed.

![Demo folders and files](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/demo_folders.png "Demo folders and files")

*Fig. 5 - Files and folders organization of filter coefficients; after Minosse installation, you will find this very structure (including demos) in /data/INTERNAL/minosse/filters/*

Another mandatory choice is the "filename" format. The underscore character (_) is allowed exclusively to separate the file name of your choice from the sampling rate. Moreover, the sampling rate has to be in extended format, that is "44100" and not "44.1" or "44.1kHz". This is crucial because in Minosse everything before the underscore is the "coefficient ID" (please, refer to Fig. 5 and [APPENDIX 1](#APPENDIX-1) screenshots). In this way you can configure your filters by simply choosing the coefficient ID, and for each audio track Minosse will pick up the right file according to the sampling rate reported after the underscore character. Very briefly, in the "filename" section you:
- modify only the string before the underscore character
- don't use further underscore characters; in the whole filename there must be only the one that is already there!

To have a better idea of how filenames work in Minosse, navigate through the [demo files](https://github.com/KarlitoswayXYZ/minosse-filters). After installation, those same demos will be in "/data/INTERNAL/minosse/filters/". That's also where you will put your own coefficient files, once you have elaborated them. Please, note that you can erase the demo files if you don't need them, but you cannot erase or change any one of the folders that contain them! The files with .rephase extention are only a convenience for the user. They were used to generate the .dbl files with the same name, and they can be opened using [Rephase](https://rephase.org/) for practicing its functionalities. Finally, the "REW" folder contains measurements used for some demos. They are just examples of [REW](https://www.roomeqwizard.com/) software and this is the only folder that can be deleted.

To sum up, the rules for creating coefficient files usable by Minosse have been explained and we also gave an idea of where to put them. Further details will be revealed in the [CONFIGURATION](#CONFIGURATION) section, where the mystery of the "delays-ms.json" files will also be unraveled.

INSTALLATION
============

Install:
--------

PLEASE, READ THE [KNOWN ISSUES](#KNOWN-ISSUES) SECTION BEFORE INSTALLING!

Enable SSH access to [Volumio](https://volumio.org/) following the official guide [here](https://volumio.github.io/docs/User_Manual/SSH.html). Then, from [Volumio](https://volumio.org/) home folder (/home/volumio), download and install Minosse from GitHub using the following commands:

```
git clone -b master --single-branch https://github.com/KarlitoswayXYZ/minosse.git /home/volumio/minosse
cd minosse
volumio plugin install
```

Please, note that you need a reliable high-speed internet connection to install Minosse. After installation, navigate to [Volumio](https://volumio.org/) 'Settings' => 'Plugins' => 'Installed plugins' and activate Minosse. The system will reboot automatically.

Uninstall:
----------

Uninstall the plugin using the 'Uninstall' button in [Volumio](https://volumio.org/) 'Settings' => 'Plugins' => 'Installed plugins' page. The system will reboot automatically, unless the plugin had previously been deactivated.

Update:
-------

Unpdate the plugin using the 'Check updates' button in [Volumio](https://volumio.org/) 'Settings' => 'Plugins' => 'Installed plugins' => 'Minosse' page. The system will reboot automatically.


CONFIGURATION
=============

We have already seen how Minosse requirements for maximum audio performance increase the complexity of coefficient files generation, audio track management, etc. As a consequence, you may expect configuration panels that are filled with obscure and cryptic options. Quite on the contrary, you will discover that they are unexpectedly simple and intuitive. How was that possible?

During Minosse development, a lot of "trial and error" has been carried out. This brought to the definition of some basic "best practice" rules. These best practices have been applied to auto configuration and coefficient generation (see [APPENDIX 1](#APPENDIX-1)). It's likely that some of these "already made" choices might be changed for the better, but the point here is that they provide a good kickoff anyway. Once the user is well acquainted with them, he will be free to experiment on his own.

Please, note that while Minosse is activated there are settings that will automatically trigger a system reboot. For example, if you enable or disable Minosse volume control, or if you change the output device through the 'Settings' => 'Playback Options' panel.

Volume settings
---------------

Volume options are quite self explanatory (see the picture below). The most important one is "Attenuation When Volume Cursor Is At Maximum (100)". Set this value so that at maximum volume your Hi-Fi gear will play loud but not yet too loud to damage anything. In this way, your expensive devices are safeguarded from accidental misuses of the volume cursor.

![Volume settings section 1](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/volume_settings_1.png "Volume settings section 1")

*Fig. 6 - Volume settings configuration panel*

Minosse volume can be disabled using the dedicated switch. In this case, the volume configuration panel of [Volumio](https://volumio.org/) will appear back so that you can choose among None, Software or Hardware type of volume. Please, note that the Software option is not recommended for two reasons: the first one is that it doesn't get on well with Minosse (in other words, it may not work). The second one is that it would be a pointless choice, because it adds an ALSA device to the audio chain while providing no advantages at all. In any case, take all the necessary precautions to avert the risk of playing music with no volume control. If, despite my advice, you decide to turn off Minosse volume control, you will have to configure a fixed attenuation value to avoid filters saturation.

![Volume settings section 2](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/volume_settings_2.png "Volume settings section 2")

*Fig. 7 - If you disable Minosse volume control (not a brilliant choice, though), you have to configure a fixed attenuation value to avoid filters saturation*

Filters and coefficients
------------------------

This is very simple. In [BEFORE INSTALLATION](#BEFORE-INSTALLATION) section we have already approached how filter and coefficient files and folders are organized in Minosse. When you select an "Output Audio Channels" option, Minosse will load all the coefficient IDs from the corresponding folder in "/data/INTERNAL/minosse/filters/" so that they will appear as "Coefficient Identifier" options (see the [demo files](https://github.com/KarlitoswayXYZ/minosse-filters) for reference). Then, you can switch among "Coefficient Identifier" options during listening sessions to make real-time comparisons. Indeed, I coudn't imagine anything simpler than that!

![Filters and coefficients options](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/filters_and_coefficients_options.png "Filters and coefficients options")

*Fig. 8 - Filters and coefficients configuration panel*

delays-ms.json files
--------------------

To configure delays, no Graphical User Interface (GUI) is available yet. However, in "/data/INTERNAL/minosse/filters/" there is a dedicated delays-ms.json file for each output channel mode (see the [demo files](https://github.com/KarlitoswayXYZ/minosse-filters) for reference). For example, the one here below is for 6.1 mode. This [json](https://json.org/json-en.html) file contains a section for each coefficient ID. Using the sections already present as model you can add your own sections, provided that they abide by the [json](https://json.org/json-en.html) syntax rules. Delay values are in milliseconds and can be written using as decimal separator either a point or a comma. Delays are applied when you set coefficient IDs (see Fig. 8). If you select a coefficient ID that doesn't have any dedicated section in delays-ms.json, all delays will be simply set to zero.

```
{

  "Demo1-110Hz-24dB-350Hz-18dB-3kHz-6dB-FLAT": {
    "left-bass":"1.4",
    "left-midrange":"183.4",
    "left-treble":"183.4",
    "right-bass":"7,777777",
    "right-midrange":"7,777777",
    "right-treble":"12.7",
    "sub":"238.7678"
  },

  "Demo2-180Hz-48dB-1kHz-36dB-5kHz-48dB-BASIN": {
    "left-bass":"184",
    "left-midrange":"183.4",
    "left-treble":"218,4",
    "right-bass":"777,777",
    "right-midrange":"7,777777",
    "right-treble":"0.777777",
    "sub":"76,78"
  },

  "your-coefficient-id-here": {
    "left-bass":"18.454566",
    "left-midrange":"183.4",
    "left-treble":"8.490888",
    "right-bass":"117,1",
    "right-midrange":"7,777777",
    "right-treble":"99",
    "sub":"238"
  },

  "another-coefficient-id-of-yours": {
    "left-bass":"318.4",
    "left-midrange":"183.4",
    "left-treble":"618.4",
    "right-bass":"77.77",
    "right-midrange":"7,777777",
    "right-treble":"0,777777",
    "sub":"343"
  }

}
```

KNOWN ISSUES
============

- If you disable Minosse volume control, the [Volumio](https://volumio.org/) "Software" option of volume control may not work at all. Be careful, because in that case you would be playing music at maximum volume!
- If Minosse seems unresponsive to new configurations and commands, it is likely that [Brutefir](https://torger.se/anders/brutefir.html) needs to be reloaded. This happens for example when you add new coefficient files to the filter folders. Their coefficient ID will appear as an option in Minosse configuration panel (see Fig. 8), but the new coefficients will be unaccessible to [Brutefir](https://torger.se/anders/brutefir.html) until it is reloaded. To reload [Brutefir](https://torger.se/anders/brutefir.html) simply play an audio track with different sampling rate and/or bit depth from the previous one.
- Minosse has been tested on several x86-64 systems (both Intel and AMD CPUs) and on a Raspberry Pi 3B+.
- Minosse responsiveness is quick enough, but don't expect it to be blazing fast, even though you are using the latest cutting-edge speed-of-light hardware. [FIR](https://en.wikipedia.org/wiki/Finite_impulse_response) filtering, sound cards and automatic configuration have latency times that cannot be eluded. As a rule of thumb, when messages from Minosse appear, like those in Fig. 1 and 2, don't do any action until they disappear.
- At the moment, only English and Italian translations are available.

CREDITS
=======

- Minosse is a plugin for [Volumio](https://volumio.org/) 3. As such, it has been created using the plugin framework devised by the [Volumio team](https://volumio.org/about-us/)

- At the heart of Minosse is [Brutefir](https://torger.se/anders/brutefir.html), a very fast convolution engine by [Anders Torger](https://torger.se/anders/)

- [Rephase](https://rephase.org/) is a [Finite Impulse Response](https://en.wikipedia.org/wiki/Finite_impulse_response) (FIR) generation tool by Thomas Drugeon (runs on Windows only)

APPENDIX 1
==========

Rephase suggested configuration for each sampling rate
------------------------------------------------------

**44100 Hz:**

![Rephase configuration for 44100 Hz](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/Rephase_44100.png "Rephase configuration for 44100 Hz")

----------------------------------------------------------------

**48000 Hz:**

![Rephase configuration for 48000 Hz](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/Rephase_48000.png "Rephase configuration for 48000 Hz")

----------------------------------------------------------------

**88200 Hz:**

![Rephase configuration for 88200 Hz](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/Rephase_88200.png "Rephase configuration for 88200 Hz")

----------------------------------------------------------------

**96000 Hz:**

![Rephase configuration for 96000 Hz](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/Rephase_96000.png "Rephase configuration for 96000 Hz")

----------------------------------------------------------------

**176400 Hz:**

![Rephase configuration for 176400 Hz](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/Rephase_176400.png "Rephase configuration for 176400 Hz")

----------------------------------------------------------------

**192000 Hz:**

![Rephase configuration for 192000 Hz](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/Rephase_192000.png "Rephase configuration for 192000 Hz")

APPENDIX 2
==========

Minosse output configurations for standard Surround audio cards
---------------------------------------------------------------

![Multi channel table](https://github.com/KarlitoswayXYZ/minosse-screenshots/blob/master/multichannel_table.png "Multi channel table")

For more info about jack colors and their function, visit [this site](https://www.phase4.org/pdfs/The%20Ins%20and%20Outs%20of%20PC%20Audio%20Jack%20Colors.pdf). Please, note that all "Sub" outputs work in mono, as they are identical (for each channel configuration) and they have both left and right channels as input.
