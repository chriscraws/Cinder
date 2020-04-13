# Bazel build target for cinder. Windows and mac supported only.

# To use, import the :cinder cc_library target.

cc_binary(
    name = "audio",
    copts = ["-std=c++17"],
    srcs = [
        "samples/_audio/NodeBasic/src/NodeBasicApp.cpp",
        "samples/_audio/common/AudioDrawUtils.h",
        "samples/_audio/common/AudioDrawUtils.cpp",
    ],
    deps = [":cinder"],
)


WINDOWS = "@bazel_tools//src/conditions:windows"
DARWIN = "@bazel_tools//src/conditions:darwin"
DARWIN_X86_64 = "@bazel_tools//src/conditions:darwin_x86_64"
DEFAULT = "//conditions:default"

# Misc

COMMON_SRC_EXCLUDE = [
    "src/cinder/params/**",
    "src/cinder/app/RendererDx.cpp",
    "src/cinder/app/msw/RendererImplDx.cpp",
    "src/cinder/app/msw/RendererImplGlAngle.cpp",
    "src/cinder/ImageSourcePng.cpp",
    "src/cinder/qtime/**",
    "src/cinder/app/cocoa/AppCocoaTouch.cpp",
    "src/cinder/app/AppScreenSaver.cpp",
]

ANDROID_SRC = [
    "**/android/**",
    "src/cinder/CaptureImplJni.cpp",
    "src/cinder/UrlImplJni.cpp",
]

LINUX_SRC = [
    "**/linux/**",
    "src/cinder/UrlImplCurl.cpp",
]

WINDOWS_RT_SRC = [
    "**/winrt/**",
]

DARWIN_SRC = [
    "**/cocoa/**",
    "src/cinder/ImageSourceFileQuartz.cpp",
    "src/cinder/ImageTargetFileQuartz.cpp",
]

WINDOWS_SRC = [
    "**/msw/**",
    "src/cinder/UrlImplWinInet.cpp",
    "src/cinder/ImageSourceFileWic.cpp",
    "src/cinder/ImageTargetFileWic.cpp",
    "src/cinder/CaptureImplDirectShow.cpp",
]

# Windows specific configuration variables

WINDOWS_DEFINES = [
    "WIN32",
    "_WIN32_WINNT=0x0601",
    "NDEBUG",
    "NOMINMAX",
    "_UNICODE",
    "UNICODE",
    "_WINDOWS",
]

WINDOWS_SRC_EXCLUDE = ANDROID_SRC + DARWIN_SRC + COMMON_SRC_EXCLUDE + LINUX_SRC + WINDOWS_RT_SRC
 
WINDOWS_LINK_OPTS = [
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
]

# Darwin / macOS specifics

DARWIN_OBJCPP_SRC = [
    "src/cinder/Clipboard.cpp",
    "src/cinder/Display.cpp",
    "src/cinder/Font.cpp",
    "src/cinder/Log.cpp",
    "src/cinder/System.cpp",
    "src/cinder/Utilities.cpp",
    "src/cinder/Capture.cpp",
    "src/cinder/app/AppBase.cpp",
    "src/cinder/app/Renderer.cpp",
    "src/cinder/app/RendererGl.cpp",
    "src/cinder/app/Window.cpp",
    "src/cinder/gl/Environment.cpp",
    "src/cinder/app/cocoa/AppMac.cpp",
    "src/cinder/app/cocoa/PlatformCocoa.cpp",
]

DARWIN_SRC_EXCLUDE = ANDROID_SRC + COMMON_SRC_EXCLUDE + LINUX_SRC + WINDOWS_RT_SRC + WINDOWS_SRC + DARWIN_OBJCPP_SRC

DARWIN_DEFINES = [
    "FT2_BUILD_LIBRARY",
    "FT_DEBUG_LEVEL_TRACE",
    "CINDER_MAC_USE_GSTREAMER",
]

