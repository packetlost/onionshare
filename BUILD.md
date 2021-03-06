# Building OnionShare

## GNU/Linux

Start by getting a copy of the source code:

```sh
git clone https://github.com/micahflee/onionshare.git
cd onionshare
```

*For .deb-based distros (like Debian, Ubuntu, Linux Mint):*

Note that python-stem appears in Debian wheezy and newer (so by extension Tails 1.1 and newer), and it appears in Ubuntu 13.10 and newer. Older versions of Debian and Ubuntu aren't supported.

```sh
sudo apt-get install -y build-essential fakeroot python-all python-stdeb python-flask python-stem python-qt4 dh-python
./install/build_deb.sh
sudo dpkg -i deb_dist/onionshare_*.deb
```

*For .rpm-based distros (Red Hat, Fedora, CentOS):*

```sh
sudo yum install -y rpm-build python-flask python-stem pyqt4
./install/build_rpm.sh
sudo yum install -y dist/onionshare-*.rpm
```

## Mac OS X

Install the [latest python 2.x](https://www.python.org/downloads/) from python.org. If you use the built-in version of python that comes with OS X, your .app might not run on other people's computers.

To install the right dependencies, you need homebrew and pip installed on your Mac. Follow instructions at http://brew.sh/ to install homebrew, and run `sudo easy_install pip` to install pip.

The first time you're setting up your dev environment:

```sh
echo export PYTHONPATH=\$PYTHONPATH:/usr/local/lib/python2.7/site-packages/ >> ~/.profile
source ~/.profile
brew install qt4 pyqt
sudo pip install py2app flask stem
```

Get the source code:

```sh
git clone https://github.com/micahflee/onionshare.git
cd onionshare
```

To build the .app:

```sh
install/build_osx.sh
```

Now you should have `dist/OnionShare.app`.

To codesign and build a .pkg for distribution:

```sh
install/build_osx.sh --sign
```

Now you should have `dist/OnionShare.pkg`.

## Windows

### Setting up your dev environment

* Download and install the latest python 2.7 from https://www.python.org/downloads/ -- make sure you install the 32-bit version.
* Right click on Computer, go to Properties. Click "Advanced system settings". Click Environment Variables. Under "System variables" double-click on Path to edit it. Add `;C:\Python27;C:\Python27\Scripts` to the end. Now you can just type `python` to run python scripts in the command prompt.
* Go to https://pip.pypa.io/en/latest/installing.html. Right-click on `get-pip.py` and Save Link As, and save it to your home folder.
* Open `cmd.exe` as an administrator. Type: `python get-pip.py`. Now you can use `pip` to install packages.
* Open a command prompt and type: `pip install flask stem pyinstaller`
* Go to http://www.riverbankcomputing.com/software/pyqt/download and download the latest PyQt4 for Windows for python 2.7, 32-bit (I downloaded `PyQt4-4.11-gpl-Py2.7-Qt4.8.6-x32.exe`), then install it.
* Go to http://sourceforge.net/projects/pywin32/ and download and install the latest 32-bit pywin32 binary for python 2.7. I downloaded `pywin32-219.win32-py2.7.exe`.
* Download and install the [Microsoft Visual C++ 2008 Redistributable Package (x86)](http://www.microsoft.com/en-us/download/details.aspx?id=29).

If you want to build the installer:

* Go to http://nsis.sourceforge.net/Download and download the latest NSIS. I downloaded `nsis-3.0b0-setup.exe`.
* Right click on Computer, go to Properties. Click "Advanced system settings". Click Environment Variables. Under "System variables" double-click on Path to edit it. Add `;C:\Program Files (x86)\NSIS` to the end. Now you can just type `makensisw [script]` to build an installer.

If you want to sign binaries with Authenticode:

* Go to http://msdn.microsoft.com/en-us/vstudio/aa496123 and install the latest .NET Framework. I installed `.NET Framework 4.5.1`.
* Go to http://www.microsoft.com/en-us/download/confirmation.aspx?id=8279 and install the Windows SDK.
* Right click on Computer, go to Properties. Click "Advanced system settings". Click Environment Variables. Under "System variables" double-click on Path to edit it. Add `;C:\Program Files\Microsoft SDKs\Windows\v7.1\Bin` to the end.
* You'll also, of course, need a code signing certificate. I roughly followed [this guide](http://blog.assarbad.net/20110513/startssl-code-signing-certificate/) to make one using my StartSSL account.
* Once you get a code signing key and certificate and covert it to a pfx file, import it into your certificate store.

### To make a .exe:

* Open a command prompt, cd into the onionshare directory, and type: `pyinstaller -y install\onionshare-win.spec`. Inside the `dist` folder there will be a folder called `onionshare` with `onionshare.exe` in it.

### To build the installer:

Note that you must have a code signing certificate installed in order to use the `install\build_exe.bat` script, because it tries code signing both `onionshare.exe` and `OnionShare_Setup.exe`.

Open a command prompt, cd to the onionshare directory, and type: `install\build_exe.bat`

A NSIS window will pop up, and once it's done you will have `dist\OnionShare_Setup.exe`.

## Tests

OnionShare includes [nose](https://nose.readthedocs.org/en/latest/) unit tests. First,

```sh
sudo pip install nose
```

To run the tests:

```sh
nosetests test
```
