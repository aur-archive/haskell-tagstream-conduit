# custom variables
_hkgname=tagstream-conduit
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-tagstream-conduit
pkgver=0.5.5.3
pkgrel=15
pkgdesc="streamlined html tag parser"
url="http://github.com/yihuang/tagstream-conduit"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.8.4-1"
         "haskell-attoparsec=0.12.1.2-3"
         "haskell-blaze-builder=0.3.3.4-3"
         "haskell-case-insensitive=1.2.0.3-3"
         "haskell-conduit=1.2.3.1-3"
         "haskell-conduit-extra=1.1.6-2"
         "haskell-data-default=0.5.3-59"
         "haskell-resourcet=1.1.3.3-3"
         "haskell-text=1.2.0.3-1"
         "haskell-xml-conduit=1.2.3.1-4")
options=('strip' 'staticlibs')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=("b296e8f0ba18ae951b5bb3fc2d9d964954666df61ea9363d667f251af17134ab")

# PKGBUILD functions

prepare() {
    cd "${srcdir}/${_hkgname}-${pkgver}"
    
    # no cabal patch
    # no source patch
}

build() {
    cd "${srcdir}/${_hkgname}-${pkgver}"
    
    runhaskell Setup configure -O --enable-library-profiling --enable-shared \
        --prefix=/usr --docdir="/usr/share/doc/${pkgname}" \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock --hoogle --html
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd "${srcdir}/${_hkgname}-${pkgver}"
    
    install -D -m744 register.sh   "${pkgdir}/usr/share/haskell/${pkgname}/register.sh"
    install    -m744 unregister.sh "${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh"
    install -d -m755 "${pkgdir}/usr/share/doc/ghc/html/libraries"
    ln -s "/usr/share/doc/${pkgname}/html" "${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}"
    runhaskell Setup copy --destdir="${pkgdir}"
    install -D -m644 "${_licensefile}" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    rm -f "${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}"
}
