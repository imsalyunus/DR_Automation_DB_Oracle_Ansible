Switchover adalah proses pembalikan peran secara terkontrol antara primary dan standby database dalam konfigurasi Oracle Data Guard. Operasi ini memastikan tidak ada kehilangan data (zero data loss) dan tetap menjaga integritas lingkungan Data Guard.

Kapan Switchover Digunakan?
Switchover biasanya digunakan untuk pemeliharaan terencana pada primary database. Prosesnya melibatkan: 
1. Mengubah primary database menjadi standby
2. Melakukan pemeliharaan pada database yang sekarang menjadi standby
3. Mengembalikan database tersebut ke peran primary setelah pemeliharaan selesai
   
Karena kedua database tetap sinkron selama proses ini, switchover memungkinkan transisi yang mulus dengan downtime minimal, memastikan kontinuitas bisnis tetap terjaga.

![image](https://github.com/user-attachments/assets/7f1951e4-c71a-49bd-a08f-132d8fa972f3)
![image](https://github.com/user-attachments/assets/7dbf4758-505b-4ead-951d-d40f15b6830a)

