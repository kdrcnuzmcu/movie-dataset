# movie-dataset
Bir rastgele verisetine manipüle ederek daha okunabilir bir hale getirmeye çalıştım.

Verisetini .read_csv() fonksiyonu ile okuyoruz. Verilere ilk bakışı atmak için .head() fonksiyonunu kullanıyoruz. Bu fonksiyon verisetinin baştan ilk beş satırını size getirir. Satırlar ve sütunları inceliyoruz. Verisetinin genel yapısı hakkında ilk bilgilerimizi ediniyoruz. 
![image](https://user-images.githubusercontent.com/28548881/209650542-f2c3014f-136a-4fbd-aa4f-6562dcb894cd.png)
  - Verisetimiz gördüğümüz kadarıyla bazı filmler ve onların çeşitli bilgilerini içeriyor.
  - MOVIES sütununda satırlarımızın en önemli değişkeni, filmlerin isimlerinin yer aldığını görüyoruz.
  - YEAR sütununun ilgili filmin vizyona çıkış yılı olarak değerlendiriyoruz. Fakat bazı satırlarda bu değer aralık olarak verilmiş. Bu verisetinde yalnızca filmlerin olmayacağı çıkarımını yapıyoruz.
  - GENRE sütununda film ve dizilerin konu kategorilerinin isimleri yer alıyor. Metinsel bir ifade olduğunu görüyoruz ve her satırda "\n" gibi bir kaçış kodu yer alıyor. Muhtemelen bu verileri bir internet sitesinden çekildiğinin ve eksik değerlerin olabileceği çıkarımını yapıyoruz.




![image](https://user-images.githubusercontent.com/28548881/209650721-3552fa14-9a9d-479a-bc13-eb0b56d62e3a.png)

![image](https://user-images.githubusercontent.com/28548881/209650752-c3f31ac9-10bf-4ff9-9869-48b6b71e1f9f.png)

![image](https://user-images.githubusercontent.com/28548881/209650873-4a69f1e5-8591-44c7-8098-59c94cf19992.png)

![image](https://user-images.githubusercontent.com/28548881/209651576-c3530692-7e1e-44b1-b318-be1bd86919c6.png)

![image](https://user-images.githubusercontent.com/28548881/209651770-9414e2ac-e9fe-420c-8eb3-568c1b19007e.png)

![image](https://user-images.githubusercontent.com/28548881/209651843-9c6afd8b-bf20-4ee6-9796-a8555c8166cc.png)

![image](https://user-images.githubusercontent.com/28548881/209651951-57e7a926-5edb-4562-a979-4e94298033b3.png)

![image](https://user-images.githubusercontent.com/28548881/209652085-ee738f4a-69d6-4f8d-8f31-ed40905e1480.png)
