# akilli-kasa-projesi
AKILLI KASA PROJESİ - ÖDEV VE DOKÜMANTASYON METNİ
1. PROJENİN AMACI Bu proje, C dilinde geliştirilmiş temel düzeyde bir akıllı kasa (para üstü hesaplama) uygulamasıdır. Kasiyerin girdiği ürün tutarı ve müşterinin ödediği parayı alarak para üstünü hesaplar. Bilgisayar mimarisindeki ondalıklı sayı (floating-point) hassasiyet hatasını raporlar ve para üstünü en az sayıda banknot/madeni para kullanarak verir.
2. DEĞİŞKENLER VE VERİ TİPLERİNİN AÇIKLAMASI
• urun_tutari (float)
* Kullanımı: Kasiyerin klavyeden girdiği ürün fiyatını tutar.
* Veri Tipi Nedeni: Kuruşlu ve ondalıklı TL fiyatlarını (örneğin 18.70 TL) saklayabilmek için ondalıklı sayı türü olan "float" seçilmiştir.
• odenen_para (float)
* Kullanımı: Müşterinin kasiyere verdiği para miktarını tutar.
* Veri Tipi Nedeni: Müşterinin ödediği kuruşlu tutarları (örneğin 20.00 TL) saklayabilmek için "float" seçilmiştir.
• para_ustu (float)
* Kullanımı: Ödenen paradan ürün tutarı çıkarılarak bulunan para üstünü tutar (20.00 - 18.70 = 1.30 TL).
* Veri Tipi Nedeni: İki float sayının farkı yine ondalıklı bir sayı olduğu için "float" seçilmiştir.
• hatali_kurus (int)
* Kullanımı: Yuvarlama yapılmadan doğrudan tam sayıya çevrilen kuruş tutarını tutar.
* Veri Tipi Nedeni: Ondalıklı sayıların hafızadaki hassasiyet hatasını (129 kuruş) tam sayı olarak göstermek için "int" seçilmiştir.
• dogru_kurus (int)
* Kullanımı: round() fonksiyonu ile düzeltilmiş gerçek kuruş tutarını tutar (130 kuruş).
* Veri Tipi Nedeni: C dilinde mod alma (%) ve tam sayı bölmesi (/) işlemleri sadece tam sayılarla yapılabildiği için "int" seçilmiştir.
• toplam_kupur_sayisi (int)
* Kullanımı: Müşteriye verilen toplam banknot ve madeni para adedini tutar.
* Veri Tipi Nedeni: Parça sayıları buçuklu olamayacağı (tam sayı olacağı) için "int" seçilmiştir.
• adet (int)
* Kullanımı: O an hesaplanan banknot veya madeni paradan kaç adet verileceğini tutar.
* Veri Tipi Nedeni: Verilecek banknot/madeni para sayısı tam sayı olduğu için "int" seçilmiştir.
1. MANTIKSAL VE MATEMATİKSEL AÇIKLAMALAR (RAPOR KISMI)
A) Neden TL'den Kuruşa Çeviriyoruz? Ondalıklı sayılarla (float) mod alma (%) işlemi yapılamaz ve virgülden sonraki küsuratlar hesaplarda kaymaya neden olur. Para üstünü en küçük birim olan Kuruş cinsine (1 TL = 100 Kuruş) çevirip tam sayılarla (int) çalışmak, para üstü dağılımının %100 doğru yapılmasını sağlar.
B) Ondalıklı Sayı Hassasiyet Hatası (Floating-Point Precision) Nedir? C dilinde "float" türündeki sayılar bilgisayar hafızasında 2'lik (binary) tabanda saklanır. 10'luk tabandaki 1.30 sayısı 2'lik tabana tam olarak çevrilemez ve hafızada 1.299999... şeklinde tutulur.
* Hatalı Dönüşüm: (int)(para_ustu * 100) yazıldığında işlem 1.299999 * 100 = 129.9999 olur. C dilindeki (int) dönüştürme işlemi virgülden sonrasını acımasızca kesip attığı için sonuç 129 Kuruş çıkar.
* Doğru Dönüşüm: <math.h> kütüphanesindeki round() fonksiyonu kullanılarak 129.9999 sayısı en yakın tam sayıya yuvarlanır ve 130 Kuruş sonucu elde edilir.
C) Neden Bölme (/) ve Mod (%) Kullanıldı? Hocanın if/else veya for döngüsü kullanılmasını istemediği durumlarda en az sayıda küpür hesabı tam sayı matematiği ile yapılır:
* Bölme (/): Tutarın içinde o paranın kaç adet olduğunu verir (Örnek: 130 / 100 = 1 adet 1 TL).
* Mod (%): O parayı verdikten sonra geriye kaç kuruş kaldığını bulur (Örnek: 130 % 100 = 30 Kuruş kaldı).
1. KULLANILAN KÜPÜRLER
* Banknotlar: 200 TL, 100 TL, 50 TL, 20 TL, 10 TL, 5 TL, 1 TL
* Madeni Paralar: 50 Kuruş, 25 Kuruş, 10 Kuruş, 5 Kuruş, 1 Kuruş
1. PROGRAMIN ÇALIŞMA ADIMLARI
2. Kullanıcıdan "urun_tutari" ve "odenen_para" alınır.
3. while (odenen_para < urun_tutari) kontrolü ile eksik ödeme engellenir; yeterli para girilene kadar yeniden istenir.
4. para_ustu = odenen_para - urun_tutari formülüyle fark hesaplanır.
5. Yuvarlamasız (int) ve round() sonuçları ekrana basılarak hassasiyet hatası raporlanır.
6. En büyük banknottan (200 TL = 20000 Kuruş) başlanarak 1 Kuruş'a kadar sırayla bölme (/) ve mod (%) işlemleriyle küpür miktarları hesaplanır.
7. Her küpürün adedi ve en sonda toplam_kupur_sayisi ekrana yazdırılarak işlem tamamlanır.
