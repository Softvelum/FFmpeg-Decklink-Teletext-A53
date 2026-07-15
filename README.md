# To add dvb teletext output to decklink cards/devices you need libklvanc and decklink sdk.

## Build this improved ffmpeg with something like 

* ./configure --enable-decklink --enable-libklvanc --disable-x86asm --enable-nonfree  --extra-cflags="-I/home/alex/Development/decklink/desktopvideo_sdk-api/Linux/include -I/home/alex/Development/decklink/libklvanc/src"  --extra-cxxflags="-I/home/alex/Development/decklink/desktopvideo_sdk-api/Linux/include -I/home/alex/Development/decklink/libklvanc/src" --extra-ldflags="-L/home/alex/Development/decklink/libklvanc/src/.libs" --extra-libs="-lklvanc"
* make clean
* make -j 12

## To send dvb teletext into Decklink use:
* ./ffmpeg -hide_banner -loglevel error -nostats -re -i /home/alex/Development/nimble/test/content/mp4/teletext-eng.ts -map 0:v:0 -map 0:s:0 -map 0:a:0 -c:v wrapped_avframe -pix_fmt uyvy422 -c:s copy -c:a pcm_s16le -ar 48000 -ac 2 -f decklink -teletext_lines all "DeckLink Duo (1)"


## To receive embedded VANC Dvb teletext from Decklink
* [Use Nimble streamer](https://softvelum.com/2026/07/dvb-teletext-closed-captions-sdi-input-nimble-streamer/). Nimble fully supports working with teletext from Decklink SDI as well as MPEGTS teletext in input/output modes and transcoding
* ./ffmpeg -hide_banner -y -format_code Hi50 -teletext_lines all -f decklink -i "DeckLink Duo (4)" -t 8 -map 0:1 -map 0:2 -c:v mpeg2video -pix_fmt
  yuv420p -b:v 10000k -c:s copy -f mpegts /tmp/decklink-v210-video-teletext.ts

# FFmpeg README

FFmpeg is a collection of libraries and tools to process multimedia content
such as audio, video, subtitles and related metadata.

## Libraries

* `libavcodec` provides implementation of a wider range of codecs.
* `libavformat` implements streaming protocols, container formats and basic I/O access.
* `libavutil` includes hashers, decompressors and miscellaneous utility functions.
* `libavfilter` provides means to alter decoded audio and video through a directed graph of connected filters.
* `libavdevice` provides an abstraction to access capture and playback devices.
* `libswresample` implements audio mixing and resampling routines.
* `libswscale` implements color conversion and scaling routines.

## Tools

* [ffmpeg](https://ffmpeg.org/ffmpeg.html) is a command line toolbox to
  manipulate, convert and stream multimedia content.
* [ffplay](https://ffmpeg.org/ffplay.html) is a minimalistic multimedia player.
* [ffprobe](https://ffmpeg.org/ffprobe.html) is a simple analysis tool to inspect
  multimedia content.
* Additional small tools such as `aviocat`, `ismindex` and `qt-faststart`.

## Documentation

The offline documentation is available in the **doc/** directory.

The online documentation is available in the main [website](https://ffmpeg.org)
and in the [wiki](https://trac.ffmpeg.org).

Also [DVB Teletext and Closed Captions Support for DeckLink SDI Input in Nimble Streamer](https://softvelum.com/2026/07/dvb-teletext-closed-captions-sdi-input-nimble-streamer/) article for the exmaple of usage.


### Examples

Coding examples are available in the **doc/examples** directory.

## License

FFmpeg codebase is mainly LGPL-licensed with optional components licensed under
GPL. Please refer to the LICENSE file for detailed information.

## Contributing

Patches should be submitted to the ffmpeg-devel mailing list using
`git format-patch` or `git send-email`. Github pull requests should be
avoided because they are not part of our review process and will be ignored.
