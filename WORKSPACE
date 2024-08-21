# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

workspace(name = "com_github_brpc_brpc")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

#
# Constants
#

BAZEL_IO_VERSION = "4.2.4"

BAZEL_IO_SHA256 = "4c179ce66bbfff6ac5d81b8895518096e7f750866d08da2d4a574d1b8029e914"

BAZEL_SKYLIB_VERSION = "1.2.1"

BAZEL_SKYLIB_SHA256 = "f7be3474d42aae265405a592bb7da8e171919d74c16f082a5457840f06054728"

BAZEL_PLATFORMS_VERSION = "0.0.10"

BAZEL_PLATFORMS_SHA256 = "218efe8ee736d26a3572663b374a253c012b716d8af0c07e842e82f238a0a7ee"

RULES_PROTO_VERSION = "4.0.0-3.20.0"

RULES_PROTO_SHA256 = "e017528fd1c91c5a33f15493e3a398181a9e821a804eb7ff5acdd1d2d6c2b18d"

RULES_CC_VERSION = "0.0.2"

RULES_CC_SHA256 = "58bff40957ace85c2de21ebfc72e53ed3a0d33af8cc20abd0ceec55c63be7de2"

#
# Starlark libraries
#

# http_archive(
#    name = "io_bazel",
#    # sha256 = BAZEL_IO_SHA256,
#    strip_prefix = "bazel-" + BAZEL_IO_VERSION,
#    url = "https://github.com/bazelbuild/bazel/archive/refs/tags/4.2.4.zip",
# )

http_archive(
    name = "bazel_skylib",
    sha256 = BAZEL_SKYLIB_SHA256,
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.2.1/bazel-skylib-1.2.1.tar.gz",
    ],
)

http_archive(
    name = "platforms",
    sha256 = BAZEL_PLATFORMS_SHA256,
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/platforms/releases/download/{version}/platforms-{version}.tar.gz".format(version = BAZEL_PLATFORMS_VERSION),
        "https://github.com/bazelbuild/platforms/releases/download/{version}/platforms-{version}.tar.gz".format(version = BAZEL_PLATFORMS_VERSION),
    ],
)

http_archive(
    name = "rules_proto",
    sha256 = RULES_PROTO_SHA256,
    strip_prefix = "rules_proto-{version}".format(version = RULES_PROTO_VERSION),
    urls = ["https://github.com/bazelbuild/rules_proto/archive/refs/tags/{version}.tar.gz".format(version = RULES_PROTO_VERSION)],
)

http_archive(
    name = "rules_cc",
    urls = ["https://github.com/bazelbuild/rules_cc/releases/download/0.0.1/rules_cc-0.0.1.tar.gz"],
    sha256 = "4dccbfd22c0def164c8f47458bd50e0c7148f3d92002cdb459c2a96a68498241",
)

http_archive(
    name = "rules_perl",
    sha256 = "3e52a9ab4162d59318adddb911794ff884c30477fb1dcdad92f8fb533edf9110",
    strip_prefix = "rules_perl-0.2.0",
    urls = [
        "https://github.com/bazelbuild/rules_perl/archive/refs/tags/0.2.0.tar.gz",
    ],
)

# Use rules_foreign_cc as fewer as possible.
#
# 1. Build very basic libraries without any further dependencies.
# 2. Build too complex to bazelize library.
http_archive(
    name = "rules_foreign_cc",
    sha256 = "4b33d62cf109bcccf286b30ed7121129cc34cf4f4ed9d8a11f38d9108f40ba74",
    strip_prefix = "rules_foreign_cc-0.11.1",
    url = "https://github.com/bazelbuild/rules_foreign_cc/releases/download/0.11.1/rules_foreign_cc-0.11.1.tar.gz",
)

#
# Starlark rules
#

load("@rules_foreign_cc//foreign_cc:repositories.bzl", "rules_foreign_cc_dependencies")

rules_foreign_cc_dependencies(register_preinstalled_tools = True)

#
# C++ Dependencies
#
# Ordered lexicographical.
#

