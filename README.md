# fuzzy-octo-engine

## 🔌 VS Code – Google Colab Bağlantısı

Bu proje, **Google Colab** runtime'ını **VS Code Remote-SSH** eklentisi üzerinden kullanmanızı sağlar. Colab'ın ücretsiz GPU/CPU kaynaklarını VS Code'un güçlü editör özellikleriyle birleştirmenize olanak tanır.

---

## Gereksinimler

| Araç | Açıklama |
|------|----------|
| [VS Code](https://code.visualstudio.com/) | Kod editörü |
| [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) | VS Code eklentisi |
| [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) | Yerel makinede SSH tüneli için |
| Google hesabı | Colab erişimi için |

---

## Kullanım

### 1. Notebook'u Colab'da Açın

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/umiterkanetc-art/fuzzy-octo-engine/blob/main/colab_vscode.ipynb)

### 2. Hücreleri Sırayla Çalıştırın

1. **Hücre 1** – SSH sunucusu ve `cloudflared` kurulumunu yapar.
2. **Hücre 2** – SSH şifresini ayarlar ve SSH servisini başlatır.
3. **Hücre 3** – Cloudflare tünelini başlatır ve size bağlantı bilgilerini gösterir.

### 3. SSH Config Dosyasını Güncelleyin

Hücre 3'ün çıktısında verilen bilgileri `~/.ssh/config` dosyanıza ekleyin:

```
Host colab
    HostName <tünelden-gelen-hostname>
    User colab
    Port 22
    ProxyCommand cloudflared access ssh --hostname %h
```

### 4. VS Code'dan Bağlanın

1. VS Code'da `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tuşlarına basın.
2. **Remote-SSH: Connect to Host...** komutunu seçin.
3. `colab` yazıp Enter'a basın.
4. SSH şifresini girin (varsayılan: `colab1234`).

---

## ⚠️ Önemli Notlar

- **Oturum süresi:** Colab oturumları belirli bir süre sonra otomatik kapanır. Her yeni oturumda notebook'u yeniden çalıştırmanız gerekir.
- **Şifre güvenliği:** `colab_vscode.ipynb` içindeki `SSH_PASSWORD` değişkenini değiştirerek kendi şifrenizi belirleyin.
- **Cloudflared kurulumu:** Yerel makinenizde `cloudflared`'ın kurulu olması gerekir. [İndir](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/)
