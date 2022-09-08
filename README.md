# 此版使用方法  
```txt
1、下载驱动  
2、解压  
3、cd到解压的文件夹  
4、执行make  
5、执行sudo make install  
6、执行sudo sh install-rapoo-keyboard-driver.sh  
```

```txt
为毛要用v500键盘呢，都用了七八年了有感情，舍不得仍，哈哈哈。
```

# 以下为原版编译方法  
# rapoo-keyboard-driver

Some bugfix for rapoo keyboard where some keys are invalid.

# Quick Usage

Compile the driver module with make, make install and run ./install-rapoo-keyboard-driver.sh.

Job done! Then enjoy typing!

# Install in UEFI Security Mode

1. Generating key pairs with openssl and signing your kernel mod.

   ```bash
   openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Rapoo-Keyboard/"
   ```

2. Import key.

   ```bash
   sudo mokutil --import MOK.der
   ```

3. Reboot system and enroll key.
   ```txt
   1、当进入蓝色背景的界面perform mok management 后，选择enroll mok,
   2、进入enroll mok 界面后，选择continue，
   3、进入enroll the key界面，选择yes，
   4、接下来输入你在安装驱动时输入的密码，
   5、之后会跳到蓝色背景的界面perform mok management 选择第一个reboot。
   ```


4. Compile.

    ```bash
    make && sudo make install
    ```
    Now your mod is installed in `/lib/modules/$(uname -r)/kernel/drivers/hid/`

5. Sign mod.

   ```bash
   sudo kmodsign sha512 MOK.priv MOK.der /lib/modules/$(uname -r)/kernel/drivers/hid/hid-rapoo.ko
   ```
   deepin v20 无法找到也无法安装kmodsign。

6. Insert mod.

   ```bash
   sudo insmod /lib/modules/$(uname -r)/kernel/drivers/hid/hid-rapoo.ko
   ```

7. Launch mod.

   ```bash
   sudo modprobe hid-rapoo 
   ```

8. Now you can use your keyboard.

9. DEBUG: print kernel logs

   ```bash
   dmesg
   ```

# Notice

After installing new kernel(contains compile and install kernel),
you must reinstall hid-rapoo module again by "make install".