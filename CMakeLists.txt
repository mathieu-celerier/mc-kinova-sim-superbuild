include(MuJoCo.cmake)

AddCatkinProject(ros_kortex
  GITHUB Kinovarobotics/ros_kortex
  GIT_TAG noetic-devel
  WORKSPACE data_ws
)

AddProject(mc_kinova
  GITHUB mathieu-celerier/mc_kinova
  GIT_TAG origin/main
  DEPENDS mc_rtc)

AddProject(mc_mujoco
  GITHUB rohanpsinghn/mc_mujoco
  GIT_TAG origin/main
  CMAKE_ARGS -DMUJOCO_ROOT_DIR=${MUJOCO_ROOT_DIR}
  DEPENDS mc_rtc
)

AddProject(kinova_mj_description
  GITHUB mathieu-celerier/kinova_mj_description
  GIT_TAG origin/main
  DEPENDS mc_mujoco
)