http_archive(
    name = "boost",  # 2021-08-05T01:30:05Z
    build_file = "@com_github_nelhage_rules_boost//:BUILD.boost",
    patch_cmds = ["rm -f doc/pdf/BUILD"],
    patch_cmds_win = ["Remove-Item -Force doc/pdf/BUILD"],
    sha256 = "5347464af5b14ac54bb945dc68f1dd7c56f0dad7262816b956138fc53bcc0131",
    strip_prefix = "boost_1_77_0",
    urls = [
        "https://boostorg.jfrog.io/artifactory/main/release/1.77.0/source/boost_1_77_0.tar.gz",
    ],
)

http_archive(
    name = "com_github_gflags_gflags",  # 2018-11-11T21:30:10Z
    sha256 = "34af2f15cf7367513b352bdcd2493ab14ce43692d2dcd9dfc499492966c64dcf",
    strip_prefix = "gflags-2.2.2",
    urls = ["https://github.com/gflags/gflags/archive/v2.2.2.tar.gz"],
)

http_archive(
    name = "com_github_google_crc32c",  # 2021-10-05T19:47:30Z
    build_file = "//bazel/third_party/crc32c:crc32c.BUILD",
    patch_tool = "patch",
    patch_args = ["-p1"],
    patches = ["//bazel:configure_template.bzl.patch"],
    sha256 = "ac07840513072b7fcebda6e821068aa04889018f24e10e46181068fb214d7e56",
    strip_prefix = "crc32c-1.1.2",
    urls = ["https://github.com/google/crc32c/archive/1.1.2.tar.gz"],
)

http_archive(
    name = "com_github_google_glog",  # 2021-05-07T23:06:39Z
    patch_args = ["-p1"],
    patches = [
        "//bazel/third_party/glog:0001-mark-override-resolve-warning.patch",
    ],
    sha256 = "21bc744fb7f2fa701ee8db339ded7dce4f975d0d55837a97be7d46e8382dea5a",
    strip_prefix = "glog-0.5.0",
    urls = ["https://github.com/google/glog/archive/v0.5.0.zip"],
)

http_archive(
    name = "com_github_google_leveldb",  # 2021-02-23T21:51:12Z
    build_file = "//bazel/third_party/leveldb:leveldb.BUILD",
    sha256 = "9a37f8a6174f09bd622bc723b55881dc541cd50747cbd08831c2a82d620f6d76",
    strip_prefix = "leveldb-1.23",
    urls = [
        "https://github.com/google/leveldb/archive/refs/tags/1.23.tar.gz",
    ],
)

http_archive(
    name = "com_github_google_snappy",
    build_file = "//bazel/third_party/snappy:snappy.BUILD",
    sha256 = "7ee7540b23ae04df961af24309a55484e7016106e979f83323536a1322cedf1b",
    strip_prefix = "snappy-1.2.0",
    urls = [
        "https://github.com/google/snappy/archive/1.2.0.zip",
    ],
)

http_archive(
    name = "com_github_libevent_libevent",  # 2020-07-05T13:33:03Z
    build_file = "//bazel/third_party/event:event.BUILD",
    sha256 = "92e6de1be9ec176428fd2367677e61ceffc2ee1cb119035037a27d346b0403bb",
    strip_prefix = "libevent-2.1.12-stable",
    urls = [
        "https://github.com/libevent/libevent/releases/download/release-2.1.12-stable/libevent-2.1.12-stable.tar.gz",
    ],
)

# TODO: SIMD optimization.
http_archive(
    name = "com_github_madler_zlib",
    build_file = "//bazel/third_party/zlib:zlib.BUILD",
    # sha256 = "1525952a0a567581792613a9723333d7f8cc20b87a81f920fb8bc7e3f2251428",
    strip_prefix = "zlib-1.3.1",
    urls = [
        "https://github.com/madler/zlib/archive/refs/tags/v1.3.1.tar.gz",
    ],
)

