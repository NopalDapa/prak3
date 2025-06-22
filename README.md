[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/V7fOtAk7)
|    NRP     |      Name      |
| :--------: | :------------: |
| 5025221000 | Student 1 Name |
| 5025221000 | Student 2 Name |
| 5025221000 | Student 3 Name |

# Praktikum Modul 4 _(Module 4 Lab Work)_

</div>

### Daftar Soal _(Task List)_

- [Task 1 - FUSecure](/task-1/)

- [Task 2 - LawakFS++](/task-2/)

- [Task 3 - Drama Troll](/task-3/)

- [Task 4 - LilHabOS](/task-4/)

### Laporan Resmi Praktikum Modul 4 _(Module 4 Lab Work Report)_

### Cara Pengerjaan Task 3

## Setup awal
```
#define FUSE_USE_VERSION 28
#include <fuse.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <dirent.h>
#include <errno.h>
#include <sys/time.h>
#include <libgen.h>
#include <time.h>
#include <stdlib.h>

#define MAX_FILTER 32

char *filter_words[MAX_FILTER];
int filter_count = 0;

char secret_file_basename[256];
int access_start_hour;
int access_end_hour;

static  const  char *dirpath = "/home/nfl/sisop/prak4";
...
...
static int xmp_mkdir(const char *path, mode_t mode) {
    return -EROFS;
}

static int xmp_rmdir(const char *path) {
    return -EROFS;
}

static int xmp_unlink(const char *path) {
    return -EROFS;
}

static int xmp_rename(const char *from, const char *to) {
    return -EROFS;
}

static int xmp_write(const char *path, const char *buf, size_t size,off_t offset, struct fuse_file_info *fi)
{
    return -EROFS;
}

static int xmp_create(const char *path, mode_t mode, struct fuse_file_info *fi) {
    return -EROFS;
}

static int xmp_truncate(const char *path, off_t size) {
    return -EROFS;
}

static struct fuse_operations xmp_oper = {
    .getattr = xmp_getattr,
    .readdir = xmp_readdir,
    .read = xmp_read,
    .access = xmp_access,
    .open = xmp_open,
    .mkdir = xmp_mkdir,
    .rmdir = xmp_rmdir,
    .unlink = xmp_unlink,
    .rename = xmp_rename,
    .write = xmp_write,
    .create = xmp_create,
    .truncate = xmp_truncate,
};

int main(int argc, char *argv[]) {
    load_config("lawak.conf");
    umask(0);
    return fuse_main(argc, argv, &xmp_oper, NULL);
}

```
Penjelasan :
- define semua library yang dibutuhkan
- declare variabel global agar bisa diakses di semua function
- "static  const  char *dirpath = "/home/nfl/sisop/prak4", mendefine folder yang ingin dimount
- static int yang return -EROFS. agar fuse hanya benar benar read only
- static struct fuse_operations xmp_oper ; untuk define operation apa aja yang digunakan
- int main untuk load lalu return fuse main
- 
## Ekstensi File Tersembunyi
```
static void remove_extension(char *filename) {
    char *dot = strrchr(filename, '.');
    if (dot != NULL && dot != filename) {
        *dot = '\0';
    }
}
```
Penjelasan:
- strrchr; untuk mencari char '.' terakhir dari filename
- jika ketemu ubah '.' menjadi NULL atau '\0'

