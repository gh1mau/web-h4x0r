### Prerequisites Setup

Kita akan menyediakan Platform Ujian Penembusan (*Pen-Test Attack Platform*) bagi tujuan Ujian Penembusan Aplikasi Web (*Web Application Penetration Testing*) dengan menggunakan Sistem Pengoperasian Ubuntu. Berikut adalah langkah-langkah konfigurasi bagi tujuan tersebut:

#### **<u>Perkara asas (*OS Virtualization*):</u>**

1. Muat turun dan install VirtualBox atau VM Player mengikut kesesuaian Sistem Pengoperasian anda.
    (https://www.virtualbox.org/wiki/Downloads)

2. Muat turun Ubuntu image melalui laman web berikut dan loadkan ke dalam VirtualBox atau VMPlayer (kita akan menggunakan OS Ubuntu 18 sebagai attack platform)
   ([Ubuntu 18.04 VMware Image](https://www.linuxvmimages.com/images/ubuntu-1804/))

3. Loadkan / import **image Ubuntu 18** yang telah dimuat turun seperti dalam langkah 2 ke dalam VirtualBox atau VMPlayer. *Default credentials* bagi OS Ubuntu berkenaan adalah seperti berikut: 
   
   | username     | ubuntu     |
   | ------------ | ---------- |
   | **password** | **ubuntu** |

#### 

#### **<u>Kemaskini  & Basic Customization OS Ubuntu(*Ubuntu Updating*):</u>**

1. Setelah *start ubuntu os* melalui VirtualBox / VMPlayer, log masuk menggunakan katanama dan katalaluan default (**username: ubuntu, password: ubuntu**)

2. Buka *terminal windows* dan taipkan *command* berikut: 
   
   (<u>pernyataan selepas tanda # merupakan nota dan tidak perlu ditaip pada terminal</u>)
   
   ```
   # memaparkan alamat ip
   $ ifconfig
   
   # memastikan os ini mempunyai internet connectivity
   $ ping google.com
   ```

3. Taipkan *command* berikut untuk mematikan(disable) fungsi ipv6 di dalam OS Ubuntu anda:
   
   ```
   # kemaskini fail berikut, dan buat perubahan:
   $ sudo nano /etc/default/grub
   
   dari:
   GRUB_CMDLINE_LINUX_DEFAULT=""
   GRUB_CMDLINE_LINUX=""
   
   kepada:
   GRUB_CMDLINE_LINUX_DEFAULT="ipv6.disable=1"
   GRUB_CMDLINE_LINUX="ipv6.disable=1"
   
   # kemaskini grub
   $ sudo update-grub
   
   # reboot ubuntu
   $ sudo init 6
   ```

4. Taipkan *command* berikut untuk mengemaskini Guest OS Ubuntu 18.04:
   
   ```
   # mengemaskini ubuntu:
   $ sudo apt update
   $ sudo apt upgrade
   
   # menyemak versi ubuntu:
   $ lsb_release -a
   
   # memasang VirtualBox Linux Additions:
   $ sudo apt install -y dkms build-essential linux-headers-generic linux-headers-$(uname -r)
   
   # enable pengguna root (masukkan katalaluan baru bagi root):
   $ sudo passwd root
   
   # memadam pengguna default (linuxvmimages):
   $ sudo deluser --remove-home linuxvmimages
   $ sudo reboot
   
   # mengubah username default ubuntu:
   # tekan kekunci Ctrl + Alt + F2 dan login sebagai root
   $ usermod -l gh1mau -d /home/gh1mau -m ubuntu
   $ reboot
   # login dan ubah display name dan ubah katalaluan default
   
   # memaparkan hostname:
   $ hostnamectl
   
   # mengubah hostname:
   $ sudo hostnamectl set-hostname web_h4x0r 
   $ sudo nano /etc/hosts (ubah kepada hostname baru)
   $ sudo reboot
   ```

5. Taipkan *command* berikut untuk memasang zsh ke dalam terminal Ubuntu 18.04:
   
   ```
   # memasang zsh
   $ sudo apt update
   $ sudo apt-get install zsh
   $ sudo apt-get install powerline fonts-powerline
   
   # memasang oh my zsh
   $ sudo apt install git
   $ git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
   $ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
   $ sudo nano .zshrc
   
   # kemaskini theme berikut:
   ZSH_THEME="agnoster"
   
   # ubah default shell kepada zsh
   $ chsh -s /bin/zsh
   $ sudo reboot
   ```

6. Taipkan *command* berikut untuk memasang *oh my zsh plugin*:
   
   ```
   # ZSH Syntax Highlighting
   $ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git "$HOME/.zsh-syntax-highlighting" --depth 1
   $ echo "source $HOME/.zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> "$HOME/.zshrc"
   ```

7. Taipkan *command* berikut untuk enable colored-man-pages plugins:
   
   ```
   $ sudo nano .zshrc
   
   # tambahkan line berikut:
   plugins=(
       colored-man-pages
   )
   ```

8. Taipkan *command* berikut untuk memasang neofetch:
   
   ```
   $ sudo apt update
   $ sudo apt install neofetch
   $ neofetch
   
   $ sudo nano .zshrc
   
   # tambahkan line berikut (kemudian restart terminal):
   if [ -f /usr/bin/neofetch ]
   then
           neofetch
   fi
   
   # fail konfigurasi neofetch:
   ${HOME}/.config/neofetch/config.config
   ```

#### 

#### **<u>Konfigurasi Asas *Remote Access*:</u>**

1. Taipkan *command* berikut untuk memasang openssh-server:
   
   ```
   $ sudo apt update
   $ sudo apt install openssh-server
   
   # memaparkan status ssh server
   $ sudo service ssh status
   
   # memulakan ssh server
   $ sudo service ssh start
   
   # mematikan ssh server
   $ sudo service ssh stop
   
   # fail konfigurasi ssh
   /etc/ssh/sshd_config
   ```

#### 

#### **<u>Memasang Tools Asas *Security Assessment*:</u>**

1. Taipkan *command* berikut untuk memasang terminator:
   
   ```
   $ sudo apt update
   $ sudo apt install terminator
   ```

2. Taipkan *command* berikut untuk memasang keepnote:
   
   ```
   $ sudo apt update
   $ sudo apt install keepnote
   ```

3. Taipkan *command* berikut untuk memasang nmap:
   
   ```
   # menggunakan snap
   $ snap info nmap
   $ sudo snap install nmap 
   ```

4. Taipkan *command* berikut untuk memasang metasploit:
   
   ```
   # dapatkan installer script dari official rapid7 github:
   https://github.com/rapid7/metasploit-framework
   
   # jalankan command berikut:
   $ sudo apt install curl
   $ curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
     chmod 755 msfinstall && \
     ./msfinstall
   ```

5. Taipkan *command* berikut untuk memasang burp suite community edition:
   
   ```
   # muat turun burp suite community edition dari url berikut:
   https://portswigger.net/burp/communitydownload
   
   $ mkdir toolz
   $ mv Downloads/burpsuite_community_linux_v2020_12.sh toolz 
   $ cd toolz
   $ chmod +x burpsuite_community_linux_v2020_12.sh 
   $ ./burpsuite_community_linux_v2020_12.sh
   ```
