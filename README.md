<div align="center">
  <h1>📜 Paper Discord</h1>
  <p>
    <strong>Advanced Modular Discord Bot Infrastructure</strong><br>
    Developed by <a href="https://acarfx.com">Acarfx</a> & <a href="https://twitch.tv/tefottas">Tefottas</a>
  </p>
  
  <p>
    <a href="#türkçe">Türkçe</a> • <a href="#english">English</a>
  </p>
</div>

---

<h2 id="türkçe">🇹🇷 Türkçe Dokümantasyon</h2>

**Paper Discord**, Discord.js v14 ile güçlendirilmiş, çoklu bot (multibot) mimarisine sahip, gelişmiş plugin sistemi sunan profesyonel bir altyapıdır. **Acarfx** tarafından geliştirilmiştir.

### ✨ Özellikler

- **Çoklu Bot Desteği:** Tek projede sınırsız sayıda bot çalıştırın (Bot1, Bot2...).
- **Plugin Sistemi:** Modüler yapı. Özellikleri plugin olarak ekleyip çıkarın.
- **Otomatik Yük Dengeleme:** Pluginleri botlara otomatik dağıtır veya tüm botlarda çalışmaya zorlar.
- **Dahili Dashboard:** Web tabanlı yönetim paneli (Plugin: Dashboard).
- **Prototip Desteği:** `String` ve `Message` sınıflarına eklenen özel metodlar.
- **CLI Araçları:** Tek komutla bot veya plugin oluşturma sihirbazı.

### 🚀 Geliştirici Araçları (CLI)

#### 1. Yeni Bot Oluşturma

```bash
npm run bot
```

_Süreç: Bot ismini alır, `src/clients` altında klasörünü açar ve PM2/Ecosystem ayarlarını otomatik günceller._

#### 2. Yeni Plugin Oluşturma

```bash
npm run plugin <Pluginİsmi>
```

_Örnek: `npm run plugin Moderation`_ (Dosya yapısını oluşturur)

---

### 🎨 ACARDJSComponentsV2 & Panel Sistemi

Discord embedleri ve bileşenleri (buton, select menu) ile çalışmayı kolaylaştıran özel bir yapı.

#### ⚡ Hızlı Kullanım (Message Prototipleri)

Normal `message.reply` yerine, daha şık ve panel formatında mesaj atmak için aşağıdaki kısayolları kullanabilirsiniz:

- **`.insend(options)`**: Kanala panel formatında mesaj atar.
- **`.inreply(options)`**: Mesaja panel formatında cevap verir.

**Örnek:**

```javascript
message.inreply({
  title: "İşlem Başarılı",
  texts: ["Kullanıcı başarıyla yasaklandı.", "Süre: 3 Gün"],
  footer: "Moderasyon Sistemi",
  color: "green", // veya hex kodu #00ff00
});
```

#### 🛠️ Gelişmiş Kullanım (Manuel Oluşturma)

Daha karmaşık yapılar (Buton, Select Menu, Dosya ekleme) için paneli kendiniz oluşturabilirsiniz:

```javascript
// 1. Paneli oluştur
const panel = message.createPanel(); // veya new ACARDJSComponentsV2(client)

// 2. İçeriği ayarla
panel
  .setContainer()
  .setColor("blue")
  .addContent("# Başlık\nBu bir deneme mesajıdır.")
  .addLine(); // Ayırıcı çizgi

// 3. Buton Ekle
panel.addComponents([
  { type: 2, style: 1, label: "Onayla", custom_id: "onayla_btn" },
  { type: 2, style: 4, label: "İptal", custom_id: "iptal_btn" },
]);

// 4. Gönder
panel.send(message.channel.id);
```

---

### 🎨 TefottasInteractionV2Modal & Modal Sistemi

Discord modal sistemi için geliştirilmiş sistem (seçim menüleri, dosya yükleme sistemi) ile çalışmayı kolaylaştırdığımız özel bir yapı

Normal şartlarda bir modal sistemine eskiden sadece text olarak kullanabiliyorduk. 

