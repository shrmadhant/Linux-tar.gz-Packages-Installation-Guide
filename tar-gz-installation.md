# Installing `.tar.gz` Packages on Linux


### Prerequisites

Before you begin, make sure you have the build tools installed. Without them, compilation will fail halfway through.

On Debian/Ubuntu/Mint:

	sudo apt install build-essential

On Arch/Manjaro:

	sudo pacman -S base-devel

On Fedora/RHEL:

	sudo dnf groupinstall "Development Tools"

Verify that `make` and `gcc` are available:

	make --version
	gcc --version


### Extracting the archive

Navigate to the directory where the file was downloaded and extract it:

	tar -xzf package.tar.gz

This creates a directory with the package name. Enter it:

	cd package-name/

If you are not sure of the exact directory name:

	ls -lh

To extract into a specific directory:

	tar -xzf package.tar.gz -C /path/to/destination/

To list the contents without extracting:

	tar -tzf package.tar.gz


### Reading the package documentation

Before anything else, read the instruction files that almost every package includes:

	ls

Look for files such as `README`, `INSTALL`, `README.md`, or similar. They may point to specific dependencies or a build process that differs from the standard one.

	cat README
	cat INSTALL


### Configuring the package

Most manually compiled packages use the `./configure` script to check dependencies and prepare the build environment. Verify that it exists:

	ls configure

If it does, run:

	./configure

To install into a directory other than the default (`/usr/local`), use the `--prefix` parameter:

	./configure --prefix=/opt/my-program

If `./configure` fails with a missing dependency error, install the indicated package and run it again.

Some newer packages use `CMake` instead of `./configure`. If there is no `configure` script but there is a `CMakeLists.txt`:

	mkdir build
	cd build
	cmake ..


### Compiling

With configuration complete, compile the source code:

	make

This process may take a while depending on the package size and your hardware. To use multiple cores and speed up compilation:

	make -j$(nproc)

Errors during compilation usually indicate missing dependencies. Install what is indicated and run `make` again. If you need to start over from scratch:

	make clean
	./configure
	make


### Installing

With compilation complete, install the program on the system:

	sudo make install

By default, files are placed in `/usr/local/bin`, `/usr/local/lib`, etc. If you used `--prefix` in the previous step, files will be installed in the specified directory.

Verify that the installation was successful:

	which program-name
	program-name --version


### Uninstalling

If the package supports uninstallation via `make` (most do):

	sudo make uninstall

If that command is not available, you will need to remove the files manually. The `make install` output usually lists every file it copied, so reviewing it can help locate what needs to be removed.


### Quick reference: `tar` flags

| Flag | Function |
|------|----------|
| `-x` | Extract |
| `-z` | Decompress gzip (`.gz`) |
| `-j` | Decompress bzip2 (`.bz2`) |
| `-J` | Decompress xz (`.xz`) |
| `-f` | Specify the archive file |
| `-t` | List contents without extracting |
| `-v` | Verbose mode (shows files being extracted) |
| `-C` | Extract into a specific directory |

For `.tar.bz2` or `.tar.xz` packages, the process is identical. Just swap the decompression flag or let `tar` detect it automatically:

	tar -xf package.tar.bz2
	tar -xf package.tar.xz
