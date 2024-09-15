# Maintainer: Bob Aarons <bob.aarons@gmail.com>
_pkgname=reliquary-archiver
pkgname=${_pkgname}-git
pkgver=0.1.11.r1.g09cd16d
pkgrel=1
pkgdesc="tool to create a relic export from network packets of a certain turn-based anime game"
arch=('x86_64')
url="https://github.com/IceDynamix/reliquary-archiver"
license=('MIT')
depends=('libpcap')
makedepends=('git' 'libcap' 'cargo')
options=(!lto)
install="${pkgname}.install"
provides=("$_pkgname")
conflicts=("$_pkgname")
source=("$_pkgname::git+https://github.com/IceDynamix/reliquary-archiver.git"
        "$pkgname.install")
sha256sums=('SKIP'
            '2a79b1dd4b000491bc35042d32f55aa0181253e240eac6cdb74c2e0f56286778')

pkgver() {
    cd "$srcdir/$_pkgname"
    git describe --long --tags --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
        cd "$srcdir/$_pkgname"
    export RUSTUP_TOOLCHAIN=stable
    cargo update
    cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
    cd "$srcdir/$_pkgname"
    export RUSTUP_TOOLCHAIN=stable
    export CARGO_TARGET_DIR=target
    cargo build --frozen --release --all-features
}

package() {
    cd "$srcdir/$_pkgname"
    install -Dm0755 -t "$pkgdir/usr/bin/" "target/release/$_pkgname"
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"
}
