android_add_library(
  TARGET _nimble_transport_socket LICENSE Apache-2.0
  SRC socket/src/android_sockets.cc socket/src/ble_hci_socket.c)
target_include_directories(_nimble_transport_socket PRIVATE socket/src
                           PUBLIC socket/include)
target_compile_options(_nimble_transport_socket
                       PRIVATE -Wno-implicit-function-declaration)
target_link_libraries(_nimble_transport_socket
                      PRIVATE _nimble_os_abstraction_headers android-emu-base)

android_add_library(TARGET _nimble_transport_test LICENSE Apache-2.0
                    SRC test/src/ble_hci_nop_transport.c)
target_include_directories(_nimble_transport_test PRIVATE test/src
                           PUBLIC test/include)
target_compile_options(_nimble_transport_test
                       PRIVATE -Wno-implicit-function-declaration)
target_link_libraries(_nimble_transport_test
                      PRIVATE _nimble_os_abstraction_headers)
