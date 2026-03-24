# fuzzy-octo-engine

VS Code'u Google Colab'a bağlama rehberi.

---

## VS Code ↔ Google Colab Bağlantısı

Bu repo, Google Colab üzerinde çalışan bir ortama **VS Code Remote - SSH** ile bağlanmayı sağlar.  
Bağlantı, [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) tüneli üzerinden kurulur; herhangi bir port açmanıza veya ücretli hesaba gerek yoktur.

---

## Ön Koşullar

Yerel bilgisayarınızda aşağıdakilerin kurulu olması gerekir:

| Yazılım | Bağlantı |
|---|---|
| VS Code | <https://code.visualstudio.com/> |
| Remote - SSH eklentisi | <https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh> |
| cloudflared | <https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/> |

---

## Kurulum Adımları

### 1. Notebook'u Colab'da Aç

Aşağıdaki butona tıklayarak `colab_vscode.ipynb` dosyasını Google Colab'da açın:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/umiterkanetc-art/fuzzy-octo-engine/blob/main/colab_vscode.ipynb)

### 2. Notebook Hücrelerini Sırayla Çalıştır

1. **Adım 1 — Bağımlılıkları kur**: `openssh-server` ve `cloudflared` yüklenir.
2. **Adım 2 — SSH şifresi ve anahtarını ayarla**: SSH sunucusu yapılandırılır ve başlatılır.
3. **Adım 3 — Cloudflared tünelini başlat**: Bir tünel URL'si üretilir ve SSH bağlantı bloğu ekrana yazdırılır.
4. **Adım 4 — Oturumu açık tut**: Oturumun zaman aşımına uğramaması için çalışır.

### 3. SSH Config Dosyanıza Ekleyin

Adım 3'ün çıktısından gelen bloğu `~/.ssh/config` dosyanıza yapıştırın:

```
Host colab
    HostName <tünel-adresi>.trycloudflare.com
    User root
    Port 22
    ProxyCommand cloudflared access ssh --hostname %h
```

> `<tünel-adresi>` kısmı her Colab oturumunda değişir; her seferinde Adım 3 çıktısından kopyalayın.

### 4. VS Code'dan Bağlanın

1. VS Code'u açın → `F1` → **Remote-SSH: Connect to Host…**
2. `colab` seçin.
3. İstendiğinde Adım 2'de belirlediğiniz şifreyi girin.
4. Bağlantı kuruldu! Colab'ın dosya sisteminde çalışmaya başlayabilirsiniz.

---

## Notlar

- Colab oturumu kapandığında tünel de kapanır; yeniden bağlanmak için notebook'u baştan çalıştırın.
- GPU/TPU kullanmak için Colab menüsünden **Runtime → Change runtime type** ile donanım hızlandırıcı seçebilirsiniz.
