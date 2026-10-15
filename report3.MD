### 7. Що таке skell_dir? Яка його структура?
```
student@CsnKhai:/$ ls -la /etc/skel
total 20
drwxr-xr-x  2 root root 4096 Sep 15  2015 .
drwxr-xr-x 83 root root 4096 Oct  8 05:39 ..
-rw-r--r--  1 root root  220 Apr  9  2014 .bash_logout
-rw-r--r--  1 root root 3637 Apr  9  2014 .bashrc
-rw-r--r--  1 root root  675 Apr  9  2014 .profile
```

---

### 8. Як видалити користувача із системи, включно з його поштовою скринькою?
```
student@CsnKhai:/$ userdel
Usage: userdel [options] LOGIN

Options:
  -f, --force                   force removal of files,
                                even if not owned by user
  -h, --help                    display this help message and exit
  -r, --remove                  remove home directory and mail spool
  -R, --root CHROOT_DIR         directory to chroot into
  -Z, --selinux-user            remove any SELinux user mapping for the user

student@CsnKhai:/$ /var/spool/mail/
bash: /var/spool/mail/: Is a directory
student@CsnKhai:/$ ls /var/spool/mail/
student@CsnKhai:/$ ls -la /var/spool/mail/
total 8
drwxrwsr-x  2 root mail 4096 Sep 15  2015 .
drwxr-xr-x 11 root root 4096 Sep 15  2015 ..
student@CsnKhai:/$ ls -la /var/spool
total 20
drwxr-xr-x  5 root   root 4096 Sep 15  2015 .
drwxr-xr-x 11 root   root 4096 Sep 15  2015 ..
drwxr-xr-x  3 root   root 4096 Sep 15  2015 cron
lrwxrwxrwx  1 root   root    7 Sep 15  2015 mail -> ../mail
drwxr-xr-x  2 root   root 4096 Apr 11  2014 plymouth
drwx------  2 syslog adm  4096 Dec  4  2013 rsyslog
student@CsnKhai:/$ ls -la /var/mail
total 8
drwxrwsr-x  2 root mail 4096 Sep 15  2015 .
drwxr-xr-x 11 root root 4096 Sep 15  2015 ..
student@CsnKhai:/$ sudo userdel -r user1_mode
[sudo] password for student:
userdel: user user1_mode is currently used by process 12985
student@CsnKhai:/$ sudo userdel -r user1_mode
userdel: user1_mode mail spool (/var/mail/user1_mode) not found
student@CsnKhai:/$
```

---

### 9. Які команди та параметри необхідно використовувати для блокування та розблокування облікового запису користувача?
```
student@CsnKhai:/$ usermod
Usage: usermod [options] LOGIN

Options:
  -c, --comment COMMENT         new value of the GECOS field
  -d, --home HOME_DIR           new home directory for the user account
  -e, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -f, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -a, --append                  append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                him/her from other groups
  -h, --help                    display this help message and exit
  -l, --login NEW_LOGIN         new value of the login name
  -L, --lock                    lock the user account
  -m, --move-home               move contents of the home directory to the
                                new location (use only with -d)
  -o, --non-unique              allow using duplicate (non-unique) UID
  -p, --password PASSWORD       use encrypted password for the new password
  -R, --root CHROOT_DIR         directory to chroot into
  -s, --shell SHELL             new login shell for the user account
  -u, --uid UID                 new UID for the user account
  -U, --unlock                  unlock the user account
  -v, --add-subuids FIRST-LAST  add range of subordinate uids
  -V, --del-subuids FIRST-LAST  remvoe range of subordinate uids
  -w, --add-subgids FIRST-LAST  add range of subordinate gids
  -W, --del-subgids FIRST-LAST  remvoe range of subordinate gids
  -Z, --selinux-user SEUSER     new SELinux user mapping for the user account

student@CsnKhai:/$ sudo -L user2
sudo: invalid option -- 'L'
usage: sudo -h | -K | -k | -V
usage: sudo -v [-AknS] [-g group] [-h host] [-p prompt] [-u user]
usage: sudo -l [-AknS] [-g group] [-h host] [-p prompt] [-U user] [-u user] [command]
usage: sudo [-AbEHknPS] [-r role] [-t type] [-C num] [-g group] [-h host] [-p prompt] [-u user] [VAR=value] [-i|-s]
            [<command>]
usage: sudo -e [-AknS] [-r role] [-t type] [-C num] [-g group] [-h host] [-p prompt] [-u user] file ...
student@CsnKhai:/$ sudo usermod -L user2
student@CsnKhai:/$ sudo usermod -U user2
```

