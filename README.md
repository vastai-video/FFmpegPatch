# FFmpegPatch

## Compiling for Linux

### Get Sources

1. Clone FFmpeg's public GIT repository

```sh
git clone https://git.ffmpeg.org/ffmpeg.git
```

2. Clone FFmpegPatch from VASTAI

```sh
git clone https://github.com/vastai-video/FFmpegPatch.git
```

3. Clone VACodecHeaders from VASTAI

```sh
git clone https://github.com/vastai-video/VACodecHeaders.git
```

### Install VACodecHeaders

```sh
# VASTAI_STREAM_INSTALL=/opt/vastai/vaststream
cd VACodecHeaders
cp -r include/vastva ${VASTAI_STREAM_INSTALL}/include
cp -r lib/dri ${VASTAI_STREAM_INSTALL}/lib
cd -
```

### Apply FFmpegPatch

Select suitable patch according ffmpeg version, and resolve conflicts if necessary.

```sh
cd ffmpeg
git apply ../FFmpegPatch/xxx.patch  # eg: n4.1.2.patch
cd -
```

### Compiling FFmpeg

```sh
cd ffmpeg
./configure --enable-shared --disable-static --enable-gpl --enable-nonfree		\
			--enable-vastapi --extra-cflags=-I${VASTAI_STREAM_INSTALL}/include		\
			--prefix=${VASTAI_STREAM_INSTALL}/ffmpeg
make -j $(nproc)
make install
cd -
```