DARWIN_LINK_OPTS = [
    "-F/Library/Frameworks",
    "-framework Cocoa",
    "-framework OpenGL",
    "-framework AudioToolbox",
    "-framework AVFoundation",
    "-framework AudioUnit",
    "-framework CoreAudio",
    "-framework CoreMedia",
    "-framework CoreVideo",
    "-framework Accelerate",
    "-framework IOSurface",
    "-framework IOKit",
    "-framework GStreamer",
]

DARWIN_COPTS = [
    "-std=c++17",
    "-F/Library/Frameworks",
    "-w",
]


ALL_CINDER_SRC = "src/cinder/**/*.cpp"

# Import this target to use cinder.
cc_library(
    name = "cinder",
    visibility = ["//visibility:public"],
    copts = select({
        DEFAULT: ["-std=c++17"],
        DARWIN: DARWIN_COPTS,
        WINDOWS: ["/std:c++17"],
    }),
    srcs = select({
        WINDOWS: glob([ALL_CINDER_SRC], WINDOWS_SRC_EXCLUDE),
        DARWIN: glob([ALL_CINDER_SRC], DARWIN_SRC_EXCLUDE),
    }),
    includes = ["include"],
    deps = select({
        DARWIN: [
            ":cinder_objc",
            ":cinder_objcpp",
        ],
        DEFAULT: [],
    }) + [
        ":cinder_hdrs",
        ":glad",
        ":jsoncpp",
        ":libtess",
        ":linebreak",
        ":ogg",
        ":vorbis",
        ":r8brain",
        ":tinyexr",
        ":zlib",
    ],
    linkopts = select({
        WINDOWS: WINDOWS_LINK_OPTS,
        DARWIN: DARWIN_LINK_OPTS,
    }),
)

cc_library(
    name = "cinder_objcpp",
    srcs = DARWIN_OBJCPP_SRC,
    copts = DARWIN_COPTS + [
        "-x",
        "objective-c++",
    ],
    deps = [":cinder_hdrs"],
)

objc_library(
    name = "cinder_objc",
    copts = DARWIN_COPTS + [
        "-fno-objc-arc",
     ],
    srcs = glob(
        include = [
          "src/cinder/**/*.mm",
        ],
        exclude = [
          "**/*Touch*",
          "**/*ScreenSaver*",
          "**/qtime/**",
          "src/cinder/CaptureImplCocoaDummy.mm",
          "src/cinder/audio/cocoa/DeviceManagerAudioSession.mm",
        ],
    ),
    deps = [":cinder_hdrs"],
)


# Cinder headers are used in some of cinders dependencies.

cc_library(
    name = "cinder_hdrs",
    defines = select({
        WINDOWS: WINDOWS_DEFINES,
        DARWIN: DARWIN_DEFINES,
        DARWIN_X86_64: DARWIN_DEFINES,
    }),
    hdrs = glob([
        "include/**/*.h",
        "include/**/*.hpp",
        "include/**/*.inl",
        "include/**/*.ipp",
    ]),
    strip_include_prefix = "include",
)


# Cinder's internal dependencies

cc_library(
    name = "glad",
    srcs = select({
        WINDOWS: ["src/glad/glad_wgl.c"],
        DEFAULT: [],
    }) + [
        "src/glad/glad.c",
    ],
    deps = [":cinder_hdrs"],
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
    name = "vorbis",
    hdrs = glob(["src/oggvorbis/vorbis/**/*.h"]),
    srcs = glob(["src/oggvorbis/vorbis/**/*.c"]),
    strip_include_prefix = "src/oggvorbis/vorbis",
    deps = [":ogg"],
)

cc_library(
    name = "ogg",
    srcs = glob([
        "src/oggvorbis/ogg/*.c",
        "src/oggvorbis/ogg/*.h",
    ]),
    hdrs = glob(["include/oggvorbis/**/*.h"]),
    includes = [
        "include/oggvorbis",
    ],
    strip_include_prefix = "include/oggvorbis",
)

