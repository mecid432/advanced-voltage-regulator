# İleri Düzey Gerilim Regülatörü Geliştirme
> **Seri: Elektronik Tartışmalar — Geliştirme Odaklı Projeler Bölümü**

## Proje Fikri
* **Amaç:** Yükün küçük ya da büyük olmasından bağımsız olarak, sabit ve kararlı bir gerilim kaynağı sağlayan bir devre tasarlamak.
* **Tasarım Metodolojisi:**
  * Temel kavramlarla başlamak ve bunları test ederek incelemek.
  * Mevcut çözümlerdeki sorunları ve kısıtları tespit etmek.
  * Tespit edilen sorunları çözerek devreyi adım adım geliştirmek.
* **Geliştirme Süreci:** İlk prototipten başlayarak, tüm gereksinimleri karşılayan nihai devre modeline ulaşana kadar kademeli bir iyileştirme süreci yürütmek..
* ---

## Aşama 1: Basit Gerilim Bölücü Devresi (Voltage Divider)

Kaynak geriliminden daha düşük bir gerilime ihtiyaç duyan yükleri beslemek için gerilim düşürücü olarak tasarlanan en temel devre yapısıdır.

<img width="500" height="352" alt="image" src="https://github.com/user-attachments/assets/a921af58-038e-4db4-94fa-cbe514ee8cbe" />

### Tespit Edilen Problemler

<img width="500" height="333" alt="image" src="https://github.com/user-attachments/assets/26cb604a-b132-4448-9943-b59878782d40" />

1. **Yük Akımına Bağlı Gerilim Çökmesi:** Yükün çektiği akım arttıkça (yük direnci düştükçe), çıkış geriliminde ciddi bir zayıflama ve çökme meydana gelmektedir.
2. **Kararsız Çıkış Noktası:** Devrenin çıkış noktası, yüksek akım çeken yükler altında sabit bir gerilim seviyesini koruyamamakta ve regülasyon işlevini kaybetmektedir.
---

## Aşama 2: Gerilim İzleyici (Emitter Follower / Buffer) Katmanı Eklenmesi


İlk aşamada karşılaşılan gerilim çökmesini engellemek ve yüksek akım çeken yüklerde dahi çıkış geriliminin kararlılığını korumak amacıyla devreye tampon (buffer) görevi gören gerilim izleyici (emitter follower) modunda bir transistör eklenmiştir.

<img width="446" height="276" alt="image" src="https://github.com/user-attachments/assets/1abc4fa0-209a-497e-808e-d3c6e94eabfa" />

### Uygulanan Çözüm ve Sonuç
* **Çıkış Noktası Kararlılığı:** Bu yapı sayesinde yükün çektiği akım artsa bile çıkış noktasındaki gerilim kararlılığı korunmuş, yük doğrudan gerilim bölücü devresini çökertmekten izole edilmiştir.
* **Gerilim Düşümü Telafisi:** Transistörün baz-emetör iletim gerilimi nedeniyle oluşan gerilim kaybı göz ardı edilmemeli; bu kaybı karşılamak adına referans gerilim seviyesi uygun şekilde artırılarak dengelenmelidir.

---

### Tespit Edilen Problemler

<img width="427" height="288" alt="image" src="https://github.com/user-attachments/assets/c4ad8805-54d5-4a44-8e09-41b00f9d7b0d" /><img width="445" height="289" alt="image" src="https://github.com/user-attachments/assets/474a011e-77fe-4b9f-be48-d2a14f68c425" />


* **Giriş Gerilimi Dalgalanmalarına Karşı Savunmasızlık:** Besleme kaynağının sabit olmadığı, yükseldiği veya dalgalandığı durumlarda bu değişim doğrudan çıkışa yansımaktadır. Gerilimin kontrolsüz biçimde yükselmesi yüke aşırı gerilim binmesine ve yükün hasar görmesine (yanmasına) yol açabilir.
  ---

## Aşama 3: Zener Diyot ile Gerilim Referansının İyileştirilmesi

Giriş gerilimindeki dalgalanmaların çıkışa yansımasını önlemek amacıyla, $R_2$ direnci yerine sabit bir referans gerilimi sağlayan bir Zener diyot entegre edilmiştir.

<img width="343" height="233" alt="image" src="https://github.com/user-attachments/assets/4dbdb53f-674f-40b4-85df-974748697ab0" />

### Uygulanan Çözüm ve Sonuç
* **Giriş Dalgalanmalarının Bastırılması:** Besleme gerilimi değişse bile Zener diyot üzerindeki gerilim sabit kalarak transistörün baz ucuna kararlı bir referans seviyesi sağlar. Böylece çıkış geriliminin giriş değişimlerinden etkilenmesi engellenir.

---

### Tespit Edilen Problemler

<img width="373" height="240" alt="image" src="https://github.com/user-attachments/assets/d6cd200f-a11b-4d43-8736-c58432bba479" />

