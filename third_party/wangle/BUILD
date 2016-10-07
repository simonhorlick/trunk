licenses(["notice"])

cc_library(
  name = "acceptor",
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ],
  hdrs = glob(["upstream/wangle/acceptor/*.h"]),
  srcs = glob(["upstream/wangle/acceptor/*.cpp"]),
	copts = ["-std=c++14"],
  deps = [
    ":ssl",
    "//third_party/folly:async",
    "//third_party/folly:ssl",
    "//third_party/boost:parameter",
  ]
)

cc_library(
  name = "bootstrap",
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ],
  hdrs = glob([
    "upstream/wangle/bootstrap/*.h"
  ]),
  srcs = [
    "upstream/wangle/bootstrap/ServerBootstrap.cpp"
  ],
	copts = ["-std=c++14"],
  deps = [
    ":acceptor",
    ":channel",
    "//third_party/folly:async",
    "//third_party/folly:futures",
    "//third_party/boost:parameter",
  ]
)

cc_library(
  name = "channel",
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ],
  hdrs = glob(["upstream/wangle/channel/*.h"]),
  srcs = glob(["upstream/wangle/channel/*.cpp"]),
	copts = ["-std=c++14"],
  deps = [
    ":acceptor",
    ":concurrent",
    "//third_party/folly:futures",
    "//third_party/boost:variant",
  ]
)

cc_library(
  name = "concurrent",
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ],
  hdrs = glob([
    "upstream/wangle/concurrent/*.h",
    "upstream/wangle/deprecated/rx/*.h",
  ]),
  srcs = glob(["upstream/wangle/concurrent/*.cpp"]),
	copts = ["-std=c++14"],
  deps = [
    "//third_party/folly:futures",
  ]
)

cc_library(
  name = "ssl",
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ],
  hdrs = glob(["upstream/wangle/ssl/*.h"]) + [
    'upstream/wangle/acceptor/SSLContextSelectionMisc.h'
  ],
  srcs = glob(["upstream/wangle/ssl/*.cpp"]),
	copts = ["-std=c++14"],
  deps = [
    "//third_party/folly:ssl",
  ]
)

# Disabled because dependings on boost/thread
#
# cc_test(
#   name = "bootstrap_test",
#   srcs = [
#     "upstream/wangle/bootstrap/BootstrapTest.cpp"
#   ],
#   deps = [
#     ":wangle",
#     "//third_party/gtest:gtest_main",
#   ]
# )

cc_test(
  name = "concurrent_test",
  srcs = [
    "upstream/wangle/concurrent/test/CodelTest.cpp",
    "upstream/wangle/concurrent/test/GlobalExecutorTest.cpp",
    "upstream/wangle/concurrent/test/ThreadPoolExecutorTest.cpp",
  ],
	copts = ["-std=c++14"],
  deps = [
    ":wangle",
    "//third_party/gtest:gtest_main",
  ]
)

cc_library(
  name = "wangle",
  visibility = ["//visibility:public"],
	copts = ["-std=c++14"],
  deps = [
    ":acceptor",
    ":bootstrap",
    ":channel",
    ":concurrent",
    ":ssl",
  ]
)
