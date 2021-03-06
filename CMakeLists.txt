cmake_minimum_required(VERSION 2.8)
project (NginxUploadModule)

#set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
#set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

set(SELF_CPPFLAGS "")
set(SELF_LDFLAGS "")
set(SELF_DEPENDS "")

set(SELF_CPPFLAGS "-I${CMAKE_BINARY_DIR}/include")
set(SELF_LDFLAGS "-L${CMAKE_BINARY_DIR}/lib")

set(TOOL_TARGET_NAME "build_NginxUploadModule")
set(TOOL_TARGET_BUILD_OUTPUT "BUILD_NginxUploadModule")
set(TOOL_TARGET_COMMENT "build NginxUploadModule")
set(TOOL_TARGET_SOURCE "nginx_upload_module-2.2.0.tar.gz")
set(TOOL_TARGET_DIR "nginx_upload_module-2.2.0")
set(TOOL_TARGET_PATCH "nginx_upload_module-2.2.0.patch")
set(TOOL_TARGET_BASE_URL "http://www.grid.net.ru/nginx/download/")
set(TOOL_TARGET_INSTALL_PREFIX ${CMAKE_BINARY_DIR})
set(CONFIG_ADD_OPTIONS "")
	if(DEFINED CMAKE_TOOLCHAIN_FILE)
		add_custom_command(
			OUTPUT ${TOOL_TARGET_BUILD_OUTPUT}
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_SOURCE} || wget ${TOOL_TARGET_BASE_URL}/${TOOL_TARGET_SOURCE} -O ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_SOURCE}
			COMMAND tar -xzvf ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_SOURCE}
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_PATCH} && cd ${TOOL_TARGET_DIR} && patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_PATCH} && echo "use patch file: ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_PATCH}" || echo "no patch file"
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module && rm -rf ${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module || echo "${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module not found"
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/../modules && cp -r ${TOOL_TARGET_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module || echo "${CMAKE_CURRENT_SOURCE_DIR}/../modules not found"
			WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
			COMMENT ${TOOL_TARGET_COMMENT}
		)
	else(DEFINED CMAKE_TOOLCHAIN_FILE)
		add_custom_command(
			OUTPUT ${TOOL_TARGET_BUILD_OUTPUT}
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_SOURCE} || wget ${TOOL_TARGET_BASE_URL}/${TOOL_TARGET_SOURCE} -O ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_SOURCE}
			COMMAND tar -xzvf ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_SOURCE}
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_PATCH} && cd ${TOOL_TARGET_DIR} && patch -p1 < ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_PATCH} && echo "use patch file: ${CMAKE_CURRENT_SOURCE_DIR}/${TOOL_TARGET_PATCH}" || echo "no patch file"
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module && rm -rf ${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module || echo "${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module not found"
			COMMAND test -e ${CMAKE_CURRENT_SOURCE_DIR}/../modules && cp -r ${TOOL_TARGET_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../modules/nginx-upload-module || echo "${CMAKE_CURRENT_SOURCE_DIR}/../modules not found"
			WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
			COMMENT ${TOOL_TARGET_COMMENT}
		)
	endif(DEFINED CMAKE_TOOLCHAIN_FILE)
	add_custom_target(${TOOL_TARGET_NAME} ALL DEPENDS ${SELF_DEPENDS} ${TOOL_TARGET_BUILD_OUTPUT} )
