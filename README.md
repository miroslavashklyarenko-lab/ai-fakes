# Домашнє завдання 5: перевірка вірусних ШІ-фейків

Звіт: `report.pdf` (чекліст, 3 кейси, порівняння детекторів, висновок, джерела).
У папці `evidence/` лежать усі файли, на які посилається звіт. Їхні хеші SHA-256 наведено у `SHA256SUMS.txt`.

## Кейси

| # | Матеріал | Файли | Архів джерела |
|---|---|---|---|
| 1 | ШІ-відео «40-годинні черги на кордоні з Польщею» (Telegram, 17.10.2026) | `case1_received.mp4`, кадри `case1_frame_3s.png`, `case1_frame_6s.png`, `case1_frame_11.5s.png` | [Wayback Machine](https://web.archive.org/web/20261025211258/https://t.me/truexaluck/31966) |
| 2 | ШІ-фото «протестувальники роздягли літню пару» (Франція, 17–18.10.2026) | `case2_fullfact_original.png`, вирізане фото `case2_photo_crop.png` | [Wayback Machine (Full Fact)](https://web.archive.org/web/20261025211355/https://fullfact.org/world/photo-elderly-french-couple-stripped-ai/) |
| 3 | ШІ-«селфі» лідерів на саміті BRICS 2026 (вересень 2026) | `case3_quint_original.webp`, вирізане селфі `case3_selfie_crop.png` | [archive.ph/1DVmJ](https://archive.ph/1DVmJ) |

## Як відтворити

**1. Перевірити, що файли ті самі.**

```
# Windows PowerShell
Get-FileHash .\evidence\* -Algorithm SHA256

# Linux / macOS
cd evidence && sha256sum -c ../SHA256SUMS.txt
```

**2. Витягти кадри з відео.** Команда (ffmpeg):

```
ffmpeg -ss 3 -i evidence/case1_received.mp4 -frames:v 1 case1_frame_3s.png
```

Для 6 і 11.5 с замініть `-ss`.

**3. Вирізати фото зі скриншотів** (Python, Pillow):

```
from PIL import Image
Image.open("evidence/case2_fullfact_original.png").convert("RGB").crop((32,105,331,390)).save("case2_photo_crop.png")
Image.open("evidence/case3_quint_original.webp").convert("RGB").crop((109,40,392,362)).save("case3_selfie_crop.png")
```

**4. Метадані:** `ffprobe evidence/case1_received.mp4` або `exiftool <файл>`.

**5. Детектори** (25.10.2026):
- [Sightengine](https://sightengine.com/detect-ai-generated-images): AI Image Detector, файли `*_crop.png` і кадри відео;
- [AI or Not](https://www.aiornot.com): `case1_received.mp4` і файли `*_crop.png`;
- [Hive Detect](https://hivedetect.ai): не повернув результатів.

Результати детекторів можуть змінюватися з оновленням їхніх моделей. Score, записані у звіті, отримано 25.10.2026.