Lalu ubah fungsi xmp_readdir untuk menampilkan
```
static int xmp_readdir(const char *path, void *buf, fuse_fill_dir_t filler, off_t offset, struct fuse_file_info *fi)
{
    char fpath[1000];
    if(strcmp(path,"/") == 0)
    {
        path=dirpath;
        sprintf(fpath,"%s",path);
    } else sprintf(fpath, "%s%s",dirpath,path);
    int res = 0;
    DIR *dp;
    struct dirent *de;
    (void) offset;
    (void) fi;
    dp = opendir(fpath);
    if (dp == NULL) return -errno;
    while ((de = readdir(dp)) != NULL) {
        struct stat st;
        char display_name[256];
        memset(&st, 0, sizeof(st));
        st.st_ino = de->d_ino;
        st.st_mode = de->d_type << 12;
        strcpy(display_name, de->d_name);
        remove_extension(display_name);
        res = (filler(buf, display_name, &st, 0));
        if(res!=0) break;
    }
    closedir(dp);
    write_log("READDIR", path);
    return 0;
}
```
Penjelasan:
- Deklarasi fungsi xmp_readdir yang digunakan oleh FUSE untuk membaca isi direktori.
- Menggabungkan path FUSE dengan dirpath untuk menghasilkan path absolut ke direktori di filesystem nyata.
- Melakukan iterasi ke setiap entri dalam direktori menggunakan readdir lalu simpan di variabel sementara
- Panggil fungsi remove_extension
- Memanggil fungsi filler untuk memasukkan hasil remove_extensiom ke dalam buffer direktori yang dikembalikan ke FUSE.
- Menutup direktori setelah selesai dibaca.
- Mencatat aktivitas pembacaan direktori dengan memanggil write_log dengan parameter "READDIR" dan path.
- Fungsi ini hanya mengembalikan visual saja tanpa extension. ketika di cat dengan extension otomatis mengarah ke file yang dimaksud

## Output Ekstensi File Tersembunyi

## Kendala yang Dialami Ekstensi File Tersembunyi
Tidak ada

## Akses Berbasis Waktu untuk File Secret
Function untuk cek apakah file/folder ini secret
```
static int is_secret_file(const char *path) {
    char *path_copy = strdup(path);
    char *filename = basename(path_copy);
    char no_ext_filename[256];
    strncpy(no_ext_filename, filename, sizeof(no_ext_filename));
    remove_extension(no_ext_filename);
    int result = (strcmp(no_ext_filename, secret_file_basename) == 0);
    free(path_copy);
    return result;
}
```
Penjelasan:
- Deklarasi fungsi is_secret_file yang mengembalikan nilai integer (1 atau 0) dan menerima parameter path file.
- Membuat salinan dari path menggunakan strdup(ambil hanya basename)
- remove extensionnya lalu bandingkan dengan "nama yang dianggap secret"
- jika sama return 1(true) , jika beda return 0(false)
- free agar tidak terjadi mem leak

Function untuk cek apakah masuk waktu akses
```
static int is_time_allowed(void) {
    time_t now = time(NULL);
    struct tm *tm_info = localtime(&now);
    return (tm_info->tm_hour >= access_start_hour && tm_info->tm_hour < access_end_hour);
}
```
Penjelasan :
- Deklarasi fungsi yang mengembalikan 1 jika true, 0 jika false
- mendapatkan waktu sekarang dan dibandingkan dengan jam akses
- jika masuk return 1 jika diluar return 0

Batasi akses
```
static int xmp_access(const char *path, int mask)
{
    if (is_secret_file(path) && !is_time_allowed()) {
        return -ENOENT;
    }
    char fpath[1000];
    sprintf(fpath, "%s%s", dirpath, path);
    int res = access(fpath, mask);
    if (res == -1) return -errno;
    write_log("ACCESS", path);
    return 0;
}


static  int  xmp_getattr(const char *path, struct stat *stbuf)
{
    char fpath[1000];
    sprintf(fpath, "%s%s", dirpath, path);
    int res = lstat(fpath, stbuf);
    if (res == -1) return -errno;
    if (is_secret_file(path) && !is_time_allowed()) {
        return -ENOENT;
    }
    write_log("GETATTR", path);
    return 0;
}

}
```
Penjelasan:
- Mengecek apakah file merupakan file rahasia (is_secret_file) dan waktu saat ini tidak diperbolehkan (!is_time_allowed). Jika ya, maka seolah-olah file tidak ada dengan langsung return mengembalikan -ENOENT.
- Jika tidak, lanjutkan proses acces/getattr
- Tulis log dengan format ("kegiatan", path)
- agar benar benar tidak dapat diakses. tambahkan juga
  '''    if (is_secret_file(path) && !is_time_allowed()) {
        return -ENOENT;
    }
  '''
  pada xmp_read dan xmp_open

