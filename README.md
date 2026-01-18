#AI-Powered Vehicle Access Control System (Akıllı Araç Erişim Sistemi)

Bu proje; Nesne Tespiti (YOLOv8), Optik Karakter Tanıma (OCR) ve Görsel Dil Modelleri (VLM)
teknolojilerini bir araya getirerek hibrit bir araç güvenlik sistemi oluşturur. Sistem, araçları tespit eder,
sınıflandırır (Tır/Araba), plakalarını okur, yetki kontrolü yapar ve araç hakkında yapay zeka destekli bir betimleme oluşturur.

#Özellikler
1-)Araç Tespiti & Sınıflandırma: YOLOv8 modeli ile görüntüdeki araçlar tespit edilir ve "Car" (Araba) veya "Truck" (Tır/Kamyon) olarak sınıflandırılır.
2-)Plaka Tanıma: Tespit edilen aracın görüntüsü kırpılır ve EasyOCR ile plaka üzerindeki metin dijital veriye dönüştürür.
3-)Whitelist (Beyaz Liste) Kontrolü: Okunan plaka, önceden tanımlanmış izinli araçlar listesiyle karşılaştırılır. Eşleşme durumuna göre "ONAYLANDI" veya "REDDEDİLDİ" kararı verilir.
4-)VLM Entegrasyonu: Salesforce BLIP modeli kullanılarak, aracın görsel özellikleri (renk, tip, konum vb.) analiz edilir ve İngilizce bir özet (caption) çıkarılır.
5-)CPU & GPU Desteği: Eğer colab üzerinden çalıştıracaksanız üyeliğiniz olmasa dahi çalıştırabilirsiniz. Proje, CUDA (GPU) varsa otomatik kullanır; yoksa CPU üzerinde sorunsuz çalışacak şekilde optimize edilmiştir. 

#Projenin Çalışma Adımları:
Arkaplanda dönen çalışma mekanikleri günlük hayattan kolayca anlaşılabilecek şekilde anlatacak olursam:

1.Öncelikle Object Detection (Nesne Tespiti) yapıyoruz. Burada Ultralytics YOLO teknolojisini kullandım. Hızlı çalışması amacıyla da YOLOv8n (Nano) kullandım. Bu sayede aracın yerini ve tipini (araba/tır) buluyoruz.
2.Daha sonra Optik Karakter Tanıma (OCR) işlemini gerçekleştiriyoruz. Burada da EasyOCR teknolojisini kullandım. Bu sayede plaka metnini okuyabiliyoruz.
3.Ardından Görsel Dil Modelini (Vision Language Model) etkinleştiriyoruz. Bu da bir transformer yardımıyla gerçekleştiriliyor (HuggingFace Transformers). Buradaki kullandığım araç da Salesforce BLIP oldu. Bu sayede görseli yorumlayabiliyoruz.
4.Görüntü İşleme işlemiyle de birlikte. Görüntü manipülasyonu ve çizim işlemini gerçekleştiriyoruz. Bu sayede görselin üzerindeki çizilen şekilleri oluşturabildik. OpenCV & PIL teknolojileri kullanıldı.

#Kurulum
Projeyi yerel makinenizde veya Google Colab'da çalıştırmak için aşağıdaki adımları izleyin.

A. Bu depoda bulunan .ipynb uzantılı GDM_Proje Jupyter Notebook'u indirin. Ardından bunu colab üzerinden açarsanız tarayıcınız üzerinden ücretsiz sürümü olsa dahi CPU desteğine uygun hale getirdiğim için çalıştırabilirsiniz.
Eğer aboneliğiniz var ise GPU seçerek daha hızlı çalıştırabilirsiniz.

B. Yine bu depoda bulunan .py uzantılı GDM_Proje Python Script'ini indirerek bilgisayarınızda çalıştırabilirsiniz.

