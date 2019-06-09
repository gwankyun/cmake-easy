# 添加子目錄為庫
function(target_add_library)
	cmake_parse_arguments(
		TARGET
		""
		"PATH;NAME;FOLDER"
		""
		${ARGN}
	)
	if(TARGET_PATH AND TARGET_NAME)
		set(FILE_PATH ${TARGET_PATH}/${TARGET_NAME})
		set(FULL_FILE_NAME ${CMAKE_SOURCE_DIR}/${FILE_PATH})
		if(EXISTS ${FULL_FILE_NAME})
			set(SRCS ${TARGET_NAME}_SRCS)
			set(INCS ${TARGET_NAME}_INCS)
			# 源目錄
			aux_source_directory(${FILE_PATH} ${SRCS})
			# 查找头文件
			file(GLOB_RECURSE ${INCS} ${FILE_PATH}/*.h)
			# 添加庫
			add_library(${TARGET_NAME} ${${SRCS}} ${${INCS}})
			# 头文件目錄
			target_include_directories(${TARGET_NAME} PUBLIC ${FILE_PATH} src)
			if(TARGET_FOLDER)
				set_property(TARGET ${TARGET_NAME} PROPERTY FOLDER ${TARGET_FOLDER})
			endif()
		endif()
	else()

	endif()
endfunction()
