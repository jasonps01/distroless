workspace(name = "distroless")

git_repository(
    name = "io_bazel_rules_go",
    remote = "https://github.com/bazelbuild/rules_go.git",
    tag = "0.11.0",
)

load("@io_bazel_rules_go//go:def.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains()

load(
    "//package_manager:package_manager.bzl",
    "dpkg_list",
    "dpkg_src",
    "package_manager_repositories",
)

package_manager_repositories()

dpkg_src(
    name = "debian_stretch",
    arch = "amd64",
    distro = "stretch",
    sha256 = "9e7870c3c3b5b0a7f8322c323a3fa641193b1eee792ee7e2eedb6eeebf9969f3",
    snapshot = "20180719T151130Z",
    url = "https://snapshot.debian.org/archive",
)

dpkg_src(
    name = "debian_stretch_backports",
    arch = "amd64",
    distro = "stretch-backports",
    sha256 = "29524787f58bc4e139e30e66bc476ff1ea33c0aa939d11638626dbe07c64b30d",
    snapshot = "20180919T095426Z",
    url = "http://snapshot.debian.org/archive",
)

dpkg_src(
    name = "debian_stretch_security",
    package_prefix = "https://snapshot.debian.org/archive/debian-security/20180919T095426Z/",
    packages_gz_url = "https://snapshot.debian.org/archive/debian-security/20180919T095426Z/dists/stretch/updates/main/binary-amd64/Packages.gz",
    sha256 = "4b7df485333ed77ccc9bb4fea9bffe302dc4d0e2303f27b39dc6a4cfcfe5fca5",
)

dpkg_src(
    name = "debian_buster",
    arch = "amd64",
    distro = "buster",
    sha256 = "0d5bf32ede4fa7c20a8149c7dfe57a8ee03c2abfc3a90a8bb6a5479ee047d404",
    snapshot = "20180919T095426Z",
    url = "http://snapshot.debian.org/archive",
)

dpkg_list(
    name = "package_bundle",
    packages = [
        # Version required to skip a security fix to the pre-release library
        # TODO: Remove when there is a security fix or dpkg_list finds the recent version
        "libc6=2.24-11+deb9u3",
        "ca-certificates",
        "openssl",
        "libssl1.0.2",
        "libssl1.1",
        "libbz2-1.0",
        "libdb5.3",
        "libffi6",
        "libncursesw5",
        "liblzma5",
        "libexpat1",
        "libreadline7",
        "libtinfo5",
        "libsqlite3-0",
        "mime-support",
        "netbase",
        "readline-common",
        "tzdata",

        #c++
        "libgcc1",
        "libgomp1",
        "libstdc++6",

        #java
        "zlib1g",
        "openjdk-8-jre-headless",
        "openjdk-10-jre-headless",

        #python
        "libpython2.7-minimal",
        "python2.7-minimal",
        "libpython2.7-stdlib",
        "dash",
        "libc-bin",

        #python3
        "libpython3.5-minimal",
        "python3.5-minimal",
        "libpython3.5-stdlib",

        #dotnet
        "libcurl3",
        "libgssapi-krb5-2",
        "libicu57",
        "liblttng-ust0",
        "libssl1.0.2",
        "libunwind8",
        "libuuid1",
        "zlib1g",
        "curl",
        "libcomerr2",
        "libidn2-0",
        "libk5crypto3",
        "libkrb5-3",
        "libldap-2.4-2",
        "libldap-common",
        "libsasl2-2",
        "libnghttp2-14",
        "libpsl5",
        "librtmp1",
        "libssh2-1",
        "libkeyutils1",
        "libkrb5support0",
        "libunistring0",
        "libgnutls30",
        "libgmp10",
        "libhogweed4",
        "libidn11",
        "libnettle6",
        "libp11-kit0",
        "libffi6",
        "libtasn1-6",
        "libsasl2-modules-db",
        "libdb5.3",
        "libgcrypt20",
        "libgpg-error0",
        "libacl1",
        "libattr1",
        "libselinux1",
        "libpcre3",
        "libbz2-1.0",
        "liblzma5",
    ],
    # Takes the first package found: security updates should go first
    # If there was a security fix to a package before the stable release, this will find
    # the older security release. This happened for stretch libc6.
    sources = [
        "@debian_stretch_security//file:Packages.json",
        "@debian_stretch_backports//file:Packages.json",
        "@debian_stretch//file:Packages.json",
        "@debian_buster//file:Packages.json",
    ],
)

# For Jetty
new_http_archive(
    name = "jetty",
    build_file = "BUILD.jetty",
    sha256 = "ca93c7f88e842fcb1e7bd551c071b3302b7be1faf9cad3ce415af19c77d6cb74",
    strip_prefix = "jetty-distribution-9.4.4.v20170414/",
    type = "tar.gz",
    urls = ["http://central.maven.org/maven2/org/eclipse/jetty/jetty-distribution/9.4.4.v20170414/jetty-distribution-9.4.4.v20170414.tar.gz"],
)

# Node
new_http_archive(
    name = "nodejs",
    build_file = "experimental/nodejs/BUILD.nodejs",
    sha256 = "0e49da19cdf4c89b52656e858346775af21f1953c308efbc803b665d6069c15c",
    strip_prefix = "node-v8.9.1-linux-x64/",
    type = "tar.gz",
    urls = ["https://nodejs.org/dist/v8.9.1/node-v8.9.1-linux-x64.tar.gz"],
)

# dotnet
new_http_archive(
    name = "dotnet",
    build_file = "experimental/dotnet/BUILD.dotnet",
    sha256 = "69ecad24bce4f2132e0db616b49e2c35487d13e3c379dabc3ec860662467b714",
    type = "tar.gz",
    urls = ["https://download.microsoft.com/download/5/F/0/5F0362BD-7D0A-4A9D-9BF9-022C6B15B04D/dotnet-runtime-2.0.0-linux-x64.tar.gz"],
)

# For the debug image
http_file(
    name = "busybox",
    executable = True,
    sha256 = "b51b9328eb4e60748912e1c1867954a5cf7e9d5294781cae59ce225ed110523c",
    urls = ["https://busybox.net/downloads/binaries/1.27.1-i686/busybox"],
)

# Docker rules.
git_repository(
    name = "io_bazel_rules_docker",
    commit = "7401cb256222615c497c0dee5a4de5724a4f4cc7",
    remote = "https://github.com/bazelbuild/rules_docker.git",
)

load(
    "@io_bazel_rules_docker//docker:docker.bzl",
    "docker_pull",
    "docker_repositories",
)

# Used to generate java ca certs.
docker_pull(
    name = "debian8",
    # From tag: 2017-09-11-115552
    digest = "sha256:6d381d0bf292e31291136cff03b3209eb40ef6342fb790483fa1b9d3af84ae46",
    registry = "gcr.io",
    repository = "google-appengine/debian8",
)

docker_repositories()

# Have the py_image dependencies for testing.
load(
    "@io_bazel_rules_docker//python:image.bzl",
    _py_image_repos = "repositories",
)

_py_image_repos()

# Have the java_image dependencies for testing.
load(
    "@io_bazel_rules_docker//java:image.bzl",
    _java_image_repos = "repositories",
)

_java_image_repos()

# Have the go_image dependencies for testing.
load(
    "@io_bazel_rules_docker//go:image.bzl",
    _go_image_repos = "repositories",
)

_go_image_repos()
