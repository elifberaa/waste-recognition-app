♻️ Derin Öğrenme Tabanlı Atık Türü Tanıma Sistemi (Waste Type Recognition System)

Bu proje, geri dönüşüm süreçlerinde doğru atık ayrıştırılmasını desteklemek ve kullanıcı hatalarını en aza indirmek amacıyla geliştirilmiş görüntü işleme ve derin öğrenme tabanlı bir nesne algılama sistemidir. Kullanıcı tarafından sağlanan tekil görüntüler veya gerçek zamanlı kamera akışı üzerinden atıkları tespit ederek sınıflandırmaktadır.

🎯 Projenin Amacı ve Kapsamı

Artan nüfus ve tüketim alışkanlıkları, çevresel sürdürülebilirliği tehdit etmektedir. Geri dönüşüm süreçlerinin verimliliğini düşüren en büyük etkenlerden biri atıkların kaynağında yanlış sınıflandırılmasıdır.

Bu çalışma kapsamında:

Kampüs ortamında çekilen özgün atık görüntülerinden bir veri seti oluşturulmuştur.

Literatürdeki benzer çalışmalardan farklı olarak Pil sınıfını da içeren 5 sınıflı bir yapı kurulmuştur.

Mobil ve gömülü sistemlerde gerçek zamanlı çalışabilecek, hız/performans dengesi en yüksek olan YOLOv8 Nano (v8n) mimarisi kullanılarak yüksek doğruluklu bir model eğitilmiştir.

🗑️ Tespit Edilen Atık Sınıfları ve Veri Seti

Veri seti, İstanbul Sabahattin Zaim Üniversitesi kampüs ortamında çekilen özgün görüntülerden oluşturulmuş ve Roboflow platformu kullanılarak YOLO formatında etiketlenmiştir.

Sınıflar (5 Sınıf):

🍾 Cam (Glass)

🥫 Metal (Metal)

🥤 Plastik (Plastic)

📄 Kâğıt (Paper)

🔋 Pil (Battery)

Veri Seti Dağılımı:
Modelin genelleme yeteneğini artırmak amacıyla veri seti dengeli bir şekilde üç alt kümeye ayrılmıştır:

%80 Eğitim (Train)

%10 Doğrulama (Validation)

%10 Test (Test)

🛠️ Metodoloji ve Mimari

Kullanılan Çerçeve ve Araçlar

Derin Öğrenme Çerçevesi: Ultralytics YOLOv8

Programlama Dili: Python

Veri Etiketleme & Yönetim: Roboflow

Eğitim Ortamı: Google Colab (NVIDIA Tesla T4 GPU)

Mobil Entegrasyon: TensorFlow Lite (TFLite - ~6MB model boyutu)

YOLOv8 Mimarisi

Tek aşamalı (one-stage) bir nesne algılama mimarisi olan YOLOv8, giriş görüntüsünü tek bir ileri besleme adımında işleyerek sınıf ve sınırlayıcı kutu (bounding box) tahminlerini üretir:

Backbone: Görüntüden ayırt edici özelliklerin (feature extraction) çıkarılmasını sağlar.

Neck: Farklı ölçeklerdeki özellik haritalarını birleştirerek çok ölçekli nesne algılamayı mümkün kılar.

Head: Nesnelerin sınıf etiketlerini ve konum bilgilerini tahmin eder.

📊 Model Seçimi ve Karşılaştırmalı Analiz

Proje geliştirme sürecinde YOLOv8n, YOLOv9c, YOLOv11n ve YOLOv12n mimarileri aynı veri seti üzerinde test edilmiştir.

Yapılan deneysel çalışmalarda daha güncel modellerin tespit doğruluğunda (mAP) istatistiksel olarak anlamlı bir fark yaratmadığı; buna karşın mobil cihazlara entegrasyon (TFLite dönüşümü) ve çıkarım süresi (inference time) açısından YOLOv8n modelinin en kararlı ve optimize sonuçları verdiği gözlemlenmiştir.

Model Türü

Parametre Sayısı

Eğitim Süresi (Dk)

$mAP_{50}$ (Genel Doğruluk)

$mAP_{50-95}$

YOLOv8n (Seçilen Model)

~3.0 M

42

0.847

0.654

YOLOv11n

~2.6 M

45

0.845

0.642

YOLOv9c

~25.3 M

125

0.843

0.628

YOLOv12n

~2.5 M

52

0.836

0.644

Analiz Notu: YOLOv9c gibi yüksek parametreli modellerin bu ölçekteki veri setlerinde aşırı öğrenme (overfitting) eğilimi gösterdiği ve gereksiz işlem yükü oluşturduğu, YOLOv8n'in özellik çıkarım katmanlarının ise veri setindeki nesne boyutlarıyla en iyi uyumu sağladığı tespit edilmiştir.

📈 Deneysel Sonuçlar ve Performans Metrikleri