## Output Akses Berbasis Waktu untuk File Secret
![alt text](<WhatsApp Image 2025-06-21 at 05.36.29_5b1dc6a3.jpg>)


## Kendala Akses Berbasis Waktu untuk File Secret
Tidak ada

## Filtering Konten Dinamis
Fungsi untuk cek file format bin/text
```
static int is_text(const char *data, int len) {
    for (int i = 0; i < len; i++) {
        if ((unsigned char)data[i] < 0x09 || ((unsigned char)data[i] > 0x0D && (unsigned char)data[i] < 0x20 && data[i] != '\t')) {
            return 0;
        }
    }
    return 1;
}
```
Penjelasan:
- Fungsi untuk mendeteksi apakah file berisi format text atau bin
- Untuk setiap karakter, dilakukan pengecekan apakah karakter tersebut berada di luar rentang karakter teks yang dianggap valid:
    * Karakter dengan nilai ASCII lebih kecil dari 0x09 (tabulasi horizontal).
    * Karakter di antara 0x0E dan 0x1F (kontrol yang tidak biasa), kecuali tab (\t).
-Jika ditemukan karakter yang tidak valid sebagai teks, maka fungsi segera mengembalikan 0 (bukan teks).

Fungsi untuk mengubah kata kata disensor menjadi "lawak"
```
static void filter_lawak(char *content, const char **lawak_words, int lawak_count) {
    for (int i = 0; i < lawak_count; ++i) {
        size_t orig_len = strlen(lawak_words[i]);
        const char *replacement = "lawak";
        size_t rep_len  = strlen(replacement);
        char *search_from = content;
        while (1) {
            char *pos = strcasestr(search_from, lawak_words[i]);
            if (!pos) break;
            size_t tail_len = strlen(pos + orig_len);
            if (rep_len != orig_len) {
                memmove(pos + rep_len,pos + orig_len,tail_len + 1);
            }
            memcpy(pos, replacement, rep_len);
            search_from = pos + rep_len;
        }
    }
}
```
Penjelasan :
- Melakukan iterasi untuk setiap kata yang disensor.
- Menggunakan strcasestr untuk mencari kemunculan kata (dengan pencocokan tidak case-sensitive) dalam content.
- Jika panjang kata berbeda, gunakan memmove agar panjang tetap valid
- Menyalin string "lawak" ke posisi kata yang ditemukan menggunakan memcpy.
- Lanjutkan sampai selesai

Fungsi untuk mengubah file biner menjadi format encodingbase64
```
char *base64_encode(const unsigned char *data, size_t input_length, size_t *output_length) {
    static const char encoding_table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
    static const int mod_table[] = {0, 2, 1};
    *output_length = 4 * ((input_length + 2) / 3);
    char *encoded_data = malloc(*output_length + 1);
    if (encoded_data == NULL) return NULL;

    for (size_t i = 0, j = 0; i < input_length;) {
        uint32_t octet_a = i < input_length ? (unsigned char)data[i++] : 0;
        uint32_t octet_b = i < input_length ? (unsigned char)data[i++] : 0;
        uint32_t octet_c = i < input_length ? (unsigned char)data[i++] : 0;
        uint32_t triple = (octet_a << 16) | (octet_b << 8) | octet_c;
        encoded_data[j++] = encoding_table[(triple >> 18) & 0x3F];
        encoded_data[j++] = encoding_table[(triple >> 12) & 0x3F];
        encoded_data[j++] = encoding_table[(triple >> 6) & 0x3F];
        encoded_data[j++] = encoding_table[triple & 0x3F];
    }
    for (int i = 0; i < mod_table[input_length % 3]; i++)
        encoded_data[*output_length - 1 - i] = '=';
    encoded_data[*output_length] = '\0';
    return encoded_data;
}

```
Penjelasan :
- Deklarasi fungsi base64_encode yang mengembalikan string hasil encoding dalam format Base64, menerima:
    * data: pointer ke data biner yang akan dienkode.
    * input_length: panjang data input.
    * output_length: pointer untuk menyimpan panjang hasil encoding.
