## Elimdeki bir film veri setinin temizlenmesi işlemlerimi sizinle paylaşacağım.

- Veri temizleme işlemi için ihtiyacımız olan kütüphaneleri ekliyoruz.

```bash
    import pandas as pd
    import numpy as np
```

- Temizleyeceğimiz veri setimizi okuyoruz ve bir değişkene aktarıyoruz.
Eklediğimiz thousands = "," parametresi ile veri setinde "," ile ayrıldığı için metinsel değer olarak algılanan sayısal değerleri sayıya çevirmemize yardımcı oluyor.

```bash
    df_ = pd.read_csv("movies.csv", thousands = ",")
```

- Eğer yanlış bir işlem yaparsak geri dönüşümüz kolay olması için veri setinin bir kopyasını çıkartıyoruz. Böylelikle her seferinde dosyadan okuyarak vakit kaybetmemiş olacağız.

```bash
    df = df_.copy()
```

- Veri setine ilk bakış çoğunlukla .head fonksiyonu ile olur. .head fonksiyonu parametre vermediğimiz zaman veri setinin ilk beş satırını getirir. Sütunları ve içerdikleri değerleri görmüş oluruz. Böylelikle veri seti hakkında genel bir fikrimiz oluşur.

- Gördüğümüz gibi film verileri taşıyan bir veri setimiz var. Genel olarak göz gezdirdiğimizde eksik değer olan sütunlar görüyoruz. Metinsel olarak yabancı karakterler içeren sütunlar var. YEAR sütununda aralık olarak verilmiş değerler var. Demek ki veri setimizde yalnızca filmler değil, diziler de olabilir. Veri setimizde hem film hem de dizi bulunması halinde MOVIES sütununda aynı değerlerle karşılaşabiliriz.

- Bir analiz yapacaksak okuması ve sınıflandırması kolay, eksik veya tekrar eden değerlerin olmadığı bir veri seti isteriz.

```bash
    df.head(10)
```

