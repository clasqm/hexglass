#
# HexGlass is a Tetris-like puzzle game.
#
# Project homepage: http://hexglass.googlecode.com/
#
# Copyright (C) 2010-2011 Alexey Michurin <a.michurin@gmail.com>
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>.
#

cmake_minimum_required(VERSION 3.10)

project(hexglass VERSION 1.2.1 LANGUAGES CXX)

# The qmake build was release only.
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

# Optional compile time defaults (formerly commented DEFINES in hexglass.pro):
# add_definitions(-DEXTRA_CONTROL)
# add_definitions(-DDEFAULT_SIZE=1)
# add_definitions(-DDEFAULT_SKIN=1)

find_package(Qt5 REQUIRED COMPONENTS Core Gui Widgets)

set(HEXGLASS_HEADERS
    src/hexglass.h
    src/logic/figure.h
    src/logic/abstract_place.h
    src/logic/lplace.h
    src/logic/pplace.h
    src/logic/figure_iter.h
    src/logic/figure_trans.h
    src/logic/figures_bank.h
    src/control/configuration.h
    src/control/controller.h
    src/control/high_score_controller.h
    src/control/signal_emiter.h
    src/control/signal_gate.h
    src/control/skin.h
    src/widgets/dialogs.h
    src/widgets/displaywidget.h
    src/widgets/glasswidget.h
    src/widgets/headwidget.h
    src/widgets/previewwidget.h
    src/widgets/scorewidget.h
    src/widgets/window.h
)

set(HEXGLASS_SOURCES
    src/hexglass.cpp
    src/logic/figure.cpp
    src/logic/abstract_place.cpp
    src/logic/lplace.cpp
    src/logic/pplace.cpp
    src/logic/figure_iter.cpp
    src/logic/figure_trans.cpp
    src/logic/figures_bank.cpp
    src/control/configuration.cpp
    src/control/controller.cpp
    src/control/high_score_controller.cpp
    src/control/signal_emiter.cpp
    src/control/signal_gate.cpp
    src/control/skin.cpp
    src/widgets/dialogs.cpp
    src/widgets/displaywidget.cpp
    src/widgets/glasswidget.cpp
    src/widgets/headwidget.cpp
    src/widgets/previewwidget.cpp
    src/widgets/scorewidget.cpp
    src/widgets/window.cpp
)

# WIN32 is ignored on non-Windows platforms; it keeps a console from popping
# up there. The icon resource is only meaningful for a Windows build.
add_executable(hexglass WIN32
    ${HEXGLASS_HEADERS}
    ${HEXGLASS_SOURCES}
    resources/hexglass.qrc
)

if(WIN32)
    target_sources(hexglass PRIVATE resources/hexglass.rc)
endif()

target_include_directories(hexglass PRIVATE
    src
    src/control
    src/logic
    src/widgets
)

# HG_VERSION is consumed through the STR() macro in hexglass.h, which
# stringifies it, so it must stay a bare token rather than a string literal.
target_compile_definitions(hexglass PRIVATE HG_VERSION=${PROJECT_VERSION})

target_link_libraries(hexglass PRIVATE Qt5::Core Qt5::Gui Qt5::Widgets)

install(TARGETS hexglass RUNTIME DESTINATION bin)
