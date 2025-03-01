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




