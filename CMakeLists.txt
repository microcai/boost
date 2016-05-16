cmake_minimum_required(VERSION 3.2)

if(WIN32)
ENABLE_LANGUAGE(ASM_MASM)
else()
ENABLE_LANGUAGE(ASM)
endif()

set(BOOST_LIBRARIES boost_static_libs PARENT_SCOPE)

set(BOOST_REGEX_SRC
	libs/regex/src/regex.cpp
	libs/regex/src/regex_traits_defaults.cpp
	libs/regex/src/wide_posix_api.cpp
	libs/regex/src/w32_regex_traits.cpp
	libs/regex/src/usinstances.cpp
	libs/regex/src/regex_debug.cpp
	libs/regex/src/instances.cpp
	libs/regex/src/fileiter.cpp
	libs/regex/src/cpp_regex_traits.cpp
	libs/regex/src/winstances.cpp
	libs/regex/src/wc_regex_traits.cpp
	libs/regex/src/posix_api.cpp
	libs/regex/src/regex_raw_buffer.cpp
	libs/regex/src/static_mutex.cpp
	libs/regex/src/internals.hpp
	libs/regex/src/c_regex_traits.cpp
	libs/regex/src/icu.cpp
	libs/regex/src/cregex.cpp
)

set(BOOST_SYSTEM_SRC
	libs/system/src/error_code.cpp
)

if(WIN32)
	set(BOOST_THREAD_SRC
		libs/thread/src/future.cpp
		libs/thread/src/win32/tss_dll.cpp
		libs/thread/src/win32/thread.cpp
		libs/thread/src/win32/tss_pe.cpp
	)
else()
	set(BOOST_THREAD_SRC
		libs/thread/src/future.cpp
		libs/thread/src/pthread/once.cpp
		libs/thread/src/pthread/thread.cpp
		libs/thread/src/pthread/once_atomic.cpp
	)
endif()

set(BOOST_CHRONO_SRC
	libs/chrono/src/chrono.cpp
	libs/chrono/src/thread_clock.cpp
	libs/chrono/src/process_cpu_clocks.cpp
)

set(BOOST_CONTEXT_SRC
	libs/context/src/execution_context.cpp
)

if(WIN32)
	list(APPEND BOOST_CONTEXT_SRC libs/context/src/windows/stack_traits.cpp)
	if( CMAKE_SIZEOF_VOID_P EQUAL 8 )
		list(APPEND BOOST_CONTEXT_SRC libs/context/src/asm/make_x86_64_ms_pe_masm.asm)
		list(APPEND BOOST_CONTEXT_SRC libs/context/src/asm/jump_x86_64_ms_pe_masm.asm)
	else()
		list(APPEND BOOST_CONTEXT_SRC libs/context/src/asm/make_i386_ms_pe_masm.asm)
		list(APPEND BOOST_CONTEXT_SRC libs/context/src/asm/jump_i386_ms_pe_masm.asm)
	endif()

	add_definitions(-DBOOST_CONTEXT_EXPORT)

else()
	list(APPEND BOOST_CONTEXT_SRC libs/context/src/posix/stack_traits.cpp)

	IF("${CMAKE_SYSTEM_PROCESSOR}" MATCHES "x86")
		if( CMAKE_SIZEOF_VOID_P EQUAL 8 )
			set(ARCH x86_64)
		else()
			set(ARCH i386)
		endif()
	else()

	endif()

	list(APPEND BOOST_CONTEXT_SRC libs/context/src/asm/make_${ARCH}_sysv_elf_gas.S)
	list(APPEND BOOST_CONTEXT_SRC libs/context/src/asm/jump_${ARCH}_sysv_elf_gas.S)

endif()


set(BOOST_COROUTINE_SRC
	libs/coroutine/src/detail/coroutine_context.cpp
	libs/coroutine/src/exceptions.cpp
)

if(WIN32)
	list(APPEND BOOST_COROUTINE_SRC libs/coroutine/src/windows/stack_traits.cpp)
else()
	list(APPEND BOOST_COROUTINE_SRC libs/coroutine/src/posix/stack_traits.cpp)
endif()

set(BOOST_DATE_TIME_SRC

	libs/date_time/src/posix_time/posix_time_types.cpp
	libs/date_time/src/gregorian/greg_weekday.cpp
	libs/date_time/src/gregorian/gregorian_types.cpp
	libs/date_time/src/gregorian/greg_month.cpp
	libs/date_time/src/gregorian/date_generators.cpp
	libs/date_time/src/gregorian/greg_names.hpp
)

set(BOOST_EXCEPTION_SRC libs/exception/src/clone_current_exception_non_intrusive.cpp)

set(BOOST_FILESYSTEM_SRC
	libs/filesystem/src/path_traits.cpp
	libs/filesystem/src/operations.cpp
	libs/filesystem/src/unique_path.cpp
	libs/filesystem/src/utf8_codecvt_facet.cpp
	libs/filesystem/src/windows_file_codecvt.hpp
	libs/filesystem/src/codecvt_error_category.cpp
	libs/filesystem/src/path.cpp
	libs/filesystem/src/windows_file_codecvt.cpp
	libs/filesystem/src/portability.cpp
)

