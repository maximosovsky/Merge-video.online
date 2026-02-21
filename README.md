<div align="center">

# 🎬 Merge Video Bot

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram_Bot-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Telegram bot for merging multiple YouTube videos into one**

</div>

> Send YouTube URLs to the bot — it downloads with youtube-dl, finds a common resolution, merges via ffmpeg concat, and sends a download link. Deployed on AWS EC2.

---

## ✨ Features

- Telegram bot (Telegraf.js)
- YouTube video download via youtube-dl
- Automatic resolution matching across videos
- FFmpeg concat merge pipeline
- Download link via EC2 public hostname
- Update deduplication middleware

---

## 🚀 Quick Start

```bash
git clone https://github.com/maximosovsky/Merge-video.online.git
cd Merge-video.online
npm install
# Set BOT_TOKEN in environment
node index.js
```

---

## 📄 License

[Maxim Osovsky](https://www.linkedin.com/in/osovsky/). Licensed under [MIT](LICENSE).
