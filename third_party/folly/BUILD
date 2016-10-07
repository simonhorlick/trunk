licenses(["notice"])

# Generators
genrule(
  name = "generate_format_tables",
  srcs = [
    "upstream/folly/build/generate_format_tables.py"
  ],
  cmd = "$(location upstream/folly/build/generate_format_tables.py) --install_dir=$(GENDIR)/third_party/folly",
  outs = [
    "FormatTables.cpp",
  ],
)

genrule(
  name = "generate_escape_tables",
  srcs = [
    "upstream/folly/build/generate_escape_tables.py"
  ],
  cmd = "$(location upstream/folly/build/generate_escape_tables.py) --install_dir=$(GENDIR)/third_party/folly",
  outs = [
    "EscapeTables.cpp",
  ],
)

genrule(
  name = "generate_varint_tables",
  srcs = [
    "upstream/folly/build/generate_varint_tables.py"
  ],
  cmd = "$(location upstream/folly/build/generate_varint_tables.py) --install_dir=$(GENDIR)/third_party/folly",
  outs = [
    "GroupVarintTables.cpp",
  ],
)

cc_binary(
  srcs = [
    "upstream/folly/build/GenerateFingerprintTables.cpp",
    "upstream/folly/detail/FingerprintPolynomial.h",
  ],
  deps = [
    ":base",
  ],
  name = "GenerateFingerprintTables",
)

genrule(
  name = "generate_fingerprint_tables",
  srcs = [
    ":GenerateFingerprintTables"
  ],
  cmd = "$(location :GenerateFingerprintTables) --install_dir=$(GENDIR)/third_party/folly",
  outs = [
    "FingerprintTables.cpp"
  ]
)

cc_library(
  name = "base",
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ] + select({
      ":linux": ["linux-k8"],
      ":darwin": ["darwin"],
  }),
  hdrs = select({
      ":linux": ["linux-k8/folly/folly-config.h"],
      ":darwin": ["darwin/folly/folly-config.h"],
  })
  + glob(["upstream/folly/*.h"])
  + glob(["upstream/folly/portability/*.h"]) + [
    "upstream/folly/detail/ExceptionWrapper.h",
    "upstream/folly/detail/BitIteratorDetail.h",
    "upstream/folly/detail/BitsDetail.h",
    "upstream/folly/experimental/io/FsUtil.h",
  ],
  copts = ["-std=gnu++14"],
  srcs = [
    ":generate_format_tables",
    ":generate_escape_tables",
    "upstream/folly/detail/Malloc.h",
    "upstream/folly/detail/RangeCommon.h",
    "upstream/folly/detail/RangeSse42.h",
    "upstream/folly/detail/UncaughtExceptionCounter.h",
    "upstream/folly/Conv.cpp",
    "upstream/folly/Demangle.cpp",
    "upstream/folly/Format.cpp",
    "upstream/folly/StringBase.cpp",
    "upstream/folly/String.cpp",
    "upstream/folly/SpookyHashV1.cpp",
    "upstream/folly/SpookyHashV2.cpp",
    "upstream/folly/Unicode.cpp",
  ],
  deps = [
    "//third_party/glog",
    "//third_party/double-conversion",
    "//third_party/boost:config",
    "//third_party/boost:conversion",
    "//third_party/boost:iterator",
    "//third_party/boost:regex",
    "//third_party/boost:type_traits",
    "//third_party/boost:utility",
  ],
)

cc_library(
  name = "FingerprintTables",
  hdrs = [
    "upstream/folly/detail/FingerprintPolynomial.h",
  ],
  srcs = [
    ":generate_fingerprint_tables"
  ],
  deps = [
    ":base",
  ],
)

# cc_library(
#   name = "checksum",
#   visibility = ["//visibility:public"],
#   hdrs = [
#     "upstream/folly/Checksum.h"
#   ],
#   srcs = [
#     "upstream/folly/Checksum.cpp",
#   ],
#   deps = [
#     ":base",
#     "//third_party/boost:crc",
#   ],
# )

