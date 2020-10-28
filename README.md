# obs-deps

Scripts to build and package dependencies for OBS on CI

## macOS (10.13+)

| lib | git commit | version |
| :--- | :---: | :---: |
|libpng|[Sourceforge](https://downloads.sourceforge.net/project/libpng/libpng16/1.6.37/libpng-1.6.37.tar.xz)|1.6.37|
|libopus|[Oregon State University FTP](https://ftp.osuosl.org/pub/xiph/releases/opus/opus-1.3.1.tar.gz)|1.3.1|
|libogg|[xiph.org](https://downloads.xiph.org/releases/ogg/libogg-1.3.4.tar.gz)|1.3.4|
|librnnoise|[90ec41e](https://github.com/xiph/rnnoise/commit/90ec41ef659fd82cfec2103e9bb7fc235e9ea66c)||
|libvorbis|[xiph.org](https://downloads.xiph.org/releases/vorbis/libvorbis-1.3.7.tar.xz)|1.3.7|
|libvpx|[Github](https://github.com/webmproject/libvpx/archive/v1.9.0.tar.gz)|1.9.0|
|libjansson|[Petri Lehtinen](https://digip.org/jansson/releases/jansson-2.13.1.tar.gz)|2.13.1|
|libx264|[db0d417](https://github.com/mirror/x264/commit/db0d417728460c647ed4a847222a535b00d3dbcb)|r3018|
|libmbedtls|[Github](https://github.com/ARMmbed/mbedtls/archive/mbedtls-2.24.0.tar.gz)|2.24.0|
|libsrt|[Github](https://github.com/Haivision/srt/archive/v1.4.2.tar.gz)|1.4.2|
|libtheora|[xiph.org](https://downloads.xiph.org/releases/theora/libtheora-1.1.1.tar.bz2)|1.1.1|
|ffmpeg|[ffmpeg.org](https://ffmpeg.org/releases/ffmpeg-4.3.1.tar.xz)|4.3.1|
|libluajit|[luajit.org](https://luajit.org/download/LuaJIT-2.1.0-beta3.tar.gz)|2.1.0-beta3|
|libfreetype|[Sourceforge](https://downloads.sourceforge.net/project/freetype/freetype2/2.10.4/freetype-2.10.4.tar.xz)|2.10.4|
|libpcre|[pcre.org](https://ftp.pcre.org/pub/pcre/pcre-8.44.tar.bz2)|8.44|
|swig|[Sourceforge](https://downloads.sourceforge.net/project/swig/swig/swig-4.0.2/swig-4.0.2.tar.gz)|4.0.2|
|Qt|[Qt.io](https://download.qt.io/official_releases/qt/5.14/5.14.1/single/qt-everywhere-src-5.14.1.tar.xz)|5.14.1|

### Prerequisites

* Homebrew (https://brew.sh)
* Python 3 - either installed via homebrew (`brew install python`) or system-provided (macOS 10.15 Catalina)
* PyYAML - installed via `pip3 install pyyaml`

### Build steps

* Checkout `obs-deps` from Github:

```
git clone https://github.com/obsproject/obs-deps.git
```

* Enter the `obs-deps` directory
* Enter the `macos` directory
* (*Optional*) Create the build scripts by running `./build_script_generator.py .github/workflows/build_deps.yml`
* Run `bash ./build-script-macos-01.sh` to build main dependencies
* Run `bash ./build-script-macos-02.sh` to build Qt dependency

### Usage

* Common directory to place obs-deps is `/tmp/obsdeps/`, but a custom path can be used
* Unpack pre-built dependencies to `/tmp` by running `tar -xf ./macos-deps-VERSION.tar.gz -C /tmp` (replace `VERSION` with the downloaded/desired version)
* Unpack pre-built Qt dependency to `/tmp` by running `tar -xf ./macos-qt-QT_VERSION-VERSION.tar.gz -C /tmp` (replace `QT_VERSION` and `VERSION` with the downloaded/desired versions)
* **IMPORTANT:** Remove the quarantine attribute from the downloaded Qt dependencies by running `xattr -r -d com.apple.quarantine /tmp/obsdeps`
* Use `/tmp/obsdeps` as `QTDIR`, `SWIGDIR`, and `DepsPath` when invoking `cmake` for OBS:

```
cmake -DCMAKE_OSX_DEPLOYMENT_TARGET=10.13 -DQTDIR="/tmp/obsdeps" -DSWIGDIR="/tmp/obsdeps" -DDepsPath="/tmp/obsdeps" [..]
```