- Menambahkan null-terminator di akhir string hasil encoding.
- Mengembalikan pointer ke string hasil encoding Base64.

Panggil melalui xmp_read
```
static int xmp_read(const char *path, char *buf, size_t size, off_t offset,struct fuse_file_info *fi)
{
    if (is_secret_file(path) && !is_time_allowed()) return -ENOENT;
    int fd = fi->fh;
    if (fd < 0) return -EBADF;
    struct stat st;
    if (fstat(fd, &st) < 0) return -errno;
    size_t full_len = st.st_size;
    unsigned char *full_buf = malloc(full_len + 1);
    if (!full_buf) return -ENOMEM;
    if (pread(fd, full_buf, full_len, 0) != (ssize_t)full_len) {
        free(full_buf); return -errno;
    }
    full_buf[full_len] = '\0';
    int teks = is_text((char*)full_buf, full_len);
    char *out_buf; size_t out_len;
    if (teks) {
        filter_lawak((char*)full_buf, (const char**)filter_words, filter_count);
        out_buf = (char*)full_buf;
        out_len = strlen((char*)full_buf);
    } else {
        out_buf = base64_encode(full_buf, full_len, &out_len);
        free(full_buf);
        if (!out_buf) return -ENOMEM;
    }
    size_t retlen = 0;
    if (offset == 0) {
        retlen = (out_len < size) ? out_len : size;
        memcpy(buf, out_buf, retlen);
    }
    write_log("READ", path);
    free(out_buf);
    return retlen;
}
```
Penjelasan:
- Deklarasi fungsi xmp_read yang bertugas membaca isi file dan digunakan oleh FUSE saat file dibuka dan dibaca.
- Jika file rahasia dan diluar waktu return -ENOENT agar file dianggap tidak ada.
- Mengambil file descriptor (fd) dari objek fuse_file_info. Jika tidak valid (fd < 0), mengembalikan error -EBADF.
- Mengambil metadata file menggunakan fstat. Jika gagal, mengembalikan error dari errno.
- Mengalokasikan buffer full_buf untuk membaca seluruh isi file dengan pread, ditambah 1 byte untuk null-terminator.
- Mengecek apakah isi file adalah teks dengan fungsi is_text.
    Jika teks: memanggil filter_lawak untuk mengubah kata sensor menjadi "lawak".
    Jika biner/non-teks: memanggil fungsi base64_encode melakukan encoding ke Base64.
- Tulis log dengan format ("READ", path)
- Membebaskan out_buf setelah digunakan.
- Mengembalikan jumlah byte (retlen) yang berhasil dibaca dan ditulis ke buffer FUSE.

  
## Output Filtering Konten Dinamis

## Kendala yang Dialami Filtering Konten Dinamis
Tidak ada

## Logging Akses
Buat struct dan declare var global
```
typedef struct {
    char action[16];
    char path[256];
    struct timeval last_time;
} LogEntry;

static LogEntry log_entries[32];
static int log_count = 0;

```
Penjelasan:
- Mendefinisikan tipe data LogEntry berupa struct yang merepresentasikan satu entri log aktivitas file.

