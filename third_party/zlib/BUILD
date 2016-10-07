package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # BSD/MIT-like license (for zlib)

cc_library(
  name = "zlib",
  srcs = [
    "upstream/adler32.c",
    "upstream/compress.c",
    "upstream/crc32.c",
    "upstream/crc32.h",
    "upstream/deflate.c",
    "upstream/deflate.h",
    "upstream/gzclose.c",
    "upstream/gzguts.h",
    "upstream/gzlib.c",
    "upstream/gzread.c",
    "upstream/gzwrite.c",
    "upstream/infback.c",
    "upstream/inffast.c",
    "upstream/inffast.h",
    "upstream/inffixed.h",
    "upstream/inflate.c",
    "upstream/inflate.h",
    "upstream/inftrees.c",
    "upstream/inftrees.h",
    "upstream/trees.c",
    "upstream/trees.h",
    "upstream/uncompr.c",
    "upstream/zconf.h",
    "upstream/zutil.c",
    "upstream/zutil.h",
  ],
  hdrs = ["upstream/zlib.h"],
  includes = ["upstream"],
)
