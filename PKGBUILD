# Maintainer: Nitroretro <nitroretro@protonmail.com>

# Based on the `minecraft-server` AUR package by:
## Maintainer: Gordian Edenhofer <gordian.edenhofer@gmail.com>
## Contributor: Philip Abernethy <chais.z3r0@gmail.com>
## Contributor: sowieso <sowieso@dukun.de>


_server_file="CatServer-abd368c-async.jar"
_minecraft_ver="1.12.x"

pkgname="catserver-async"
pkgver=20.02.11
pkgrel=1
pkgdesc="Minecraft Catserver server unit files, script and jar"
arch=('any')
url="https://github.com/Luohuayu/CatServer"
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
sha512sums=('6756a921508c5b3e11d4045d406f19e2808c584963977428d4beae3dfba2441976ffd37a89992af7cd69a4683e0eb50a4e0bb40fbc1d3e4e3eb459fa612d7988'
            'da7f1771a52381287ecb0e029f8b8b24021546b2ed5c1e8153c2b5b2643db0e921905faf36903da7f505563a97ac1bb39284bae042d5742895391e8495bc923f'
            'ac20258176d027f728f77d19c66a127a2ec62dab612351a20a98aecff711e4e24411f1c80601495894e1a55e5027a9eab9a7225be48d1c1bc1a28e98973322bc'
            'ab357f4acbe0b2966c98e2312f29b8942f780337b70aa1a7247f4ca74f92a751ae0edd9769ed5c122b195bdac20ea1b0c75dc487b0d3c1afb67ec0a01e474910'
            '09454da8cc55e6ef8793a561d0724dccdb2ddae76f73372f6576a7331d0adbf031005eb9eab7d5ced9f927d60bca1b197a121dd56198a8c583d4f07142c4f603'
            '18b653119351d57807a1be9c3b9c563a7ab038b6ba530d35a50186547ae03a2ce225cc0e7c8b3d568cf80650863264e62f449e04944cd2392f3c5ef3576b91aa'
            '7f158bed6957e5285ce45a480f6a222065af5427bd48481ef24eb770ff540aa67b2d1c1ed976d216db94323017f7c7ee1dfe16e3f222b14189f9823e0b49f0f3'
            '2c9bdefe7d022be139e7aec2e5f1cc1f83ea9d35d2c945e26422e140027b5107ce32c56f0b97e7dbf6b6edb282075df4a18c156a6ed6b064bcb10a3b4481a9aa'
            'c890315962cbc180897094b3558e19ef2452f5ad587bb759e2af1808a86be4c925e7ba767746b2f6b54b24b27d66437593000c7406db5d5dc2824b0fff9775bb'
            '6a21e9f6706dacb99162dd4c70ec704e3fdf283b93ca2cc1521e08e55e0727db4c7384d027d54739f100cd26c8d3d5be717715c6b21086a9bb22efb893c34fcf')

# Catserver jar
source+=("${_server_file}"::"https://github.com/Luohuayu/CatServer/releases/download/${pkgver}/${_server_file}")

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

	# Install catserver-asyncd
	install -Dm644 "${pkgname}d-backup.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}d-backup.service"
	install -Dm644 "${pkgname}d-backup.timer" "${pkgdir}/usr/lib/systemd/system/${pkgname}d-backup.timer"
	install -Dm644 "${pkgname}d.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}d.service"
	install -Dm644 "${pkgname}d.conf" "${pkgdir}/etc/conf.d/${pkgname}"
	install -Dm755 "${pkgname}d.sh" "${pkgdir}/usr/bin/${pkgname}d"

	# Install Catserver
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
