#
# This file is part of the CMaNGOS Project. See AUTHORS file for Copyright information
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
#

if(BUILD_GAME_SERVER OR BUILD_LOGIN_SERVER OR BUILD_EXTRACTORS)
  add_subdirectory(framework)
  add_subdirectory(shared)
endif()

# Playerbots module
if(BUILD_PLAYERBOTS)
  include(FetchContent)

  FetchContent_Declare(
    PlayerBots
    SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/modules/PlayerBots"
    GIT_REPOSITORY "https://github.com/Malofeus/playerbots.git"
    GIT_TAG "master"
  )

  FetchContent_GetProperties(PlayerBots)
  if (NOT playerbots_POPULATED)
    FetchContent_MakeAvailable(PlayerBots)
    message(STATUS "Playerbots module source dir: ${playerbots_SOURCE_DIR}")
  else()
    message(STATUS "Playerbots module already populated: ${playerbots_POPULATED}")
  endif()

  add_subdirectory(${playerbots_SOURCE_DIR})

else()
  if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/modules/PlayerBots)
    message(STATUS "PlayerBots module exists, but not building. You can remove it manually if you don't need it. (rm -rf ${CMAKE_CURRENT_SOURCE_DIR}/modules/PlayerBots)")

    #file(REMOVE_RECURSE ${CMAKE_CURRENT_SOURCE_DIR}/modules/PlayerBots)
  endif()
endif()

# SoloCraft module
if(BUILD_SOLOCRAFT)
  include(FetchContent)

  FetchContent_Declare(
    SoloCraft
    SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/modules/SoloCraft"
    GIT_REPOSITORY "https://github.com/Malofeus/SoloCraft.git"
    GIT_TAG "master"
  )

  FetchContent_GetProperties(SoloCraft)
  if (NOT solocraft_POPULATED)
    FetchContent_MakeAvailable(SoloCraft)
    message(STATUS "SoloCraft module source dir: ${solocraft_SOURCE_DIR}")
  else()
    message(STATUS "SoloCraft module already populated: ${solocraft_POPULATED}")
    add_subdirectory(${solocraft_SOURCE_DIR})
  endif()

else()
  if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/modules/SoloCraft)
    message(STATUS "SoloCraft module exists, but not building. You can remove it manually if you don't need it. (rm -rf ${CMAKE_CURRENT_SOURCE_DIR}/modules/SoloCraft)")

    #file(REMOVE_RECURSE ${CMAKE_CURRENT_SOURCE_DIR}/modules/SoloCraft)
  endif()
endif()

if(BUILD_GAME_SERVER)
  add_subdirectory(game)
  add_subdirectory(mangosd)
endif()

if(BUILD_LOGIN_SERVER)
  add_subdirectory(realmd)
endif()