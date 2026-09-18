# Файлы для ASUS ZenFone 2 Laser ZE500KL / Z00ED

Этот файл дополняет основную инструкцию в [README.md](./README.md) и служит как каталог установочных файлов, их назначений, источников и контрольных сумм.

## Набор, использованный при успешной установке

| Файл | Назначение | Размер | SHA-256 | Источник |
|---|---|---:|---|---|
| `twrp-3.0.2.0-Z00E-MM.img` | Временный TWRP для штатного Android 6.0.x / Marshmallow перед разблокировкой bootloader | 18,044,928 bytes | `4826bc049eecdb0821fa664d5fd034d3437f98dfafed8b34498014f7818f50a6` | GitHub release LightouchDev |
| `ZE500KL_BootloaderUnlock.zip` | Неофициальная разблокировка bootloader ZE500KL | 3,395 bytes | `0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73` | Старый unlock-пакет ZE500KL; сохраняйте локальную проверенную копию |
| `TWRP-3.2.0-Z00ED-20171205.img` | TWRP для установки LineageOS на Z00ED | 19,773,440 bytes | `17ce666634a596cdabc9d48374ce863aa55190dadb189f0ee7de9915addf37d9` | SourceForge Android_Rom_OTA |
| `lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip` | LineageOS 14.1 / Android 7.1.2 для Z00ED | ~533.6 MB на SourceForge / ~509 MiB в `ls -lh` | `5050cbd8f90450f92118224f879e9164b9a00e39af9857f70aa9cb5205880b6d` | SourceForge Android_Rom_OTA |

> Важно: SHA-256 для LineageOS — контрольная сумма именно того ZIP, который был использован в успешной установке. Это не опубликованная разработчиком checksum.

---

## 1. twrp-3.0.2.0-Z00E-MM.img

### Для чего нужен

Этот образ использовался **только временно** на штатной прошивке Android 6.0.1:

```bash
fastboot boot twrp-3.0.2.0-Z00E-MM.img
```

Не требуется прошивать его командой `fastboot flash recovery`.

### Проверенная SHA-256

```text
4826bc049eecdb0821fa664d5fd034d3437f98dfafed8b34498014f7818f50a6
```

### Источник

GitHub release:

https://github.com/LightouchDev/android_device_asus_Z00E-twrp/releases/tag/3.0.2-0_6.0

Прямой asset:

https://github.com/LightouchDev/android_device_asus_Z00E-twrp/releases/download/3.0.2-0_6.0/twrp-3.0.2.0-Z00E-MM.img

---

## 2. ZE500KL_BootloaderUnlock.zip

### Для чего нужен

Неофициальный unlock-пакет для ZE500KL. Применяется из временно загруженного TWRP через:

```bash
adb sideload ZE500KL_BootloaderUnlock.zip
```

После установки обязательно проверить:

```bash
fastboot oem device-info
```

Ожидаемый результат:

```text
Device unlocked: true
```

### Проверенная SHA-256

```text
0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73
```

### Важное замечание

Старые публичные зеркала этого файла нестабильны или исчезли. Если у вас есть проверенная копия с указанной SHA-256, сохраните её отдельно.

Не используйте случайный файл с тем же именем без проверки SHA-256.

---

## 3. TWRP-3.2.0-Z00ED-20171205.img

### Для чего нужен

Используется после успешной разблокировки bootloader для установки LineageOS:

```bash
fastboot boot TWRP-3.2.0-Z00ED-20171205.img
```

### Проверенная SHA-256

```text
17ce666634a596cdabc9d48374ce863aa55190dadb189f0ee7de9915addf37d9
```

### Источник

Каталог recovery:

https://sourceforge.net/projects/android-rom-ota/files/TWRP_Recovery/

Прямое скачивание:

https://sourceforge.net/projects/android-rom-ota/files/TWRP_Recovery/TWRP-3.2.0-Z00ED-20171205.img/download

---

## 4. lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip

### Для чего нужен

Это проверенная сборка LineageOS 14.1 на базе Android 7.1.2 для Z00ED.

Установка выполнялась без SD-карты через ADB Sideload:

```bash
adb sideload lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
```

### Проверенная SHA-256 использованной копии

```text
5050cbd8f90450f92118224f879e9164b9a00e39af9857f70aa9cb5205880b6d
```

### Источник

Прямое скачивание:

https://sourceforge.net/projects/android-rom-ota/files/LineageOS/Z00ED/lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip/download

Каталог Z00ED:

https://sourceforge.net/projects/android-rom-ota/files/LineageOS/Z00ED/

### Почему именно 20180526

В каталоге есть более поздняя сборка `20180729`, но эта инструкция фиксирует именно `20180526`, потому что именно она была фактически установлена и проверена на описанном ZE500KL.

---

## Быстрая проверка файлов перед прошивкой

Положите все файлы в один каталог и выполните:

```bash
sha256sum \
  twrp-3.0.2.0-Z00E-MM.img \
  ZE500KL_BootloaderUnlock.zip \
  TWRP-3.2.0-Z00ED-20171205.img \
  lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
```

Ожидаемые значения:

```text
4826bc049eecdb0821fa664d5fd034d3437f98dfafed8b34498014f7818f50a6  twrp-3.0.2.0-Z00E-MM.img
0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73  ZE500KL_BootloaderUnlock.zip
17ce666634a596cdabc9d48374ce863aa55190dadb189f0ee7de9915addf37d9  TWRP-3.2.0-Z00ED-20171205.img
5050cbd8f90450f92118224f879e9164b9a00e39af9857f70aa9cb5205880b6d  lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
```

Если хотя бы одна сумма отличается — не прошивайте этот файл, пока не выяснена причина.

---

## Что не хранить публично

Не загружайте в публичный GitHub свои резервные копии разделов:

```text
factory.img
factorybak.img
modemst1.img
modemst2.img
fsg.img
asuskey*.img
aboot.img
abootbak.img
```

В таких дампах могут находиться уникальные данные конкретного телефона.

---

## Рекомендуемый локальный архив

Для будущего восстановления удобно сохранить локально отдельную папку:

```text
ZE500KL-LineageOS-14.1/
├── twrp-3.0.2.0-Z00E-MM.img
├── ZE500KL_BootloaderUnlock.zip
├── TWRP-3.2.0-Z00ED-20171205.img
├── lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
├── SHA256SUMS.txt
└── backup/
```

Создать файл контрольных сумм:

```bash
sha256sum \
  twrp-3.0.2.0-Z00E-MM.img \
  ZE500KL_BootloaderUnlock.zip \
  TWRP-3.2.0-Z00ED-20171205.img \
  lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip \
  > SHA256SUMS.txt
```

Проверить позже:

```bash
sha256sum -c SHA256SUMS.txt
```
