try below things:
 
1. Install dependencies
For Ubuntu/Debian/Kali:
sudo apt update
sudo apt install -y build-essential libpam0g-dev libssl-dev libldap2-dev libselinux1-dev pkg-config
 
2. Download sudo-1.9.14 source
Directly from Sudo project archive:
wget https://www.sudo.ws/dist/sudo-1.9.14.tar.gz
tar -xzvf sudo-1.9.14.tar.gzcd sudo-1.9.14
✔️ 3. Configure build
./configure --prefix=/usr/local/sudo-1.9.14 --with-all-insults
This keeps it away from your system sudo, preventing damage.
✔️ 4. Build & Install
make
sudo make install
✔️ 5. Create a test binary without overwriting system sudo
sudo ln -s /usr/local/sudo-1.9.14/bin/sudo /usr/local/bin/sudo-vuln
Now your vulnerable sudo is available as:
sudo-vuln
✔️ 6. Verify version
sudo-vuln -V
Output should show Sudo version 1.9.14.
✔️ 7. (Optional) Replace system sudo (dangerous)
Only inside a disposable VM:
sudo cp /usr/local/sudo-1.9.14/bin/sudo /usr/bin/sudo
sudo cp /usr/local/sudo-1.9.14/libexec/sudo/* /usr/libexec/sudo/
 
