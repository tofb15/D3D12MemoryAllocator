# set the project name and version
project(D3D12MemoryAllocator VERSION 1.0)
ADD_DEFINITIONS(-DUNICODE)
ADD_DEFINITIONS(-D_UNICODE)

#set include directories
set(INC_DIR
${PROJECT_SOURCE_DIR}/src/
)

#include all source files
file(GLOB_RECURSE SRC_FILES 
${PROJECT_SOURCE_DIR}/Common.h
${PROJECT_SOURCE_DIR}/Common.cpp
${PROJECT_SOURCE_DIR}/D3D12MemAlloc.h
${PROJECT_SOURCE_DIR}/D3D12MemAlloc.cpp
)

macro(GroupSources curdir)
   file(GLOB children RELATIVE ${PROJECT_SOURCE_DIR}/${curdir}
${PROJECT_SOURCE_DIR}/${curdir}/*)
   foreach(child ${children})
          if(IS_DIRECTORY ${PROJECT_SOURCE_DIR}/${curdir}/${child})
                  GroupSources(${curdir}/${child})
          else()
                  string(REPLACE "/" "\\" groupname ${curdir})
            source_group(${groupname} FILES
${PROJECT_SOURCE_DIR}/${curdir}/${child})
          endif()
   endforeach()
endmacro()

# Group files within the "src" folder in the correct folder strcuture
GroupSources(src)

include_directories(${INC_DIR})
add_library(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE ${SRC_FILES})
target_link_libraries(${PROJECT_NAME} d3d12.lib) 
target_link_libraries(${PROJECT_NAME} DXGI.lib) 


#=============================================================================
project(D3D12MemoryAllocatorSample VERSION 1.0)
file(GLOB_RECURSE Sample_SRC_FILES 
${PROJECT_SOURCE_DIR}/D3D12Sample.h
${PROJECT_SOURCE_DIR}/D3D12Sample.cpp
${PROJECT_SOURCE_DIR}/Tests.h
${PROJECT_SOURCE_DIR}/Tests.cpp
)
GroupSources(src)

add_executable(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE ${Sample_SRC_FILES})
target_link_libraries(${PROJECT_NAME} D3D12MemoryAllocator) 

#set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY "${EXECUTABLE_OUTPUT_PATH}")