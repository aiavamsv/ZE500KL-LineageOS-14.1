# ASUS ZenFone 2 Laser ZE500KL (Z00ED) → LineageOS 14.1

Проверенная последовательность для **ASUS ZenFone 2 Laser ZE500KL / Z00ED** с использованием Ubuntu и USB, **без microSD-карты**.

> [!WARNING]
> Это архивная, неофициальная процедура для старого устройства. Разблокировка загрузчика и прошивка могут привести к потере данных или неработоспособности телефона. Перед записью разделов обязательно сделайте резервные копии.
>
> Эта инструкция была фактически пройдена на ZE500KL со штатной системой **Android 6.0.1**, build **MMB29P**, firmware **WW-13.1010.1612.53 / 13.1010.1612.53-20170202**. Если у вашего экземпляра другая модель или другая ветка прошивки, не применяйте шаг разблокировки вслепую.

## Что устанавливается

- временный TWRP для штатного Android 6.0.x: `twrp-3.0.2.0-Z00E-MM.img`;
- неофициальный unlock-пакет: `ZE500KL_BootloaderUnlock.zip`;
- TWRP для Z00ED: `TWRP-3.2.0-Z00ED-20171205.img`;
- LineageOS 14.1 / Android 7.1.2: `lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip`.

## Проверенные файлы и контрольные суммы

| Файл | Размер | SHA-256 | Назначение |
|---|---:|---|---|
| `twrp-3.0.2.0-Z00E-MM.img` | 18,044,928 bytes | `4826bc049eecdb0821fa664d5fd034d3437f98dfafed8b34498014f7818f50a6` | временно загрузить TWRP на штатном Android 6.0.x |
| `ZE500KL_BootloaderUnlock.zip` | 3,395 bytes | `0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73` | разблокировать bootloader ZE500KL |
| `TWRP-3.2.0-Z00ED-20171205.img` | 19,773,440 bytes | `17ce666634a596cdabc9d48374ce863aa55190dadb189f0ee7de9915addf37d9` | TWRP для установки LineageOS на Z00ED |
| `lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip` | около 533.6 MB на SourceForge / около 509 MiB в `ls -lh` | `5050cbd8f90450f92118224f879e9164b9a00e39af9857f70aa9cb5205880b6d` | LineageOS 14.1 для Z00ED |

Последняя SHA-256 для LineageOS — сумма экземпляра, использованного при этой установке. Она была вычислена локально после скачивания; это не опубликованная разработчиком checksum.

## Ссылки на исходные файлы

### 1. TWRP 3.0.2.0 для Z00E / Android 6.0

GitHub release Miau Lightouch / LightouchDev:

- https://github.com/LightouchDev/android_device_asus_Z00E-twrp/releases/tag/3.0.2-0_6.0
- прямой asset: https://github.com/LightouchDev/android_device_asus_Z00E-twrp/releases/download/3.0.2-0_6.0/twrp-3.0.2.0-Z00E-MM.img

### 2. TWRP 3.2.0 для Z00ED

SourceForge, проект Android_Rom_OTA:

- каталог recovery: https://sourceforge.net/projects/android-rom-ota/files/TWRP_Recovery/
- прямое скачивание: https://sourceforge.net/projects/android-rom-ota/files/TWRP_Recovery/TWRP-3.2.0-Z00ED-20171205.img/download

### 3. LineageOS 14.1 для Z00ED

Использовалась именно сборка:

**`lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip`**

Ссылка на скачивание:

https://sourceforge.net/projects/android-rom-ota/files/LineageOS/Z00ED/lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip/download

Каталог Z00ED:

https://sourceforge.net/projects/android-rom-ota/files/LineageOS/Z00ED/

Почему именно эта сборка: она предназначена для **Z00ED**, основана на LineageOS 14.1 / Android 7.1.2 и была успешно установлена на описанный ZE500KL. В каталоге есть более поздняя `20180729`, но в данной инструкции зафиксирован именно вариант `20180526`, который был фактически проверен.

> LineageOS ZIP намеренно указан **как внешняя ссылка**, а не хранится в этом репозитории. Перед будущей установкой скачайте именно указанный файл и проверьте SHA-256. Если источник или файл изменились, не прошивайте его вслепую.

### 4. ZE500KL_BootloaderUnlock.zip

Это старый **неофициальный** unlock-пакет для ZE500KL. Старые публичные зеркала нестабильны или исчезли, поэтому при наличии рабочей копии сохраняйте её вместе с указанной выше SHA-256:

```text
0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73
```

Не заменяйте его случайным файлом с тем же именем без проверки содержимого и SHA-256.

---

# Пошаговая установка

