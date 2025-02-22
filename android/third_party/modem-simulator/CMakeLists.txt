set(CUTTLEFISH_ROOT ${ANDROID_QEMU2_TOP_DIR}/../../device/google/cuttlefish/)
set(MODEM_SIMULATOR_ROOT ${CUTTLEFISH_ROOT}/host/commands/modem_simulator/)

android_add_library(
  TARGET modem_simulator_lib
  LIBNAME
    modem_simulator_lib
    URL
    "https://android.googlesource.com/device/google/cuttlefish/+/refs/heads/master/host/commands/modem_simulator"
  REPO "${ANDROID_QEMU2_TOP_DIR}/../../device/google/cuttlefish}"
  LICENSE "Apache-2.0"
  SRC # cmake-format: sortable
      ${MODEM_SIMULATOR_ROOT}/call_service.cpp
      ${MODEM_SIMULATOR_ROOT}/channel_monitor.cpp
      ${MODEM_SIMULATOR_ROOT}/command_parser.cpp
      ${MODEM_SIMULATOR_ROOT}/data_service.cpp
      ${MODEM_SIMULATOR_ROOT}/misc_service.cpp
      ${MODEM_SIMULATOR_ROOT}/modem_service.cpp
      ${MODEM_SIMULATOR_ROOT}/modem_simulator.cpp
      ${MODEM_SIMULATOR_ROOT}/network_service.cpp
      ${MODEM_SIMULATOR_ROOT}/nvram_config.cpp
      ${MODEM_SIMULATOR_ROOT}/pdu_parser.cpp
      ${MODEM_SIMULATOR_ROOT}/sim_service.cpp
      ${MODEM_SIMULATOR_ROOT}/sms_service.cpp
      ${MODEM_SIMULATOR_ROOT}/stk_service.cpp
      ${MODEM_SIMULATOR_ROOT}/sup_service.cpp
      ${MODEM_SIMULATOR_ROOT}/thread_looper.cpp
      android_modem_v2.cpp
      common/libs/fs/shared_fd.cpp
      common/libs/utils/files.cpp
      emulator_config.cpp
      json/jsoncpp.cpp
      modem_main.cpp
      ModemLegacy.cpp
      ModemSimulator.cpp
      tinyxml2/tinyxml2.cpp)

target_link_libraries(modem_simulator_lib PRIVATE android-emu GLIB2::GLIB2)
target_include_directories(
  modem_simulator_lib
  PUBLIC ${MODEM_SIMULATOR_ROOT}/
         ${ANDROID_QEMU2_TOP_DIR}/android/third_party/modem-simulator
         # note: this needs to come after the
         # ${ANDROID_QEMU2_TOP_DIR}/android/third_party/modem-simulator
         # otherwise, it is going to use cuttlefish's impl. of dependencies,
         # such as sharedFD/etc.
         ${CUTTLEFISH_ROOT}/
  PRIVATE tinyxml2 json)

target_compile_definitions(modem_simulator_lib PRIVATE -DGOLDFISH_SPECIFIC)

# Exceptions and typeinfo for this target.
target_compile_options(modem_simulator_lib PRIVATE "-fexceptions" "-frtti")

if(WINDOWS_MSVC_X86_64 AND CROSSCOMPILE)
  # Workaround for the clang-cl driver
  target_compile_definitions(modem_simulator_lib PRIVATE "_HAS_STATIC_RTTI")
endif()