Artık içerisinde bulunan;
- **`.addTextInput`** Normal yazım sistemini açar.
- **`.addStringSelect`** Kendi oluşturduğunuz menü seçme formatını açar.
- **`.addUserSelect`** Kullanıcı seçme menü formatını açar.
- **`.addRoleSelect`** Rol seçme menü formatını açar.
- **`.addChannelSelect`** Kanal seçme menü formatını açar.
- **`.addFileUpload`** Dosya yükleme formatını açar.

Sistemimiz sayesinde hem kolay bir kullanım sunarken, hemde zarif ve güzel tasarımlı modallarınızı kullanabilirsiniz.


### 🧩 Plugin Geliştirme Rehberi

#### 1. Ayarları Tanımlama (`manifest.json`)

```json
{
  "name": "Moderation",
  "version": "1.0.0",
  "settings": [
    {
      "name": "logChannel",
      "type": "channel",
      "description": "Log kanalı",
      "default": null
    }
  ]
}
```

#### 2. Ayarlara Kod İçinden Erişme

```javascript
async run(client, message) {
    const guildSettings = await client.getGuildSettings(message.guild.id);
    const logVal = guildSettings.pluginSettings?.get('Moderation')?.settings?.get('logChannel');

    if (!logVal) return;
    const channel = message.guild.channels.cache.get(logVal);
    if (channel) channel.send("Veri çekildi!");
}
```

---

### 🛠️ Diğer Prototipler

#### Message Prototipleri

- **`.timedReply(content, ms)`**: Mesajı cevaplar ve belirtilen süre sonra (milisaniye) siler.
  ```javascript
  message.timedReply("Bu mesaj 5 saniye sonra silinecek!", 5000);
  ```

#### String Prototipleri

- **`.splitMessage(options)`**: Uzun metinleri böler ve codeblock içine alır.
  ````javascript
  const chunks = text.splitMessage({
    maxLength: 1900,
    prepend: "```js\n",
    append: "\n```",
  });
  ````
- **`.toTitleCase()`**: Baş harfleri büyütür.

---

### 🌐 Dashboard Plugini

- **Adres:** `http://localhost:3000`
- **Şifre:** `acarfx2025`
- **Özellikler:** Pluginleri aç/kapa, ayarları webden düzenle.
<img width="1912" height="887" alt="image" src="https://github.com/user-attachments/assets/78cadfe2-c2ba-493c-b2d8-4dd61c4a46cd" />
<img width="1911" height="888" alt="image" src="https://github.com/user-attachments/assets/21a0d269-9bb6-4d48-b094-a224a99770b7" />
<img width="1907" height="886" alt="image" src="https://github.com/user-attachments/assets/36a7987c-de4a-4e28-bcec-a687a7ccead6" />
<img width="1909" height="886" alt="image" src="https://github.com/user-attachments/assets/5db03990-edca-4c52-a117-ce45471235c2" />
<img width="1911" height="887" alt="image" src="https://github.com/user-attachments/assets/0e340815-7636-4bc1-a417-7e78def03989" />
<img width="1863" height="919" alt="image" src="https://github.com/user-attachments/assets/597f4fdd-abb2-4da3-a647-2019e9abad79" />






---

<h2 id="english">🇺🇸 English Documentation</h2>

**Paper Discord** is a professional Discord bot infrastructure powered by Discord.js v14, featuring multi-bot architecture and an advanced plugin system. Developed by **Acarfx**.

### ✨ Features

- **Multi-Bot Support:** Run unlimited bot instances in a single project.
- **Plugin System:** Modular architecture. Add or remove features as plugins.
- **Auto Load Balancing:** Automatically distributes plugins across bots.
- **Built-in Dashboard:** Web-based administration panel.
- **Prototype Support:** Custom methods added to `String` and `Message` classes.
- **CLI Tools:** Wizards for creating bots and plugins effortlessly.

### 🎨 ACARDJSComponentsV2 & Panel System

A powerful utility to simplify working with Discord embeds and components (buttons, select menus).

#### ⚡ Quick Usage (Message Prototypes)

Instead of standard replies, use these shortcuts for stylish panel messages:

- **`.insend(options)`**: Sends a panel message to the channel.
- **`.inreply(options)`**: Replies with a panel message.

