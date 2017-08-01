# Minimal GIFT-Grab version

Below we document the steps to building a minimal GIFT-Grab such that the [SciPy example][scipy-ex] can be executed.
The steps are for Debian 9, but are mostly identical to Ubuntu.
Wherever applicable we highlight the differences.

[scipy-ex]: http://gift-grab.readthedocs.io/en/latest/scipy.html#full-source-code


# Installing needed tools and libraries

1. GCC: `apt install build-essential`
1. Python libraries: `apt install python-dev`
1. Git: `apt install git`
1. CMake: `apt install cmake`
1. FFmpeg (for Ubuntu, please see [these instructions][ffmpeg-ubuntu] instead):
   - `apt install libavfilter-dev`
   - `apt install libavutil-dev`
   - `apt install libswscale-dev`
   - `apt install libavcodec-dev`
   - `apt install libavformat-dev`
1. NumPy: `apt install python-numpy`
1. SciPy: `apt install python-scipy`
1. Boost:
   - `wget https://sourceforge.net/projects/boost/files/boost/1.63.0/boost_1_63_0.tar.bz2`
   - `tar xvfj boost_1_63_0.tar.bz2`
   - `cd boost_1_63_0`
   - `./bootstrap.sh --with-libraries=python`
   - `./b2`
   - `./b2 install`


# Obtaining and building GIFT-Grab

1. Clone the GIFT-Grab repository: `git clone https://github.com/gift-surg/GIFT-Grab.git`
1. Create a build folder and change into it: `mkdir gift-grab-build; cd gift-grab-build`
1. Configure GIFT-Grab for [reading video files][gg-read] and [encoding HEVC files][gg-hevc]: `cmake -D BUILD_PYTHON=ON -D USE_FILES=ON -D USE_HEVC=ON -D USE_NUMPY=ON -D ENABLE_GPL=ON -D USE_X265=ON ../GIFT-Grab/src`
   - This configuration uses [x265][x265] as x265 is supported by FFmpeg on Debian 9 by default.
   - However [GIFT-Grab supports other libraries for HEVC as well][gg-hevc].
1. Compile GIFT-Grab: `make -j` (This will create a `pygiftgrab.so` in the build folder).

[gg-read]: doc/build.md#reading-video-files
[x265]: http://x265.org/
[gg-hevc]: doc/build.md#hevc


# Running the SciPy example

1. Download a sample HEVC video file (for instance from [this website][hevc-website]) and save it as `/tmp/myinput.mp4`.
1. Save the [SciPy example][scipy-ex] as `ggscipyex.py` within the build folder.
1. Execute the SciPy example within the build folder: `python2 ggscipyex.py`
1. See the produced output file `/tmp/myoutput.mp4`.
   - It should contain a Gaussian-smoothed version of the downloaded HEVC video.
   - Note that this output file should always contain the first frames from the downloaded file; however it may not contain all frames to the end.
   - This is because encoding is a computationally intensive task and to be able to encode all frames you will need GPU-accelerated encoding. [NVENC][gg-nvenc] can be used for that.

[ffmpeg-ubuntu]: doc/tips.md#ubuntu
[hevc-website]: https://x265.com/hevc-video-files/
[gg-nvenc]: doc/tips.md#nvenc