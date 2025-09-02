---
title: "Python PTY ile Shell Upgrade"
date: 2025-09-02 10:58:00 +0300
image: /assets/img/posts/python-pty-ile-shell-upgrade/cover.jpg
categories: [Pentest]
---

Reverse shell elde ettiğinizde genellikle kısıtlı bir shell ile karşılaşırsınız. Örneğin `Ctrl+C` ile komut iptali yapamazsınız, `Tab` ile otomatik tamamlama çalışmaz ve `clear` komutu ekranı temizlemez. Neyse ki Python'un `pty` modülü interaktif bir shell elde etmek için kullanılabilir.

> Hedef makinede Python yüklü değilse bu yöntem işe yaramayacaktır.
{: .prompt-warning }

## PTY Modülü ile Spawn

İlk olarak PTY modülünü kullanarak `/bin/bash` shell'ini spawn edelim:

```bash 
python -c 'import pty; pty.spawn("/bin/bash")'
```

![Python PTY ile Spawn Edilen Shell](/assets/img/posts/python-pty-ile-shell-upgrade/img1.png){: width="972" height="589" }

Böylece daha interaktif bir shell elde etmiş olduk. Ancak hala bazı kısıtlamalar mevcut. Örneğin `Ctrl+C` hala çalışmıyor. Bunun için terminal ayarlarını değiştirmemiz gerekiyor.

## Raw Mode Konfigürasyonu

