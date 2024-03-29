set(eapp_bin my-hello)
set(eapp_src eapp/my-hello.c)
set(a eapp/string.c)
set(c eapp/syscall.c)
set(d eapp/encret.s)

set(eapp_bin1 hello1)
set(eapp_src1 eapp/hello1.cpp)
set(host_bin hello-runner)
set(host_src host/host.cpp)
set(package_name "my-hello.ke")

set(package_script "./hello-runner my-hello hello1 eyrie-rt")
if(RISCV32)
    set(eyrie_plugins "freemem io_syscall linux_syscall env_setup rv32")
else()
    set(eyrie_plugins "freemem io_syscall linux_syscall env_setup")
endif()

# eapp

add_executable(${eapp_bin} ${eapp_src} ${a} ${c} ${d})
target_link_libraries(${eapp_bin} "-static")

add_executable(${eapp_bin1} ${eapp_src1} ${a} ${c} ${d})
target_link_libraries(${eapp_bin1} "-static")
#--------------------------------
target_include_directories(${eapp_bin}
        PUBLIC ${KEYSTONE_SDK_DIR}/include/app
        PUBLIC ${KEYSTONE_SDK_DIR}/include/edge)
target_include_directories(${eapp_bin1}
        PUBLIC ${KEYSTONE_SDK_DIR}/include/app
        PUBLIC ${KEYSTONE_SDK_DIR}/include/edge)
#------------------------------------------
# host

add_executable(${host_bin} ${host_src})
target_link_libraries(${host_bin} ${KEYSTONE_LIB_HOST} ${KEYSTONE_LIB_EDGE})
#--------------------------------------------
set_target_properties(${host_bin}
        PROPERTIES CXX_STANDARD 11 CXX_STANDARD_REQUIRED YES CXX_EXTENSIONS NO
)
target_include_directories(${host_bin}
        PUBLIC ${KEYSTONE_SDK_DIR}/include/host
        PUBLIC ${KEYSTONE_SDK_DIR}/include/edge)
#----------------------------------------------
# add target for Eyrie runtime (see keystone.cmake)

set(eyrie_files_to_copy .options_log eyrie-rt)
add_eyrie_runtime(${eapp_bin}-eyrie
        ${eyrie_plugins}
        ${eyrie_files_to_copy})

# add target for packaging (see keystone.cmake)

add_keystone_package(${eapp_bin}-package
        ${package_name}
        ${package_script}
        ${eyrie_files_to_copy} ${eapp_bin} ${eapp_bin1} ${host_bin})

add_dependencies(${eapp_bin}-package ${eapp_bin}-eyrie)

# add package to the top-level target
add_dependencies(examples ${eapp_bin}-package)