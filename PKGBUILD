# Contributor: Thomas Laroche <tho.laroche@gmail.com>
# Maintainer: Thomas Fanninger <thomas@fanninger.at>

_branch=master
_pkgname=gogs
_gourl=github.com/gogits
_gopath=${GOPATH}
pkgname=gogs-git
pkgver=r4186.2bec8a4
pkgrel=1
pkgdesc="Self Hosted Git Service in the Go Programming Language. This is the current git version from branch ${_branch}."
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url="http://gogs.io/"
license=('MIT')
depends=('git>=1.7.1' 'bash')
optdepends=('sqlite: SQLite support'
            'mariadb: MariaDB support'
            'postgresql: PostgreSQL support'
            'redis: Redis support'
            'memcached: MemCached support'
            'openssh: GIT over SSH support'
            'tidb-git: TiDB support')
makedepends=('go>=1.4' 'git' 'glide')
conflicts=('gogs')
provides=('gogs')
options=('!strip' '!emptydirs')
backup=('etc/gogs/app.ini')
install=gogs.install

source=('gogs.service'
        "${_pkgname}::git+https://${_gourl}/${_pkgname}.git#branch=${_branch}")

pkgver(){
    _builddir=$srcdir/build
    cd "${_builddir}/src/${_gourl}/${_pkgname}"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    _builddir=$srcdir/build
    GOPATH="${_builddir}${_gopath:+:${_gopath}}"
    rm -rf "${_builddir}/src/${_gourl}"
    mkdir -p "${_builddir}/src/${_gourl}"
    mv  ${_pkgname} "${_builddir}/src/${_gourl}/${_pkgname}"
    cd "${_builddir}/src/${_gourl}/${_pkgname}"
    git remote set-url origin https://${_gourl}/${_pkgname}
    git checkout -q -f master
    go get -u -x -d -tags='sqlite pam cert' ./...
}

build() {
    _builddir=$srcdir/build
    GOPATH="${_builddir}${_gopath:+:${_gopath}}"
    cd "${_builddir}/src/${_gourl}/${_pkgname}"
    go fix
    go build -x -tags='sqlite pam cert'
}

package() {
    _builddir=$srcdir/build
    install -Dm0755 "${_builddir}/src/${_gourl}/${_pkgname}/${_pkgname}" "$pkgdir/usr/share/${_pkgname}/${_pkgname}"
    cp -r "${_builddir}/src/${_gourl}/${_pkgname}/conf" "$pkgdir/usr/share/${_pkgname}"
    install -dm755 "$pkgdir/usr/share/themes/gogs/default"
    cp -r "${_builddir}/src/${_gourl}/${_pkgname}/public" "$pkgdir/usr/share/themes/gogs/default"
    cp -r "${_builddir}/src/${_gourl}/${_pkgname}/templates" "$pkgdir/usr/share/themes/gogs/default"
    install -Dm0644 "${_builddir}/src/${_gourl}/${_pkgname}/conf/app.ini" "$pkgdir/etc/${_pkgname}/app.ini"
    install -Dm0644 "$srcdir/gogs.service" "$pkgdir/usr/lib/systemd/system/gogs.service"
}

sha512sums=('95c697a38ebf6a6a2c65fd10aa0ecf1a4aa62f41b5224012366cd0976e20876b02ed1422b00adb4a9fe6f199be2fb6ab0ca7cbd9565e3a790ccfd4627b39d586'
            'SKIP')
