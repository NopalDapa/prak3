[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/Eu-CByJh)
|    NRP     |      Name      |
| :--------: | :------------: |
| 5025221000 | Student 1 Name |
| 5025221000 | Student 2 Name |
| 5025221000 | Student 3 Name |

# Praktikum Modul 3 _(Module 3 Lab Work)_

### Laporan Resmi Praktikum Modul 3 _(Module 3 Lab Work Report)_

Di suatu pagi hari yang cerah, Budiman salah satu mahasiswa Informatika ditugaskan oleh dosennya untuk membuat suatu sistem operasi sederhana. Akan tetapi karena Budiman memiliki keterbatasan, Ia meminta tolong kepadamu untuk membantunya dalam mengerjakan tugasnya. Bantulah Budiman untuk membuat sistem operasi sederhana!

_One sunny morning, Budiman, an Informatics student, was assigned by his lecturer to create a simple operating system. However, due to Budiman's limitations, he asks for your help to assist him in completing his assignment. Help Budiman create a simple operating system!_

### Soal 1

> Sebelum membuat sistem operasi, Budiman diberitahu dosennya bahwa Ia harus melakukan beberapa tahap terlebih dahulu. Tahap-tahapan yang dimaksud adalah untuk **mempersiapkan seluruh prasyarat** dan **melakukan instalasi-instalasi** sebelum membuat sistem operasi. Lakukan seluruh tahapan prasyarat hingga [perintah ini](https://github.com/arsitektur-jaringan-komputer/Modul-Sisop/blob/master/Modul3/README-ID.md#:~:text=sudo%20apt%20install%20%2Dy%20busybox%2Dstatic) pada modul!

> _Before creating the OS, Budiman was informed by his lecturer that he must complete several steps first. The steps include **preparing all prerequisites** and **installing** before creating the OS. Complete all the prerequisite steps up to [this command](https://github.com/arsitektur-jaringan-komputer/Modul-Sisop/blob/master/Modul3/README-ID.md#:~:text=sudo%20apt%20install%20%2Dy%20busybox%2Dstatic) in the module!_

**Answer:**

- **Code:**

    ```
    - sudo apt -y update
     
    - sudo apt -y install qemu-system build-essential bison flex libelf-dev libssl-dev bc grub-common grub-pc libncurses-dev libssl-dev mtools grub-pc-bin xorriso tmux

    - mkdir -p osboot cd osboot

    - wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.1.tar.xz tar -xvf linux-6.1.1.tar.xz cd linux-6.1.1

    - make tinyconfig make menuconfig

    - make -j$(nproc)

    - cp arch/x86/boot/bzImage ..

    - sudo apt install -y busybox-static

    - whereis busybox
    ```

- **Explanation:**
    ```
    - sudo apt -y update : Perintah ini memperbarui daftar paket di sistem agar mendapatkan versi terbaru dari repositori.

    - sudo apt -y install qemu-system build-essential bison flex libelf-dev libssl-dev bc grub-common grub-pc libncurses-dev libssl-dev mtools grub-pc-bin xorriso tmux : Perintah ini menginstal semua paket yang dibutuhkan untuk membangun dan menjalankan kernel Linux, termasuk emulator, tools build, dan dependensi penting.

    - mkdir -p osboot cd osboot : Membuat direktori baru bernama “osboot” dan berpindah ke direktori tersebut.

    - wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.1.tar.xz tar -xvf linux-6.1.1.tar.xz cd linux-6.1.1 : Mengunduh source code kernel Linux versi 6.1.1, mengekstraknya, lalu masuk ke direktori hasil ekstrak.

    - make tinyconfig make menuconfig : Mengatur konfigurasi kernel ke mode minimal (tinyconfig) dan membuka menu konfigurasi interaktif untuk penyesuaian.

    - make -j$(nproc) : mencompile kernel menggunakan semua inti prosesor yang tersedia agar proses lebih cepat.

    - cp arch/x86/boot/bzImage .. : Menyalin file kernel hasil kompilasi (bzImage) ke direktori sebelumnya.

    - sudo apt install -y busybox-static : Menginstal BusyBox versi statis.

    - whereis busybox : Mencari lokasi file BusyBox yang sudah diinstal di sistem.
    ```

- **Screenshot:**
    ![alt text](<Screenshot from 2025-05-27 19-39-10.png>)
    ![alt text](<Screenshot from 2025-05-27 19-39-26.png>)
    ![alt text](<Screenshot from 2025-05-27 19-39-40.png>)
    ![alt text](<Screenshot from 2025-05-27 19-40-11.png>)
    ![alt text](<Screenshot from 2025-05-27 19-41-39.png>)
    ![alt text](<Screenshot from 2025-05-27 19-47-35.png>)
    ![alt text](<Screenshot from 2025-05-27 19-48-14.png>)

### Soal 2

> Setelah seluruh prasyarat siap, Budiman siap untuk membuat sistem operasinya. Dosen meminta untuk sistem operasi Budiman harus memiliki directory **bin, dev, proc, sys, tmp,** dan **sisop**. Lagi-lagi Budiman meminta bantuanmu. Bantulah Ia dalam membuat directory tersebut!

> _Once all prerequisites are ready, Budiman is ready to create his OS. The lecturer asks that the OS should contain the directories **bin, dev, proc, sys, tmp,** and **sisop**. Help Budiman create these directories!_

**Answer:**

- **Code:**

  ```
  - sudo bash

  - mkdir -p myramdisk/{bin,dev,proc,sys,etc,home/{root,Budiman,guest,praktikan1,praktikan2}}

  - cp -a /dev/null myramdisk/dev
    cp -a /dev/tty* myramdisk/dev
    cp -a /dev/zero myramdisk/dev
    cp -a /dev/console myramdisk/dev

  - cp /usr/bin/busybox myramdisk/bin
    cd myramdisk/bin
    ./busybox --install.
  ```

- **Explanation:**

  ```
    - sudo bash : Perintah ini menjalankan shell Bash dengan hak akses superuser (root), sehingga pengguna dapat menjalankan perintah dengan izin administratif.

    - mkdir -p myramdisk/{bin,dev,proc,sys,etc,root,home/root, Budiman, guest, praktikan1, praktikan2} : Perintah ini membuat sebuah direktori bernama myramdisk beserta beberapa subdirektori di dalamnya secara bersamaan. Subdirektori yang dibuat seperti bin, dev, proc, sys, etc, dan root, serta beberapa folder pengguna seperti home/root,Budiman,guest, praktikan1,danpraktikan2.

    - Perintah-perintah ini menyalin beberapa file perangkat penting dari sistem ke dalam folder myramdisk/dev, seperti file khusus untuk menangani input/output kosong, terminal, dan konsol. Setelah itu, program BusyBox yang berisi banyak perintah dasar Linux disalin ke dalam folder myramdisk/bin. Kemudian, dengan menjalankan BusyBox, berbagai perintah dasar otomatis dibuat sebagai shortcut di folder tersebut, sehingga bisa menggunakan banyak fungsi penting langsung dari sini. Jadi, ini seperti menyiapkan lingkungan mini Linux yang lengkap dengan alat-alat dasarnya.
    ```

- **Screenshot:**

    ![alt text](<Screenshot from 2025-05-27 19-53-25.png>)
    ![alt text](<Screenshot from 2025-05-27 19-55-28.png>)
    ![alt text](<Screenshot from 2025-05-27 19-56-03.png>)

### Soal 3

> Budiman lupa, Ia harus membuat sistem operasi ini dengan sistem **Multi User** sesuai permintaan Dosennya. Ia meminta kembali kepadamu untuk membantunya membuat beberapa user beserta directory tiap usernya dibawah directory `home`. Buat pula password tiap user-usernya dan aplikasikan dalam sistem operasi tersebut!

> _Budiman forgot that he needs to create a **Multi User** system as requested by the lecturer. He asks your help again to create several users and their corresponding home directories under the `home` directory. Also set each user's password and apply them in the OS!_

**Format:** `user:pass`

```
root:Iniroot
Budiman:PassBudi
guest:guest
praktikan1:praktikan1
praktikan2:praktikan2
```

**Answer:**

- **Code:**

  ```
    cd myramdisk/etc
    touch passwd

    openssl passwd -1 IniIniroot
    openssl passwd -1 PassBudi  
    openssl passwd -1 guest
    openssl passwd -1 praktikan1
    openssl passwd -1 praktikan2

    nano passwd
    // isi dari passwd//
    root:$1$vtfR2llK$9lm2KVJM04c3eV5mafn9N:0:0:root:/home/root:/bin/sh
    Budiman:$1$DoK/JCDs$WrXegqfSbGfEOaqAcbZaW1:1001:1001:Budiman:/home/Budiman:/bin/sh
    guest:$1$.GYzrWRn$MF2UQTM0jppDkiCiYRZX..:1002:1002:guest:/home/guest:/bin/sh
    praktikan1:$1$yOpbmybx$qRbo6JHcYdL5otBJC9ocs1:1003:1003:praktikan1:/home/praktikan1:/bin/sh
    praktikan2:$1$EMHe.z9s$RxWfgUHNuHAJtw/Sw1WE/1:1004:1004:praktikan2:/home/praktikan2:/bin/sh

    touch group

    nano group
    // isi dari group//
    root:x:0:
    bin:x:1:root
    sys:x:2:root
    tty:x:5:root,Budiman,guest,praktikan1,praktikan2
    disk:x:6:root
    wheel:x:10:root,Budiman,guest,praktikan1,praktikan2
    users:x:100:Budiman,guest,praktikan1,praktikan2

    cd ..
    touch init

    nano init
    // isi dari init//
    #!/bin/sh
    /bin/mount -t proc none /proc
    /bin/mount -t sysfs none /sys

    while true
    do
        /bin/getty -L tty1 115200 vt100
        sleep 1
    done

    chmod +x init

    find . | cpio -oHnewc | gzip > ../myramdisk.gz
    ```

- **Explanation:**

    ```
    Pertama, masuk ke folder myramdisk/etc dan membuat file kosong bernama passwd yang akan berisi data pengguna. Lalu, dengan menggunakan perintah openssl passwd -1, membuat password terenkripsi untuk beberapa pengguna seperti root, Budiman, guest, praktikan1, dan praktikan2. Password ini kemudian dimasukkan ke dalam file passwd bersama informasi penting seperti user ID, grup, direktori rumah, dan shell yang digunakan. Setelah itu, buat file group yang berisi daftar grup pengguna dan siapa saja anggotanya, seperti grup root, bin, sys, dan grup pengguna biasa. Selanjutnya, kembali ke direktori myramdisk dan membuat file init, yaitu sebuah skrip sederhana yang menjalankan mounting filesystem penting dan membuka terminal login agar pengguna bisa masuk ke sistem. File init ini kemudian dibuat bisa dijalankan dengan memberi izin eksekusi. Terakhir, seluruh isi folder myramdisk diubah menjadi sebuah file arsip terkompresi bernama myramdisk.gz yang nantinya bisa dipakai sebagai ramdisk saat sistem booting.
    ```

- **Screenshot:**

    ![alt text](<Screenshot from 2025-05-27 20-12-33.png>) 
    ![alt text](<Screenshot from 2025-05-27 19-57-58.png>) 
    ![alt text](<Screenshot from 2025-05-27 19-58-40.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-08-04.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-08-37.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-09-35.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-10-11.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-10-36.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-10-55.png>)

### Soal 4

> Dosen meminta Budiman membuat sistem operasi ini memilki **superuser** layaknya sistem operasi pada umumnya. User root yang sudah kamu buat sebelumnya akan digunakan sebagai superuser dalam sistem operasi milik Budiman. Superuser yang dimaksud adalah user dengan otoritas penuh yang dapat mengakses seluruhnya. Akan tetapi user lain tidak boleh memiliki otoritas yang sama. Dengan begitu user-user selain root tidak boleh mengakses `./root`. Buatlah sehingga tiap user selain superuser tidak dapat mengakses `./root`!

> _The lecturer requests that the OS must have a **superuser** just like other operating systems. The root user created earlier will serve as the superuser in Budiman's OS. The superuser should have full authority to access everything. However, other users should not have the same authority. Therefore, users other than root should not be able to access `./root`. Implement this so that non-superuser accounts cannot access `./root`!_

**Answer:**

- **Code:**

    ```
    chown 0:0 home/root
    chmod 700 home/root
    ```

- **Explanation:**

    ```
    Perintah chown 0:0 home/root mengubah kepemilikan direktori home/root menjadi milik user dan grup dengan ID 0, yaitu user dan grup root. Ini memastikan bahwa direktori tersebut secara resmi dimiliki oleh superuser. Selanjutnya, perintah chmod 700 home/root mengatur hak akses direktori tersebut sehingga hanya pemiliknya, yaitu root, yang dapat membaca, menulis, dan memasuki direktori itu. Dengan pengaturan ini, user selain root tidak memiliki izin apapun untuk mengakses home/root, sehingga menjaga keamanan dan privasi direktori superuser sesuai dengan yang diinginkan.
    ```
- **Screenshot:**

    ![alt text](<Screenshot from 2025-05-25 23-11-06.png>) 
    ![alt text](<Screenshot from 2025-05-27 20-59-30.png>)

### Soal 5

> Setiap user rencananya akan digunakan oleh satu orang tertentu. **Privasi dan otoritas tiap user** merupakan hal penting. Oleh karena itu, Budiman ingin membuat setiap user hanya bisa mengakses dirinya sendiri dan tidak bisa mengakses user lain. Buatlah sehingga sistem operasi Budiman dalam melakukan hal tersebut!

> _Each user is intended for an individual. **Privacy and authority** for each user are important. Therefore, Budiman wants to ensure that each user can only access their own files and not those of others. Implement this in Budiman's OS!_

**Answer:**

- **Code:**

    ```
    chown 1001:1001 home/Budiman
    chown 1002:1002 home/guest
    chown 1003:1003 home/praktikan1
    chown 1004:1004 home/raktikan2
    chmod 700 home/Budiman
    chmod 700 home/guest
    chmod 700 home/praktikan1
    chmod 700 home/praktikan2
    ```

- **Explanation:**

    ```
    Perintah-perintah tersebut mengatur kepemilikan dan hak akses pada direktori home beberapa pengguna. Dengan chown, direktori home/Budiman, home/guest, home/praktikan1, dan home/praktikan2 masing-masing diberikan kepemilikan oleh user dan grup dengan ID yang sesuai, memastikan bahwa setiap direktori dimiliki oleh pengguna yang tepat. Selanjutnya, perintah chmod 700 diterapkan pada setiap direktori tersebut, yang berarti hanya pemilik direktori yang memiliki izin penuh untuk membaca, menulis, dan mengakses isi direktori, sementara pengguna lain tidak memiliki akses sama sekali. Pengaturan ini menjaga privasi dan keamanan data masing-masing pengguna dengan membatasi akses hanya untuk pemiliknya.
    ```

-**Screenshot:**

  ![alt text](<Screenshot from 2025-05-25 23-12-52.png>) 
  ![alt text](<Screenshot from 2025-05-25 21-40-07.png>)

### Soal 6

> Dosen Budiman menginginkan sistem operasi yang **stylish**. Budiman memiliki ide untuk membuat sistem operasinya menjadi stylish. Ia meminta kamu untuk menambahkan tampilan sebuah banner yang ditampilkan setelah suatu user login ke dalam sistem operasi Budiman. Banner yang diinginkan Budiman adalah tulisan `"Welcome to OS'25"` dalam bentuk **ASCII Art**. Buatkanlah banner tersebut supaya Budiman senang! (Hint: gunakan text to ASCII Art Generator)

> _Budiman wants a **stylish** operating system. Budiman has an idea to make his OS stylish. He asks you to add a banner that appears after a user logs in. The banner should say `"Welcome to OS'25"` in **ASCII Art**. Use a text to ASCII Art generator to make Budiman happy!_ (Hint: use a text to ASCII Art generator)

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 7

> Melihat perkembangan sistem operasi milik Budiman, Dosen kagum dengan adanya banner yang telah kamu buat sebelumnya. Kemudian Dosen juga menginginkan sistem operasi Budiman untuk dapat menampilkan **kata sambutan** dengan menyebut nama user yang login. Sambutan yang dimaksud berupa kalimat `"Helloo %USER"` dengan `%USER` merupakan nama user yang sedang menggunakan sistem operasi. Kalimat sambutan ini ditampilkan setelah user login dan setelah banner. Budiman kembali lagi meminta bantuanmu dalam menambahkan fitur ini.

> _Seeing the progress of Budiman's OS, the lecturer is impressed with the banner you created. The lecturer also wants the OS to display a **greeting message** that includes the name of the user who logs in. The greeting should say `"Helloo %USER"` where `%USER` is the name of the user currently using the OS. This greeting should be displayed after user login and after the banner. Budiman asks for your help again to add this feature._

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 8

> Dosen Budiman sudah tua sekali, sehingga beliau memiliki kesulitan untuk melihat tampilan terminal default. Budiman menginisiatif untuk membuat tampilan sistem operasi menjadi seperti terminal milikmu. Modifikasilah sistem operasi Budiman menjadi menggunakan tampilan terminal kalian.

> _Budiman's lecturer is quite old and has difficulty seeing the default terminal display. Budiman takes the initiative to make the OS look like your terminal. Modify Budiman's OS to use your terminal appearance!_

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 9

> Ketika mencoba sistem operasi buatanmu, Budiman tidak bisa mengubah text file menggunakan text editor. Budiman pun menyadari bahwa dalam sistem operasi yang kamu buat tidak memiliki text editor. Budimanpun menyuruhmu untuk menambahkan **binary** yang telah disiapkan sebelumnya ke dalam sistem operasinya. Buatlah sehingga sistem operasi Budiman memiliki **binary text editor** yang telah disiapkan!

> _When trying your OS, Budiman cannot edit text files using a text editor. He realizes that the OS you created does not have a text editor. Budiman asks you to add the prepared **binary** into his OS. Make sure Budiman's OS has the prepared **text editor binary**!_

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 10

> Setelah seluruh fitur yang diminta Dosen dipenuhi dalam sistem operasi Budiman, sudah waktunya Budiman mengumpulkan tugasnya ini ke Dosen. Akan tetapi, Dosen Budiman tidak mau menerima pengumpulan selain dalam bentuk **.iso**. Untuk terakhir kalinya, Budiman meminta tolong kepadamu untuk mengubah seluruh konfigurasi sistem operasi yang telah kamu buat menjadi sebuah **file .iso**.

> After all the features requested by the lecturer have been implemented in Budiman's OS, it's time for Budiman to submit his assignment. However, Budiman's lecturer only accepts submissions in the form of **.iso** files. For the last time, Budiman asks for your help to convert the entire configuration of the OS you created into a **.iso file**.

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

---

Pada akhirnya sistem operasi Budiman yang telah kamu buat dengan susah payah dikumpulkan ke Dosen mengatasnamakan Budiman. Kamu tidak diberikan credit apapun. Budiman pun tidak memberikan kata terimakasih kepadamu. Kamupun kecewa tetapi setidaknya kamu telah belajar untuk menjadi pembuat sistem operasi sederhana yang andal. Selamat!

_At last, the OS you painstakingly created was submitted to the lecturer under Budiman's name. You received no credit. Budiman didn't even thank you. You feel disappointed, but at least you've learned to become a reliable creator of simple operating systems. Congratulations!_