![image](https://user-images.githubusercontent.com/28548881/209811319-bd3a3c99-b7ed-42c2-88fb-9e4214595861.png)


- Aşağıdaki tabloda gördüğümüz gibi hem eksik değerler hem de tekrarlanan değerler var.

```bash
    print("Toplam satır ve sütun sayısı: " + str(df.shape))
    pd.concat([df.nunique().to_frame("Eşsiz Değerler"), \
           df.isnull().sum().to_frame("Eksik Değerler")], axis = 1,).T 
```

![image](https://user-images.githubusercontent.com/28548881/209811400-a255f9da-0d52-4b21-a642-a64217fb7424.png)

- Dizi ve filmlerin en ayırt edici özelliği dizinin sürekli olarak devam eden bir yapım olmasıdır. YEAR sütununda aralık olarak belirtilen değerler dizileri bulmamıza yardımcı olacaktır. YEAR sütunu üstünde gezerek "uzun tire" karakterini arıyoruz. .str.contains() ifadesi boolean bir değişken geri döner. Tanımladığımız boş listeye sırasıyla 1 ve 0 yani True-False olacak şekilde ekliyoruz.  Sonrasında veri setimize yeni bir sütun olarak ekliyoruz.

```bash
    isSeries = []
    for i in df["YEAR"].str.contains("–"):
        if i:
            isSeries.append(1)
        else:
            isSeries.append(0)
    df["IS_SERIES"] = isSeries
    df.head(1)    
```

![image](https://user-images.githubusercontent.com/28548881/209811429-7ccbcbb1-ff50-426d-bcaa-6b9964096562.png)


- GENRE sütunundaki metinsel değerlerin daha rahat okunabilmesi için istemeyeceğimiz karakterleri siliyoruz. .replace fonksiyonu aldığı eski değer metin içinde arar ve yeni ile değiştirir.Yeni değeri boş verirseniz eski değeri metnin içinden siler. .split fonksiyonu verdiğiniz parametreyi metnin içinde arar ve metni verdiğiniz parametreyi bulduğu yerlerden parçalar.

```bash
genre = []
for i in df["GENRE"]:
    genre.append(str(i).replace("\n","").replace(" ", "").split(","))
df["GENRE"] = genre
df.head(1)    
```

![image](https://user-images.githubusercontent.com/28548881/209811463-8597d978-b14a-4d3f-9559-47d321e15d40.png)


- White Space problemi yaşamamak için filmlerin başındaki ve sonundaki boşlukları temizliyoruz.

```bash
df["MOVIES"] = df["MOVIES"].str.strip()     
```

- MOVIES sütununda hangi film veya dizi kaç kere tekrar ettiğini saydırıyoruz.

```bash
counter = 0
countedMovies = {}
for i in df["MOVIES"].unique().tolist():
    countedMovies.update({i:df["MOVIES"].tolist().count(i)})
    
countedMovies = dict(sorted(countedMovies.items(), key = lambda x:x[1], reverse = True))
countedMovies    
```
![image](https://user-images.githubusercontent.com/28548881/209811557-9f751a30-45ee-45fb-9ec2-555f75a4ddc2.png)


- Bir diziyi örnek olması için inceliyoruz. GENRE sütununda bir değişiklik görünmüyor fakat diğer sütunlarda farklılıklar var. Tekrar eden satırlardan birleştirerek kurtulabiliriz. RATING ve VOTES sütunlarını orantılı bir şekilde birleştirerek dizi hakkında daha doğru yorumlar yapabiliriz.

```bash
df[df["MOVIES"] == "Vikings"]    
```

![image](https://user-images.githubusercontent.com/28548881/209811591-1eff5ca9-4fe1-4275-841b-b343cab041f5.png)

- RATING ile VOTES sütununu çarparak yeni bir sütun elde ettik. Satırları birleştirirken bu sütunun toplamını VOTES sütununun toplamına böldüğümüzde gerçek RATING değerimizi alacağız.

```bash
df["RATING_VOTES"] = df["RATING"] * df["VOTES"]
df[df["MOVIES"] == "Vikings"]    
```

![image](https://user-images.githubusercontent.com/28548881/209811648-22e96111-c970-43dc-a541-b3e036d94a6f.png)


- GENRE sütununda yaptığımız gibi istenmeyen karakterleri temizliyoruz.
```bash
df["STARS"] = df["STARS"].str.replace("\n", "")
df["STARS"] = df["STARS"].str.strip()    
```

- Aşağıda metin içinde find fonksiyonu ile bir değeri arıyoruz. Eğer bulursa bize değerle karşılaştığı indeksi geri getiriyor.

- Bazı satırlarda Director yok, bazı satırlarda birden fazla var. Çoğu satırda Stars varken bazı satırlarda Star olarak bırakılmış. Bazı satırlarda ise hiçbir şey yok.

- Director/Directors veya Star/Stars olarak girilen metinleri döngü içinde düzeltiyoruz. if bloğu içinde -1'e eşitledik. find fonksiyonu aradığımız değeri bulamazsa -1 değeri döndürür. Eğer -1 değeri ile karşılaşırsak "Unknown" yazdırıyoruz. Böylelikle eksik veya anlamsız değerleri tek bir formata sokuyoruz.

```bash
directors = []
for i in df["STARS"]:
    i.replace("Directors", "Director")
    if i.find("Director") == -1:
        directors.append("Unknown")
    else:
        directors.append(i[i.find("Director") + 9: i.find("|")])

stars = []
for i in df["STARS"]:
    i.replace("Star", "Stars")
    if i.find("Stars") == -1:
        stars.append("Unknown")
    else:
        stars.append(i[i.find("Stars") + 6:])    
```

- Aradığımız değerleri listelere kaydedip veri setimize yeni sütun olarak ekliyoruz.
        
```bash
df["Directors"] = directors        
df["Stars"] = stars

df.head(3)    
```

![image](https://user-images.githubusercontent.com/28548881/209811774-01e4ef3c-8d33-4ca3-8906-ce2af8e9def8.png)


- Film veya dizilerin içeriğini birer cümleyle anlatan sütunumuz. Tekrar eden satırları birleştirirken en çok VOTES değerine sahip satırın metnini kullanabiliriz.

```bash
oneline = []
for i in df["ONE-LINE"]:
    oneline.append(str(i).replace("\n",""))
df["ONE-LINE"] = oneline
df.head(1)    
```

![image](https://user-images.githubusercontent.com/28548881/209811800-bb784246-fa98-489e-8ec2-77fab2134a6e.png)


- Daha önce film ve dizileri birbirinden ayıran değeri taşıyan bir sütun eklemiştik. Aynı sütun yardımı ile film ve dizileri iki veri seti olacak şekilde ayırıyoruz.

```bash
Series = df[df["IS_SERIES"] == 1]
Movies = df[df["IS_SERIES"] == 0]    
```

- Ana veri setimizi ikiye ayırdıktan yeniden birleştirmeyeceğimiz için indekslerini düzeltmemiz gerekiyor.

```bash
Movies.head()    
```

![image](https://user-images.githubusercontent.com/28548881/209811856-165997cc-a68d-41aa-9d9c-1ba2b4fbc5af.png)

- .reset_index fonksiyonu ile indekslerin yeniden yazılmasını sağladık.
```bash
Series = Series.reset_index()
Movies = Movies.reset_index()
```

- İkiye ayırdığımız veri setlerinin isim taşıyan sütunlarında white space problemi yaşamamak için tedbir amaçlı yeniden boşlukları temizliyoruz.

```bash
Series["MOVIES"] = Series["MOVIES"].str.strip()
Movies["MOVIES"] = Movies["MOVIES"].str.strip()    
```

- Her iki veri setinde de yeni birer veri seti oluşturacağız. Bu oluşturduğumuz son veri seti olacak ve tekrar etmeyen verileri içerecek.

```bash
namesSeries = Series["MOVIES"].unique().tolist()
namesMovies = Movies["MOVIES"].unique().tolist()    
```

- Boş bir liste oluşturuyoruz. for döngüsü ile eşsiz değerlerimizin olduğu liste üstünde geziyoruz. for döngüsü içinde boş bir liste daha tanımlıyoruz. Döngümüzde her bir ismi veri setimizde arayıp ilgili sütunlarını düzenleyerek ikinci listemize ekliyoruz. Döngünün her turunda ikinci listemize eklenen verileri dıştaki listemize aktarıyoruz. Böylelikle her bir isme ait bilgiler dıştaki listemizde satır satır tutulmuş olacak. En sonunda veri setine çevirirken bunun faydasını göreceğiz.

```bash
rowsSeries = []
for i in namesSeries:
    a = []
    # İlk olarak yapımın ismini ekliyoruz.
    a.append(i)

    # Yapımın konu kategorileri her satırda aynı olduğu için yalnızca bir tanesini almamız yeterlidir.
    a.append(list(Series[Series["MOVIES"] == i]["GENRE"])[0])

    # Ana veri setimize eklediğimiz RATING_VOTES sütunu yardımıyla gerçek puanları hesaplıyoruz.
    a.append(np.round(float(Series[Series["MOVIES"] == i]["RATING_VOTES"].sum() / Series[Series["MOVIES"] == i]["VOTES"].sum()), 1))

    # Ardından toplam oy sayısını ekliyoruz.
    a.append(Series[Series["MOVIES"] == i]["VOTES"].sum())

    # Önceden oyuncu isimlerini yeni bir sütuna düzenleyerek aktarmıştık. Oyuncu isimlerinde büyük bir değişiklik olmayacağı için yalnızca bir satırı almamız yeterlidir.
    a.append(list(Series[Series["MOVIES"] == i]["Stars"])[0])

    # Yapımın süresi veya yönetmen sütunları da alınabilir fakat anlam çıkarılamayacak kadar eksik değer bulunduğu için ben eklemedim.
    rowsSeries.append(a)    
```

- Aynı işlemleri film veri setinde de yapıyoruz.    

```bash
rowsMovies = []
for i in namesMovies:
    a = []
    a.append(i)
    a.append(list(Movies[Movies["MOVIES"] == i]["GENRE"])[0])
    a.append(np.round(float(Movies[Movies["MOVIES"] == i]["RATING_VOTES"].sum() / Movies[Movies["MOVIES"] == i]["VOTES"].sum()), 1))
    a.append(Movies[Movies["MOVIES"] == i]["VOTES"].sum())
    a.append(list(Movies[Movies["MOVIES"] == i]["Stars"])[0])
    rowsMovies.append(a)    
```

- İki dolu listemiz var. Listelerimizi veri setine çeviriyoruz. 

```bash
series_df = pd.DataFrame(rowsSeries, columns = ["Names", "Genres", "Ratings", "Votes", "Stars"])
movies_df = pd.DataFrame(rowsMovies, columns = ["Names", "Genres", "Ratings", "Votes", "Stars"])    
```

- Liste şeklinde görünen konu kategorilerimizi listeden çıkartıyoruz.

```bash
series_df["Genres"] = [",".join(map(str, i)) for i in series_df["Genres"]]
movies_df["Genres"] = [",".join(map(str, i)) for i in movies_df["Genres"]]  
series_df.head()  
```

![image](https://user-images.githubusercontent.com/28548881/209812055-1539d9a2-9f21-471f-991f-a509214be26e.png)

- Oylama sayılarını tam sayı haline getiriyoruz.

```bash
series_df["Votes"] = series_df["Votes"].astype("int64")
movies_df["Votes"] = movies_df["Votes"].astype("int64")   
```

![image](https://user-images.githubusercontent.com/28548881/209812089-2cf68028-90f0-42ef-ad98-73b9d11e5547.png)

```bash
print("Toplam satır ve sütun sayısı: " + str(series_df.shape))
pd.concat([series_df.nunique().to_frame("Eşsiz Değerler"), 
           series_df.isnull().sum().to_frame("Eksik Değerler")], axis = 1,).T     
```

![image](https://user-images.githubusercontent.com/28548881/209812179-7acd1902-3836-447e-af12-3b8ee5d4d17e.png)
