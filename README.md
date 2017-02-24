# Tutorial for how to make deplay a server, install shadowsocks and serverspeeder

---

1. First choose your favorite service provider, I choose [Vultr](http://www.vultr.com/?ref=7102608-3B) and its LA node.
2. Deloy a server on it, choose **Ubuntu 14.04** version and login with default password.
3. Change root's password: `passwd root`.
4. Edit `sshd_config` file: `vi /etc/ssh/sshd_config`.
  - Comment `Port 22` with **#**: `#Port 22`. (Disable default ssh port)
  - Append `Port 2017` just after it. (Enable a new port for ssh, you can change 2017 to the number you like).
  - Comment `PasswordAuthentication no` with **#** if it exists.
  - Append `PasswordAuthentication yes` after it.
  - save the file and exit vi editor.
5. Run `service ssh restart`.
6. Run command below to install shadowsocks.

   ```shell
   apt-get install python-pip
   pip install shadowsocks
   ```
7. Create **/etc/shadowsocks.json** with content below:

   ```json
   {
      "server": "your-server-ip",
      "server_port": your-shadowsocks-port,
      "local_address": "127.0.0.1",
      "local_port": 1080,
      "password": "your-shadowsocks-password",
      "timeout":300,
      "method":"aes-256-cfb"
   }
   ```
   **Note**: Don't forget to replace **your-server-ip**, **your-shadowsocks-port**, **your-shadowsocks-password**.
8. Run `uname -a` to make sure your server os's kernel is **3.13.0-24-generic**, if it is not, continue the tutorial, if it is, go to **step 13** directly.
9. Run `apt-get install linux-image-extra-3.13.0-24-generic` to install the kernel.
10. Run `dpkg -l|grep linux-image` to list all your installed kernels.
11. Unstall all kernels which is not **3.13.0-24-generic**, for my case, **step 10** gives me:

  ```
ii  linux-image-3.13.0-105-generic       3.13.0-105.152                    amd64        Linux kernel image for version 3.13.0 on 64 bit x86 SMP
ii  linux-image-3.13.0-24-generic        3.13.0-24.47                      amd64        Linux kernel image for version 3.13.0 on 64 bit x86 SMP
ii  linux-image-extra-3.13.0-105-generic 3.13.0-105.152                    amd64        Linux kernel extra modules for version 3.13.0 on 64 bit x86 SMP
ii  linux-image-extra-3.13.0-24-generic  3.13.0-24.47                      amd64        Linux kernel extra modules for version 3.13.0 on 64 bit x86 SMP
ii  linux-image-generic                  3.13.0.105.113                    amd64        Generic Linux kernel image
  ```
  So I have to run `apt-get purge linux-image-3.13.0-105-generic linux-image-extra-3.13.0-105-generic` to delete redundant kernels.
  **Note**, you should replace your own redundant kernels in the last command.
12. Reboot server `reboot`.
13. Run `wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh` to install Serverspeeder.
14. Run `ssserver -c /etc/shadowsocks.json -d start` to start shadowsocks.

--- 

For now, your own vps is deployed, and just set up your shadowsocks client with the ip, port and password which you have just written in /etc/shadowsocks, and now, all works!

[Sign up with $20 promo](http://www.vultr.com/?ref=7102608-3B)

---
*Reference*:
1. http://wuchong.me/blog/2015/02/02/shadowsocks-install-and-optimize/
2. https://github.com/91yun/serverspeeder