http_archive(
    name = "com_github_nelhage_rules_boost",  # 2021-08-27T15:46:06Z
    patch_cmds = ["sed -i 's/net_zlib_zlib/com_github_madler_zlib/g' BUILD.boost"],
    patch_cmds_win = [
        """$content = (Get-Content 'BUILD.boost') -replace "net_zlib_zlib", "com_github_madler_zlib"
Set-Content BUILD.boost -Value $content -Encoding UTF8
""",
    ],
    sha256 = "2d0b2eef7137730dbbb180397fe9c3d601f8f25950c43222cb3ee85256a21869",
    strip_prefix = "rules_boost-fce83babe3f6287bccb45d2df013a309fa3194b8",
    urls = [
        "https://github.com/nelhage/rules_boost/archive/fce83babe3f6287bccb45d2df013a309fa3194b8.tar.gz",
    ],
)

http_archive(
    name = "com_google_absl",  # 2021-09-27T18:06:52Z
    sha256 = "2f0d9c7bc770f32bda06a9548f537b63602987d5a173791485151aba28a90099",
    strip_prefix = "abseil-cpp-7143e49e74857a009e16c51f6076eb197b6ccb49",
    urls = ["https://github.com/abseil/abseil-cpp/archive/7143e49e74857a009e16c51f6076eb197b6ccb49.zip"],
)

http_archive(
    name = "com_google_googletest",  # 2021-07-09T13:28:13Z
    sha256 = "12ef65654dc01ab40f6f33f9d02c04f2097d2cd9fbe48dc6001b29543583b0ad",
    strip_prefix = "googletest-8d51ffdfab10b3fba636ae69bc03da4b54f8c235",
    urls = ["https://github.com/google/googletest/archive/8d51ffdfab10b3fba636ae69bc03da4b54f8c235.zip"],
)

http_archive(
    name = "com_google_protobuf",
    build_file = "//bazel/third_party/protobuf:protobuf.BUILD",
    # sha256 = "87407cd28e7a9c95d9f61a098a53cf031109d451a7763e7dd1253abf8b4df422",
    strip_prefix = "protobuf-21.12",
    urls = ["https://github.com/protocolbuffers/protobuf/archive/refs/tags/v21.12.tar.gz"],
)

http_archive(
    name = "openssl",  # 2021-12-14T15:45:01Z
    build_file = "//bazel/third_party/openssl:openssl.BUILD",
    sha256 = "f89199be8b23ca45fc7cb9f1d8d3ee67312318286ad030f5316aca6462db6c96",
    strip_prefix = "openssl-1.1.1m",
    urls = [
        "https://www.openssl.org/source/openssl-1.1.1m.tar.gz",
        "https://github.com/openssl/openssl/archive/OpenSSL_1_1_1m.tar.gz",
    ],
)

# https://github.com/google/boringssl/blob/master/INCORPORATING.md
git_repository(
    name = "boringssl", # 2021-05-01T12:26:01Z
    commit = "853ca1ea1168dff08011e5d42d94609cc0ca2e27", # fips-20210429
    remote = "https://boringssl.googlesource.com/boringssl",
)

http_archive(
    name = "org_apache_thrift",  # 2021-09-11T11:54:01Z
    build_file = "//bazel/third_party/thrift:thrift.BUILD",
    sha256 = "d5883566d161f8f6ddd4e21f3a9e3e6b8272799d054820f1c25b11e86718f86b",
    strip_prefix = "thrift-0.15.0",
    urls = ["https://archive.apache.org/dist/thrift/0.15.0/thrift-0.15.0.tar.gz"],
)

#
# Perl Dependencies
#

load("@rules_perl//perl:deps.bzl", "perl_register_toolchains")

perl_register_toolchains()

#
# Tools Dependencies
#
# Hedron's Compile Commands Extractor for Bazel
# https://github.com/hedronvision/bazel-compile-commands-extractor
http_archive(
    name = "hedron_compile_commands",
    url = "https://github.com/hedronvision/bazel-compile-commands-extractor/archive/3dddf205a1f5cde20faf2444c1757abe0564ff4c.tar.gz",
    strip_prefix = "bazel-compile-commands-extractor-3dddf205a1f5cde20faf2444c1757abe0564ff4c",
    sha256 = "3cd0e49f0f4a6d406c1d74b53b7616f5e24f5fd319eafc1bf8eee6e14124d115",
)
load("@hedron_compile_commands//:workspace_setup.bzl", "hedron_compile_commands_setup")
hedron_compile_commands_setup()