---

### 10. Як видалити пароль користувача та надати йому можливість входу без пароля з подальшою зміною пароля?
```
student@CsnKhai:/$ passwd --help
Usage: passwd [options] [LOGIN]

Options:
  -a, --all                     report password status on all accounts
  -d, --delete                  delete the password for the named account
  -e, --expire                  force expire the password for the named account
  -h, --help                    display this help message and exit
  -k, --keep-tokens             change password only if expired
  -i, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --lock                    lock the password of the named account
  -n, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -q, --quiet                   quiet mode
  -r, --repository REPOSITORY   change password in REPOSITORY repository
  -R, --root CHROOT_DIR         directory to chroot into
  -S, --status                  report password status on the named account
  -u, --unlock                  unlock the password of the named account
  -w, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
  -x, --maxdays MAX_DAYS        set maximum number of days before password
                                change to MAX_DAYS

student@CsnKhai:/$ sudo passwd -e -d  user2
passwd: password expiry information changed.
student@CsnKhai:/$ sudo passwd user2
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
student@CsnKhai:/$ sudo passwd -e user2
passwd: password expiry information changed.
```

---

### 11. Виведіть розширений формат інформації про каталог. Поясніть призначення стовпців інформації, що відображаються в терміналі.
```
student@CsnKhai:~$ ls -l /bin
total 9392
-rwxr-xr-x 1 root root  986672 Oct  7  2014 bash
-rwxr-xr-x 3 root root   30240 Oct 21  2013 bunzip2
-rwxr-xr-x 1 root root 1713424 Nov 14  2013 busybox
-rwxr-xr-x 3 root root   30240 Oct 21  2013 bzcat
lrwxrwxrwx 1 root root       6 Oct 21  2013 bzcmp -> bzdiff
-rwxr-xr-x 1 root root    2140 Oct 21  2013 bzdiff
lrwxrwxrwx 1 root root       6 Oct 21  2013 bzegrep -> bzgrep
-rwxr-xr-x 1 root root    4877 Oct 21  2013 bzexe
lrwxrwxrwx 1 root root       6 Oct 21  2013 bzfgrep -> bzgrep
-rwxr-xr-x 1 root root    3642 Oct 21  2013 bzgrep
-rwxr-xr-x 3 root root   30240 Oct 21  2013 bzip2
-rwxr-xr-x 1 root root    9624 Oct 21  2013 bzip2recover
lrwxrwxrwx 1 root root       6 Oct 21  2013 bzless -> bzmore
-rwxr-xr-x 1 root root    1297 Oct 21  2013 bzmore
-rwxr-xr-x 1 root root   46884 Jan 14  2015 cat
-rwxr-xr-x 1 root root   55048 Jan 14  2015 chgrp
-rwxr-xr-x 1 root root   50920 Jan 14  2015 chmod
-rwxr-xr-x 1 root root   59144 Jan 14  2015 chown
-rwxr-xr-x 1 root root    9708 Feb 18  2013 chvt
-rwxr-xr-x 1 root root  124932 Jan 14  2015 cp
-rwxr-xr-x 1 root root  138900 Jan  7  2015 cpio
-rwxr-xr-x 1 root root  112204 Feb 19  2014 dash
-rwxr-xr-x 1 root root   59144 Jan 14  2015 date
-rwxr-xr-x 1 root root    9736 Nov 25  2014 dbus-cleanup-sockets
-rwxr-xr-x 1 root root  457048 Nov 25  2014 dbus-daemon
-rwxr-xr-x 1 root root    5604 Nov 25  2014 dbus-uuidgen
-rwxr-xr-x 1 root root   59208 Jan 14  2015 dd
-rwxr-xr-x 1 root root   96428 Jan 14  2015 df
-rwxr-xr-x 1 root root  108708 Jan 14  2015 dir
-rwxr-xr-x 1 root root   22040 Aug  5  2015 dmesg
lrwxrwxrwx 1 root root       8 Dec 13  2013 dnsdomainname -> hostname
lrwxrwxrwx 1 root root       8 Dec 13  2013 domainname -> hostname
-rwxr-xr-x 1 root root   58012 Feb 18  2013 dumpkeys
-rwxr-xr-x 1 root root   26280 Jan 14  2015 echo
-rwxr-xr-x 1 root root   42716 Jul 16  2013 ed
-rwxr-xr-x 1 root root  174088 Jan 18  2014 egrep
-rwxr-xr-x 1 root root   26280 Jan 14  2015 false
-rwxr-xr-x 1 root root    9712 Feb 18  2013 fgconsole
-rwxr-xr-x 1 root root  133000 Jan 18  2014 fgrep
-rwxr-xr-x 1 root root   34888 Aug  5  2015 findmnt
-rwxr-xr-x 1 root root   30776 Nov 29  2012 fuser
-rwsr-xr-x 1 root root   30112 May 15  2015 fusermount
-rwxr-xr-x 1 root root  178216 Jan 18  2014 grep
-rwxr-xr-x 2 root root    2303 Jan 10  2014 gunzip
-rwxr-xr-x 1 root root    5937 Jan 10  2014 gzexe
-rwxr-xr-x 1 root root   88712 Jan 10  2014 gzip
-rwxr-xr-x 1 root root   13868 Dec 13  2013 hostname
-rwxr-xr-x 1 root root  315804 Jul 23  2015 ip
-rwxr-xr-x 1 root root    9708 Feb 18  2013 kbd_mode
-rwxr-xr-x 1 root root   22160 Feb 10  2015 kill
-rwxr-xr-x 1 root root  157536 Apr 10  2014 kmod
-rwxr-xr-x 1 root root  145404 Jun 10  2013 less
-rwxr-xr-x 1 root root    9692 Jun 10  2013 lessecho
lrwxrwxrwx 1 root root       8 Jun 10  2013 lessfile -> lesspipe
-rwxr-xr-x 1 root root   14540 Jun 10  2013 lesskey
-rwxr-xr-x 1 root root    7758 Jun 10  2013 lesspipe
-rwxr-xr-x 1 root root   50952 Jan 14  2015 ln
-rwxr-xr-x 1 root root   86932 Feb 18  2013 loadkeys
-rwxr-xr-x 1 root root   43432 Jul 15  2015 login
-rwxr-xr-x 1 root root   91628 Jul 15  2015 loginctl
-rwxr-xr-x 1 root root   63068 Dec 18  2013 lowntfs-3g
-rwxr-xr-x 1 root root  108708 Jan 14  2015 ls
-rwxr-xr-x 1 root root   43320 Aug  5  2015 lsblk
lrwxrwxrwx 1 root root       4 Apr 10  2014 lsmod -> kmod
-rwxr-xr-x 1 root root   46856 Jan 14  2015 mkdir
-rwxr-xr-x 1 root root   34504 Jan 14  2015 mknod
-rwxr-xr-x 1 root root   38664 Jan 14  2015 mktemp
-rwxr-xr-x 1 root root   34512 Aug  5  2015 more
-rwsr-xr-x 1 root root   88752 Aug  5  2015 mount
-rwxr-xr-x 1 root root    5600 Jun 15  2015 mountpoint
lrwxrwxrwx 1 root root      20 Sep 15  2015 mt -> /etc/alternatives/mt
-rwxr-xr-x 1 root root   67668 Jan  7  2015 mt-gnu
-rwxr-xr-x 1 root root  116712 Jan 14  2015 mv
-rwxr-xr-x 1 root root  190576 Oct  1  2012 nano
lrwxrwxrwx 1 root root      20 Sep 15  2015 nc -> /etc/alternatives/nc
-rwxr-xr-x 1 root root   30320 Dec  4  2012 nc.openbsd
lrwxrwxrwx 1 root root      24 Sep 15  2015 netcat -> /etc/alternatives/netcat
-rwxr-xr-x 1 root root  113832 Aug  5  2014 netstat
lrwxrwxrwx 1 root root       8 Dec 13  2013 nisdomainname -> hostname
-rwxr-xr-x 1 root root   58988 Dec 18  2013 ntfs-3g
-rwxr-xr-x 1 root root    9628 Dec 18  2013 ntfs-3g.probe
-rwxr-xr-x 1 root root   71044 Dec 18  2013 ntfs-3g.secaudit
-rwxr-xr-x 1 root root   13672 Dec 18  2013 ntfs-3g.usermap
-rwxr-xr-x 1 root root   26036 Dec 18  2013 ntfscat
-rwxr-xr-x 1 root root   30080 Dec 18  2013 ntfsck
-rwxr-xr-x 1 root root   34228 Dec 18  2013 ntfscluster
-rwxr-xr-x 1 root root   34228 Dec 18  2013 ntfscmp
-rwxr-xr-x 1 root root   21864 Dec 18  2013 ntfsdump_logfile
-rwxr-xr-x 1 root root   42428 Dec 18  2013 ntfsfix
-rwxr-xr-x 1 root root   58820 Dec 18  2013 ntfsinfo
-rwxr-xr-x 1 root root   31228 Dec 18  2013 ntfsls
-rwxr-xr-x 1 root root   26000 Dec 18  2013 ntfsmftalloc
-rwxr-xr-x 1 root root   34228 Dec 18  2013 ntfsmove
-rwxr-xr-x 1 root root   38280 Dec 18  2013 ntfstruncate
-rwxr-xr-x 1 root root   46776 Dec 18  2013 ntfswipe
lrwxrwxrwx 1 root root       6 Feb 18  2013 open -> openvt
-rwxr-xr-x 1 root root   13924 Feb 18  2013 openvt
lrwxrwxrwx 1 root root      14 Jun 15  2015 pidof -> /sbin/killall5
-rwsr-xr-x 1 root root   38932 May  7  2014 ping
-rwsr-xr-x 1 root root   43316 May  7  2014 ping6
-rwxr-xr-x 1 root root   34480 Dec  2  2014 plymouth
-rwxr-xr-x 1 root root   30512 Dec  2  2014 plymouth-upstart-bridge
-rwxr-xr-x 1 root root   87996 Feb 10  2015 ps
-rwxr-xr-x 1 root root   26340 Jan 14  2015 pwd
lrwxrwxrwx 1 root root       4 Oct  7  2014 rbash -> bash
-rwxr-xr-x 1 root root   38604 Jan 14  2015 readlink
-rwxr-xr-x 1 root root      89 Jul 16  2013 red
-rwxr-xr-x 1 root root   55044 Jan 14  2015 rm
-rwxr-xr-x 1 root root   38600 Jan 14  2015 rmdir
lrwxrwxrwx 1 root root       4 Oct  1  2012 rnano -> nano
-rwxr-xr-x 1 root root     254 Jul 18  2014 running-in-container
-rwxr-xr-x 1 root root   14088 Aug 28  2013 run-parts
-rwxr-xr-x 1 root root   63692 Feb 13  2014 sed
-rwxr-xr-x 1 root root   38680 Feb 18  2013 setfont
-rwxr-xr-x 1 root root   12052 Jan 29  2014 setupcon
lrwxrwxrwx 1 root root       4 Feb 19  2014 sh -> dash
lrwxrwxrwx 1 root root       4 Sep 15  2015 sh.distrib -> dash
-rwxr-xr-x 1 root root   26280 Jan 14  2015 sleep
-rwxr-xr-x 1 root root   75540 Jul 23  2015 ss
lrwxrwxrwx 1 root root       7 Nov 14  2013 static-sh -> busybox
-rwxr-xr-x 1 root root   59112 Jan 14  2015 stty
-rwsr-xr-x 1 root root   35300 Jul 15  2015 su
-rwxr-xr-x 1 root root   26280 Jan 14  2015 sync
-rwxr-xr-x 1 root root   13856 Aug  5  2015 tailf
-rwxr-xr-x 1 root root  347540 Feb  4  2014 tar
-rwxr-xr-x 1 root root    9604 Aug 28  2013 tempfile
-rwxr-xr-x 1 root root   55080 Jan 14  2015 touch
-rwxr-xr-x 1 root root   26280 Jan 14  2015 true
-rwxr-xr-x 1 root root  243220 Jul 15  2015 udevadm
-rwxr-xr-x 1 root root   13672 May 15  2015 ulockmgr_server
-rwsr-xr-x 1 root root   67704 Aug  5  2015 umount
-rwxr-xr-x 1 root root   26312 Jan 14  2015 uname
-rwxr-xr-x 2 root root    2303 Jan 10  2014 uncompress
-rwxr-xr-x 1 root root    2762 Feb 18  2013 unicode_start
-rwxr-xr-x 1 root root  108712 Jan 14  2015 vdir
-rwxr-xr-x 1 root root     946 Aug 28  2013 which
-rwxr-xr-x 1 root root   22252 Mar 23  2014 whiptail
lrwxrwxrwx 1 root root       8 Dec 13  2013 ypdomainname -> hostname
-rwxr-xr-x 1 root root    1939 Jan 10  2014 zcat
-rwxr-xr-x 1 root root    1779 Jan 10  2014 zcmp
-rwxr-xr-x 1 root root    5766 Jan 10  2014 zdiff
-rwxr-xr-x 1 root root     142 Jan 10  2014 zegrep
-rwxr-xr-x 1 root root     142 Jan 10  2014 zfgrep
-rwxr-xr-x 1 root root    2133 Jan 10  2014 zforce
-rwxr-xr-x 1 root root    5940 Jan 10  2014 zgrep
-rwxr-xr-x 1 root root    2039 Jan 10  2014 zless
-rwxr-xr-x 1 root root    1912 Jan 10  2014 zmore
-rwxr-xr-x 1 root root    5049 Jan 10  2014 znew
```

