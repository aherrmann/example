workspace(name = "bazel_and_package_managers")

load(
    "@bazel_tools//tools/build_defs/repo:http.bzl",
    "http_archive"
)

http_archive(
    name = "rules_haskell",
    strip_prefix = "rules_haskell-0.10",
    urls = ["https://github.com/tweag/rules_haskell/archive/v0.10.tar.gz"],
    sha256 = "48be5de64e883905fd29b6a42fc091f13f44102b9efac9228ba7b958a7e03e32",
)

load(
    "@rules_haskell//haskell:repositories.bzl",
    "rules_haskell_dependencies",
    "rules_haskell_toolchains",
)

rules_haskell_dependencies()
rules_haskell_toolchains()
