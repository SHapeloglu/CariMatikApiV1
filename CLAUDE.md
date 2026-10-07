# CLAUDE.md — CariMatik API v1 (FastAPI REST katmanı)

> 🗄️ **ARŞİV (2026-10-07):** Bu repo artık geliştirilmiyor. Tüm endpoint'leri (117) **[CariMatikApiV2](https://github.com/SHapeloglu/CariMatikApiV2)** içinde, JWT kimlik doğrulamasıyla birlikte var; aynı kod CariMatik reposunda da (`api/`) duruyor.

CariMatik (FinansApp) Flask uygulamasının MySQL şemasını, Flask koduna dokunmadan REST olarak dışarı açan **ilk** FastAPI denemesi. Tek dosya (`api.py`, ~1.900 satır): SQLAlchemy modelleri + Pydantic şemaları + ~120 endpoint. **Kimlik doğrulama yok.**

- GitHub: https://github.com/SHapeloglu/CariMatikApiV1 (2026-04-29 → 05-02, web yüklemeleri)
- Halefi: **CariMatikApiV2** (JWT + API kaynağı yönetimi eklendi). Yeni iş V2'de yapılmalı; bu repo arşiv niteliğinde.
- Ana uygulama: **CariMatik** · Mimari: `architect.md` · Görevler: `task.md` · Fikirler: `backlog.md` · Günlük: `session.md`

## Çalıştırma

```bash
pip install fastapi uvicorn sqlalchemy pymysql cryptography pydantic werkzeug   # requirements.txt yok
# CariMatik'in config.py dosyası bu klasöre kopyalanmalı (DB_HOST/PORT/USER/PASSWORD/NAME)
uvicorn api:app --reload --port 8000     # Swagger: http://localhost:8000/docs
```

## Kurallar ve Tuzaklar

- **Modeller CariMatik `app.py`'deki tabloların elle kopyası.** CariMatik'te şema değişince burası kendiliğinden güncellenmez; kolon uyuşmazlığı çalışma anında SQL hatası verir.
- Hiçbir endpoint korumalı değil ve CORS `allow_origins=["*"]` — internete açık bir sunucuda çalıştırma.
- Yollar `/api/v2/...` önekiyle başlıyor (dosya adı v1 olsa da, CariMatik'in "v2 şeması"nı hedefliyor).
- `config.py` commit edilmez (repo'da `.gitignore` bile yok — eklerken dikkat).
- Oturum sonunda `session.md`'ye kayıt düş, `task.md`'yi güncelle.
