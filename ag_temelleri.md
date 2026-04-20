                                        **AĞ TEMELLERİ - NETWORKING**
# 1. İletişimin Temeli: IP ve Port Nedir?

    Bir bilgisayarın ağ üzerindeki başka bir bilgisayarla konuşabilmesi için önce onu bulması, sonra da doğru kapıyı çalması gerekir.

    ## IP (Internet Protocol) Adresi:

        - Ağa bağlı her cihazın (bilgisayar, telefon, sunucu) sahip olduğu benzersiz kimlik numarasıdır. Bunu bir apartmanın veya sokağın açık adresi gibi düşünebilirsin. Veri gönderirken hedef cihazın IP adresini (örneğin:         192.168.1.15 veya 104.21.45.120) bilmen gerekir.
    
    ## Port:

        - IP adresi seni doğru daireye (bilgisayara) ulaştırır. Ancak o binanın içinde bir sürü daire ve o dairelerde çalışan farklı şirketler (uygulamalar) vardır. Hangi uygulamanın kapısını çalacaksın? İşte Port, o binadaki daire numarasıdır.
        - Bir bilgisayar aynı anda hem web sitesi yayınlıyor hem de e-posta alıyor olabilir. Dışarıdan gelen verinin hangi programa ait olduğunu portlar belirler.

        Örnek: Geliştirdiğin bir web sayfasını tarayıcıdıdan açtığında genellikle 80 (HTTP) veya 443 (HTTPS) numaralı portu kullanırsın. Kendi bilgisayarında Python ile bir sunucuyu ayağa kaldırdığında ise genellikle 5000 veya 8000 numaralı portlardan yayın yaparsın.

----------------------------------------------------------------------------------------

# 2. İnternetin Telefon Rehberi: DNS (Domain Name System)

    İnsan beyni 142.250.190.46 gibi karmaşık IP adreslerini akılda tutmakta zorlanır, ancak google.com adresini kolayca hatırlar. Bilgisayarlar ise sadece IP adreslerinden anlar.
    DNS, girdiğin alan adlarını (domain) cihazların anlayabileceği IP adreslerine çeviren devasa ve dağıtık bir telefon rehberidir. Tarayıcına bir adres yazdığında arka planda şu yaşanır:

        1. Bilgisayarın DNS sunucusuna sorar: "Bana github.com'un IP adresini söyler misin?"
        2. DNS sunucusu rehbere bakar ve cevap verir: "Evet, IP adresi 140.82.113.3"
        3. Senin bilgisayarın artık doğrudan bu IP adresine giderek iletişim kurar.

----------------------------------------------------------------------------------------

# 3. Veri Nasıl Taşınır? TCP ve UDP

    IP adresini bulduk ve veriyi göndereceğiz. Peki bu veriyi nasıl bir yöntemle göndereceğiz? Karşımızdakinin veriyi alıp almadığını umursayacak mıyız, yoksa sadece hızlıca fırlatıp geçecek miyiz? İşte Taşıma Katmanı (Transport Layer) protokolleri burada devreye girer.

    ## TCP (Transmission Control Protocol) - Güvenilir Kurye

        - Mantığı: TCP, verinin karşı tarafa eksiksiz, sırasıyla ve hatasız ulaşacağını garanti eder. İletişim başlamadan önce iki cihaz arasında "3-Way Handshake" (Üçlü El Sıkışma) adı verilen bir bağlantı kurulur:

            1. Cihaz A: "Seninle konuşmak istiyorum, müsait mmisin?"
            2. Cihaz B: "Seni duydum, evet müsaaitim, başlayalım."
            3. Cihaz A: "Harika, veriyi göndermeye başlıyorum."
        
        - Veri yolda kaybolursa, TCP o veriyi tekrar gönderir.
        - Kullanım Alanı: Web sitelerinde gezinirken (HTTP/HTTPS), dosya indirirken veya e-posta gönderirken verinin eksiksiz olması şarttır. Bu yüzden buralarda TCP kullanılır.
    
    ## UDP (User Datagram Protocol) - Hızlı ve Umursamaz Postacı:

        - Mantığı: UDP, bağlantı kurmakla veya verinin ulaşıp ulaşmadığını kontrol etmekle vakit kaybetmez. Veriyi alır ve "ateşle ve unut" (fire and forget) mantığıyla karşı tarafa yollar.
        - Hata kontrolü yapmadığı için TCP'den çok daha hızlıdır ancak paketlerin yolda kaybolma veya sırasının karışma ihtimali vardır.
        - Kullanım Alanı: Online rekabetçi oyunlar, canlı yayınlar veya video konferanslar. Bir video konferansta 1 saniye önceki görüntünün kaybolan pikselini beklemek (TCP mantığı), yayının donmasına sebep olur. Bunun yerine o anı atlayıp canlı akışa devam etmek (UDP mantığı) tercih edilir.

