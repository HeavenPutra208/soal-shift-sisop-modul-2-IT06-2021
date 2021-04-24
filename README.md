# soal-shift-sisop-modul-2-IT06-2021
Penyelesaian Soal Shift 2 Sistem Operasi 2021\
Kelompok IT06
* Alessandro Tionardo (05311940000018)
* Heaven Happyna P F (0531940000026)
* Zuhairaja M T (05311940000033)

---
## Daftar Isi
* [Soal 1](#soal-1)
  * [Soal 1.a.](#soal-1a)
  * [Soal 1.b.](#soal-1b)
  * [Soal 1.c.](#soal-1c)
  * [Soal 1.d.](#soal-1d)
  * [Soal 1.e.](#soal-1e)
  * [Soal 1.f.](#soal-1f)
* [Soal 2](#soal-2)
  * [Soal 2.a.](#soal-2a)
  * [Soal 2.b.](#soal-2b)
  * [Soal 2.c.](#soal-2c)
  * [Soal 2.d.](#soal-2d)
  * [Soal 2.e.](#soal-2e)
* [Soal 3](#soal-3)
  * [Soal 3.a.](#soal-3a)
  * [Soal 3.b.](#soal-3b)
  * [Soal 3.c.](#soal-3c)
  * [Soal 3.d.](#soal-3d)
  * [Soal 3.e.](#soal-3e)
---

## Soal 1
Pada suatu masa, hiduplah seorang Steven yang hidupnya pas-pasan. Steven punya pacar, namun sudah putus sebelum pacaran. Ketika dia galau memikirkan mantan, ia selalu menonton https://www.youtube.com/watch?v=568DH_9CMKI untuk menghilangkan kesedihannya. 

Di lain hal Steven anak yang tidak amat sangat super membenci matkul sisop, beberapa jam setelah diputus oleh pacarnya dia menemukan wanita lain bernama Stevany, namun Stevany berkebalikan dengan Steven karena menyukai sisop. Steven ingin terlihat jago matkul sisop demi menarik perhatian Stevany.

Pada hari ulang tahun Stevany, Steven ingin memberikan Stevany zip berisikan hal-hal yang disukai Stevany. Steven ingin isi zipnya menjadi rapi dengan membuat folder masing-masing sesuai extensi.
### Soal 1.a
Dikarenakan Stevany sangat menyukai huruf Y, Steven ingin nama folder-foldernya adalah Musyik untuk mp3, Fylm untuk mp4, dan Pyoto untuk jpg.

**Penyelesaian**\
Mula-mula, kami menentukan library yang kami gunakan, yaitu sebagai berikut:
```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <errno.h>
#include <unistd.h>
#include <syslog.h>
#include <string.h>
#include <time.h>
#include <wait.h>
```

Kemudian, kami membuat ketiga folder yang diinginkan, yaitu `Musyik`, `Fylm`, dan `Pyoto` berupa `child process`:
```c
  if (child_id == 0) {
    char *argv[] = {"mkdir", "Musyik", "Fylm", "Pyoto",  NULL};
    execv("/bin/mkdir", argv);
  }
```
### Soal 1.b
untuk musik Steven mendownloadnya dari link di bawah, film dari link di bawah lagi, dan foto dari link dibawah juga

**Penyelesaian**\
Setelah ketiga folder dibuat pada soal 1a, kemudian kami membuat code untuk mendownload zip yang disediakan pada gdrive dengan bantuan pointer arr `*argv[]` yang menggunakan format downloading yang disediakan (`wget --no-check-certificate "https://drive.google.com/uc?id=**ID-FILE**&export=download" -O **Nama_untuk_filenya.ext**`). Agar satu-persatu, maka untuk file foto dan film diberikan fungsi wait `((wait(&status)) > 0)`

```c
child_id = fork();
    if (child_id == 0) {
      char *argv[] = {"Wget --no-check-certificate ", "https://drive.google.com/uc?id=1ZG8nRBRPquhYXq_sISdsVcXx5VdEgi-J&export=download", "-O", "Musik_for_Stevany.zip",  NULL};
      execv("/usr/bin/wget", argv);
    }

	child_id = fork();
	if (child_id == 0) {
	  while ((wait(&status)) > 0);
      char *argv[] = {"Wget --no-check-certificate ", "https://drive.google.com/uc?id=1FsrAzb9B5ixooGUs0dGiBr-rC7TS9wTD&export=download", "-O", "Foto_for_Stevany.zip",  NULL};
      execv("/usr/bin/wget", argv);
    }

	child_id = fork();
	if (child_id == 0) {
	  while ((wait(&status)) > 0);
      char *argv[] = {"Wget --no-check-certificate ", "https://drive.google.com/uc?id=1ktjGgDkL0nNpY-vT7rT7O6ZI47Ke9xcp&export=download", "-O", "Film_for_Stevany.zip",  NULL};
      execv("/usr/bin/wget", argv);
    }
```

### Soal 1.c
Steven tidak ingin isi folder yang dibuatnya berisikan zip, sehingga perlu meng-extract-nya setelah didownload

**Penyelesaian**\
Untuk mengekstrak file zip yang telah didownload sebelumnya, maka di sini digunakan bantuan pointer arr `*argv[]` dengan perintah `unzip`:

```c
while(wait(NULL) > 0);
    child_id = fork();
    if (child_id == 0) {
      char *argv[] = {"unzip", "*.zip", NULL};
      execv("/usr/bin/unzip", argv);
    }
```

### Soal 1.d
memindahkannya ke dalam folder yang telah dibuat (hanya file yang dimasukkan).

**Penyelesaian**\
Setelah file diunzip, kemudian untuk file dalam folder **FOTO** yang memiliki format selain **.jpg** akan dihapus:

```c
    while(wait(NULL) > 0);
    child_id = fork();
    if (child_id == 0) {

    glob_t globbuf;
    globbuf.gl_offs = 2;
    glob("FOTO/*.png", GLOB_DOOFFS, NULL, &globbuf);
    globbuf.gl_pathv[0] = "rm";
    globbuf.gl_pathv[1] = "-rf";
    execv("/bin/rm", globbuf.gl_pathv);
    }  

    while(wait(NULL) > 0);
    child_id = fork();
    if (child_id == 0) {

    glob_t globbuf;
    globbuf.gl_offs = 2;
    glob("FOTO/*.jpeg", GLOB_DOOFFS, NULL, &globbuf);
    globbuf.gl_pathv[0] = "rm";
    globbuf.gl_pathv[1] = "-rf";
    execv("/bin/rm", globbuf.gl_pathv);
    }  
```

Kemudian, isi file ketiga folder dimasukkan ke dalam masing-masing folder sesuai bentuk filenya:

```c
  	while(wait(NULL) > 0);
    child_id = fork();
    	if (child_id == 0) {
    	char *argv[] = {"mv", "MUSIK", "Musyik", NULL};
    	execv("/bin/mv", argv);
    }
	
	while(wait(NULL) > 0);
    child_id = fork();
    	if (child_id == 0) {
    	char *argv[] = {"mv", "FOTO", "Pyoto", NULL};
    	execv("/bin/mv", argv);
    }
	
	while(wait(NULL) > 0);
    child_id = fork();
    	if (child_id == 0) {
    	char *argv[] = {"mv", "FILM", "Fylm", NULL};
    	execv("/bin/mv", argv);
    }
```

### Soal 1.e
Untuk memudahkan Steven, ia ingin semua hal di atas berjalan otomatis 6 jam sebelum waktu ulang tahun Stevany).

**Penyelesaian**\
Di sini kami menggunakan fungsi `localtime(&t)` untuk mendapatkan waktu sekarang, lalu kami menggunakan fungsi `strcmp` untuk mengecek apakah waktu sekarang sama dengan waktu 6 jam sebelum ulang tahun Stevany.

```c
    while (1) {

        time_t t = time(NULL);
        struct tm *tm = localtime(&t);
        char now[80];
        strftime(now, 80, "%Y-%m-%d %H:%M:%S", tm);
        if(strcmp(now,"2021-04-09 16:22:00") == 0)
```

### Soal 1.f
Setelah itu pada waktu ulang tahunnya Stevany, semua folder akan di zip dengan nama Lopyu_Stevany.zip dan semua folder akan di delete(sehingga hanya menyisakan .zip).

**Penyelesaian**\
Di sini kami menggunakan fungsi `localtime(&t)` untuk mendapatkan waktu sekarang, lalu kami menggunakan fungsi `strcmp` untuk mengecek apakah waktu sekarang sama dengan waktu  ulang tahun Stevany.

```c
    while (1) {

        time_t t = time(NULL);
        struct tm *tm = localtime(&t);
        char now[80];
        strftime(now, 80, "%Y-%m-%d %H:%M:%S", tm);
        if(strcmp(now,"2021-04-09 22:22:00") == 0)
```

Dimisalkan sudah waktu ulang tahun Stevany, maka folder-folder yang dibuat Steven dimasukkan jadi satu sekaligus di zip dengan code berikut:

```c
	while(wait(NULL) > 0);
	child_id = fork();
    if (child_id == 0) {
        char *argv[] = {"zip", "-r", " Lopyu_Stevany.zip", "Musyik", "Pyoto", "Fylm", NULL};
        execv("/usr/bin/zip", argv);
```

Dan semua folder-folder selain Lopyu_Stevany.zip akan didelete dengan `rm`:

```c
	while(wait(NULL) > 0);
    child_id = fork();
    	if (child_id == 0) {
    	char *argv[] = {"rm ", "-r", "Musyik", "Pyoto", "Fylm", NULL};
    	execv("/bin/rm", argv);
    }
```

### Screenshot
**Hasil Running Code**

<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal1-1.png" />
<br>
<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal1-2.png" />
<br>

### Kendala
* Kebingungan dalam mengeksekusi code yang diinginkan pada waktu spesifik

## Soal 2
Loba bekerja di sebuah petshop terkenal, suatu saat dia mendapatkan zip yang berisi banyak sekali foto peliharaan dan Ia diperintahkan untuk mengkategorikan foto-foto peliharaan tersebut. Loba merasa kesusahan melakukan pekerjaanya secara manual, apalagi ada kemungkinan ia akan diperintahkan untuk melakukan hal yang sama. Kamu adalah teman baik Loba dan Ia meminta bantuanmu untuk membantu pekerjaannya.

### Soal 2.a.
Pertama-tama program perlu mengextract zip yang diberikan ke dalam folder “/home/[user]/modul2/petshop”. Karena bos Loba teledor, dalam zip tersebut bisa berisi folder-folder yang tidak penting, maka program harus bisa membedakan file dan folder sehingga dapat memproses file yang seharusnya dikerjakan dan menghapus folder-folder yang tidak dibutuhkan.

**Penyelesaian**\
Mula-mula, kami menuliskan library yang dibutuhkan:

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <errno.h>
#include <unistd.h>
#include <syslog.h>
#include <string.h>
#include <dirent.h>
#include <wait.h>
```

Kemudian, kami mendownload terlebih dulu zip yang diberikan pada cd `home/aless/modul2`:

```c
    chdir("/home/aless/modul2/");

    child_id = fork();
    if (child_id == 0) {
    char *argv[] = {"unzip", "pets.zip", "-d", "petshop", NULL};
    execv("/usr/bin/unzip", argv);
    }
```

Lalu, kami menghapus isi zip berupa folder-folder yang tidak berguna:

```c
    while(wait(NULL) > 0);
    child_id = fork();
        if (child_id == 0) {
        char *argv[] = {"rm ", "-r", "apex_cheats", "musics", "unimportant_files", NULL};
        execv("/bin/rm", argv);
    }    
```

### Soal 2.b.
Foto peliharaan perlu dikategorikan sesuai jenis peliharaan, maka kamu harus membuat folder untuk setiap jenis peliharaan yang ada dalam zip. Karena kamu tidak mungkin memeriksa satu-persatu, maka program harus membuatkan folder-folder yang dibutuhkan sesuai dengan isi zip.\
Contoh: Jenis peliharaan kucing akan disimpan dalam “/petshop/cat”, jenis peliharaan kura-kura akan disimpan dalam “/petshop/turtle”.\

**Penyelesaian**\

Pertama, kami melakukan directory listing pada semua file yang ada di direktori `petshop`, setiap file akan dimasukkan ke sebuah fungsi yang bernama `input`:

```c
    chdir("petshop");

    while(wait(NULL) > 0);
    while (dp = readdir(dir)) {
        if (strcmp(dp->d_name, "..") != 0 && strcmp(dp->d_name, ".") != 0) {
            input(dp->d_name);
        }
    }
```

Jadi, pada fungsi `input`, kami memecahkan nama file yang diterima menjadi 3 bagian, yaitu jenis hewan, nama hewan, dan umur hewan. Untuk memecahkannya, kami menggunakan sebuah fungsi dari library `<string.h>` berupa `strtok`. Setelah dipecah, nama file akan disimpan di tiga variabel, yaitu variabel folder untuk jenis hewan, variabel nama untuk nama hewan, dan variabel age untuk umur hewan.

```c
void input(char *nama){

  char regular[100];
  sprintf(regular, "%s", nama);
  DIR *dir = opendir(nama);
  char *cek;
  char *ket[3];
  int i;
  cek = strtok (nama,"_;");
  while (cek != NULL)
  {
      for ( i = 0; i < 3; i++)
      {
        ket[i] = cek;
        cek = strtok (NULL, "_;");
      }
    char *folder = ket[0];
    char *nama = ket[1];
    char *age = ket[2];

    char *umur;
    umur = strstr(age ,".jpg");
    if (umur != NULL) {
        int save = umur - age;
        sprintf(age, "%.*s", save, age);
    }

    mkfolder(folder);
    move(folder, nama, regular);
    detail(folder, nama, age);
  }
  closedir(dir);
}
```

Selanjutnya, kami menggunakan variabel folder pada fungsi `mkfolder` untuk membuat sebuah folder jenis hewan.

```c
void mkfolder(char *nama){

    char hewan[20];
    sprintf(hewan, "%s", nama);
	child_id = fork();
    if (child_id == 0) {
	    while ((wait(&status)) > 0);
        char *argv[] = {"mkdir", hewan, NULL};
        execv("/bin/mkdir", argv);
    }

}
```

### Soal 2.c.
Setelah folder kategori berhasil dibuat, programmu akan memindahkan foto ke folder dengan kategori yang sesuai dan di rename dengan nama peliharaan.
Contoh: “/petshop/cat/joni.jpg”.

**Penyelesaian**\
Untuk memindahkan foto, kami menggunakan sebuah fungsi `move` di mana menggunakan 3 variabel `fold` untuk folder jenis hewan, `nama` untuk nama hewan, dan `reg` untuk nama asli dari filenya. Di fungsi `move`, terdapat 2 `execv("/bin/mv", argv);`, di mana yang pertama digunakan untuk me-rename nama file aslinya dengan nama hewannya saja. Lalu, yang kedua digunakan untuk memindahkan nama hewan ke folder hewan.

```c
void move(char *fold, char *nama, char *reg){
    
    char hewan[20];
    sprintf(hewan, "%s", fold);
    char *name[20];
    sprintf(name, "%s.jpg", nama);
    char *regular[100];
    sprintf(regular, "%s", reg);

    while ((wait(&status)) > 0);
    child_id = fork();
    if (child_id == 0) {
    	char *argv[] = {"mv", regular, name, NULL};
    	execv("/bin/mv", argv);
    }

	while ((wait(&status)) > 0);
    child_id = fork();
    if (child_id == 0) {
    	char *argv[] = {"mv", name, hewan, NULL};
    	execv("/bin/mv", argv);
    }
}
```

### Soal 2.d.
Karena dalam satu foto bisa terdapat lebih dari satu peliharaan maka foto harus di pindah ke masing-masing kategori yang sesuai. Contoh: foto dengan nama “dog;baro;1_cat;joni;2.jpg” dipindah ke folder “/petshop/cat/joni.jpg” dan “/petshop/dog/baro.jpg”.

**Penyelesaian**\
Sudah dijelaskan pada 2.c. karena pengerjaannya bersamaan.

### Soal 2.e.
Di setiap folder buatlah sebuah file "keterangan.txt" yang berisi nama dan umur semua peliharaan dalam folder tersebut. Format harus sesuai contoh.

**Penyelesaian**\

Untuk menampilkan sebuah file `keterangan.txt` di setiap folder, maka dibuatlah fungsi `detail` yang menggunakan 3 variabel untuk jenis hewan, nama hewan, dan umur hewan. Lalu, fungsi `detail` akan membuat sebuah file yang bernama `keterangan.txt` yang berisi tentang nama dan umur dari hewan:

```c
void detail(char *fold, char *nama, char *umur){
    while ((wait(&status)) > 0);
    child_id = fork();
    if (child_id == 0) {
        FILE  *keterangan;
        char t[100];
        sprintf(t, "%s/keterangan.txt", fold);
        keterangan = fopen(t , "a");
        fprintf(keterangan, "nama : %s\numur : %s\n\n", nama, umur);
        fclose(keterangan);
        exit(EXIT_SUCCESS);
    }
}
```

### Screenshot
<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal2-1.png" />
<br>
<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal2-2.png" />
<br>

### Kendala
* Kebingungan menentukan syntax yang sesuai untuk menghapus folder-folder yang tidak penting
* Kebingungan syntax dalam pengklasifikasian hewan-hewan ke folder yang sesuai
* Kebingungan membuat file .txt yang menyantumkan nama dan umur setiap hewan

## Soal 3
Ranora adalah mahasiswa Teknik Informatika yang saat ini sedang menjalani magang di perusahan ternama yang bernama “FakeKos Corp.”, perusahaan yang bergerak dibidang keamanan data. Karena Ranora masih magang, maka beban tugasnya tidak sebesar beban tugas pekerja tetap perusahaan. Di hari pertama Ranora bekerja, pembimbing magang Ranora memberi tugas pertamanya untuk membuat sebuah program.

### Soal 3.a.
Ranora harus membuat sebuah program C yang dimana setiap 40 detik membuat sebuah direktori dengan nama sesuai timestamp [YYYY-mm-dd_HH:ii:ss].

**Penyelesaian**\
Untuk menyelesaikan 3a, kami mula-mula terpikirkan untuk membuat C program berupa daemon. Sebelumnya, kami memasukkan beberapa library daemon yang diperlukan. Beberapa library yang kami gunakan adalah:

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <errno.h>
#include <unistd.h>
#include <syslog.h>
#include <string.h>
#include <time.h>
#include <wait.h>
```

Lalu, kami menggunakan format template daemon yang disediakan di modul (kecuali branch if chdir). Nantinya, semua program akan dimasukkan ke dalam fungsi `main()`. Untuk mengatur pembuatan direktori setiap 40 detik, maka nilai 'sleep()' diubah menjadi 'sleep(40)'.

```c
int main() {
  pid_t pid, sid;

  pid = fork();

  if (pid < 0) {
    exit(EXIT_FAILURE);
  }

  if (pid > 0) {
    exit(EXIT_SUCCESS);
  }

  umask(0);

  sid = setsid();
  if (sid < 0) {
    exit(EXIT_FAILURE);
  }

  close(STDIN_FILENO);
  close(STDOUT_FILENO);
  close(STDERR_FILENO);

  while (1) {

    sleep(40);
  }
}
```

Kemudian, kami mendeklarasikan variabel awal sebagai berikut:
```c
time_t t = time(NULL);
struct tm *tm = localtime(&t);
char now[80];
```

Kemudian, dideklarasikan juga code strftime sebagai formatting direktori yang diinginkan:
```c
strftime(now, 80, "%Y-%m-%d_%H:%M:%S", tm);
```

Lalu baru dibuat sebuah direktori:
```
pid_t child_id;
child_id = fork();

if (child_id == 0) {
  char *argv[] = {"mkdir", now, NULL};
  execv("/bin/mkdir", argv);
}
```

Lalu, process akan di `fork()` dan *child process* akan melakukan `execv()` terhadap perintah `mkdir` dengan *argument* `now`. Sementara *parent process* tidak akan menunggu child process dan akan langsung `sleep()` selama 40 detik.
### Soal 3.b.
Setiap direktori yang sudah dibuat diisi dengan 10 gambar yang didownload dari https://picsum.photos/, dimana setiap gambar akan didownload setiap 5 detik. Setiap gambar yang didownload akan diberi nama dengan format timestamp [YYYY-mm-dd_HH:ii:ss] dan gambar tersebut berbentuk persegi dengan ukuran (n%1000) + 50 pixel dimana n adalah detik Epoch Unix.

**Penyelesaian**\
Setelah membuat direktori pada soal 3a, pada 3b kami membuat child process untuk mendownload 10 gambar, di mana program main nya tidak menunggu 10 gambar terdownload dan langsung `sleep(40)`/sleep selama 40 detik setelah fungsi `fork()` berjalan. Pada picsum photos, kita dapat mengatur format pixel gamabar dengan memodifikasi link dengan format `(t%1000)+50)`.

```c
child_id = fork();
if (child_id == 0) {

   for (int i = 0; i < 10; i++) {

      child_id = fork();
      if (child_id == 0) {

          t = time(NULL);
          tm = localtime(&t);
          char new[80], lokasi[160], link[80];
          strftime(new, 80, "%Y-%m-%d_%H:%M:%S", tm);
          sprintf(lokasi, "%s/%s", now, new);
          sprintf(link, "https://picsum.photos/%ld", ((t%1000)+50));
          char *argv[] = {"wget", "-O", lokasi, link, NULL};
          execv("/usr/bin/wget", argv);

      }

      sleep(5);

}
```

Child process/downloading gambar akan ter looping sebanyak 10 kali dan melakukan `sleep()` setiap 5 detik.

### Soal 3.c.
Setelah direktori telah terisi dengan 10 gambar, program tersebut akan membuat sebuah file “status.txt”, dimana didalamnya berisi pesan “Download Success” yang terenkripsi dengan teknik Caesar Cipher dan dengan shift 5. Caesar Cipher adalah Teknik enkripsi sederhana yang dimana dapat melakukan enkripsi string sesuai dengan shift/key yang kita tentukan. Misal huruf “A” akan dienkripsi dengan shift 4 maka akan menjadi “E”. Karena Ranora orangnya perfeksionis dan rapi, dia ingin setelah file tersebut dibuat, direktori akan di zip dan direktori akan didelete, sehingga menyisakan hanya file zip saja.

**Penyelesaian**\
Di sini, setelah child process dari soal 3b. selesai berjalan, maka kami meneruskan program dengan membuat file bernama `status.txt`:

```c
while(wait(NULL) > 0);
    child_id = fork();
    if(child_id == 0){

        char message[20] = "Download Success", ch;
        int i, key = 5;
        
... // program caesar cipher

       FILE  *status;
       char place[] = "/status.txt" ;
       char t[100];
       strcpy(t,now); 
       strcat(t,place);
       status = fopen(t , "w");
       fprintf(status, "%s",  message);
       fclose(status);
    }
```

Mengikuti permintaan soal, maka kami menyisipkan kode algoritma caesar cipher untuk mengenkripsi kalimat "download success". Berikut adalah algoritma caesar cipher yang kami gunakan:

```c
for(i = 0; message[i] != '\0'; ++i){
            ch = message[i];
            
            if(ch >= 'a' && ch <= 'z'){
                ch = ch + key;
                
                if(ch > 'z'){
                    ch = ch - 'z' + 'a' - 1;
                }
                
                message[i] = ch;
            }
            else if(ch >= 'A' && ch <= 'Z'){
                ch = ch + key;
                
                if(ch > 'Z'){
                    ch = ch - 'Z' + 'A' - 1;
                }
                
                message[i] = ch;
            }
        }
```

Setelah itu, kami melakukan zip terhadap folder yang ada, baru kemudian menghapus folder tersebut (Child process akan melakukan zip terhadap folder dan parent process akan menunggu child process selesai dan menghapus directory folder yang telah di zip):

```c
      while(wait(NULL) > 0);
      child_id = fork();
      if (child_id == 0) {
        char nama[80];
        sprintf(nama, "%s.zip", now);
        char *argv[] = {"zip", "-r", nama, now, NULL};
        execv("/usr/bin/zip", argv);
        
        }
  
      while(wait(NULL) != child_id);
      char *argv[] = {"rm", "-r", now, NULL};
      execv("/bin/rm", argv);
```

### Soal 3.d.
Untuk mempermudah pengendalian program, pembimbing magang Ranora ingin program tersebut akan men-generate sebuah program “Killer” yang executable, dimana program tersebut akan menterminasi semua proses program yang sedang berjalan dan akan menghapus dirinya sendiri setelah program dijalankan. Karena Ranora menyukai sesuatu hal yang baru, maka Ranora memiliki ide untuk program “Killer” yang dibuat nantinya harus merupakan program bash.

**Penyelesaian**\
Mula-mula, kami membuat sebuah file baru bernama `kill.sh` (Deklarasi file ini dilakukan sebelum bagian loop utama daemon dan setelah mendapatkan **Session ID** (`sid`)):

```c
  FILE *killer;
  killer = fopen("kill.sh", "w");
```

Selanjutnya, kami mendeklarasikan sebuah variabel `func` yang berisi program yang akan kami masukkan pada file `kill.sh`. Lalu file `kill.sh` akan kami inputkan dengan string yang sesuai untuk melakukan perintah `kill -9` sesuai dengan ***Session ID*** (`sid`) dari program utama (Penggunaan ***Session ID*** agar seluruh *child process* juga ikut di **kill** melalui perintah `kill -9`).

```c
char *func = "\n"
    "#!/bin/bash\n"
    "/usr/bin/kill -9 \"%d\"\n"
    "/bin/rm kill.sh\n";
    fprintf(killer, func, sid);
  }
```

Lalu program killer tersebut akan menghapus dirinya sendiri. Lalu string tersebut diwrite kedalam `killer` dengan fungsi `fprintf()`. Lalu pointer file `killer` akan ditutup koneksinya menggunakan fungsi fclose().

```c
 fclose(killer);
```

Kemudian, di sini juga digunakan argumen `chmod +x` untuk menyimpan dan ubah permission file script agar dapat dieksekusi.

```c
  pid = fork();
  if (pid == 0) {
    char *argv[] = {"chmod","+x","kill.sh", NULL};
    execv("/bin/chmod", argv);
  }
  while(wait(NULL) != pid);
```

### Soal 3.e.
Pembimbing magang Ranora juga ingin nantinya program utama yang dibuat Ranora dapat dijalankan di dalam dua mode. Untuk mengaktifkan mode pertama, program harus dijalankan dengan argumen -z, dan Ketika dijalankan dalam mode pertama, program utama akan langsung menghentikan semua operasinya Ketika program Killer dijalankan. Sedangkan untuk mengaktifkan mode kedua, program harus dijalankan dengan argumen -x, dan Ketika dijalankan dalam mode kedua, program utama akan berhenti namun membiarkan proses di setiap direktori yang masih berjalan hingga selesai (Direktori yang sudah dibuat akan mendownload gambar sampai selesai dan membuat file txt, lalu zip dan delete direktori).

**Penyelesaian**\
Karena ada 2 mode, maka kami menggunakan `strcmp` untuk menyesuaikan format argumen yang dijalankan pada terminal. Argumen -z menggunakan format `fprintf(killer, func, sid)` untuk memberhentikan semua proses. Sedangkan, argumen -x menggunakan format `fprintf(killer, func, sid)` untuk memberhentikan program utama, namun proses di folder masih berjalan:

```c
  if (strcmp(argv[1], "-z") == 0) {
    char *func = "\n"
    "#!/bin/bash\n"
    "/usr/bin/pkill -9 -s\"%d\"\n"
    "/bin/rm kill.sh\n";
    fprintf(killer, func, sid);
  }

  if (strcmp(argv[1], "-x") == 0) {
    char *func = "\n"
    "#!/bin/bash\n"
    "/usr/bin/kill -9 \"%d\"\n"
    "/bin/rm kill.sh\n";
    fprintf(killer, func, getpid());
  }
```
### Screenshot
**Menggunakan argumen -z**
<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal3-1.png" />
<br>
**Menggunakan argumen -x**
<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal3-2.png" />
<br>
<br>
<img height="500" src="https://github.com/HeavenPutra208/soal-shift-sisop-modul-2-IT06-2021/blob/main/img/Soal3-3.png" />
<br>
### Kendala
* Perlu memodifikasi sedikit pada format daemon yang ada pada modul