#Konfigürasyon ve Kullanım
1. Whitelist Ayarı:
İzin vermek istediğiniz plakaları Whitelist'e (izin verilen plakalar listesi) eklemeniz gerekiyor. Ben örnek olarak bu depoda 3 adet görsel yükledim. Bunlardan sadece birisi Whitelist'te bulunuyor. Örnek uygulamayı deneyerek gözlemleyebilirsiniz.
Kendiniz de bir plaka eklemek isterseniz listeye plakayı boşluksuz bir şekilde yazmanız yeterli.

2. Güven Eşiği (Confidence Threshold)
Sistem varsayılan olarak %40 (0.4) üzerindeki metin okuma olasılıklarını plaka olarak kabul eder. Bu ayarı değiştirmek için analyze_vehicle fonksiyonundaki satırı istediğiniz sayı olarak değiştirmelisiniz.
Bu hassasiyeti değiştirirseniz arabaları daha kolay bulabilirsiniz ancak bu sefer de hatalı bir bulma işlemi gerçekleşebilir.

#Örnek Çıktı
Projeyi çalıştırdığınızda sizden görsel yüklemenizi isteyecek. Burada kendi görsellerinizi seçerek bunları drive'a yüklemiş oluyorsunuz. Program daha sonra bunları drive'a kaydediyor.
Ardından da projenin çalışma aşamaları gerçekleştirilip şöyle bir çıktı oluşuyor:
>>> araba1.png Analiz Ediliyor...
Araç Tipi: ARABA
Okunan Plaka: 16BZC729
Erişim Durumu: ONAYLANDI
Yapay Zeka Yorumu: a photo of a vehicle parked in a parking lot with a license plate
------------------------------

>>> araba2.png Analiz Ediliyor...
Araç Tipi: ARABA
Okunan Plaka: OKUNAMADI
Erişim Durumu: REDDEDİLDİ
Yapay Zeka Yorumu: a photo of a vehicle parked in a parking lot with a sky reflection on the windshield
------------------------------

>>> tır (1).png Analiz Ediliyor...
Araç Tipi: TIR/KAMYON
Okunan Plaka: OKUNAMADI
Erişim Durumu: REDDEDİLDİ
Yapay Zeka Yorumu: a photo of a vehicle driving down a road with a mountain in the background
------------------------------

<img width="523" height="620" alt="image" src="https://github.com/user-attachments/assets/d034bc68-f203-4355-a726-90405d230b7d" />

<img width="462" height="413" alt="image" src="https://github.com/user-attachments/assets/156f1043-e189-4ea0-aacc-ed6edc6d4e4c" />

<img width="462" height="565" alt="image" src="https://github.com/user-attachments/assets/f1a5358d-b2d3-4762-b97c-094004cfb650" />

Burada yüklediğiniz görsellerin net ve OCR'ın plakayı okuyabileceği şekilde olmasına dikkat edin. Çıktılarda da görüldüğü üzere öncelikle aracın tipini belirliyoruz. Ardından Plakayı okuyoruz ve bu plaka Whitelist'te ise izin veriyoruz, değilse izin yok.
Son olarak da VLM ile aracın bir yorumunu yapıyoruz.

--------------------------------
AD-Soyad: Kubilay İnanç
Numara:22360859047
3.Sınıf BTÜ Bilgisayar Mühendisliği Öğrencisi

-Bu projeyi gerçekleştirmeden önce bana bilgi katan öğretmenime teşekkürler ve bu süreçte aldığım kurslar:

[Sıfırdan_İleri_Seviye_Python_Programlama_Sertifika.pdf](https://github.com/user-attachments/files/24696193/Sifirdan_Ileri_Seviye_Python_Programlama_Sertifika.pdf)

[Makine_Öğrenmesi_Sertifika.pdf](https://github.com/user-attachments/files/24696194/Makine_Ogrenmesi_Sertifika.pdf)

[Yapay_Zekaya_Giriş_Sertifika.pdf](https://github.com/user-attachments/files/24696195/Yapay_Zekaya_Giris_Sertifika.pdf)

[Büyük_Dil_Modellerine_Giriş_Sertifika.pdf](https://github.com/user-attachments/files/24696196/Buyuk_Dil_Modellerine_Giris_Sertifika.pdf)