----------------------------------------------------------------------------------------

# 4. Paket Yapısı (Packet Structure) Nasıl Çalışır?

    İnternette 1 GB'lık bir film indirirken, bu dosya tek bir devasa blok halinde gelmez. Ağ cihazlarının (router, switch) bunu işleyebilmesi için veri küçük parçalara bölünür. Bu parçaların her birine Paket (Packet) denir.
    Bir paket temelde iki kısımdan oluşur:

        1. Header (Başlık): Tıpkı bir kargo paketinin üzerindeki etiket gibidir. Üstveri (metadata) içerir.
            - Gönderenin IP adresi ve Portu
            - Alıcının IP adresi ve Portu
            - Protokol türü (TCP mi, UDP mi?)
            - Paket numarası (Paketler karşı tarafta tekrar doğru sırayla birleştirilebilsin diye)
            - Checksum (Verinin yolda bozulup bozulmadığını kontrol eden matematiksel bir sağlama değeri).
        2. Payload (Yük): Asıl taşınan veridir. Geliştirdiğin bir web sayfasının HTML ve CSS kodlarının bir kısmı, ya da bir resmin pikselleri bu kısmın içindedir.

----------------------------------------------------------------------------------------

# 5. Teşhis ve Analiz Araçları - Ping, Traceroute, Nslookup

    Sistemlerin çalışıp çalışmadığını anlamak veya sorunları tespit etmek için komut satırından (CLI) kullandığımız hayat kurtarıcı araçlar vardır:

    ## Ping:

        Ağdaki bir cihazın "hayatta" ve ulaşılabilir olup olmadığını test etmenin en temel yoludur. Karşı tarafa küçük bir ICMP (Internet Control Message Protocol) yankı paketi gönderir ve "Orada mısın?" diye sorar. Cihaz oradaysa "Buradayım" diye cevap verir. Aynı zamanda bu paketin gidiş-dönüş süresini (milisaniye cinsinden) ölçerek gecikmeyi (latency/ping süresi) görmeni sağlar.

    ## Traceroute (Windows'ta tracet):

        Senin bilgisayarından çıkan bir paketin, hedef sunucuya (örneğin bir Google sunucusuna) ulaşana kadar hangi yönlendiricilerden (router) ve ağlardan sırayla geçtiğini adım adım listeler. Eğer bağlantıda bir yavaşlık veya kopukluk varsa, paketin yolculuğun hangi durağında takıldığını bulmak için kullanılır. Paketin yaşam süresini (TTL - Time to Live) her adımda artırarak bu haritayı çıkartır.

    ## Nslookup (Name Server Lookup):

        Daha önce bahsettiğimiz DNS sistemini manuel olarak sorgulamanı sağlar. Komut satırına "nslookup wikipedia.org" yazdığında, sistem senin için DNS sunucusuna gider ve "Bu alan adının arkasında hangi IP adresleri var?" sorusunun cevabını getirir. Web projelerinde domaşn yönlendirmeleri yaparken DNS kayıtlarını (A, CNAME vb.) doğru oturup oturmadığını test etmek için çok sık kullanılır.


