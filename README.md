# FFmpegPatch

## Compiling for Linux

### Get Sources

1. Clone FFmpeg's public GIT repository.

```sh
# clone code or eg: wget https://github.com/FFmpeg/FFmpeg/archive/refs/tags/n5.0.tar.gz
git clone -b n5.0 https://git.ffmpeg.org/ffmpeg.git
```

2. Clone FFmpegPatch from VASTAI repository.

```sh
git clone https://github.com/vastai-video/FFmpegPatch.git
```

3. Clone VACodecHeaders from VASTAI repository.

```sh
git clone https://github.com/vastai-video/VACodecHeaders.git
```

### Install VACodecHeaders

```sh
# eg: VASTAI_STREAM_INSTALL=/opt/vastai/vaststream
# Make sure that include directory exist, or if it doesn't, it needs to be created beforehand.
mkdir -p ${VASTAI_STREAM_INSTALL}/include

cd VACodecHeaders
cp -r include/vastva ${VASTAI_STREAM_INSTALL}/include/
cd -
```

### Apply FFmpegPatch

Select suitable patch according ffmpeg version, and resolve conflicts if necessary.

```sh
cd ffmpeg
git apply ../FFmpegPatch/n5.0.patch  # eg: n5.0.patch
cd -
```

### Compiling FFmpeg

```sh
# build for x86_64
cd ffmpeg
./configure --enable-shared --disable-static --enable-gpl --enable-nonfree \
	--enable-vastapi --extra-cflags=-I${VASTAI_STREAM_INSTALL}/include \
	--prefix=${VASTAI_STREAM_INSTALL}/ffmpeg
make -j $(nproc)
make install
cd -
```

```sh
# build for arm64
cd ffmpeg
./configure --arch=arm64 --enable-shared --disable-static --enable-gpl --enable-nonfree \
	--enable-vastapi --extra-cflags=-I${VASTAI_STREAM_INSTALL}/include \
	--prefix=${VASTAI_STREAM_INSTALL}/ffmpeg
make -j $(nproc)
make install
cd -
```

## Running

You need to install driver and runtime libraries. They can be downloaded from the official website.

1. Install runtime libraries.

```sh
# eg: VASTAI_STREAM_INSTALL=/opt/vastai/vaststream
# Make sure that lib directory exist, or if it doesn't, it needs to be created beforehand.
mkdir -p ${VASTAI_STREAM_INSTALL}/lib

# eg: vmpp_2.8.1_x86_64_rel.tar.gz
tar -zxvf vmpp_2.8.1_x86_64_rel.tar.gz
cp -r vmpp_2.8.1_x86_64_rel/lib/ ${VASTAI_STREAM_INSTALL}
```

2. Install driver package and set env. See the official release version.

Official download website: https://developer.vastaitech.com/downloads