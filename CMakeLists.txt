set(EXTENSION_DIR ${CMAKE_CURRENT_LIST_DIR}/../superbuild-extensions)
include(${EXTENSION_DIR}/interfaces/mc_mujoco)

if(NOT WITH_ROS_SUPPORT)
  message(FATAL_ERROR "ROS support is required to use the Kinova robot")
endif()

AptInstall(
  ros-${ROS_DISTRO}-gazebo-ros
  ros-${ROS_DISTRO}-controller-manager
  ros-${ROS_DISTRO}-control-msgs
  ros-${ROS_DISTRO}-control-toolbox
  ros-${ROS_DISTRO}-moveit-ros-planning-interface
)

# Install conan if needed
find_program(CONAN conan)
if(NOT CONAN)
  find_program(PYTHON3 python3)
  if(NOT PYTHON3)
    message(FATAL_ERROR "Must have python3 to install conan to use the Kinova robot")
  endif()
  execute_process(COMMAND sudo python3 -m pip install conan==1.60.1 COMMAND_ERROR_IS_FATAL ANY)
  find_program(CONAN conan)
  if(NOT CONAN)
    message(FATAL_ERROR "Conan installation went wrong")
  endif()
  execute_process(COMMAND ${CONAN} profile new default --detect COMMAND_ERROR_IS_FATAL ANY)
  execute_process(COMMAND ${CONAN} profile update settings.compiler.libcxx=libstdc++11 default COMMAND_ERROR_IS_FATAL ANY)
endif()
execute_process(COMMAND ${CONAN} config set general.revisions_enabled=1 COMMAND_ERROR_IS_FATAL ANY)

AddCatkinProject(ros_kortex
  GITHUB Kinovarobotics/ros_kortex
  GIT_TAG origin/${ROS_DISTRO}-devel
  WORKSPACE data_ws
)

AddProject(mc_kinova
  GITHUB mathieu-celerier/mc_kinova
  GIT_TAG origin/main
  DEPENDS mc_rtc ros_kortex
)

function(AddKinovaMujocoModel)
  if(NOT TARGET mc_mujoco)
    return()
  endif()
  AddProject(kinova_mj_description
    GITHUB mathieu-celerier/kinova_mj_description
    GIT_TAG origin/main
    DEPENDS mc_mujoco mc_kinova
  )
endfunction()
cmake_language(DEFER CALL AddKinovaMujocoModel)
