# Gün Projesi: Dinamik Metinler ve Resimler

Bu repo; **JS DOM** kullanarak, tek bir `index.js` dosyası üzerinden HTML belgesine **dinamik metinler ve resimler** eklemeyi gösteren küçük bir çalışma içerir. Hedef, verilen `siteContent` nesnesini **DOM API** ile okuyup arayüzdeki ilgili elemanlara yerleştirmektir. Stil ve semantik yapı hazırdır; **yalnızca JavaScript** dosyasında çalışılır.

> Tasarım referansı: [https://i.ibb.co/9qfK22h/sayfa.png](https://i.ibb.co/9qfK22h/sayfa.png)
> Hedef HTML şeması: `original.html`

---

## 🗂️ Proje Yapısı

```
.
├── index.html      # Çalışan sayfa (container, header, nav, sections, footer)
├── original.html   # Hedeflenen HTML şeması (referans)
├── index.css       # Stil dosyası (tablet/telefon kırılımları dahil)
├── reset.css       # CSS reset
└── index.js        # Tüm DOM manipülasyonlarının yapıldığı tek dosya
```

---

## 🎯 Amaç

* `siteContent` nesnesindeki metin ve görsel kaynaklarını **doğru elementlere** yerleştirmek
* Navigasyondaki linklere `italic` sınıfını **JS ile** eklemek
* Footer’daki bağlantıya `bold` sınıfını **JS ile** eklemek
* Görsellerin `src` ve `alt` niteliklerini ayarlamak

> **Not:** Tüm değişiklikler **sadece** `index.js` içinde yapılmalıdır.

---

## ⚙️ Çalıştırma

1. Bu projeyi klonlayın veya indirin.
2. Bir HTTP sunucusu kullanın **ya da** `index.html` dosyasını doğrudan tarayıcıda açın.

   * Hızlı bir yerel sunucu için (opsiyonel):

     ```bash
     # Python 3
     python -m http.server 5500
     # sonra tarayıcıda: http://localhost:5500/
     ```
3. Arayüzdeki metinler ve resimler JS aracılığıyla yüklenecektir.

---

## 🧠 `siteContent` ve DOM Eşlemesi

`index.js` dosyasında tanımlı `siteContent` nesnesi **değiştirilmeden** kullanılır. Aşağıdaki anahtarlar DOM’a şöyle yansıtılır:

### 1) Navigasyon

* `siteContent.nav['nav-item-1'..'6']` → `header nav a` etiketlerinin **textContent**
* Tüm `nav a` öğelerine: `.classList.add('italic')`

### 2) CTA (Call to Action)

* Başlık: `siteContent.cta.h1` → `.cta-text h1`
* Buton: `siteContent.cta.button` → `.cta-text button`
* Görsel: `siteContent.images['cta-img']` → `#cta-img.src`

### 3) Üst İçerik (Top Content)

* Sol blok başlık/içerik: `left-h4`, `left-content`
* Sağ blok başlık/içerik: `right-h4`, `right-content`

### 4) Orta Görsel

* `siteContent.images['accent-img']` → `#middle-img.src`

### 5) Alt İçerik (Bottom Content)

* `left-h4`, `left-content`
* `middle-h4`, `middle-content`
* `right-h4`, `right-content`

### 6) İletişim

* Başlık: `contact-h4`
* Adres / Telefon / E‑posta: `address`, `phone`, `email`

### 7) Footer

* Metin: `siteContent.footer.copyright` → `footer a`
* Sınıf: `footer a` → `.classList.add('bold')`

### 8) Logo

* `siteContent.images['logo-img']` → `#logo-img.src`

---

## 🧩 Kullanılan DOM Seçicileri ve Yöntemleri

* `document.querySelector`, `document.querySelectorAll`
* `Element.textContent`, `HTMLImageElement.src`
* `Element.classList.add`
* NodeList üzerinde basit **for** döngüsü ve `forEach`

> Bu yaklaşım, **sıra bağımlılığını** (örn. `nav-item-1..6`) dikkatli kullanır ve `NodeList` indekslerini `siteContent` anahtarlarıyla eşler.

---

## ✅ Tamamlananlar (Özet)

* [x] Navigasyondaki 6 bağlantının metinleri atandı
* [x] CTA başlığı ve buton metni atandı
* [x] Top/Bottom içerik başlık ve paragrafları atandı
* [x] Orta görsel ve CTA görseli `src` ile güncellendi
* [x] Logo görseli `src` ile güncellendi
* [x] `nav a` → `italic` sınıfı eklendi
* [x] `footer a` → `bold` sınıfı eklendi

> Proje durumu: **%100**

---

## 📸 Ekran Görüntüsü

Tasarım hedefi: [https://i.ibb.co/9qfK22h/sayfa.png](https://i.ibb.co/9qfK22h/sayfa.png)
(Responsive kırılımlar için `@media` kurallarını `index.css` içinde inceleyebilirsiniz.)

---

## 🧪 Hızlı Gözden Geçirme (Koddan Örnekler)

```js
// NAV bağlantılarını doldur
const headerLink = document.querySelectorAll('.container nav a');
for (let i = 0; i < headerLink.length; i++) {
  headerLink[i].textContent = siteContent.nav['nav-item-' + (i + 1)];
}

// CTA
document.querySelector('.cta-text h1').textContent = siteContent.cta.h1;
document.querySelector('.cta-text button').textContent = siteContent.cta.button;

// Görseller
document.getElementById('logo-img').src = siteContent.images['logo-img'];
document.getElementById('cta-img').src = siteContent.images['cta-img'];
document.getElementById('middle-img').src = siteContent.images['accent-img'];

// Sınıf eklemeleri
document.querySelectorAll('.container nav a').forEach((link) => link.classList.add('italic'));
document.querySelector('footer a').classList.add('bold');
```

> **İpucu:** `textContent` kullanımı HTML enjeksiyon riskini önler. İhtiyaç olursa satır sonu vb. için `innerHTML` tercih edilebilir; fakat bu projede gerek yoktur.

---

## 📱 Responsive Notları

* Tablet: `max-width: 768px` — `cta img` gizlenir, `middle-img` tam genişlik olur
* Telefon: `max-width: 400px` — Nav **kolona** döner, top/bottom content blokları **üst üste** dizilir

Bu davranışlar **yalnızca CSS** ile sağlanır; JS tarafında ek işleme gerekmez.

---

## 🚀 Geliştirme Önerileri (Opsiyonel)

* `siteContent` içeriğini uzak bir JSON’dan **fetch** ederek yüklemek
* `cta` butonuna **scroll** veya **modal** davranışı eklemek
* Nav linklerine **aktif** durum ve **klavye erişilebilirliği** geliştirmeleri
* İçerik alanlarında **fade‑in** animasyonları (CSS/JS)

---

## 🪪 Lisans

Bu proje eğitim amaçlıdır. Kendi projelerinizde kullanabilir, değişiklik yapabilirsiniz.