---

### 12. Які існують права доступу та для кого вони визначаються (тобто опишіть основні категорії користувачів)? Коротко поясніть позначення прав доступу.
```
У Linux права доступу визначають, які дії користувачі можуть виконувати з файлами та каталогами. Для перегляду прав використовують команди ls -l і stat.
Основні типи прав доступу
r	Read — читання	Перегляд вмісту	Перегляд списку файлів
w	Write — запис	Зміна вмісту	Створення, видалення та перейменування записів за відповідних умов
x	Execute — виконання	Запуск програми або сценарію	Доступ до файлів усередині каталогу за відомим ім'ям
Категорії користувачів
Права доступу поділяються на три категорії:
u (user) — власник файлу або каталогу.
g (group) — користувачі, які належать до групи, призначеної файлу.
o (others) — усі інші користувачі, які не належать до перших двох категорій.
Адміністратор root має широкі повноваження обходити звичайні обмеження доступу, але деякі обмеження та механізми безпеки можуть діяти й для нього.
```
```
student@CsnKhai:~$ ls -l /etc/passwd
-rw-r--r-- 1 root root 1154 Oct  8 06:07 /etc/passwd
student@CsnKhai:~$
```

---

### 13. Яка послідовність визначення взаємозв’язку між файлом та користувачем?
```
student@CsnKhai:~$ ls -l /etc/passwd
-rw-r--r-- 1 root root 1154 Oct  8 06:07 /etc/passwd
student@CsnKhai:~$ stat /etc/passwd
  File: ‘/etc/passwd’
  Size: 1154            Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d      Inode: 60808       Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2026-10-14 09:06:52.258467661 +0000
Modify: 2026-10-08 06:07:25.486506946 +0000
Change: 2026-10-08 06:07:25.486506946 +0000
 Birth: -
student@CsnKhai:~$ id root
uid=0(root) gid=0(root) groups=0(root)
student@CsnKhai:~$
```

