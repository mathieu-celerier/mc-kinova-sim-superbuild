This project is a superbuild project accompanying the paper **Explicit safety and compliance on torque controlled robots for physical
interaction** submitted to RA-L.

It will build all dependencies and the mc_rtc control framework, allowing to run the controller used for this paper's experiments.

Requirements
--

- [CMake >= 3.20](https://cmake.org/download/)
- [Git](https://git-scm.com/)
- [Visual Studio 2019 and later](https://visualstudio.microsoft.com/) (Windows)

### Bootstraping

You can fullfill the requirements above by invoking our bootstraping script:

- on Debian like distributions: `./utils/bootstrap-linux.sh`
- on macOS: `./utils/bootstrap-macos.sh`

Usage
--

```shell
git clone https://github.com/mathieu-celerier/mc-kinova-sim-superbuild mc-rtc-superbuild
# Run the bootstrap script in mc-rtc-superbuild/utils folder if required
cmake -S mc-rtc-superbuild -B mc-rtc-superbuild/build -DSOURCE_DESTINATION=${HOME}/devel/src -DBUILD_DESTINATION=${HOME}/devel/build
cd mc-rtc-superbuild/build
cmake --build . --config RelWithDebInfo --target clone
cd ${HOME}/devel/src/mc_rtc
git remote add EXP https://github.com/mathieu-celerier/mc_rtc.git
git checkout EXP/topic/external-forces
cd ${HOME}/devel/build
cmake . -DBUILD_TESTING=off
# Go back to mc-rtc-superbuild clone location
cd mc-rtc-superbuild
cmake --build . --config RelWithDebInfo
```
