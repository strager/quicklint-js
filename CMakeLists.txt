# quick-lint-js finds bugs in JavaScript programs.
# Copyright (C) 2020  Matthew Glazar
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.

cmake_minimum_required(VERSION 3.10)
if (POLICY CMP0091)
  cmake_policy(SET CMP0091 NEW)
endif ()
project(quick-lint-js)

option(QUICK_LINT_JS_ENABLE_BENCHMARKS "Build microbenchmark programs" OFF)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(QuickLintJSCompiler)

quick_lint_js_set_cxx_standard()
quick_lint_js_enable_char8_t_if_supported()
quick_lint_js_configure_exception_handling()
quick_lint_js_work_around_implicit_link_directories()

find_package(PythonInterp 3)  # Force Python 3 (instead of Python 2).

add_subdirectory(vendor)

if (QUICK_LINT_JS_ENABLE_BENCHMARKS)
  add_subdirectory(benchmark)
endif ()
add_subdirectory(docs)
add_subdirectory(fuzz)
add_subdirectory(plugin/vim)
add_subdirectory(plugin/vscode)
add_subdirectory(src)
add_subdirectory(tools)
add_subdirectory(website/demo)
include(CTest)
if (BUILD_TESTING)
  add_subdirectory(test)
endif ()
