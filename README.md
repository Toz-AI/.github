<div align="center">

# Töz AI

**Sıfırdan eğitilmiş küçük Türkçe dil modelleri**

[🤗 Hugging Face](https://huggingface.co/TozAI) · [💬 Töz Chat](https://toz-chat.onrender.com/) · [🔁 Töz-Çevir](https://github.com/Toz-AI/toz-cevir)

</div>

Töz AI, Türkçe için küçük ve açık dil modelleri geliştiriyor. Tokenizer'dan ağırlıklara kadar her şeyi
sıfırdan eğitiyoruz; hazır bir modelin üstüne ince ayar yapmıyoruz. Modeller sıradan bir bilgisayarda,
GPU olmadan da çalışacak boyutta. Hepsi Apache-2.0 ile yayınlanıyor.

## Modeller

| model | ne yapar | boyut | bağlantı |
|---|---|---|---|
| **Töz-1.5** 🆕 | Türkçe dil modeli, en yeni sürüm | 604M parametre, 15B token | [TozAI/Toz-1.5](https://huggingface.co/TozAI/Toz-1.5) |
| **Töz-Çevir-1** | İngilizce ↔ Türkçe çeviri, iki yön tek model | 74M | [HF](https://huggingface.co/TozAI/Toz-Cevir-1) · [GitHub](https://github.com/Toz-AI/toz-cevir) |
| **Töz-1** | Türkçe sohbet modeli (SFT) | 195M | [TozAI/Toz-1](https://huggingface.co/TozAI/Toz-1) |
| **Töz-1-Base** | Töz-1'in ham ön eğitim modeli | 195M | [TozAI/Toz-1-Base](https://huggingface.co/TozAI/Toz-1-Base) |

### Töz-1.5

Töz-1'in üç katı büyüklüğünde, üç kattan fazla veriyle eğitildi. Türkçe benchmark'larda,
aynı scriptle ölçülen ufakzeka-1-base ve Töz-1-Base'in önünde:

| benchmark | Töz-1.5 (604M) | ufakzeka-1-base | Töz-1-Base (195M) |
|---|---|---|---|
| HellaSwag-tr (acc_norm) | **%40,8** | %36,8 | %35,0 |
| XCOPA-tr (acc_norm) | **%60,6** | %59,8 | %55,8 |
| TurBLiMP (acc) | **%93,6** | %90,4 | %91,8 |

Turkish-MMLU bu boyutta henüz şans düzeyinde; o yüzden tabloya koymadık.

### Töz-Çevir-1

74M parametrelik İngilizce ↔ Türkçe çeviri modeli. Kaggle'ın ücretsiz 2×T4 GPU'larında,
ticari olmayan (NC) veri kullanılmadan ve başka bir çeviri modelinden damıtma yapılmadan eğitildi.
İngilizce→Türkçe'de yaklaşık üç kat büyük OPUS-MT tc-big ile aynı seviyede:

| test seti | yön | Töz-Çevir-1 (74M) | OPUS-MT base (~75M) | OPUS-MT tc-big (~235M) |
|---|---|---|---|---|
| FLORES-200 | en→tr | **60,0** | 55,5 | 59,2 |
| FLORES-200 | tr→en | 59,1 | 56,7 | **62,1** |
| WMT24++ | en→tr | **53,1** | 46,8 | 50,8 |

<sub>chrF++, her setten 500 cümle, beam 4. Ayrıntılar model kartında.</sub>

```bash
pip install git+https://github.com/Toz-AI/toz-cevir.git
python -m tozcevir --to tr "Could you please send me the report by Friday?"
```

### Töz-1

Töz AI'ın ilk modeli. Gündelik sohbet, yaygın bilgi soruları, takip soruları ve basit biçim
talimatları için. `transformers` ile doğrudan yüklenir (mimari Qwen3 olarak kayıtlı,
ağırlıklar Qwen'den türetilmedi):

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

tok = AutoTokenizer.from_pretrained("TozAI/Toz-1")
model = AutoModelForCausalLM.from_pretrained("TozAI/Toz-1")
```

## Dene

Modellerle tarayıcıdan konuşmak için: **[toz-chat.onrender.com](https://toz-chat.onrender.com/)**

> Ücretsiz sunucuda çalıştığı için ilk açılış biraz sürebilir.

## Veri setleri

| veri seti | içerik |
|---|---|
| [Toz-1-SFT](https://huggingface.co/datasets/TozAI/Toz-1-SFT) | Töz-1'in sohbet eğitimi verisi ve ölçüm setleri |
| [mihenk-3d](https://huggingface.co/datasets/TozAI/mihenk-3d) | 3D / CAD kod üretimi benchmark'ı (OpenSCAD, three.js) |
| [TR-coding-data](https://huggingface.co/datasets/TozAI/TR-coding-data) | Türkçe kodlama verisi |

## Nasıl çalışıyoruz

- **Sıfırdan eğitim.** Tokenizer dahil her şey bizim; başka bir modelin ağırlıklarından başlamıyoruz.
- **Lisans temizliği.** Ticari olmayan (NC) lisanslı veri ve öğretmen model kullanmıyoruz, böylece modeller Apache-2.0 ile yayınlanabiliyor.
- **Dürüst ölçüm.** Karşılaştırdığımız modelleri kendi scriptlerimizle, aynı ayarlarla yeniden ölçüyoruz; model kartlarında sınırlamaları açıkça yazıyoruz.
- **Az donanım.** Modeller Kaggle'ın ücretsiz GPU/TPU'ları ve tek bir ev bilgisayarı ile eğitiliyor.

## Sırada

**Töz-2** üzerinde çalışıyoruz: daha fazla ve daha temiz Türkçe veri, Türkçe kısa düşünme ve hesap aracı.

---

<details>
<summary>English</summary>

Töz AI builds small, open Turkish language models trained entirely from scratch, tokenizer included.
**Töz-1.5** (604M) is our latest Turkish language model; **Töz-Çevir-1** (74M) is an English ↔ Turkish
translation model that matches OPUS-MT tc-big on English→Turkish; **Töz-1** (195M) is our first chat model.
All models are Apache-2.0 and trained without non-commercial data.
Models: [huggingface.co/TozAI](https://huggingface.co/TozAI) · Demo: [toz-chat.onrender.com](https://toz-chat.onrender.com/)

</details>
