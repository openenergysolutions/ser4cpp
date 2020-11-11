set(ser4cpp_headers
    ./ser4cpp/container/Array.h
    ./ser4cpp/container/ArrayView.h
    ./ser4cpp/container/Buffer.h
    ./ser4cpp/container/HasLength.h
    ./ser4cpp/container/Pair.h
    ./ser4cpp/container/RSeq.h
    ./ser4cpp/container/SequenceTypes.h
    ./ser4cpp/container/Settable.h
    ./ser4cpp/container/StaticBuffer.h
    ./ser4cpp/container/WSeq.h

    ./ser4cpp/serialization/BigEndian.h
    ./ser4cpp/serialization/DoubleFloat.h
    ./ser4cpp/serialization/EndianHelpers.h
    ./ser4cpp/serialization/FloatByteOrder.h
    ./ser4cpp/serialization/LittleEndian.h
    ./ser4cpp/serialization/SerializationTemplates.h
    ./ser4cpp/serialization/SingleFloat.h
    ./ser4cpp/serialization/UInt48Type.h

    ./ser4cpp/util/Comparisons.h
    ./ser4cpp/util/HexConversions.h
    ./ser4cpp/util/Uncopyable.h
)

add_library(ser4cpp INTERFACE)
#target_sources(ser4cpp INTERFACE ${ser4cpp_public_headers})
target_compile_features(ser4cpp INTERFACE cxx_std_14)
target_include_directories(ser4cpp INTERFACE 
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>"
    "$<INSTALL_INTERFACE:include>")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${PROJECT_BINARY_DIR}/ser4cppConfigVersion.cmake"
    VERSION 1.0
    COMPATIBILITY AnyNewerVersion
)
install(TARGETS ser4cpp
    EXPORT ser4cppTargets
    LIBRARY DESTINATION lib COMPONENT Runtime
    ARCHIVE DESTINATION lib COMPONENT Development
    RUNTIME DESTINATION bin COMPONENT Runtime
    PUBLIC_HEADER DESTINATION include COMPONENT Development
    BUNDLE DESTINATION bin COMPONENT Runtime
    )
include(CMakePackageConfigHelpers)
configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/cmake/ser4cppConfig.cmake.in"
    "${PROJECT_BINARY_DIR}/ser4cppConfig.cmake"
    INSTALL_DESTINATION lib/cmake/ser4cpp
)
install(EXPORT ser4cppTargets DESTINATION lib/cmake/ser4cpp)
install(FILES "${PROJECT_BINARY_DIR}/ser4cppConfigVersion.cmake"
              "${PROJECT_BINARY_DIR}/ser4cppConfig.cmake"
        DESTINATION lib/cmake/ser4cpp)
install(FILES ${ser4cpp_public_headers} DESTINATION include/ser4cpp)
install(DIRECTORY ./ser4cpp/container DESTINATION include/ser4cpp)
install(DIRECTORY ./ser4cpp/serialization DESTINATION include/ser4cpp)
install(DIRECTORY ./ser4cpp/util DESTINATION include/ser4cpp)