set(BOOST_LOCALE_SRC
	libs/locale/src/encoding/codepage.cpp
	libs/locale/src/shared/date_time.cpp
	libs/locale/src/shared/generator.cpp
	libs/locale/src/shared/message.cpp
	libs/locale/src/shared/format.cpp
	libs/locale/src/shared/ids.cpp
	libs/locale/src/shared/mo_lambda.cpp
	libs/locale/src/shared/formatting.cpp
	libs/locale/src/shared/localization_backend.cpp
	libs/locale/src/std/collate.cpp
	libs/locale/src/std/numeric.cpp
	libs/locale/src/std/all_generator.hpp
	libs/locale/src/std/codecvt.cpp
	libs/locale/src/std/std_backend.cpp
	libs/locale/src/std/std_backend.hpp
	libs/locale/src/std/converter.cpp
	libs/locale/src/util/locale_data.cpp
	libs/locale/src/util/default_locale.cpp
	libs/locale/src/util/locale_data.hpp
	libs/locale/src/util/numeric.hpp
	libs/locale/src/util/info.cpp
	libs/locale/src/util/codecvt_converter.cpp
	libs/locale/src/util/timezone.hpp
	libs/locale/src/util/gregorian.hpp
	libs/locale/src/util/gregorian.cpp
	libs/locale/src/util/iconv.hpp
)

if (WIN32)
	list(APPEND BOOST_LOCALE_SRC
		libs/locale/src/win32/collate.cpp
		libs/locale/src/win32/lcid.cpp
		libs/locale/src/win32/win_backend.cpp
		libs/locale/src/win32/numeric.cpp
		libs/locale/src/win32/win_backend.hpp
		libs/locale/src/win32/api.hpp
		libs/locale/src/win32/all_generator.hpp
		libs/locale/src/win32/converter.cpp
		libs/locale/src/win32/lcid.hpp
	)
else()
	list(APPEND BOOST_LOCALE_SRC
		libs/locale/src/posix/collate.cpp
		libs/locale/src/posix/numeric.cpp
		libs/locale/src/posix/posix_backend.cpp
		libs/locale/src/posix/posix_backend.hpp
		libs/locale/src/posix/all_generator.hpp
		libs/locale/src/posix/codecvt.cpp
		libs/locale/src/posix/converter.cpp
		libs/locale/src/posix/codecvt.hpp
	)
endif()

if(CMAKE_COMPILER_IS_GNUCXX)
	add_definitions(-DBOOST_LOCALE_WITH_ICONV)
endif()

set(BOOST_PROGRAM_OPTIONS_SRC
	libs/program_options/src/winmain.cpp
	libs/program_options/src/value_semantic.cpp
	libs/program_options/src/convert.cpp
	libs/program_options/src/variables_map.cpp
	libs/program_options/src/parsers.cpp
	libs/program_options/src/options_description.cpp
	libs/program_options/src/utf8_codecvt_facet.cpp
	libs/program_options/src/split.cpp
	libs/program_options/src/positional_options.cpp
	libs/program_options/src/cmdline.cpp
	libs/program_options/src/config_file.cpp
)

set(BOOST_TIMER_SRC
	libs/timer/src/auto_timers_construction.cpp
	libs/timer/src/cpu_timer.cpp
)

set(BOOST_RANDOM_SRC libs/random/src/random_device.cpp)

set(BOOST_SRCS ${BOOST_SYSTEM_SRC} ${BOOST_THREAD_SRC} ${BOOST_CHRONO_SRC} ${BOOST_REGEX_SRC} ${BOOST_CONTEXT_SRC}
	${BOOST_COROUTINE_SRC} ${BOOST_DATE_TIME_SRC} ${BOOST_EXCEPTION_SRC} ${BOOST_FILESYSTEM_SRC}
	${BOOST_LOCALE_SRC} ${BOOST_PROGRAM_OPTIONS_SRC} ${BOOST_TIMER_SRC} ${BOOST_RANDOM_SRC})

add_library(boost_static_libs STATIC ${BOOST_SRCS})

target_include_directories(boost_static_libs PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

target_compile_definitions(boost_static_libs PUBLIC -DBOOST_ALL_NO_LIB -DBOOST_ALL_STATIC_LINK -DBOOST_THREAD_USE_LIB -DBOOST_FILESYSTEM_STATIC_LINK)

target_compile_definitions(boost_static_libs PRIVATE -DBOOST_THREAD_BUILD_LIB)

if(WIN32)
	target_compile_definitions(boost_static_libs PUBLIC -DWIN32_LEAN_AND_MEAN)
	target_compile_definitions(boost_static_libs PUBLIC -DNOMINMAX)
endif()