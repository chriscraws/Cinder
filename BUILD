# Bazel build target for cinder. Windows supported only.

cc_library(
    name = "cinder",
    visibility = ["//visibility:public"],
    copts = ["/std:c++17"],
    srcs = glob(
        include = [
            "src/cinder/**/*.cpp",
        ],
        exclude = [
            # android stuff
            "**/android/**",
            "src/cinder/CaptureImplJni.cpp",
            "src/cinder/UrlImplJni.cpp",

            # mac stuff
            "**/cocoa/**",
            "src/cinder/ImageSourceFileQuartz.cpp",
            "src/cinder/ImageTargetFileQuartz.cpp",

            # linux stuff
            "**/linux/**",
            "src/cinder/UrlImplCurl.cpp",

            # windows RT
            "**/winrt/**",

            # misc
            "src/cinder/params/**",
            "src/cinder/app/RendererDx.cpp",
            "src/cinder/app/msw/RendererImplDx.cpp",
            "src/cinder/app/msw/RendererImplGlAngle.cpp",
            "src/cinder/ImageSourcePng.cpp",
        ],
    ),
    includes = ["include"],
    deps = [
        ":cinder_hdrs",
        ":glad",
        ":jsoncpp",
        ":libtess",
        ":linebreak",
        ":oggvorbis",
        ":r8brain",
        ":tinyexr",
        ":zlib",
    ],
    linkopts = [
        "OpenGL32.lib",
        "kernel32.lib",
        "user32.lib",
        "gdi32.lib",
        "winspool.lib",
        "comdlg32.lib",
        "advapi32.lib",
        "shell32.lib",
        "ole32.lib",
        "oleaut32.lib",
        "uuid.lib",
        "odbc32.lib",
        "odbccp32.lib",
        "Ws2_32.lib",
        "shlwapi.lib",
        "wmvcore.lib",
        "Strmiids.lib",
        "Msimg32.lib",
        "/SUBSYSTEM:WINDOWS",
        "/OPT:REF",
    ],
)

cc_library(
    name = "cinder_hdrs",
    defines = [
        "WIN32",
        "_WIN32_WINNT=0x0601",
        "NDEBUG",
        "NOMINMAX",
        "_UNICODE",
        "UNICODE",
        "_WINDOWS",
    ],
    hdrs = glob(
        include = ["include/**/*.h"],
    ),
    strip_include_prefix = "include",
)

cc_library(
    name = "glad",
    srcs = [
        "src/glad/glad.c",
        "src/glad/glad_wgl.c",
    ],
    includes = ["include"],
)

cc_library(
    name = "jsoncpp",
    hdrs = [
        "include/jsoncpp/json.h",
        "include/jsoncpp/json-forwards.h",
    ],
    strip_include_prefix = "include",
    srcs = ["src/jsoncpp/jsoncpp.cpp"],
)

cc_library(
    name = "linebreak",
    hdrs = glob(["src/linebreak/*.h"]),
    srcs = glob([
        "src/linebreak/**/*.h",
        "src/linebreak/**/*.c",
    ]),
    strip_include_prefix = "src/linebreak",
)

cc_library(
    name = "libtess",
    hdrs = ["src/libtess2/tesselator.h"],
    srcs = glob([
        "src/libtess2/**/*.h",
        "src/libtess2/**/*.c",
    ]),
)

cc_library(
    name = "tinyexr",
    hdrs = ["include/tinyexr/tinyexr.h"],
    strip_include_prefix = "include/tinyexr",
    srcs = ["src/tinyexr/tinyexr.cc"],
)

cc_library(
    name = "zlib",
    hdrs = glob(["src/zlib/*.h"]),
    srcs = glob([
        "src/zlib/**/*.h",
        "src/zlib/**/*.c",
    ]),
    strip_include_prefix = "src/zlib",
)

cc_library(
    name = "r8brain",
    hdrs = glob([
        "src/r8brain/**/*.h",
    ]),
    srcs = glob([
        "src/r8brain/**/*.h",
        "src/r8brain/**/*.cpp",
    ]),
    strip_include_prefix = "src/r8brain",
    deps = [":cinder_hdrs"],
)

cc_library(
    name = "oggvorbis",
    srcs = glob([
        "src/oggvorbis/**/*.c",
        "src/oggvorbis/**/*.h",
    ]),
    hdrs = glob(["include/oggvorbis/**/*.h"]),
    includes = ["include/oggvorbis"],
    strip_include_prefix = "include/oggvorbis",
)