# movie-dataset
Bir rastgele verisetine manipüle ederek daha okunabilir bir hale getirmeye çalıştım.

![image](https://user-images.githubusercontent.com/28548881/209658161-4f74918b-a55e-4065-9c4e-056663ec1425.png)
Verisetini .read_csv() fonksiyonu ile okuyoruz. "thousands=','" ifadesi ile basamakları virgül ile ayrılmış ve metinsel olarak algılanan değişkenleri kolaylıkla sayısal değişkene çevirebiliyoruz.

Verilere ilk bakışı atmak için .head() fonksiyonunu kullanıyoruz. Bu fonksiyon verisetinin baştan ilk beş satırını size getirir. Satırlar ve sütunları inceliyoruz. Verisetinin genel yapısı hakkında ilk bilgilerimizi ediniyoruz. 
![image](https://user-images.githubusercontent.com/28548881/209650542-f2c3014f-136a-4fbd-aa4f-6562dcb894cd.png)
  - Verisetimiz gördüğümüz kadarıyla bazı filmler ve onların çeşitli bilgilerini içeriyor.
  - MOVIES sütununda satırlarımızın en önemli değişkeni, filmlerin isimlerinin yer aldığını görüyoruz.
  - YEAR sütununun ilgili filmin vizyona çıkış yılı olarak değerlendiriyoruz. Fakat bazı satırlarda bu değer aralık olarak verilmiş. Bu verisetinde yalnızca filmlerin olmayacağı çıkarımını yapıyoruz.
  - GENRE sütununda film veya dizilerin konu kategorilerinin isimleri yer alıyor. Metinsel bir ifade olduğunu görüyoruz ve her satırda "\n" gibi bir kaçış kodu yer alıyor. Muhtemelen bu verileri bir internet sitesinden çekildiğinin ve eksik değerlerin olabileceği çıkarımını yapıyoruz.
  - RATING sütununda film veya dizilerin oylamaların sonuncunda aldığı puanlar yer alıyor. İlk satırımızda 5.0 ile 9.2 arasında değişen puanların 10 üzerinden derecelendiğini kuvvetle düşünürüz.
  - ONE-LINE sütunu ilgili film ve dizilerin konusu hakkında kısa bir bilgi içeriyor olabilir.
  - STARS sütununu dikkatli incelediğimizde "Director" ve "Stars" başlıklarını görüyoruz. Ardından isim soyisimler geliyor. Çoğu satırda "Director" bilgisinin de eksik olduğunu görüyoruz.
  - VOTES sütunu film veya dizilerin oylanma sayısını gösteriyor. Popülerlik araştırması yaparken kullanabileceğimiz bir değişken olabilir.
  - RunTime sütununda film veya dizilerin süresi yer almaktadır. Genel olarak filmler dizilerin bir bölümünden daha uzundur. Ayrıca dizilerin kendi bölümleri içinde de bir eşitlik olması söz konusu olmadığı için kullanılmasının verimli olmadığını düşündüğüm bir değişkendir. 
  - Gross sütunu filmlerin gişede kazandırdığı toplam paradır. Diziler bu konu dışında kalmaktadır. İlk on satırda gördüğümüz üzere filmlerde bile eksik veri mevcut.


![image](https://user-images.githubusercontent.com/28548881/209650721-3552fa14-9a9d-479a-bc13-eb0b56d62e3a.png)

![image](https://user-images.githubusercontent.com/28548881/209650752-c3f31ac9-10bf-4ff9-9869-48b6b71e1f9f.png)

![image](https://user-images.githubusercontent.com/28548881/209650873-4a69f1e5-8591-44c7-8098-59c94cf19992.png)

![image](https://user-images.githubusercontent.com/28548881/209651576-c3530692-7e1e-44b1-b318-be1bd86919c6.png)

![image](https://user-images.githubusercontent.com/28548881/209651770-9414e2ac-e9fe-420c-8eb3-568c1b19007e.png)

![image](https://user-images.githubusercontent.com/28548881/209651843-9c6afd8b-bf20-4ee6-9796-a8555c8166cc.png)

![image](https://user-images.githubusercontent.com/28548881/209651951-57e7a926-5edb-4562-a979-4e94298033b3.png)

![image](https://user-images.githubusercontent.com/28548881/209652085-ee738f4a-69d6-4f8d-8f31-ed40905e1480.png)