## 0. Требования

Нужно:

- ASUS ZenFone 2 Laser **ZE500KL**;
- USB-кабель с передачей данных;
- Ubuntu;
- заряженный аккумулятор, желательно не менее 50%;
- сохранённые установочные файлы;
- понимание, что `Wipe Data` удалит пользовательские данные.

Установить ADB/Fastboot:

```bash
sudo apt update
sudo apt install adb fastboot android-sdk-platform-tools-common
```

Проверить:

```bash
adb version
fastboot --version
```

## 1. Войти в Fastboot

На выключенном телефоне удерживать:

```text
Volume Up + Power
```

Подключить USB и проверить:

```bash
fastboot devices
```

Должен появиться идентификатор устройства и слово `fastboot`.

## 2. Убедиться, что это ZE500KL и проверить загрузчик

```bash
fastboot getvar product
fastboot oem device-info
```

Для проверенного аппарата вывод содержал:

```text
Device project: ZE500KL
Device unlocked: false
```

`product: MSM8916` для этого аппарата нормален.

> Не прошивайте recovery или ROM, пока не удостоверились, что `Device project: ZE500KL`.

## 3. Проверить версию штатной прошивки

Если версия не видна в Fastboot, войти в штатный Recovery и посмотреть верхние строки.

На проверенном телефоне было:

```text
ASUS_Z00E_2
Android 6.0.1
MMB29P
13.1010.1612.53-20170202
```

Именно для этой ветки Marshmallow использовался временный `twrp-3.0.2.0-Z00E-MM.img`.

## 4. Временно загрузить TWRP для Marshmallow

В Fastboot:

```bash
cd ~/Загрузки
fastboot boot twrp-3.0.2.0-Z00E-MM.img
```

Важно: здесь используется **`fastboot boot`**, а не `fastboot flash recovery`.

Проверить ADB:

```bash
adb devices
adb shell id
```

Ожидается режим `recovery` и `uid=0(root)`.

## 5. Снять резервные копии критичных разделов

Создать каталог:

```bash
mkdir -p ~/ze500kl/backup
cd ~/ze500kl/backup
```

Посмотреть разметку:

```bash
adb shell 'ls -l /dev/block/bootdevice/by-name'
adb shell 'cat /proc/partitions'
```

На проверенном ZE500KL отсутствовал раздел `devinfo`; присутствовали, среди прочего, `aboot`, `abootbak`, `factory`, `factorybak`, `modemst1`, `modemst2`, `fsg`, `asuskey*`, `persist`.

Снять резервные копии:

```bash
for p in \
sbl1 sbl1bak rpm rpmbak tz tzbak hyp hypbak DDR ssd sec \
aboot abootbak fsg fsc modemst1 modemst2 \
factory factorybak \
asuskey asuskey2 asuskey3 asuskey4 \
persistent asusgpt asusgpt1 asusgpt2 \
misc keystore config abootdebug oem boot persist asusfw recovery
do
    echo "=== $p ==="
    adb exec-out "cat /dev/block/bootdevice/by-name/$p" > "$p.img"
done
```

Проверить, что пустых файлов нет:

```bash
find . -name '*.img' -size 0 -print
```

Если команда ничего не вывела, это хорошо.

Создать список SHA-256:

```bash
sha256sum *.img | tee SHA256SUMS.txt
```

Особенно берегите:

```text
factory.img
factorybak.img
modemst1.img
modemst2.img
fsg.img
aboot.img
abootbak.img
asuskey*.img
```

Не публикуйте эти дампы: они могут содержать уникальные данные устройства.

Дополнительно упаковать backup и сохранить на другом носителе:

```bash
cd ~/ze500kl
tar -czf ZE500KL-before-unlock-backup.tar.gz backup
sha256sum ZE500KL-before-unlock-backup.tar.gz
```

## 6. Разблокировать bootloader

Проверить unlock ZIP:

```bash
sha256sum ZE500KL_BootloaderUnlock.zip
unzip -t ZE500KL_BootloaderUnlock.zip
```

Для использованного файла должно быть:

```text
0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73
```

В TWRP:

```text
Advanced -> ADB Sideload -> Swipe to Start Sideload
```

Проверить:

```bash
adb devices
```

Должно быть `sideload`.

Запустить:

```bash
adb sideload ZE500KL_BootloaderUnlock.zip
```

На проверенной установке TWRP сообщил успешное выполнение (`Done!` / `Successful`).

После этого:

```text
Reboot -> Bootloader
```

Проверить:

```bash
fastboot oem device-info
```

Обязательный результат перед дальнейшей прошивкой:

```text
Device unlocked: true
```