* **Yüksek Akımlarda ($1\text{ A}$ ve üzeri) Gerilim Çökmesi:** Yük yüksek akım çekmeye başladığında regülasyon bozulmakta ve çıkış gerilimi çökmektedir.
* **Transistörün Aşırı Isınması ve Hasar Riski:** Transistörün akım kazancı ($\beta$) sınırlı olduğu için, emetörden çekilen yüksek akım baz ucundan da yüksek akım çekilmesine yol açar.
* **Zener Çalışma Noktasının Bozulması:** Baz akımının aşırı artması, Zener diyota giden akımın azalmasına neden olur. Zener akımı kritik seviyenin (diz akımı) altına düştüğünde Zener regülasyon bölgesinden çıkar ve kararlı referans gerilimini koruyamaz.
---

## Aşama 4: Darlington Çifti ile Akım Kazancının Artırılması

Yüksek akım talebini karşılamak amacıyla ana transistör, daha yüksek akım taşıma kapasitesine sahip bir güç transistörü ile değiştirilmiştir. Ancak bu transistörün tek başına sürülmesi, baz ucundan hâlâ yüksek akım çekilmesine ve ana referans noktasında bozulmaya neden olmaktadır. Bu sorunu çözmek için güç transistörünü sürmek üzere standart bir transistör eklenmiş ve bir Darlington çifti yapılandırılmıştır.

<img width="444" height="342" alt="image" src="https://github.com/user-attachments/assets/c37f06a4-0941-4664-9fff-d83cd578fd4d" />


### Uygulanan Çözüm ve Sonuç
* **Yüksek Akım Kazancı:** Darlington bağlantısı sayesinde toplam akım kazancı büyük ölçüde artırılmıştır. Böylece referans noktasından (ve Zener diyottan) çekilen akım minimum seviyeye indirilerek referans geriliminin kararlılığı korunmuştur.

---

### Tespit Edilen Problemler

<img width="381" height="275" alt="image" src="https://github.com/user-attachments/assets/de628a23-2500-4b43-94ee-0a198c63ff15" />

* **Çift Baz-Emetör Gerilim Düşümü ($2 \times V_{be}$):** İki transistörün seri bağlı baz-emetör jonksiyonları nedeniyle referans noktasından iki kat gerilim düşümü meydana gelmektedir.
* **Uygun Zener Diyot Bulma Zorluğu:** Çıkışta istenen gerilim değerini elde edebilmek için bu iki katlık gerilim kaybını telafi edecek tam ve hassas değerde bir Zener diyot bulmak pratikte zorlaşmaktadır.
  ---

## Aşama 5: PNP Transistör Kullanımı

Darlington yapısındaki çift baz-emetör gerilim düşümünü ve buna bağlı uygun Zener diyot bulma zorluğunu aşmak amacıyla devreye PNP tipi transistör entegre edilmiştir.

<img width="347" height="240" alt="image" src="https://github.com/user-attachments/assets/8b50acae-fd78-44fd-967c-f9208b2d333e" />


### Uygulanan Çözüm ve Sonuç
* **Gerilim Düşümü Probleminin Çözülmesi:** PNP transistör tabanlı yapı kullanılarak önceki aşamadaki çift gerilim kaybı ve Zener değerini dengeleme zorluğu giderilmiştir.

---

### Tespit Edilen Problemler
* **Negatif Besleme Kaynağı İhtiyacı / Ters Polarizasyon:** Bu yapıda devrenin çalışması negatif gerilim referansına bağlı hale gelmektedir; besleme kaynağının pozitif olması durumunda devrenin çalışması durmakta ve sistem kilitlenmektedir.
---

## Aşama 6: Push-Pull Darlington Yapısının Kullanımı

Önceki aşamadaki polarite problemini çözmek, devrenin hem pozitif hem de negatif gerilim durumlarında çalışabilmesini sağlamak ve aynı zamanda yüksek akım taşıma kapasitesini korumak amacıyla Push-Pull Darlington transistör topolojisi uygulanmıştır.

<img width="302" height="262" alt="image" src="https://github.com/user-attachments/assets/bab9f6a4-dfc6-4b6e-96c9-dbd9d533773e" />


### Uygulanan Çözüm ve Sonuç
* **Çift Yönlü Çalışma ve Yüksek Akım:** Push-Pull mimarisi sayesinde polariteye bağlı kilitlenme sorunu aşılmış, Darlington bağlantısı ile yüksek akım gereksinimi karşılanmaya devam etmiştir.

---

### Tespit Edilen Problemler
* **Hassas Gerilim Seviyesine Ulaşamama:** Önceki tüm aşamalarda olduğu gibi, çıkış gerilimi hedeflenen seviyeye ($5\text{ V}$) çok yakın olsa da tam olarak $5\text{ V}$ değerinde sabitlenememektedir.
* **Hassas Entegreler (IC) İçin Risk:** Bu küçük gerilim sapmaları, kararlı ve tam $5\text{ V}$ besleme gerilimi gerektiren hassas entegre devrelerin (ICs) beslenmesinde kararsızlıklara veya çalışma bozukluklarına yol açabilir.
---

## Aşama 7: Geri Besleme (Feedback) ve İşlemsel Yükselteç (Op-Amp) Entegrasyonu

