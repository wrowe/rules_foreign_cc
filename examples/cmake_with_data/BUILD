load("@rules_cc//cc:defs.bzl", "cc_test")
load("@rules_foreign_cc//tools/build_defs:cmake.bzl", "cmake_external")

cmake_external(
    name = "cmake_with_data",
    cache_entries = select({
        "@bazel_tools//src/conditions:windows": {
            "CMAKE_BUILD_TYPE": "Release",
            "CMAKE_CXX_FLAGS": "$CMAKE_CXX_FLAGS /MD",
            "CMAKE_CXX_FLAGS_DEBUG": "$CMAKE_CXX_FLAGS_DEBUG /MD",
        },
        "//conditions:default": {},
    }),
    cmake_options = select({
        "@bazel_tools//src/conditions:windows": ["-G \"Visual Studio 15 2017\" -A x64"],
        "//conditions:default": [],
    }),
    data = [
        "cmake_with_data.txt",
        "//cmake_with_data/lib_b",
    ],
    generate_crosstool_file = select({
        "@bazel_tools//src/conditions:windows": True,
        "//conditions:default": False,
    }),
    lib_source = "//cmake_with_data/lib_a:srcs",
    make_commands = select({
        "@bazel_tools//src/conditions:windows": ["MSBuild.exe INSTALL.vcxproj"],
        "//conditions:default": [
            "make",
            "make install",
        ],
    }),
    static_libraries = select({
        "@bazel_tools//src/conditions:windows": ["lib_a.lib"],
        "//conditions:default": ["lib_a.a"],
    }),
)

# Demonstrates that files can be made available at runtime
cc_test(
    name = "test_with_data",
    srcs = [
        "tests/test_cmake_with_data.cpp",
    ],
    deps = [
        ":cmake_with_data",
    ],
)

# Demonstrates that target outputs can be made available at runtime
cc_test(
    name = "test_with_shared_lib",
    srcs = [
        "tests/test_cmake_with_shared_lib.cpp",
    ],
    deps = [
        ":cmake_with_data",
    ],
)

test_suite(
    name = "data_attr_tests",
    tests = [
        ":test_with_data",
        ":test_with_shared_lib",
    ],
)