cc_library(
  name = "folly",
  visibility = ["//visibility:public"],
  hdrs = glob([
    "upstream/folly/stats/*.h",
    "upstream/folly/gen/*.h",
    "upstream/folly/*.h",
    "upstream/folly/detail/*.h",
    "upstream/folly/io/*.h",
  ]) + [
    "upstream/folly/experimental/ReadMostlySharedPtr.h",
    "upstream/folly/experimental/TLRefCount.h",
    "upstream/folly/experimental/AsymmetricMemoryBarrier.h",
  ],
  copts = [
    "-std=gnu++14",
    "-pthread",
  ],
  srcs = glob([
    "upstream/folly/detail/*.h",
  ]) + [
    ":generate_varint_tables",
    "upstream/folly/Bits.cpp",
    "upstream/folly/File.cpp",
    "upstream/folly/FileUtil.cpp",
    "upstream/folly/GroupVarint.cpp",
    "upstream/folly/IPAddress.cpp",
    "upstream/folly/IPAddressV4.cpp",
    "upstream/folly/IPAddressV6.cpp",
    "upstream/folly/LifoSem.cpp",
    "upstream/folly/detail/CacheLocality.cpp",
    "upstream/folly/detail/Futex.cpp",
    "upstream/folly/dynamic.cpp",
    "upstream/folly/io/Compression.cpp",
    "upstream/folly/io/IOBuf.cpp",
    "upstream/folly/io/IOBufQueue.cpp",
    "upstream/folly/io/ShutdownSocketSet.cpp",
    "upstream/folly/SharedMutex.cpp",
    "upstream/folly/detail/ThreadLocalDetail.cpp",
    # These require libevent
    "upstream/folly/MacAddress.cpp",
    "upstream/folly/Random.cpp",
    "upstream/folly/SafeAssert.cpp",
    "upstream/folly/Singleton.cpp",
    "upstream/folly/SocketAddress.cpp",
    "upstream/folly/Subprocess.cpp",
    "upstream/folly/ThreadCachedArena.cpp",
    "upstream/folly/TimeoutQueue.cpp",
    "upstream/folly/Uri.cpp",
    "upstream/folly/detail/MemoryIdler.cpp",
    "upstream/folly/json.cpp",
    "upstream/folly/stats/Instantiations.cpp",
  ],
  deps = [
    ":base",
    "//third_party/double-conversion",
    "//third_party/glog",
    "//third_party/gflags",
    # "//third_party/libevent:libevent",
    "//third_party/boost:algorithm",  # For boost/non_copyable
    "//third_party/boost:core",  # For boost/non_copyable
    "//third_party/boost:container",
    "//third_party/boost:intrusive",
    "//third_party/boost:iterator",
    "//third_party/boost:multi_index",
    "//third_party/boost:regex",
    "//third_party/boost:thread",
    "//third_party/boost:utility",  # For boost/operators.hpp
  ],
  linkopts = [
    "-ldl",
  ],
)

cc_library(
  name = "async",
  visibility = ["//visibility:public"],
  hdrs = glob([
    "upstream/folly/io/async/**/*.h",
  ], exclude = [
    "upstream/folly/io/async/test/**",
    "upstream/folly/io/async/ssl/test/**",
  ]) + [
    "upstream/folly/experimental/ExecutionObserver.h",
    "upstream/folly/futures/DrivableExecutor.h",
  ],
  srcs = glob(["upstream/folly/io/async/**/*.cc"]),
  defines = [
    "FOLLY_HAVE_VLA=0",
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    # folly won't currently compile with boringssl
    # (see https://github.com/facebook/folly/issues/198).
    "//third_party/openssl:ssl",
    "//third_party/libevent",
    "//third_party/boost:iterator",
  ]
)

cc_library(
  name = "futures",
  visibility = ["//visibility:public"],
  srcs = glob([
    "upstream/folly/futures/*.cpp",
    "upstream/folly/futures/detail/*.cpp",
    "upstream/folly/fibers/*.cpp",
  ]),
  hdrs = glob([
    "upstream/folly/futures/*.h",
    "upstream/folly/futures/detail/*.h",
    "upstream/folly/fibers/*.h",
  ]),
  copts = ["-std=gnu++14"],
  deps = [
    ":async",
    ":folly",
    "//third_party/boost:context",
  ]
)

cc_library(
  name = "ssl",
  visibility = ["//visibility:public"],
  hdrs = glob(["upstream/folly/io/async/ssl/**/.h"]),
  srcs = glob(["upstream/folly/io/async/ssl/**/.cc"]),
  defines = [
    "FOLLY_HAVE_VLA=0",
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":async",
    "//external:libssl",
  ]
)

