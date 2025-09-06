---
title: "Python PTY ile Shell Upgrade"
date: 2025-09-02 10:58:00 +0300
image: /assets/img/posts/python-pty-ile-shell-upgrade/cover.jpg
categories: [Pentest]
tags: [python, shell, linux]
---

Reverse shell elde ettiğinizde genellikle kısıtlı bir shell ile karşılaşırsınız. Örneğin `Ctrl+C` ile komut iptali yapamazsınız, `Tab` ile otomatik tamamlama çalışmaz ve `clear` komutu ekranı temizlemez. Neyse ki Python'un `pty` modülü interaktif bir shell elde etmek için kullanılabilir.

> Bu işlem için hedef makinede Python kurulu olmalıdır.
{: .prompt-warning }

## PTY Spawn

İlk olarak PTY modülünü kullanarak `/bin/bash` shell'ini spawn edelim:

```bash 
python -c 'import pty; pty.spawn("/bin/bash")'
```

![Python PTY ile Spawn Edilen Shell](/assets/img/posts/python-pty-ile-shell-upgrade/img1.png){: width="972" height="589" }

Böylece daha interaktif bir shell elde etmiş olduk. Ancak hala bazı kısıtlamalar mevcut. Örneğin `Ctrl+C` hala çalışmıyor. Bunun için terminal ayarlarını değiştirmemiz gerekiyor.

## Raw Mode Ayarı

Raw mode ayarını yapabilmek için önce `Ctrl+Z` kombinasyonu ile shell'i arka plana gönderelim. Ardından terminal ayarlarını değiştirip shell'i tekrar aktif hale getirelim:

```bash
stty raw -echo; fg
```

> Bazı bağlantı türlerinde `Ctrl+Z` shell'i kapatabilir. Bu durumda bu başlığı atlayın.
{: .prompt-tip }

## TERM Ayarı

Son olarak terminal komutlarının tam işlevsel olması için TERM değişkenini belirleyelim:

```bash
export TERM=xterm
```

Tebrikler! Artık tam yetkili bir terminal deneyimi yaşayabilirsiniz.