Если остаётся `false`, не продолжать с `Wipe` и LineageOS.

## 7. Временно загрузить TWRP 3.2.0 для Z00ED

```bash
cd ~/Загрузки
fastboot boot TWRP-3.2.0-Z00ED-20171205.img
```

Пример нормального результата:

```text
Sending 'boot.img' ... OKAY
Booting ... OKAY
```

В обычном режиме этого TWRP `adb devices` может показывать `unauthorized`. Для установки ROM это не мешает: после запуска **ADB Sideload** устройство должно перейти в состояние `sideload`.

## 8. Проверить LineageOS ZIP перед установкой

```bash
cd ~/Загрузки
ls -lh lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
sha256sum lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
```

Для экземпляра, использованного в этой инструкции:

```text
5050cbd8f90450f92118224f879e9164b9a00e39af9857f70aa9cb5205880b6d
```

## 9. Очистить старую систему

В TWRP:

```text
Wipe -> Advanced Wipe
```

Выбрать только:

```text
Dalvik / ART Cache
System
Data
Cache
```

Не выбирать:

```text
Boot
Recovery
Persist
Modem
```

И не стирать вручную `factory`, `factorybak`, `modemst1/2`, `fsg`, `asuskey*`, `aboot*`.

Выполнить Swipe.

## 10. Установить LineageOS 14.1 без SD-карты

В TWRP:

```text
Advanced -> ADB Sideload -> Swipe to Start Sideload
```

Проверить:

```bash
adb devices
```

Ожидается:

```text
<id>    sideload
```

Установить:

```bash
cd ~/Загрузки
adb sideload lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
```

На успешно пройденной установке команда завершилась:

```text
Total xfer: 1.01x
```

а TWRP показал:

```text
ADB Sideload Complete
Successful
script succeeded: result was [1.000000]
```

Ориентируйтесь на итоговый статус TWRP, а не только на процент/коэффициент ADB.

## 11. Первый запуск

После успешной установки:

```text
Reboot System
```

Если TWRP предлагает установить своё приложение, SuperSU или root — для обычной установки LineageOS это не требуется.

Первый запуск может занять несколько минут.

После загрузки проверить:

- Wi-Fi;
- SIM и мобильную сеть;
- входящие SMS;
- звук;
- камеру;
- зарядку и USB.

## 12. WhatsApp

Если телефон нужен для WhatsApp, сначала проверяйте актуальные требования на официальном сайте WhatsApp, потому что минимальная версия Android со временем меняется:

https://www.whatsapp.com/download/android

На этой установке используется Android 7.1.2. Не добавляйте root/Magisk без необходимости: чем меньше модификаций поверх старой системы, тем проще диагностика совместимости приложений.

## 13. Обновление LineageOS

В архиве SourceForge есть более поздняя сборка `lineage-14.1-20180729-UNOFFICIAL-Z00ED.zip`, однако эта инструкция фиксирует именно `20180526`, которая была фактически проверена.

Если текущая система стабильна и используется как рабочий телефон, обновлять её только ради более новой даты необязательно.

При любом будущем обновлении сначала делайте backup и не стирайте `Data`, если цель — обновление поверх существующей системы.

---

# Краткая шпаргалка

```bash
# Fastboot
fastboot devices
fastboot oem device-info

# Временный TWRP для штатного MM
fastboot boot twrp-3.0.2.0-Z00E-MM.img

# Unlock через TWRP ADB Sideload
adb sideload ZE500KL_BootloaderUnlock.zip

# Проверка разблокировки
fastboot oem device-info
# Device unlocked: true

# TWRP для Z00ED
fastboot boot TWRP-3.2.0-Z00ED-20171205.img

# После Wipe System/Data/Cache/Dalvik и запуска ADB Sideload
adb sideload lineage-14.1-20180526-UNOFFICIAL-Z00ED.zip
```

# Важные замечания

1. `ZE500KL` у ASUS также встречается под кодом `Z00ED`; в старой документации LineageOS семейство могло обозначаться иначе. Для файлов в этой инструкции используется именно **Z00ED**.
2. Не применять инструкции для `devinfo` с других MSM8916-устройств: на проверенном ZE500KL раздела `devinfo` не было.
3. Не прошивать случайный `ZE500KL_BootloaderUnlock.zip`. Для проверенного экземпляра зафиксирована SHA-256 `0377dacef945220753cd68aacc95be2d44ecd6e627aa174c771a6c585be3bd73`.
4. Не публиковать дампы `factory`, `modemst*`, `fsg`, `asuskey*` и другие персональные backup-разделы.
5. LineageOS 14.1 и используемые recovery являются архивными и не получают современных обновлений безопасности.