# Broken because missing dwarf.h
#
# cc_library(
#   name = "symbolizer",
#   visibility = ["//visibility:public"],
#   hdrs = glob([
#     "upstream/folly/experimental/symbolizer/*.h",
#   ]),
#   srcs = glob([
#     "upstream/folly/experimental/symbolizer/*.cpp",
#   ]),
#   copts = [
#     #'-DHAVE_LINK_H=1'
#   ],
#   deps = [
#     ":base",
#     ":folly",
#     "//third_party/boost:assert",
#     "//third_party/boost:container",
#     "//third_party/boost:functional",
#     "//third_party/boost:iterator",
#     "//third_party/boost:intrusive",
#     "//third_party/boost:throw_exception",
#     "//third_party/boost:type_index",
#     "//third_party/boost:variant",
#   ],
# )

cc_library(
  name = "recordio",
  visibility = ["//visibility:public"],
  hdrs = [
    "upstream/folly/io/RecordIO.h",
    "upstream/folly/io/RecordIO-inl.h",
    "upstream/folly/MemoryMapping.h",
    "upstream/folly/experimental/io/FsUtil.h",
    "upstream/folly/experimental/io/HugePages.h",
  ],
  srcs = [
    "upstream/folly/io/RecordIO.cpp",
    "upstream/folly/MemoryMapping.cpp",
    "upstream/folly/experimental/io/FsUtil.cpp",
    "upstream/folly/experimental/io/HugePages.cpp",
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    "//third_party/boost:filesystem",
  ],
)

# Test libraries and tests.
cc_library(
  name = "testutil",
  visibility = ["//visibility:public"],
  hdrs = [
    "upstream/folly/experimental/TestUtil.h",
  ],
  srcs = [
    "upstream/folly/experimental/TestUtil.cpp",
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":base",
    ":folly",
    "//third_party/boost:filesystem",
    "//third_party/boost:regex",
  ],
  testonly = 1
)

cc_library(
  name = "benchmark",
  visibility = ["//visibility:public"],
  hdrs = [
    "upstream/folly/Benchmark.h",
  ],
  srcs = [
    "upstream/folly/Benchmark.cpp",
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    "//third_party/boost:function_types",
    "//third_party/boost:regex",
  ],
  testonly = 1
)

cc_test(
  name = "AsciiCaseInsensitiveTest",
  srcs = [
    "upstream/folly/test/AsciiCaseInsensitiveTest.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":base",
    "//third_party/gflags",
    "//third_party/gtest",
  ]
)

# cc_test(
#   name = "ChecksumTest",
#   srcs = [
#     "upstream/folly/test/ChecksumTest.cpp"
#   ],
#   deps = [
#     ":checksum",
#     ":benchmark",
#     "//third_party/gflags",
#     "//third_party/gtest",
#   ]
# )

cc_test(
  name = "ProducerConsumerQueueTest",
  srcs = [
    "upstream/folly/test/ProducerConsumerQueueTest.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":base",
    "//third_party/boost:core",
    "//third_party/gtest:gtest_main",
  ]
)

cc_test(
  name = "StringTest",
  srcs = [
    "upstream/folly/test/StringTest.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":base",
    ":benchmark",
    "//third_party/gflags",
    "//third_party/gtest:gtest_main",
  ]
)

cc_binary(
  name = "subprocess_test_parent_death_helper",
  srcs = [
    "upstream/folly/test/SubprocessTestParentDeathHelper.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    ":recordio",
    "//third_party/gflags",
  ],
  testonly = 1,
)

cc_test(
  name = "SubprocessTest",
  srcs = [
    "upstream/folly/test/SubprocessTest.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    ":benchmark",
    ":recordio",
    ":testutil",
    "//third_party/gflags",
    "//third_party/gtest:gtest_main",
  ],
  data = [
    ":subprocess_test_parent_death_helper",
  ]
)

cc_test(
  name = "UriTest",
  srcs = [
    "upstream/folly/test/UriTest.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    ":benchmark",
    "//third_party/gflags",
    "//third_party/gtest",
  ]
)

cc_test(
  name = "VarintTest",
  srcs = [
    "upstream/folly/test/VarintTest.cpp"
  ],
  copts = ["-std=gnu++14"],
  deps = [
    ":folly",
    ":benchmark",
    "//third_party/gflags",
    "//third_party/gtest",
  ]
)

config_setting(
    name = "darwin",
    values = {"cpu": "darwin"},
)

config_setting(
    name = "linux",
    values = {"cpu": "k8"},
)
