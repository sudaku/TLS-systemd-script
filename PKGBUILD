# Maintainer: Nitroretro <nitroretro@protonmail.com>

# Based on the `minecraft-server` AUR package by:
## Maintainer: Gordian Edenhofer <gordian.edenhofer@gmail.com>
## Contributor: Philip Abernethy <chais.z3r0@gmail.com>
## Contributor: sowieso <sowieso@dukun.de>


_server_file="Mohist-6b9ba59-server.jar"
_minecraft_ver="1.12.x"

pkgname="mohist"
pkgver=r439.6b9ba59
pkgrel=1
pkgdesc="Minecraft Mohist server unit files, script and jar"
arch=('any')
url="https://github.com/Mohist-Community/Mohist"
license=('custom')
depends=('java-runtime-headless=8' 'screen' 'sudo' 'bash' 'awk' 'sed')
optdepends=("tar: needed in order to create world backups"
	"netcat: required in order to suspend an idle server")
backup=("etc/conf.d/${pkgname}")
install="${pkgname}.install"
source=("${pkgname}d-backup.service"
	"${pkgname}d-backup.timer"
	"${pkgname}d.service"
	"${pkgname}d.conf"
	"${pkgname}d.sh")
noextract=("${_server_file}")
sha512sums=('53e2139014d0e219b89994875fd6f064ce64f1bb724b442b1e6c7cf0e4f404c8338e0d9b118b6539a92d04fd93d2b820267ef48d67f84c438676e98e97599c5c'
            '2dd37cfd974d07384ec3d7087b70b298518de701da6d69d89fe0a3ed7488fe511e872e07783bc5f46efd560c061505098fac79c0306f225fa37333e3e3b6da3f'
            'a5ccf6f61ec9a6b70049ce92801850bab5d1ae4967b1d96380e8127f679e4984f09e6a92185d9be2a213e035ee909a4bb1edd387859ea02d171b3db428e07f2d'
            '9471e7fcb0008cbdf4df31a59c56b8b3392169de2be81c8eca4a40bc0980cf572684243ab77c889f557490ea9387bd01d915963c9ebe535ab325e9f95ddc4d51'
            '26a557ad67599a102e384b226ab58d4c5525d2b76c3772c7e4b295f4a1d6f3b2b6401849d04a141a52150dcbb1c2366aa7dab74101b5b2eaf4aa6a8e9a30e39f'
            'a5605cae8b699ab02983ec11184dc6b43b9dfb1cac1b73a62fcefcc3bbf1110b5f3ef0dbb57cdcb1734fba89482985a56fbf61d38b32f4c31203c61971e7c283'
            '7f158bed6957e5285ce45a480f6a222065af5427bd48481ef24eb770ff540aa67b2d1c1ed976d216db94323017f7c7ee1dfe16e3f222b14189f9823e0b49f0f3'
            '2c9bdefe7d022be139e7aec2e5f1cc1f83ea9d35d2c945e26422e140027b5107ce32c56f0b97e7dbf6b6edb282075df4a18c156a6ed6b064bcb10a3b4481a9aa'
            'c890315962cbc180897094b3558e19ef2452f5ad587bb759e2af1808a86be4c925e7ba767746b2f6b54b24b27d66437593000c7406db5d5dc2824b0fff9775bb'
            '6a21e9f6706dacb99162dd4c70ec704e3fdf283b93ca2cc1521e08e55e0727db4c7384d027d54739f100cd26c8d3d5be717715c6b21086a9bb22efb893c34fcf')

# Mohist jar
source+=("${_server_file}"::"https://ci.codemc.io/job/Mohist-Community/job/Mohist-1.12.2/lastSuccessfulBuild/artifact/build/distributions/${_server_file}")

# -- Licenses -- #
_licenses=()
_license_suffix="-${pkgname}-${pkgver}.txt"

_add_license() {
	_path=$1
	_repo=${2:-MinecraftForge}
	_github_branch=${3:-"${_minecraft_ver}"}
	_filename="$(basename "$_path")${_license_suffix}"

	_licenses+=("$_filename")
	source+=("$_filename"::"https://raw.githubusercontent.com/MinecraftForge/${_repo}/${_github_branch}/${_path}.txt")
}

_add_license "LICENSE-Paulscode%20IBXM%20Library"
_add_license "LICENSE-Paulscode%20SoundSystem%20CodecIBXM"
_add_license "LICENSE"

_licenses+=("LICENSE-LGPLv3${_license_suffix}")
source+=("LICENSE-LGPLv3${_license_suffix}"::"https://www.gnu.org/licenses/lgpl-3.0.txt")
# -- /Licenses -- #

package() {
	_server_root="${pkgdir}/srv/${pkgname}"

	# Install mohist
	install -Dm644 "${pkgname}d-backup.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}d-backup.service"
	install -Dm644 "${pkgname}d-backup.timer" "${pkgdir}/usr/lib/systemd/system/${pkgname}d-backup.timer"
	install -Dm644 "${pkgname}d.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}d.service"
	install -Dm644 "${pkgname}d.conf" "${pkgdir}/etc/conf.d/${pkgname}"
	install -Dm755 "${pkgname}d.sh" "${pkgdir}/usr/bin/${pkgname}d"

	# Install mohist.jar
	install -Dm644 "${_server_file}" "${_server_root}/${_server_file}"
	ln -s "${_server_file}" "${_server_root}/${pkgname}.jar"

	# Link log files
	mkdir -p "${pkgdir}/var/log/"
	install -dm2755 "${_server_root}/logs"
	ln -s "/srv/${pkgname}/logs" "${pkgdir}/var/log/${pkgname}"

	# Install licenses
	for _license in "${_licenses[@]}"; do
		_filename="$(basename "$_license" "$_license_suffix").txt"
		_filename="${_filename//\%20/ }"
		install -Dm644 "$_license" "${pkgdir}/usr/share/licenses/${pkgname}/$_filename"
	done

	chmod g+ws "${_server_root}"
}
