licenses(["notice"])

filegroup(
  name = "platform_config",
  srcs = select({
      ":darwin": ["glog_build/darwin/src/config.h"],
      ":linux": ["glog_build/k8/src/config.h"],
  }),
)

filegroup(
  name = "platform_hdrs",
  srcs = select({
      ":darwin": [
        "glog_build/darwin/include/glog/logging.h",
        "glog_build/darwin/include/glog/raw_logging.h",
        "glog_build/darwin/include/glog/stl_logging.h",
        "glog_build/darwin/include/glog/vlog_is_on.h",
      ],
      ":linux": [
        "glog_build/k8/include/glog/logging.h",
        "glog_build/k8/include/glog/raw_logging.h",
        "glog_build/k8/include/glog/stl_logging.h",
        "glog_build/k8/include/glog/vlog_is_on.h",
      ],
  }),
  visibility = ["//visibility:public"],
)

cc_library(
  visibility = ["//visibility:public"],
  name = "glog",
  deps = select({
      ":darwin": [],
      ":linux": ["//third_party/libunwind:libunwind-k8"],
  }) + ["//third_party/gflags:gflags"],
  includes = [
    "upstream/src",
  ] + select({
      ":darwin": ["glog_build/darwin/include", "glog_build/darwin/src"],
      ":linux": ["glog_build/k8/include", "glog_build/k8/src"],
  }),
  copts = [
    "-DNO_FRAME_POINTER",
  ],
  srcs = [
    "upstream/src/demangle.cc",
    "upstream/src/logging.cc",
    "upstream/src/raw_logging.cc",
    "upstream/src/signalhandler.cc",
    "upstream/src/symbolize.cc",
    "upstream/src/utilities.cc",
    "upstream/src/vlog_is_on.cc",
    # Private headers
    ":platform_config",
    "upstream/src/base/commandlineflags.h",
    "upstream/src/base/googleinit.h",
    "upstream/src/base/mutex.h",
    "upstream/src/glog/log_severity.h",
    "upstream/src/stacktrace.h",
    "upstream/src/stacktrace_generic-inl.h",
    "upstream/src/stacktrace_libunwind-inl.h",
    "upstream/src/stacktrace_powerpc-inl.h",
    "upstream/src/stacktrace_x86-inl.h",
    "upstream/src/stacktrace_x86_64-inl.h",
  ],
  hdrs = [
    "upstream/src/demangle.h",
    "upstream/src/mock-log.h",
    "upstream/src/symbolize.h",
    "upstream/src/utilities.h",
    ":platform_hdrs"
  ],
  linkopts = [
    "-pthread"
  ],
)

cc_library(
  name = "testlib",
  testonly = 1,
  includes = [
    "glog_build/k8/src",
  ],
  hdrs = [
    "upstream/src/googletest.h",
    "upstream/src/config_for_unittests.h",
  ]
)

cc_test(
  name = "demangle_unittest",
  deps = [
    ":glog",
    ":testlib",
  ],
  srcs = [
    "upstream/src/demangle_unittest.cc",
  ],
)

cc_test(
  name = "logging_unittest",
  deps = [
    ":glog",
    ":testlib",
  ],
  srcs = [
    "upstream/src/logging_unittest.cc",
  ],
)

cc_test(
  name = "symbolize_unittest",
  deps = [
    ":glog",
    ":testlib",
  ],
  srcs = [
    "upstream/src/symbolize_unittest.cc",
  ],
)

config_setting(
    name = "darwin",
    values = {"cpu": "darwin"},
)

config_setting(
    name = "linux",
    values = {"cpu": "k8"},
)