---

### 14. Які команди використовуються для зміни власника файлу (каталогу), а також режиму доступу до файлу? Наведіть приклади та продемонструйте їх виконання в терміналі.
```
student@CsnKhai:~$ touch test.txt
student@CsnKhai:~$ ls -l test.txt
-rw-rw-r-- 1 student student 0 Oct 14 09:56 test.txt
student@CsnKhai:~$ sudo chown user3 test.txt
student@CsnKhai:~$ ls -l test.txt
-rw-rw-r-- 1 user3 student 0 Oct 14 09:56 test.txt
student@CsnKhai:~$ sudo chown user3:user3 test.txt
student@CsnKhai:~$ ls -l test.txt
-rw-rw-r-- 1 user3 user3 0 Oct 14 09:56 test.txt
```

---

### 15. Наведіть приклад восьмкового (octal) представлення прав доступу. Поясніть призначення команди umask.
```
student@CsnKhai:~$ sudo chmod 755 test.txt
student@CsnKhai:~$ umask
0002
student@CsnKhai:~$ umask 022
student@CsnKhai:~$ umask
0022
student@CsnKhai:~$
```

---

### 16. Дайте визначення sticky bit та механізму підстановки ідентифікатора (SUID/SGID). Наведіть приклади файлів і каталогів із такими атрибутами.
```
Sticky bit — спеціальний атрибут доступу, який для каталогів обмежує видалення та перейменування файлів: зазвичай це можуть робити власник файлу, власник каталогу або адміністратор root.
SUID (Set User ID) — спеціальний біт для виконуваних файлів, який дозволяє програмі виконуватися з ефективними правами власника файлу, а не лише користувача, який її запустив.
SGID (Set Group ID) — спеціальний біт, який для виконуваного файлу встановлює ефективну групу процесу відповідно до групи файлу. Для каталогу він зазвичай забезпечує успадкування групи каталогу новими файлами та підкаталогами.
```
```
student@CsnKhai:~$ find /usr/bin -perm -4000 -type f 2>/dev/null
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/sudo
/usr/bin/chfn
/usr/bin/passwd
/usr/bin/mtr
/usr/bin/chsh
/usr/bin/traceroute6.iputils
student@CsnKhai:~$ find /usr/bin -perm -2000 -type f 2>/dev/null
/usr/bin/bsd-write
/usr/bin/expiry
/usr/bin/wall
/usr/bin/ssh-agent
/usr/bin/mail-lock
/usr/bin/chage
/usr/bin/dotlockfile
/usr/bin/crontab
/usr/bin/mlocate
/usr/bin/mail-touchlock
/usr/bin/mail-unlock
student@CsnKhai:~$ ls -ld /tmp
drwxrwxrwt 2 root root 4096 Oct 14 09:17 /tmp
student@CsnKhai:~$
```

---

### 17. Які атрибути доступу повинні бути встановлені для командного сценарію (shell script)?
```
student@CsnKhai:~$ nano script.sh
student@CsnKhai:~$ chmod u+x script.sh
student@CsnKhai:~$ ./script.sh
Hello, Ubuntu!
student@CsnKhai:~$ ls -l script.sh
-rwxrw-r-- 1 student student 36 Oct 14 10:14 script.sh
```
```
Для запуску сценарію безпосередньо через ./script.sh потрібне право виконання, а також право читання файлу для інтерпретатора. Альтернативно сценарій можна запустити командою bash script.sh, якщо файл доступний для читання.
```