**Example:**

```javascript
message.inreply({
  title: "Operation Successful",
  texts: ["User has been banned.", "Duration: 3 Days"],
  footer: "Moderation System",
  color: "green",
});
```

#### 🛠️ Advanced Usage (Manual Builder)

For complex UIs (Buttons, Select Menus, Files):

```javascript
// 1. Create Panel
const panel = message.createPanel();

// 2. Set Content
panel
  .setContainer()
  .setColor("blue")
  .addContent("# Title\nThis is a test message.")
  .addLine();

// 3. Add Buttons
panel.addComponents([
  { type: 2, style: 1, label: "Confirm", custom_id: "confirm_btn" },
  { type: 2, style: 4, label: "Cancel", custom_id: "cancel_btn" },
]);

// 4. Send
panel.send(message.channel.id);
```

---

### 🚀 Developer Tools (CLI)

#### 1. Creating a New Bot

```bash
npm run bot
```

#### 2. Creating a New Plugin

```bash
npm run plugin <PluginName>
```

---

### 🎨 TefottasInteractionV2Modal & Modal System

A specialized structure we developed to simplify working with the Discord modal system (enhancing it with selection menus and a file upload system).

Previously, modals could strictly be used for text inputs only.

Now, with the built-in features:
- **`.addTextInput`** Opens the standard text input system.
- **`.addStringSelect`** Opens a custom menu selection format that you define.
- **`.addUserSelect`** Opens the user selection menu format.
- **`.addRoleSelect`** Opens the role selection menu format.
- **`.addChannelSelect`** Opens the channel selection menu format.
- **`.addFileUpload`** Opens the file upload format.

Thanks to our system, you can enjoy both ease of use and deploy elegant, beautifully designed modals.

### 🧩 Plugin Development Guide

#### 1. Defining Settings (`manifest.json`)

```json
{
  "name": "Moderation",
  "version": "1.0.0",
  "settings": [
    {
      "name": "logChannel",
      "type": "channel",
      "description": "Log channel",
      "default": null
    }
  ]
}
```

#### 2. Accessing Settings (Code Example)

```javascript
async run(client, message) {
    const guildSettings = await client.getGuildSettings(message.guild.id);
    const logVal = guildSettings.pluginSettings?.get('Moderation')?.settings?.get('logChannel');

    if (!logVal) return;
    const channel = message.guild.channels.cache.get(logVal);
    if (channel) channel.send("Data fetched!");
}
```

---

### 🛠️ Other Prototypes

#### Message Prototypes

- **`.timedReply(content, ms)`**: Replies and auto-deletes after X ms.
  ```javascript
  message.timedReply("This message will self-destruct in 5s!", 5000);
  ```

#### String Prototypes

- **`.splitMessage(options)`**: Splits long text into chunks with codeblocks.
- **`.toTitleCase()`**: Converts string to Title Case.

---

### 🌐 Dashboard Plugin

- **URL:** `http://localhost:3000`
- **Password:** `acarfx2025`
- **Features:** Toggle plugins, manage settings via UI.

<img width="1912" height="887" alt="image" src="https://github.com/user-attachments/assets/78cadfe2-c2ba-493c-b2d8-4dd61c4a46cd" />
<img width="1911" height="888" alt="image" src="https://github.com/user-attachments/assets/21a0d269-9bb6-4d48-b094-a224a99770b7" />
<img width="1907" height="886" alt="image" src="https://github.com/user-attachments/assets/36a7987c-de4a-4e28-bcec-a687a7ccead6" />
<img width="1909" height="886" alt="image" src="https://github.com/user-attachments/assets/5db03990-edca-4c52-a117-ce45471235c2" />
<img width="1911" height="887" alt="image" src="https://github.com/user-attachments/assets/0e340815-7636-4bc1-a417-7e78def03989" />
<img width="1863" height="919" alt="image" src="https://github.com/user-attachments/assets/84df31d3-4b13-4d53-9e00-45610baf3603" />





---

<div align="center">
  <p>© 2026 Paper Discord. Developed by Acarfx.</p>
</div>
