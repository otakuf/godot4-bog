# Godot build options generator

Тёмная веб-версия генератора опций сборки Godot — инструмента, который создаёт
файл SCons-опций `custom.py` для компиляции уменьшенных export-шаблонов движка.

Основан на проекте [godot-build-options-generator.github.io](https://github.com/godot-build-options-generator/godot-build-options-generator.github.io)
(Hugo Locurcio and contributors) — интерфейс реализован независимо, без внешних
зависимостей (в оригинале используется Alpine.js), с современным дизайном в
стиле Material Design 3 и двуязычным интерфейсом.

## Как пользоваться

1. Отметьте нужные опции и модули на странице генератора.
2. Скачайте файл кнопкой **Download** (или скопируйте кнопкой **Copy to clipboard**).
3. Положите `custom.py` в корень исходного репозитория Godot.
4. Соберите export-шаблоны: `scons platform=<платформа> target=template_release`.
5. Остальные опции подхватятся из `custom.py` автоматически.

Подробнее об оптимизации сборки по размеру:
[Optimizing a build for size](https://docs.godotengine.org/en/stable/engine_details/development/compiling/optimizing_for_size.html)
(документация Godot 4.x).

## Возможности интерфейса

- Живой поиск по опциям и счётчик «показано X из Y».
- Переключатель языка **🇬🇧 EN / 🇷🇺 RU** (все описания переведены, выбор
  сохраняется в `localStorage`).
- Генерация `custom.py` в реальном времени: в файл попадают только опции,
  отличающиеся от значений по умолчанию.
- Кнопки Download / Copy to clipboard и сброс к значениям по умолчанию.
- Тёмная тема Material Design 3: M3-переключатели, segmented button,
  elevation, ripple, адаптивная двухколоночная сетка.

## Доступные опции

### Общие опции движка (SCons)

| Опция | По умолчанию | Описание |
|---|---|---|
| `extra_suffix` | `custom_build` | Меняет суффикс сборки; удобно добавить название игры |
| `production` | выкл. | Значения по умолчанию для production-сборки |
| `disable_3d` | выкл. | Отключает 3D-ноды (~15% выигрыш для 2D-игр) |
| `optimize` | `speed` | `size` — оптимизация по размеру; чекбокс «Extra» даёт `size_extra` (Godot 4.5+) |
| `threads` | вкл. | Поддержка потоков; отключение убирает требования SharedArrayBuffer для web-экспорта |
| `precision` | `single` | `double` — числа двойной точности для крупных миров (нужны GDExtension под неё) |
| `disable_advanced_gui` | выкл. | Отключает Tree, TextEdit, RichTextLabel, GraphEdit, FileDialog и др. |
| `deprecated` | вкл. | Устаревшие возможности; отключение немного уменьшает бинарник |
| `disable_physics_2d` | выкл. | Полностью отключает 2D-физику |
| `disable_physics_3d` | выкл. | Полностью отключает 3D-физику |
| `minizip` | вкл. | Нужен для ZIPReader/ZIPWriter и данных в ZIP вместо PCK |
| `brotli` | вкл. | Нужен для Brotli-компрессии и шрифтов WOFF2 |
| `vulkan` | вкл. | Рендеринг Forward+ и Mobile через Vulkan |
| `opengl3` | вкл. | Рендеринг Compatibility (OpenGL 3.3 / GLES 3.0 / WebGL 2.0) |
| `d3d12` | выкл. | Forward+/Mobile через Direct3D 12 (только Windows) |
| `custom_modules` | пусто | Путь к папке с пользовательскими C++-модулями |

### Модули (module_<имя>_enabled)

В генераторе доступны те же модули, что и в оригинальном проекте
(актуально для Godot 4.x; editor-only модули не перечисляются):

- **Изображения:** `bmp`, `dds`, `hdr`, `jpg`, `ktx`, `svg`, `tga`,
  `basis_universal`, `squish`
- **Аудио и видео:** `minimp3`, `vorbis`, `ogg`, `theora`, `interactive_music`
- **3D:** `gltf`, `fbx`, `csg`, `gridmap`, `vhacd`, `meshoptimizer`, `raycast`,
  `noise`
- **Навигация и физика:** `navigation_2d`, `navigation_3d`, `godot_physics_3d`,
  `jolt`
- **Сеть:** `enet`, `mbedtls`, `multiplayer`, `webrtc`, `websocket`, `upnp`,
  `jsonrpc`
- **Текст и шрифты:** `freetype`, `msdfgen`, `text_server_adv`,
  `text_server_fb` (включайте, если выключили Advanced — иначе текст не
  отобразится)
- **Платформы и ввод:** `camera`, `mobile_vr`, `openxr`, `webxr`
- **Прочее:** `gdscript`, `mono` (C#/.NET), `regex`, `zip`

Полный список с описаниями — на самой странице генератора.

## Установка и запуск

Статический сайт — достаточно любой папки, отдаваемой веб-сервером:

```bash
# nginx
root /path/to/public;
index index.html;

# или для быстрой проверки
python3 -m http.server -d public 8080
```

## Структура проекта

```
public/
└── index.html   # страница-генератор (HTML + CSS + JS в одном файле)
```

## Лицензия и авторство

Интерфейс — независимая реализация; оригинальный проект:
© 2020–present Hugo Locurcio and contributors
([GitHub](https://github.com/godot-build-options-generator/godot-build-options-generator.github.io)).
