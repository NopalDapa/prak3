### Soal 6

> Dosen Budiman menginginkan sistem operasi yang **stylish**. Budiman memiliki ide untuk membuat sistem operasinya menjadi stylish. Ia meminta kamu untuk menambahkan tampilan sebuah banner yang ditampilkan setelah suatu user login ke dalam sistem operasi Budiman. Banner yang diinginkan Budiman adalah tulisan `"Welcome to OS'25"` dalam bentuk **ASCII Art**. Buatkanlah banner tersebut supaya Budiman senang! (Hint: gunakan text to ASCII Art Generator)

> _Budiman wants a **stylish** operating system. Budiman has an idea to make his OS stylish. He asks you to add a banner that appears after a user logs in. The banner should say `"Welcome to OS'25"` in **ASCII Art**. Use a text to ASCII Art generator to make Budiman happy!_ (Hint: use a text to ASCII Art generator)

**Answer:**

- **Code:**

  ```
      - cd osboot/myramdisk/etc
      - nano motd
        //isi motd//
        $$\      $$\           $$\                                                     $$\                      $$$$$$\   $$$$$$\  $$\  $$$$$$\  $$$$$$$\  
        $$ | $\  $$ |          $$ |                                                    $$ |                    $$  __$$\ $$  __$$\ $  |$$  __$$\ $$  ____| 
        $$ |$$$\ $$ | $$$$$$\  $$ | $$$$$$$\  $$$$$$\  $$$$$$\$$$$\   $$$$$$\        $$$$$$\    $$$$$$\        $$ /  $$ |$$ /  \__|\_/ \__/  $$ |$$ |      
        $$ $$ $$\$$ |$$  __$$\ $$ |$$  _____|$$  __$$\ $$  _$$  _$$\ $$  __$$\       \_$$  _|  $$  __$$\       $$ |  $$ |\$$$$$$\       $$$$$$  |$$$$$$$\  
        $$$$  _$$$$ |$$$$$$$$ |$$ |$$ /      $$ /  $$ |$$ / $$ / $$ |$$$$$$$$ |        $$ |    $$ /  $$ |      $$ |  $$ | \____$$\     $$  ____/ \_____$$\ 
        $$$  / \$$$ |$$   ____|$$ |$$ |      $$ |  $$ |$$ | $$ | $$ |$$   ____|        $$ |$$\ $$ |  $$ |      $$ |  $$ |$$\   $$ |    $$ |      $$\   $$ |
        $$  /   \$$ |\$$$$$$$\ $$ |\$$$$$$$\ \$$$$$$  |$$ | $$ | $$ |\$$$$$$$\         \$$$$  |\$$$$$$  |       $$$$$$  |\$$$$$$  |    $$$$$$$$\ \$$$$$$  |
        \__/     \__| \_______|\__| \_______| \______/ \__| \__| \__| \_______|         \____/  \______/        \______/  \______/     \________| \______/
        //
  ```

- **Explanation:**

  `buat file motd di direktori osboot/myramdisk/etc, lalu isi dengan ASCII ART yang ingin ditampilkan. lalu ASCII ART akan otomatis muncul ketika user sudah berhasil login`

- **Screenshot:**

  `put your answer here`

### Soal 7

> Melihat perkembangan sistem operasi milik Budiman, Dosen kagum dengan adanya banner yang telah kamu buat sebelumnya. Kemudian Dosen juga menginginkan sistem operasi Budiman untuk dapat menampilkan **kata sambutan** dengan menyebut nama user yang login. Sambutan yang dimaksud berupa kalimat `"Helloo %USER"` dengan `%USER` merupakan nama user yang sedang menggunakan sistem operasi. Kalimat sambutan ini ditampilkan setelah user login dan setelah banner. Budiman kembali lagi meminta bantuanmu dalam menambahkan fitur ini.

> _Seeing the progress of Budiman's OS, the lecturer is impressed with the banner you created. The lecturer also wants the OS to display a **greeting message** that includes the name of the user who logs in. The greeting should say `"Helloo %USER"` where `%USER` is the name of the user currently using the OS. This greeting should be displayed after user login and after the banner. Budiman asks for your help again to add this feature._

**Answer:**

- **Code:**

  ```
    - nano profile
      //isi etc/profile//
        #!/bin/sh
        echo "Helloo $USER"
        export PS1='\[\e[32m\]\u@\h:\w\$ \[\e[0m\]'
      //
  ```

- **Explanation:**

  ```- buat file profile di direktori osboot/myramdisk/etc.
     - #!/bin/sh adalah shebang. menunjukkan bahwa skrip ini akan dijalankan menggunakan shell interpreter /bin/sh.
     - echo "Helloo $USER". akan menampilkan user yang sedang login
     - export PS1='\[\e[32m\]\u@\h:\w\$ \[\e[0m\] . adalah string dengan kode escape ANSI untuk memberi warna dan format pada prompt.
  ```

- **Screenshot:**

  `put your answer here`
