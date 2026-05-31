<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://ai.google.dev/static/site-assets/images/share-ais-513315318.png" />
</div>

# Hardened Client-Side Monte Carlo Financial Simulator

An industry-grade, serverless quantitative finance simulator engineered with high-security standards. Bu proje, kullanıcı dostu trading-terminal stilinde bir arayüz ile geleceğe yönelik yatırım tahminleri yapmayı sağlayan ve tarayıcı tabanlı (Hardened Security Posture) zırhlandırılmış bir finansal simülasyon motorudur.

**Live Production Link:** [https://tuncay-sahin.github.io/monte-carlo-simulator/](https://tuncay-sahin.github.io/monte-carlo-simulator/)

---

## Security & DevSecOps Architecture

Uygulama, sunucu bağımsız (*Serverless*) mimarilerin kullanıcı tarafında (*Client-Side*) maruz kalabileceği en kritik zafiyet vektörlerine karşı **Defense in Depth** (Derinlemesine Savunma) stratejisiyle tasarlanmıştır.

### 1. Cross-Site Scripting (XSS) & Input Validation
* **HTML Level Constraints:** Tüm girdi alanları tarayıcı manipülasyonlarını engellemek adına strict `type="number"`, `min="0"` ve `required` öznitelikleriyle kısıtlanmıştır.
* **Backend Error Catching:** Kullanıcı tarayıcı konsolu (DOM) üzerinden bu kısıtlamaları bypass etmeye çalışsa bile, PyScript motoru içerisindeki tüm veri okuma süreçleri güçlü `try-except ValueError` bloklarıyla sarmalanmıştır. Zararlı kod içeren veya numerik olmayan payload'lar sisteme sızdırılmadan anında imha edilir ve kullanıcıya güvenli hata logu basılır.

### 2. Content Security Policy (CSP) Implementation
Tarayıcı seviyesinde bir güvenlik duvarı kurularak **Data Exfiltration** (Veri Sızdırma) ve üçüncü parti zararlı eklentilerin müdahaleleri tamamen bloke edilmiştir:
* `default-src 'self'` kuralı ile tüm ham kaynak yüklemeleri projenin kendi orijiniyle sınırlandırılmıştır.
* `script-src` ve `connect-src` yönergelerinde sadece PyScript Core ve Chart.js (JSDelivr) resmi kütüphane adreslerine izin verilmiştir. WebAssembly (WASM) derleme mimarisi için zorunlu olan `'unsafe-eval'` esnekliği, girdi katmanındaki katı numerik kontrollerle dengelenmiştir.

---

## Core Technical Stack & Serverless Engine

* **Mathematical Simulation Engine:** Python (via PyScript 2024.1.1 Core). 2000 bağımsız simülasyon evrenini rastgele Gauss dağılımı (`random.gauss`) kullanarak anlık olarak hesaplar. Volatilite parametresini finansal standartlara uygun olarak `math.sqrt(12)` ile zaman ölçeğine uyarlar.
* **Data Visualization:** Chart.js v4.4.7 Responsive Line Chart. Hesaplanan 2000 evreni sıralayarak 10th (Worst Case), 50th (Median) ve 90th (Best Case) percentile yörüngelerini dinamik olarak çizer.
* **UI/UX Design:** Dark-themed financial dashboard layout featuring glassmorphism elements and custom responsive CSS.

---

## Architectural Challenges & Prompt Engineering Methodology

Bu projenin hayata geçirilmesinde geleneksel manuel kodlama yerine, bir **Orkestra Şefi** gibi yapay zekayı yönlendiren gelişmiş **Prompt Engineering** teknikleri uygulanmıştır.

### Developed with Google AI Studio & Gemini
Proje, ham algoritma tasarımından güvenlik sıkılaştırmasına kadar **Google AI Studio** ortamında, gelişmiş bir **Prompt Chaining** (Komut Zincirleme) metodolojisiyle parça parça inşa edilmiştir:
1. **Phase 1 (Core Engine):** Matematiksel simülasyon mantığı Python'da izole olarak doğrulatıldı.
2. **Phase 2 & 4 (UI & Visual Enclosure):** HTML iskeleti ve Chart.js grafik yapısı entegre edildi.
3. **Phase 3 (Bridge):** Python motoru ile JavaScript dünyası PyScript köprüsüyle birleştirildi.
4. **Phase 5 (UX Refinement):** Form resetleme, bellek yönetimi ve kullanıcı deneyimi optimize edildi.
5. **Phase 6 (Hardening):** Güvenlik denetimi (*Security Audit*) işletilerek sistem güvenli hale getirildi.
6. 
---
### Faced Challenges & Trade-offs (Karşılaşılan Zorluklar)
* **WASM vs. CSP Friction:** İlk güvenlik aşamasında katı bir CSP duvarı örüldüğü için PyScript'in WebAssembly tabanlı motoru tamamen engellendi ve sistem kilitlendi. Yapılan mimari analizle, kütüphanelerin WASM derleme gereksinimleri için `'unsafe-eval'` esnekliği tanınırken, iç kaledeki girdi filtreleri maksimum düzeye çıkarılarak risk optimize edildi (*Risk Acceptance*).
* **AI SRI Hallucination:** Modelin ürettiği sahte SRI (Subresource Integrity) hash imzalarının tarayıcı tarafından reddedilmesi problemi, zafiyet analizi sırasında tespit edilerek kütüphane çağrıları pürüzsüzleştirildi.

---
*Developed by Tuncay Şahin as a professional Business Analyst and Data Engineering Portfolio Project.*# monte-carlo-simulator
A client-side Monte Carlo Financial Simulator built with Python, PyScript, and Chart.js. Sunucu bağımsız (Serverless) çalışan interaktif yatırım tahmin ve risk analiz motoru.

```text
+-----------------------------------------------------------------------+
|                    GOOGLE AI STUDIO & GEMINI FLOW                     |
+-----------------------------------------------------------------------+
|                                                                       |
|   [ Prompt 1: Core Engine ] ──> Python Monte Carlo Simulation Logic   |
|                                     │                                 |
|                                     ▼                                 |
|   [ Prompt 2: Front-End ]   ──> Dark-Themed UI & Chart.js Enclosure   |
|                                     │                                 |
|                                     ▼                                 |
|   [ Prompt 3: PyScript Bridge]─> Unified HTML + PyScript Engine       |
|                                     │                                 |
|                                     ▼                                 |
|   [ Prompt 4: Hardening ]   ──> CSP Firewall + Strong Input Defense   |
|                                                                       |
+-----------------------------------------------------------------------+
|              NİHAİ ÜRÜN: SECURE & SERVERLESS APP (index.html)         |
+-----------------------------------------------------------------------+

