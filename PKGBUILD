# Maintainer: Josh VanderLinden <arch@cloudlery.com>
_gitname=vnstat-ui
pkgname=vnstatui
pkgver=r41.03e769b
pkgrel=1
pkgdesc="Basic web UI that displays traffic graphs from vnstat"
arch=('any')
url="http://bitbucket.org/instarch/vnstat-ui"
license=('BSD')
depends=('vnstat' 'python' 'python-bottle' 'python-setuptools' 'gd' 'uwsgi' 'uwsgi-plugin-python')
makedepends=('git' 'python-docutils')
optdepends=(
  'nginx: fast server to proxy requests to vnstatui'
)
backup=(
  'etc/conf.d/vnstatui.conf'
  'etc/conf.d/vnstatui.systemd'
  'etc/nginx/sites-available/vnstatui.conf'
)
source=(
  "${_gitname}::git+http://bitbucket.org/instarch/vnstat-ui"
  #"${_gitname}::git+file://$(pwd)"
  'vnstatui.service'
  'vnstatui.conf'
  'vnstatui.nginx'
  'vnstatui.uwsgi'
)
md5sums=('SKIP'
         '1134b69b67dcd94fa59a76ade201c5e0'
         'f89554c432a7c415fb884ab09078ce96'
         'e92c608e35b68b97f043029fc6cc72cd'
         '168123ba305c08587f576ca28078e448')

pkgver() {
  cd "${srcdir}/${_gitname}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

package() {
  cd "${srcdir}/${_gitname}"

  python setup.py install --optimize=1 --root="${pkgdir}/"

  local path=$(dirname $(find ${pkgdir} -name app.py ))
  for f in license.html index.tpl; do
    pwd
    install -Dm644 vnstatui/${f} ${path}/
  done

  msg "Copying configuration files"
  install -Dm644 vnstatui.conf "${pkgdir}/etc/conf.d/vnstatui.conf"
  install -Dm644 vnstatui.uwsgi "${pkgdir}/etc/conf.d/vnstatui.uwsgi"
  install -Dm644 vnstatui.nginx "${pkgdir}/etc/nginx/sites-available/vnstatui.conf"
  install -Dm644 vnstatui.service "${pkgdir}/usr/lib/systemd/system/vnstatui.service"
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  msg "Creating manpage"
  rst2man README.rst | gzip > vnstatui.1.gz
  install -Dm644 vnstatui.1.gz "${pkgdir}/usr/share/man/man1/vnstatui.1.gz"
}

# vim:set ts=2 sw=2 et:
