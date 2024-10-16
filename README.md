<h1 align="center">Flip Clock HTML CSS JS</h1>


<h2 align="center">Web sitesini ziyaret et

<a href="https://mustafakahsofficial.github.io/FlipClock_src_code/"><img src="https://img.icons8.com/?size=100&id=5X9gz4zZu1PV&format=png&color=000000" /></a>




 
</h2>

<details >
<summary align="center">FlipClock kodunun JavaScript satırları ne işe yarıyor açıklaması</summary>
  
### 1. Plugin Başlangıcı

```jsx
(function($) {
    var pluginName = 'flipclock';
```

- **Anlamı:** Bu satırda, anonim bir fonksiyon tanımlanıyor. `$` parametresi jQuery'yi ifade eder. `pluginName` değişkeni `flipclock` ismini tutuyor, yani bu eklentinin adı.

### 2. `methods` Nesnesi

```jsx
var methods = {
```

- **Anlamı:** `methods` nesnesi, flip clock işlevlerini barındıran bir nesne tanımlar. İçinde kullanılacak tüm fonksiyonlar burada toplanır.

### `pad` Fonksiyonu

```jsx
pad: function(n) { return (n < 10) ? '0' + n : n; },
```

- **Anlamı:** Bu fonksiyon, bir sayıyı iki haneli hale getirmek için kullanılır. Örneğin, 5'i '05' olarak döndürür. Zaman göstergelerinde, örneğin dakikada 1 haneli bir sayı olduğu zaman (5) '05' olarak gösterilmesini sağlar.

### `time` Fonksiyonu

```jsx
time: function(date) {
            if (date) {
                var e = new Date(date);
                var b = new Date();
                var d = new Date(e.getTime() - b.getTime());
            } else var d = new Date
```

- **Anlamı:** Bu fonksiyon şu anki zamanı döndürür. Eğer bir `date` parametresi verilirse, bu tarihle şu an arasındaki farkı hesaplar. `e` verilen tarihi temsil eder, `b` ise şu anki zamanı temsil eder. `d` değişkeni ise bu iki tarih arasındaki farkı tutar.

```jsx
var t = methods.pad(date ? d.getFullYear() - 70 : d.getFullYear())
                    + '' + methods.pad(date ? d.getMonth() : d.getMonth() + 1)
                    + '' + methods.pad(date ? d.getDate() - 1 : d.getDate())
                    + '' + methods.pad(d.getHours())
                    + '' + methods.pad(d.getMinutes())
                    + '' + methods.pad(d.getSeconds());
```

