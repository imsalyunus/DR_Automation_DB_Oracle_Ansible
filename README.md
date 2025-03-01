# Step by Step Manual Data Guard Switchover #
## 1. Pastikan sudah login sebagai user oracle dan memuat profile environment: ##
`su - oracle`  
`. .bash_profile`

## 2. Pastikan listener dalam keadaan aktif: ##
`lsnrctl start`

## 3. Masuk ke SQL*Plus: ##
`sqlplus / as sysdba`

## 4. Verifikasi Status Database: ##
`SELECT database_role, switchover_status FROM v$database;`
![image](https://github.com/user-attachments/assets/5754ab70-19e2-4813-a3f9-dcd1bd2f55e2)
![image](https://github.com/user-attachments/assets/4f423772-fd6a-4a9c-a136-73faa1e56f32)

## 5. Di Standby, pastikan proses recovery sedang berjalan: ##
`ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT;`
![image](https://github.com/user-attachments/assets/2bbd3df6-9032-4caf-8702-fb1b5c877fbb)

## 6. Cek gap/lag antara Primary dan Standby dengan perintah berikut: ##
`SELECT inst_id, name, value FROM gv$dataguard_stats WHERE name='apply lag';`
![image](https://github.com/user-attachments/assets/c8fc1054-42a4-4a35-a6c5-8bc0c64a3c5b)
![image](https://github.com/user-attachments/assets/8fbccccf-bf50-43f8-b375-d62f811733cb)
*Di Primary, hasilnya harus "no rows selected"  
*Di Standby, akan terlihat apply lag jika ada keterlambatan replikasi

## 7. Sinkronisasi & Cek daftar Archive Log: ##
`ALTER SYSTEM ARCHIVE LOG CURRENT;`
`ALTER SYSTEM ARCHIVE LOG CURRENT;`
`ALTER SYSTEM ARCHIVE LOG CURRENT;`
`ARCHIVE LOG LIST;`
![image](https://github.com/user-attachments/assets/03194028-6638-40ee-91a5-63b0288fe503)

## 8. Lakukan Switchover dari Primary ke Standby: ##
`ALTER DATABASE COMMIT TO SWITCHOVER TO STANDBY;`
![image](https://github.com/user-attachments/assets/a0464d9a-9108-40a9-a813-94758ab6bfd9)

## 9. Startup Mount di Standby yang Baru: ##
`STARTUP MOUNT;`
![image](https://github.com/user-attachments/assets/4f9cb690-893c-4e64-89eb-02308c33755a)

## 10. Verifikasi Status di Standby yang Baru: ##
`SELECT name, open_mode, database_role FROM v$database;`
![image](https://github.com/user-attachments/assets/b6c92878-a33f-4832-a88e-6fe8410a68b0)

## 11. Pastikan Standby Lama Siap Menjadi Primary: ##
`SELECT database_role, switchover_status FROM v$database;`
![image](https://github.com/user-attachments/assets/48916361-6fb5-4b38-b83a-2155b637e0f7)

## 12. Lakukan Switchover dari Standby ke Primary (di Standby Lama yang akan menjadi Primary): ##
`ALTER DATABASE COMMIT TO SWITCHOVER TO PRIMARY;`
![image](https://github.com/user-attachments/assets/899558b8-8c61-4259-b600-a8f221fdc235)

## 13. Open Database di Primary yang Baru: ##
`ALTER DATABASE OPEN;`
![image](https://github.com/user-attachments/assets/03c9e47a-82d3-4141-93b6-a215d27b170f)

## 14. Konfigurasi Log Archive di Primary yang Baru: ##
`ALTER SYSTEM SET log_archive_dest_state_2=ENABLE SCOPE=BOTH;`
![image](https://github.com/user-attachments/assets/dfba1898-5404-4e77-a36b-6bec0672c009)

## 15. Konfigurasi Log Archive di Standby yang Baru: ##
`ALTER SYSTEM SET log_archive_dest_state_2=DEFER SCOPE=BOTH;`
![image](https://github.com/user-attachments/assets/3b9ee0e6-cbf0-40f5-a79d-7faa93973efb)

## 16. Restart Managed Recovery Process (MRP) di Standby yang Baru: ##
`ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;`
![image](https://github.com/user-attachments/assets/bec25a42-5b0e-48d3-95ae-e66dfc36ff07)

## 17. Verifikasi Replikasi Primary ke Standby: ##
`ALTER SYSTEM ARCHIVE LOG CURRENT;`
`ALTER SYSTEM ARCHIVE LOG CURRENT;`
`ALTER SYSTEM ARCHIVE LOG CURRENT;`
![image](https://github.com/user-attachments/assets/f68e0d02-8811-49d2-80a8-d9c720dd3ec2)

## 18. Cek Lag di Standby yang Baru: ##
`SELECT inst_id, name, value FROM gv$dataguard_stats WHERE name='apply lag';`
![image](https://github.com/user-attachments/assets/fa1d179c-f772-4d8d-8dfc-eb759d4872b0)

#########################################################  
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




