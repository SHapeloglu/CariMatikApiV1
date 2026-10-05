# architect.md — CariMatik API v1 Mimarisi

```
İstemci ──HTTP JSON──► FastAPI (api.py, uvicorn :8000) ──SQLAlchemy/PyMySQL──► CariMatik MySQL DB
                                                           ▲
                                     CariMatik Flask (app.py) aynı tabloları kullanır
```

## api.py Bölümleri

1. **Bağlantı** — `import config as cfg` ile `mysql+pymysql://…?charset=utf8mb4` engine, `SessionLocal`, `get_db()` bağımlılığı.
2. **Modeller** — CariMatik'in v1 (Birim, Cari, Stok, Belge) ve v2 (Şirket, Depo, Banka/Kasa, Çek-Senet, Taksit, Hesap Grubu, Rapor, Kullanıcı + yetki, adres) tablolarının SQLAlchemy karşılıkları.
3. **Pydantic şemaları** — her kaynak için `XxxCreate` / `XxxUpdate` / `XxxRead`.
4. **Endpoint'ler** — `/api/v2/<kaynak>` altında CRUD: ~56 GET, 24 POST, 14 PUT, 22 DELETE; `/api/v2/ozet` (özet sayılar), `/health`.
5. `get_or_404` yardımcısı, CORS (`*`).

## Mimari Kararlar

- **Flask uygulamasına dokunmadan ayrı süreç**: mobil/entegrasyon istemcileri için REST, mevcut web uygulamasını riske atmadan.
- **Modelleri kopyalama** (paylaşmak yerine): Flask-SQLAlchemy ile saf SQLAlchemy'yi aynı modellerle kullanmanın zorluğundan kaçınmak için. Bedeli şema kayması.
- **Kimlik doğrulama sonraya bırakıldı** → V2'de eklendi.