Fungsi Write log
```
static void write_log(const char *action, const char *path) {
    struct timeval now;
    gettimeofday(&now, NULL);
    for (int i = 0; i < log_count; i++) {
        if (strcmp(log_entries[i].action, action) == 0 &&
            strcmp(log_entries[i].path, path) == 0) {
            long diff_ms = (now.tv_sec - log_entries[i].last_time.tv_sec) * 1000L + (now.tv_usec - log_entries[i].last_time.tv_usec) / 1000L;
            if (diff_ms < 200)return; 
            log_entries[i].last_time = now;
            goto write_to_file;
        }
    }
    if (log_count < 32) {
        strncpy(log_entries[log_count].action, action, sizeof(log_entries[log_count].action) - 1);
        strncpy(log_entries[log_count].path, path, sizeof(log_entries[log_count].path) - 1);
        log_entries[log_count].last_time = now;
        log_count++;
    }
write_to_file:
    FILE *log_file = fopen("/var/log/lawakfs.log", "a");
    if (!log_file) return;
    time_t now_sec = time(NULL);
    struct tm *t = localtime(&now_sec);
    uid_t uid = getuid();
    char timestr[100];
    strftime(timestr, sizeof(timestr), "%Y-%m-%d %H:%M:%S", t);
    fprintf(log_file, "[%s] [%d] [%s] [%s]\n", timestr, uid, action, path);
    fclose(log_file);
}
```
Penjelasan :
- Fungsi write_log digunakan untuk mencatat aktivitas file (seperti READ, ACCESS, GETATTR) ke file log /var/log/lawakfs.log.
- Fungsi ini mencegah pencatatan berulang dalam waktu kurang dari 200 milidetik untuk aksi dan path yang sama.
- Jika aktivitas belum pernah dicatat atau cukup lama dari log sebelumnya, maka aktivitas disimpan ke memori dan ditulis ke file log.
- Log berisi sesuai format [YYYY-MM-DD HH:MM:SS] [UID] [ACTION] [PATH]

## Output Logging Akses

## Kendala yang Dialami Logging Akses
Tidak ada


## Konfigurasi
Buat file lawak.conf
```
FILTER_WORDS=ducati,ferrari,mu,chelsea,prx,onic,sisop
SECRET_FILE_BASENAME=secret
ACCESS_START=11:00
ACCESS_END=17:00
```
Penjelasan:
- sesuai di contoh soal

Variabel untuk menyimpan data hasil read
```
char secret_file_basename[256];
int access_start_hour;
int access_end_hour;
```
Penjelasan :
- Deklarasi variable untuk menyimpan data hasil baca dari lawak.conf

Fungsi untuk load config
```
void load_config(const char *config_path) {
    FILE *fp = fopen(config_path, "r");
    if (!fp) {
        perror("Gagal membuka lawak.conf");
        exit(EXIT_FAILURE);
    }
    char line[512];
    while (fgets(line, sizeof(line), fp)) {
        char *eq = strchr(line, '=');
        if (!eq) continue;

        *eq = '\0';
        char *key = line;
        char *value = eq + 1;
        value[strcspn(value, "\r\n")] = '\0';

        if (strcmp(key, "FILTER_WORDS") == 0) {
            char *token = strtok(value, ",");
            while (token && filter_count < MAX_FILTER) {
                filter_words[filter_count++] = strdup(token);
                token = strtok(NULL, ",");
            }
        } else if (strcmp(key, "SECRET_FILE_BASENAME") == 0) {
            strncpy(secret_file_basename, value, sizeof(secret_file_basename));
        } else if (strcmp(key, "ACCESS_START") == 0) {
            sscanf(value, "%d", &access_start_hour);
        } else if (strcmp(key, "ACCESS_END") == 0) {
            sscanf(value, "%d", &access_end_hour);
        }
    }

    fclose(fp);
}
```
Penjelasan:
- Fungsi load_config digunakan untuk membaca konfigurasi dari file (misalnya lawak.conf).
- Membuka file konfigurasi, jika gagal maka program akan keluar dengan pesan error.
- Membaca setiap baris file dan memproses baris yang mengandung tanda =, lalu memisahkan antara key dan value.
- Jika key adalah FILTER_WORDS, maka value dipisah dengan koma dan setiap kata dimasukkan ke array filter_words.
- Jika key adalah SECRET_FILE_BASENAME, maka nilainya disalin ke variabel secret_file_basename.
- Jika key adalah ACCESS_START atau ACCESS_END, maka nilainya dikonversi menjadi integer dan disimpan ke variabel access_start_hour dan access_end_hour.
- Setelah semua konfigurasi dibaca, file ditutup.

Panggil di main
```
int main(int argc, char *argv[]) {
    load_config("lawak.conf");
    umask(0);
    return fuse_main(argc, argv, &xmp_oper, NULL);
}
```
Penjelasan:
- untuk menjalankan load_config di awal program
## Output Konfigurasi

## Kendala yang Dialami Konfigurasi
Tidak ada