Önceki aşamalardaki gerilim sapmalarını tamamen ortadan kaldırmak, çıkış gerilimini tam olarak hedeflenen değerde tutmak ve kararlılığı sağlamak amacıyla sisteme bir geri besleme (feedback) döngüsü eklenmiştir. Bu kontrol mekanizmasını gerçekleştirmek için en uygun çözümlerden biri olan işlemsel yükselteç (Op-Amp) tercih edilmiştir; çünkü Op-Amp, yapısı gereği giriş uçlarındaki gerilim farkını sıfırlamaya (girişleri eşit tutmaya) çalışır.

<img width="480" height="325" alt="image" src="https://github.com/user-attachments/assets/ad1820bb-85d5-416f-83db-9b3abb1396a3" />


### Uygulanan Çözüm ve Sonuç
* **Hassas Gerilim Regülasyonu:** Negatif geri besleme döngüsü sayesinde çıkış gerilimi sürekli izlenmekte, referans gerilim ile karşılaştırılarak çıkıştaki sapmalar anlık olarak düzeltilmektedir. Böylece çıkışın tam olarak istenen değerde kalması hedeflenmiştir.

---

### Tespit Edilen Problemler
* **Yetersiz Çıkış Akımı:** Piyasada yaygın olarak bulunan çoğu işlemsel yükseltecin çıkış akımı sınırlıdır ve yüksek akım gerektiren yükleri doğrudan süremez. Bu kısıt, devrenin yeniden akım yetersizliği problemine dönmesine neden olmaktadır.
---

## Aşama 8: İzleyici (Follower / Buffer) Devresi ile Akım Kapasitesinin Artırılması

İşlemsel yükseltecin (Op-Amp) çıkış akımının yetersiz kalması problemini çözmek amacıyla, Op-Amp çıkışına tampon görevi gören bir gerilim izleyici (follower) devresi entegre edilmiştir.

<img width="365" height="235" alt="image" src="https://github.com/user-attachments/assets/3fff5eb7-e83d-4cc2-8df3-efe1ca1b1117" />


### Uygulanan Çözüm ve Sonuç
* **Akım Güçlendirme:** İzleyici katmanı sayesinde Op-Amp hassas gerilim kontrolünü ve geri besleme görevini sürdürürken, yükün ihtiyaç duyduğu yüksek akım bu tampon katman üzerinden sağlanmış ve akım yetersizliği sorunu çözülmüştür.
---

## Aşama 9 (Nihai Aşama): Koruma ve Güvenlik Devrelerinin Entegrasyonu

Bu son aşamada, devrenin zorlu çalışma koşullarında güvenli bir şekilde çalışabilmesi ve bileşenlerin zarar görmesini engellemek için koruma devreleri sisteme entegre edilmiştir.

### 1. Akım Sınırlama ve Termal Kaçak (Thermal Runaway) Koruması
* **Problem:** Transistörden yüksek akım çekildiğinde sıcaklığı yükselir. Transistörün yapısı gereği artan sıcaklık çekilen akımı daha da artırır ve bu durum sıcaklık ile akımın birbirini sürekli tetiklediği bir kısır döngüye (termal kaçak) yol açarak transistörün yanmasına neden olur.
* **Uygulanan Çözüm:** Yük akımını güvenli çalışma sınırları içinde tutan bir akım sınırlama (Current Limiting) katmanı eklenmiştir. Bu mekanizma, akımı transistöre zarar vermeyecek güvenli bir maksimum eşik değerinde (örneğin $1\text{ A}$) sınırlandırarak devreyi korur.

### 2. Kısa Devre ve Aşırı Gerilim Koruması
* **Problem:** Beslenen yük devresinde meydana gelebilecek olası bir arıza durumunda, hat kısa devreye geçebilir ve kaynaktan kontrolsüzce maksimum akım çekilmesine neden olabilir.
* **Uygulanan Çözüm:** Beklenmeyen kısa devre veya besleme tarafındaki aşırı gerilim durumlarına karşı devreyi ve yükü korumak amacıyla aşırı gerilim ve arıza koruma mekanizmaları eklenerek devre nihai, güvenli ve kararlı formuna ulaştırılmıştır.

<img width="487" height="329" alt="image" src="https://github.com/user-attachments/assets/c25c6891-f402-4376-bb7a-b2c7d22ce37b" />
---

## Aşama 10: Baskı Devre Kartı (PCB) Tasarımı ve Yerleşim İlkeleri

Tasarımı doğrulanan nihai regülatör devresinin fiziksel bir karta (PCB) dönüştürülmesinde, sinyal bütünlüğü, ısıl yönetim ve kararlılık kriterleri dikkate alınmıştır.

 <img width="500" height="298" alt="image" src="https://github.com/user-attachments/assets/e674abcd-e4b3-49b3-b5cf-b6cb1289f84c" />
<img width="354" height="383" alt="image" src="https://github.com/user-attachments/assets/c12c2442-9d6c-4297-8ce6-92033b4365ae" />


