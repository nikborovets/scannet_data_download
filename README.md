## Загрузка ScanNet++ (скрипт с сайта авторов)

- Код и конфиг взяты с официального сайта: https://scannetpp.mlsg.cit.tum.de/scannetpp  
- Назначение: выборочная загрузка данных ScanNet++ (в примере — iPhone с глубиной, ограничение ~20 сцен).

### Быстрый старт
1. Установить зависимости: `pip install -r requirements.txt`
2. В `download_scannetpp.yml` указать:
   - `token`: персональный токен, полученный на сайте.
   - `data_root`: абсолютный путь для выгрузки.
   - `download_options: [iphone]` — тянет iPhone RGB/маски/poses/IMU/COLMAP/Nerfstudio + depth.
   - `scene_limit: 20` — берёт первые 20 сцен из выбранных сплитов (можно убрать или изменить).
3. Запуск: `python download_scannetpp.py download_scannetpp.yml`
4. Можно не править YAML, а задать переменные окружения:
   - `SCANNETPP_TOKEN`
   - `SCANNETPP_DATA_ROOT`
   (читаются из `.env` автоматически, если файл есть)
5. Пример: `cp .env.template .env` и отредактируйте значения.
   Шаблон `.env.template`:
   ```
   SCANNETPP_TOKEN=<YOUR_TOKEN_HERE>
   SCANNETPP_DATA_ROOT=<ABSOLUTE_DOWNLOAD_PATH>
   ```

### Настройка под конкретные сцены
- Если известен список сцен, раскомментируйте `download_scenes` и перечислите ID; закомментируйте `download_splits`/`scene_limit`.
- Для меньшего объёма без depth можно заменить `download_options: [iphone]` на `download_options: [nvs_iphone]`.

### Примечания
- Полный датасет большой (в исходном скрипте предупреждение ~1.5TB). При частичной загрузке объём зависит от числа сцен и выбранных активов.
- Исключения для тестовых сплитов прописаны в `exclude_assets` и применяются автоматически.

### Быстрый venv без sudo (Debian/Ubuntu, нет ensurepip)
- Проблема: `python3 -m venv` ругается на отсутствующий ensurepip, `apt install python3.10-venv` недоступен без sudo.
- Решение:
  ```
  python3 -m venv .venv --without-pip
  source .venv/bin/activate
  curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  python get-pip.py
  rm get-pip.py
  ```
