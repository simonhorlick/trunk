licenses(["notice"])

filegroup(
  name = "platform_headers",
  srcs = select({
      ":darwin": [
          "darwin/config.h",
          "darwin/event-config.h",
      ],
      ":linux": [
          "linux-k8/config.h",
          "linux-k8/event-config.h",
          "upstream/epoll.c",
      ],
  }),
)
cc_library(
  visibility = ["//visibility:public"],
  includes = [
    "upstream",
  ] + select({
    ":linux": ["linux-k8"],
    ":darwin": ["darwin"],
  }),
  srcs = [":platform_headers"] + [
    "upstream/evdns.h",
    "upstream/evsignal.h",
    "upstream/log.h",
    "upstream/evrpc.h",
    "upstream/evutil.h",
    "upstream/min_heap.h",
    "upstream/event-internal.h",
    "upstream/evrpc-internal.h",
    "upstream/http-internal.h",
    "upstream/strlcpy-internal.h",
    "upstream/event.c",
    "upstream/buffer.c",
    "upstream/evbuffer.c",
    "upstream/log.c",
    "upstream/evutil.c",
    "upstream/event_tagging.c",
    "upstream/http.c",
    "upstream/evdns.c",
    "upstream/strlcpy.c",
    "upstream/signal.c",
    "upstream/poll.c",
    "upstream/select.c",
    "upstream/compat/sys/queue.h",
  ],
  hdrs = [
    "upstream/event.h",
    "upstream/evhttp.h",
  ],
  name = "libevent",
  copts = [
    "-Wno-unused-but-set-variable",
    "-DHAVE_CONFIG_H",
    "-Ithird_party/libevent/upstream",
    "-Ithird_party/libevent/upstream/compat",
    "-Ithird_party/libevent/upstream/compat/sys",
  ] + select({
      ":darwin" : ["-Ithird_party/libevent/darwin"],
      ":linux": ["-Ithird_party/libevent/linux-k8"],
      "//conditions:default": [],
  }),
)

config_setting(
    name = "darwin",
    values = {"cpu": "darwin"},
)

config_setting(
    name = "linux",
    values = {"cpu": "k8"},
)