- **Anlamı:** Burada tarih, ay, gün, saat, dakika ve saniye değerleri birleştirilip `t` değişkenine atanıyor. `methods.pad()` ile her bir değerin iki haneli olmasına dikkat ediliyor (örneğin 9'u '09' yapıyor).

```jsx
return {
				'Y': {'d2': t.charAt(2), 'd1': t.charAt(3)},
				'M': {'d2': t.charAt(4), 'd1': t.charAt(5)},
				'D': {'d2': t.charAt(6), 'd1': t.charAt(7)},
				'h': {'d2': t.charAt(8), 'd1': t.charAt(9)},
				'm': {'d2': t.charAt(10), 'd1': t.charAt(11)},
				's': {'d2': t.charAt(12), 'd1': t.charAt(13)}
};
```

- **Anlamı:** Bu satırda, `t`'den alınan her iki basamağı birer objeye ayırıyoruz. Örneğin, yılın ikinci ve birinci basamağı `Y`'de, ayın ikinci ve birinci basamağı `M`'de olacak şekilde ayarlanır.

### `play` Fonksiyonu

```jsx
play: function(c) {
            $('body').removeClass('play');
            var a = $('ul' + c + ' section.active');
            if (a.html() == undefined) {
                a = $('ul' + c + ' section').eq(0);
                a.addClass('ready').removeClass('active').next('section').addClass('active').closest('body').addClass('play');
            } else if (a.is(':last-child')) {
                $('ul' + c + ' section').removeClass('ready');
                a.addClass('ready').removeClass('active');
                a = $('ul' + c + ' section').eq(0);
                a.addClass('active').closest('body').addClass('play');
            } else {
                $('ul' + c + ' section').removeClass('ready');
                a.addClass('ready').removeClass('active').next('section').addClass('active').closest('body').addClass('play');
            }
        },
```

- **Anlamı:** Bu fonksiyon, flip animasyonlarını başlatır. Aktif olan sayfa (yani o anki zamanın gösterildiği bölüm) değiştirilir ve bir sonraki bölüme geçilir. `ready` sınıfı ve `active` sınıfı ile bu geçişler yapılır.

### `ul` ve `li` Fonksiyonları

```jsx
ul: function(c, d2, d1) {
            return '<ul class="flip ' + c + '">' + this.li('d2', d2) + this.li('d1', d1) + '</ul>';
        },
        li: function(c, n) {
            return '<li class="' + c + '"><section class="ready"><div class="up">'
                    + '<div class="shadow"></div>'
                    + '<div class="inn"></div></div>' // ve diğer HTML yapısı
```

- **Anlamı:** Bu fonksiyonlar, her bir sayı (saat, dakika, vb.) için HTML elemanlarını oluşturur. `ul` fonksiyonu, `li` elemanlarını içeren bir `ul` öğesi döndürür. Bu öğe, saat, dakika veya diğer zaman birimlerini göstermek için kullanılır.

### 3. `Plugin` Constructor (Eklenti Başlatıcısı)

```jsx
function Plugin(element, options) {
        this.element = element;
        this.options = options;
        this._name = pluginName;
        this.init();
```

- **Anlamı:** Bu fonksiyon, flip clock eklentisini başlatır. Eklenti, `element` parametresini ve `options` (ayarlar) parametresini alır.

### 4. `init` Fonksiyonu

```jsx
init: function() {
            var t, full = false;
            if (!this.options || this.options == 'clock') {
                t = methods.time();
            } else if (this.options == 'date') {
                t = methods.time();
                full = true;
            } else {
                t = methods.time(this.options);
                full = true;
            }
```

- **Anlamı:** Bu fonksiyon, flip clock'u başlatmak için kullanılır. Eğer herhangi bir seçenek verilmemişse, şu anki zamanı gösterir. Eğer 'date' verilirse, tarihi tam formatta gösterir. Diğer seçenekler de bir tarih belirtebilir.

### 5. HTML Yapısı ve Güncellemeler

```jsx
$(this.element).addClass('flipclock')
                    .html((full ? methods.ul('year', t.Y.d2, t.Y.d1)
                    + methods.ul('month', t.M.d2, t.M.d1)
                    + methods.ul('day', t.D.d2, t.D.d1) : '')
                    + methods.ul('hour', t.h.d2, t.h.d1)
                    + methods.ul('minute', t.m.d2, t.m.d1)
                    + methods.ul('second', t.s.d2, t.s.d1));
```

- **Anlamı:** Burada, saat, dakika, saniye (ve varsa yıl, ay, gün) için HTML yapıları oluşturulup sayfaya eklenir.

### 6. `refresh` Fonksiyonu

```jsx
refresh: function() {
            var el = $(this.element);
            var t = this.options && this.options != 'clock' && this.options != 'date' ? methods.time(this.options) : methods.time();

            el.find(".second .d1 .ready .inn").html(t.s.d1);
            methods.play('.second .d1');
            if ((t.s.d1 === '0')) {
                el.find(".second .d2 .ready .inn").html(t.s.d2);
                methods.play('.second .d2');
```

- **Anlamı:** Zaman her saniye yenilendiğinde bu fonksiyon tetiklenir. Güncel saat, dakika, saniye verisi alınır ve ilgili HTML elemanlarına yerleştirilir.

### 7. jQuery Plugin Başlatma

```jsx
$.fn[pluginName] = function(options) {
        return this.each(function() {
            if (!$(this).data('plugin_' + pluginName)) {
                $(this).data('plugin_' + pluginName, new Plugin(this, options));
            }
        });
    };
```

- **Anlamı:** Bu satır, jQuery eklentisini başlatır. Belirli bir HTML öğesine `flipclock` eklentisini uygular. Eğer daha önce bu öğede bu eklenti çalıştırılmamışsa, başlatılır.

### 8. FlipClock'u Çalıştırma

```jsx
$('#container').flipclock('');
```

- **Anlamı:** Bu satırda, `#container` id'li HTML öğesinde flip clock başlatılır.

</details>
