workspace(name = "bazel_and_package_managers")

# Load the repository rule to download an http archive.
load(
    "@bazel_tools//tools/build_defs/repo:http.bzl",
    "http_archive"
)

# Download rules_haskell and make it accessible as "@rules_haskell".
http_archive(
    name = "rules_haskell",
    strip_prefix = "rules_haskell-0.12",
    urls = ["https://github.com/tweag/rules_haskell/archive/v0.12.tar.gz"],
    sha256 = "56a8e6337df8802f1e0e7d2b3d12d12d5d96c929c8daecccc5738a0f41d9c1e4",
)

load(
    "@rules_haskell//haskell:repositories.bzl",
    "rules_haskell_dependencies",
)

# Setup all Bazel dependencies required by rules_haskell.
rules_haskell_dependencies()

load(
    "@rules_haskell//haskell:toolchain.bzl",
    "rules_haskell_toolchains",
)

# Download a GHC binary distribution from haskell.org and register it as a toolchain.
rules_haskell_toolchains(version = "8.6.5")

http_archive(
    name = "zlib",
    build_file_content = """
cc_library(
    name = "zlib",
    srcs = [":z"],
    hdrs = glob(["*.h"]),
    visibility = ["//visibility:public"],
)
cc_library(name = "z", srcs = glob(["*.c"]), hdrs = glob(["*.h"]))
""",
    sha256 = "c3e5e9fdd5004dcb542feda5ee4f0ff0744628baf8ed2dd5d66f8ca1197cb1a1",
    strip_prefix = "zlib-1.2.11",
    urls = ["http://zlib.net/zlib-1.2.11.tar.gz"],
)

load(
    "@rules_haskell//haskell:cabal.bzl",
    "stack_snapshot",
)

stack_snapshot(
    name = "stackage",
    packages = [
        "aeson",
        "base",
        "directory",
        "filepath",
        "hspec",
        "hspec-discover",
        "http-client",
        "http-types",
        "servant",
        "servant-client",
        "servant-server",
        "transformers",
        "wai",
        "warp",
    ],
    extra_deps = {
        "zlib": ["@zlib"],
    },
    snapshot = "lts-14.11",
)

http_archive(
    name = "hspec-discover",
    strip_prefix = "hspec-discover-2.6.1",
    urls = [
        "http://hackage.haskell.org/package/hspec-discover-2.6.1/hspec-discover-2.6.1.tar.gz",
    ],
    sha256 = "9d569a9587d2034272d287442855490a06266192eba1da871cae7d971b922fa1",
    # Thanks to this archive, we can make executable "hspec-discover"
    # available to other rules (see "@hspec-discover" references).
    # To build the executable, we use the haskell_cabal_binary rule:
    build_file_content = """
load(
    "@rules_haskell//haskell:cabal.bzl",
    "haskell_cabal_binary",
)
haskell_cabal_binary(
    name = "hspec-discover",
    srcs = glob(["**"]),
    deps = [
        "@stackage//:base",
        "@stackage//:directory",
        "@stackage//:filepath",
        "@stackage//:hspec-discover",
    ],
    visibility = ["//visibility:public"],
)
""",
)