Model eğitimi, Google Colab üzerinde T4 GPU kullanılarak epochs=50, imgsz=640 ve batch=16 hiperparametreleri ile gerçekleştirilmiştir. İlk 10 epoch içerisinde hızlı bir öğrenme gerçekleşmiş, 40. epoch'tan sonra model yakınsamasını (convergence) tamamlayarak stabilize olmuştur.

Genel Performans (50. Epoch Sonu - best.pt)

$mAP_{50}$ (Mean Average Precision @ 0.50): %84.7

$mAP_{50-95}$ (Daha Sıkı Kesişim Oranı): %65.4

Precision (Kesinlik): %83.3

Recall (Duyarlılık): %78.4

Sınıf Bazlı Başarı Oranları

Sınıf

Görüntü Sayısı

Precision (P)

Recall (R)

$mAP_{50}$

$mAP_{50-95}$

Tümü (Genel)

123

0.833

0.784

0.847

0.654

🥫 Metal

30

0.915

0.902

0.943

0.731

🔋 Pil

21

0.878

0.898

0.940

0.819

🍾 Cam

26

0.826

0.878

0.906

0.699

📄 Kâğıt

27

0.938

0.688

0.826

0.631

🥤 Plastik

33

0.606

0.556

0.618

0.390

Sonuçların Yorumlanması

En Başarılı Sınıflar (Metal %94.3 & Pil %94.0): Bu nesnelerin şekilsel rijitliği, standart geometrik formları (silindirik pil, teneke kutu formu) ve belirgin yüzey dokuları modelin öğrenmesini kolaylaştırmıştır.

Zorlayıcı Sınıf (Plastik %61.8): Şeffaf pet şişelerin, buruşmuş naylonların ve farklı renkli ambalajların yüksek görsel çeşitliliği nedeniyle tüm YOLO modellerinde en düşük başarı bu sınıfta görülmüştür. Şeffaf plastiklerin Cam sınıfı ile, buruşuk ambalajların ise Kâğıt sınıfı ile karışma eğilimi gösterdiği gözlemlenmiştir.

🚀 Kurulum ve Kullanım

Projeyi yerel ortamınızda veya Google Colab üzerinde çalıştırmak için aşağıdaki adımları izleyebilirsiniz:

1. Gereksinimlerin Yüklenmesi

pip install ultralytics roboflow opencv-python


2. Modelin Eğitilmesi (Train)

from ultralytics import YOLO

# YOLOv8 Nano modelini yükle
model = YOLO("yolov8n.pt")

# Modeli eğit
results = model.train(
    data="data.yaml",
    epochs=50,
    imgsz=640,
    batch=16,
    device=0 # GPU kullanımı için
)


3. Tahmin / Çıkarım (Inference) Yapılması

Eğitilen en iyi ağırlık dosyası (best.pt) ile tekil görüntüler veya kamera akışı üzerinde tespit yapabilirsiniz:

from ultralytics import YOLO

# Eğitilmiş modeli yükle
model = YOLO("runs/detect/train/weights/best.pt")

# Tekil görüntü üzerinde tahmin
results = model.predict(source="ornek_atik.jpg", save=True, conf=0.50)

# Gerçek zamanlı webcam akışı üzerinde tahmin
# results = model.predict(source="0", show=True, conf=0.50)


4. Mobil Cihazlar İçin TFLite Dönüşümü

# Modeli mobil uygulama entegrasyonu için TFLite formatına dışa aktar (~6MB)
model.export(format="tflite")


🔮 Gelecek Çalışmalar (Future Work)

Plastik Sınıfı İyileştirmeleri: Plastik sınıfındaki başarıyı artırmak adına veri artırma (data augmentation) yöntemlerinin çeşitlendirilmesi ve şeffaf nesneler için özel görüntü filtreleme tekniklerinin uygulanması.

Veri Seti Genişletme: Farklı ışık koşulları ve karmaşık arka planlara sahip daha fazla görüntünün veri setine dahil edilmesi.

İleri Seviye Analizler: Çoklu nesne senaryolarında (üst üste binmiş atıklar) daha hassas tespit için örneklem segmentasyonu (Instance Segmentation) yaklaşımlarının uygulanması.

👥 Hazırlayanlar ve Akademik Bilgiler

Bu çalışma, İstanbul Sabahattin Zaim Üniversitesi - Yazılım Mühendisliği Bölümü bünyesinde hazırlanmıştır.

Merve AYDIN - 030723095

Elif Beraa ÇIĞRIKÇI - 030721059

📚 Kaynakça

Redmon, J., Divvala, S., Girshick, R., & Farhadi, A. (2016). You Only Look Once: Unified, Real-Time Object Detection. Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR).

Bochkovskiy, A., Wang, C. Y., & Liao, H. Y. M. (2020). YOLOv4: Optimal Speed and Accuracy of Object Detection. arXiv preprint arXiv:2004.10934.

Ultralytics. (2023). YOLOv8 Documentation. https://docs.ultralytics.com

Roboflow. (2023). Roboflow Documentation. https://roboflow.com
