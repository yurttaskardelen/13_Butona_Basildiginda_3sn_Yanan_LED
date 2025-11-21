# 13_Butona_Basildiginda_3sn_Yanan_LED (Timed Output & Debounce)

Bu proje, **STM32F407-Discovery** kartı üzerinde bir butona basıldığında LED'i 3 saniye boyunca yakan ve süre sonunda otomatik olarak söndüren bir uygulamadır.

Bu depo, basit giriş-çıkış işlemlerinin ötesine geçerek **zamanlayıcı tabanlı kontrolü** ve mekanik butonlarda oluşan gürültüyü engellemek için **Yazılımsal Ark Söndürme (Software Debouncing)** tekniğini gösterir.

---

### 🎯 Proje Senaryosu ve "Debounce" Mantığı

Mekanik butonlara basıldığında, içindeki metal kontaklar milisaniyeler içinde defalarca çarpışır (buna "Bouncing" denir). İşlemci çok hızlı olduğu için bu titreşimleri birden fazla basış gibi algılayabilir.

Bu kod, bu hatayı önlemek için **çift kontrol mekanizması** kullanır:

1.  **İlk Tespit:** Butonun basıldığı (`RESET` olduğu) algılanır.
2.  **Ark Söndürme (Debounce) Beklemesi:** `HAL_Delay(200)` ile 200 ms beklenir. Bu süre zarfında mekanik titreşimlerin durması ve sinyalin kararlı hale gelmesi sağlanır.
3.  **Doğrulama:** 200 ms sonra buton hala basılı mı diye tekrar kontrol edilir.
4.  **Eylem:** Eğer hala basılıysa, bu gerçek bir basıştır. LED yakılır, 3 saniye beklenir ve söndürülür.

---

### ⚙️ Pull-Up Konfigürasyonu

Projenin düzgün çalışması için `.ioc` dosyasında buton pininin (`PA0`) **Pull-Up** olarak ayarlanması gereklidir.

* **Pin:** `PA0` -> `GPIO_Input`
* **Resistor:** `Pull-up`

<img width="843" height="644" alt="image" src="https://github.com/user-attachments/assets/a5bccc60-b813-4f18-9e9a-a4f0fd3519bf" />

---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **1x** LED
* **1x** 220 Ohm Direnç
* **1x** Push-Button
* **Breadboard ve Jumper Kablolar**

---

### 🔌 Devre Şeması

Buton bağlantısı **Pull-Up** mantığına göre (GND'ye) yapılmalıdır.

| Bileşen | STM32 Pini | Bağlantı Detayı |
| :--- | :--- | :--- |
| **Buton** | `PA0` | Bir bacak **PA0**, diğer bacak **GND** |
| **LED** | `PA1` | Anot -> **PA1**, Katot -> Direnç -> **GND** |

<img width="403" height="560" alt="image" src="https://github.com/user-attachments/assets/1f49cc65-e2c6-4f64-a080-7d3470171d79" />


---

### 💻 Kod Bloğu

<img width="597" height="344" alt="image" src="https://github.com/user-attachments/assets/21957bd6-adee-4a43-a0be-2b703b96a7d1" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
