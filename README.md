# Step by Step Manual Data Guard Switchover #
## 1. Pastikan sudah login sebagai user oracle dan memuat profile environment: ##
`su - oracle`  
`. .bash_profile`

3. Pastikan listener dalam keadaan aktif:
`lsnrctl start`

4. Masuk ke SQL*Plus:
`sqlplus / as sysdba`

5. Verifikasi Status Database:
`SELECT database_role, switchover_status FROM v$database;`

6. Di Standby, pastikan proses recovery sedang berjalan:
`ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;`

7. Cek gap/lag antara Primary dan Standby dengan perintah berikut:
`SELECT inst_id, name, value FROM gv$dataguard_stats WHERE name='apply lag';`  
*Di Primary, hasilnya harus "no rows selected"  
*Di Standby, akan terlihat apply lag jika ada keterlambatan replikasi

8. Sinkronisasi Archive Log:
ALTER SYSTEM ARCHIVE LOG CURRENT;
ALTER SYSTEM ARCHIVE LOG CURRENT;
ALTER SYSTEM ARCHIVE LOG CURRENT;

9. Cek daftar archive log:
ARCHIVE LOG LIST;

10. Lakukan Switchover dari Primary ke Standby:
ALTER DATABASE COMMIT TO SWITCHOVER TO STANDBY;

11. Startup Mount di Standby yang Baru:
STARTUP MOUNT;

12. Verifikasi Status di Standby yang Baru:
SELECT name, open_mode, database_role FROM v$database;

13. Pastikan Standby Lama Siap Menjadi Primary:
SELECT database_role, switchover_status FROM v$database;

14. Lakukan Switchover dari Standby ke Primary (di Standby Lama yang akan menjadi Primary):
ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY;

15. Open Database di Primary yang Baru:
ALTER DATABASE OPEN;

16. Konfigurasi Log Archive di Primary yang Baru:
ALTER SYSTEM SET log_archive_dest_state_2=ENABLE SCOPE=BOTH;

17. Konfigurasi Log Archive di Standby yang Baru:
ALTER SYSTEM SET log_archive_dest_state_2=DEFER SCOPE=BOTH;

18. Restart Managed Recovery Process (MRP) di Standby yang Baru:
ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

19. Verifikasi Replikasi Primary ke Standby:
ALTER SYSTEM ARCHIVE LOG CURRENT;
ALTER SYSTEM ARCHIVE LOG CURRENT;
ALTER SYSTEM ARCHIVE LOG CURRENT;

20. Cek Lag di Standby yang Baru:
SELECT inst_id, name, value FROM gv$dataguard_stats WHERE name='apply lag';



Check database role and database name
SELECT database_role, switchover_status from v$database;
![image](https://github.com/user-attachments/assets/c8fc1054-42a4-4a35-a6c5-8bc0c64a3c5b)

select inst_id, name, value from gv$dataguard_stats where name='apply lag';
![image](https://github.com/user-attachments/assets/aeeb29f7-29f3-41b9-b157-31b49f5235b9)

Switchover adalah proses pembalikan peran secara terkontrol antara primary dan standby database dalam konfigurasi Oracle Data Guard. Operasi ini memastikan tidak ada kehilangan data (zero data loss) dan tetap menjaga integritas lingkungan Data Guard.

Kapan Switchover Digunakan?

Switchover biasanya digunakan untuk pemeliharaan terencana pada primary database. Prosesnya melibatkan: 
1. Mengubah primary database menjadi standby
2. Melakukan pemeliharaan pada database yang sekarang menjadi standby
3. Mengembalikan database tersebut ke peran primary setelah pemeliharaan selesai
   
Karena kedua database tetap sinkron selama proses ini, switchover memungkinkan transisi yang mulus dengan downtime minimal, memastikan kontinuitas bisnis tetap terjaga.

![image](https://github.com/user-attachments/assets/7f1951e4-c71a-49bd-a08f-132d8fa972f3)
![image](https://github.com/user-attachments/assets/7dbf4758-505b-4ead-951d-d40f15b6830a)

LOGIC DIAGRAM !!!
![image](https://github.com/user-attachments/assets/09ef4723-eea5-487c-97da-c98a29600a8c)
![image](https://github.com/user-attachments/assets/377b1a86-8b6c-4524-ac51-367aaa6e9b4e)

#########################################################

![image](https://github.com/user-attachments/assets/b4ad322e-35b0-4f8d-ade5-1eeac69fe64b)
![image](https://github.com/user-attachments/assets/39224372-0719-4829-821a-451f784416f1)

LOGIC DIAGRAM !!!
![image](https://github.com/user-attachments/assets/aca8b4e1-3512-4566-bea4-68c933fffc2e)
![image](https://github.com/user-attachments/assets/45b2afa5-b35d-4cd6-8874-aed0dd508ee2)




