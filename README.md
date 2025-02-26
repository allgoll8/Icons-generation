# DALL-E 3 Icon Generator с загрузкой в S3 Beget

![Version](https://img.shields.io/badge/version-1.0-blue)
![Python](https://img.shields.io/badge/python-3.7%2B-green)
![License](https://img.shields.io/badge/license-MIT-yellow)

Инструмент для автоматизированной генерации векторных иконок через DALL-E 3 API от OpenAI с последующей загрузкой в S3-совместимое хранилище Beget. Разработан для использования в Google Colab.

## Возможности

- 🎨 Генерация высококачественных векторных иконок через DALL-E 3
- 🔄 Автоматическая загрузка изображений в S3 хранилище Beget
- 🔒 Безопасное хранение API ключей через систему Secrets в Colab
- 📋 Детальная настройка параметров генерации (стиль, качество, размер)
- 🖼️ Предварительный просмотр сгенерированных изображений в Colab
- 📝 Получение публичных ссылок на загруженные изображения
- 🔄 Резервное локальное сохранение в случае проблем с загрузкой

## Требования

- Аккаунт OpenAI с доступом к API и положительным балансом
- Доступ к Google Colab
- S3-совместимое облачное хранилище Beget
- Python 3.7+

## Установка

1. Откройте Google Colab
2. Создайте новый блокнот
3. Скопируйте исходный код из `dall_e_icon_generator.py` в ячейку Colab
4. Добавьте необходимые API ключи в секреты Colab:
   - `OPENAI_API_KEY`: Ваш API ключ OpenAI
   - `S3_ENDPOINT`: URL вашего S3 хранилища (например, https://s3.ru1.storage.beget.cloud)
   - `S3_ACCESS_KEY`: Ключ доступа к S3
   - `S3_SECRET_KEY`: Секретный ключ S3

## Использование

### Базовая генерация

```python
# Генерация иконки с стандартными параметрами
generated_urls = generate_icon(
    prompt="Minimalistic flat vector icon with a blue cat",
    quality="hd",
    style="vivid"
)

if generated_urls:
    s3_urls = download_and_upload_images(generated_urls, "cat_icon")
```

### Настройка промпта

Структура эффективного промпта для генерации иконок:

```
Create a minimalistic flat vector icon for [purpose].
Subject: [primary subject description].
Style: [style description].
Colors: [color palette].
Must have: [required elements].
Must not have: [prohibited elements].
Requirements: [functional requirements].
Design aesthetics: [design style reference].
Additional notes: [any other details].
--ar 1:1 --no text --style flat vector --no gradients
```

### Пример полного промпта

```python
icon_prompt = """
Create a minimalistic flat vector icon for mobile app.
Subject: A stylized mountain peak with a sun rising behind it.
Style: Clean, geometric, flat design with minimal details.
Colors: Use a palette of teal, light orange, and white.
Must have: The mountain should be recognizable, elegant sun rays.
Must not have: No text, no complex textures, no photorealistic elements.
Requirements: The icon should be recognizable at small sizes.
Design aesthetics: Material Design inspired.
Additional notes: Suitable for both light and dark app themes.
--ar 1:1 --no text --style flat vector --no gradients
"""
```

## Структура кода

- `generate_icon()`: Генерирует изображение через DALL-E 3 API
- `upload_to_s3()`: Загружает изображение в S3 хранилище
- `download_and_upload_images()`: Скачивает изображения по URL и загружает их в S3

## Особенности S3 хранилища Beget

При работе с S3 Beget учитывайте следующие моменты:

1. Beget использует свою специфическую реализацию S3 протокола
2. Для успешной загрузки необходимо использовать `signature_version='s3'`
3. URL для доступа к файлам формируется по шаблону: `https://[bucket-name].s3.ru1.storage.beget.cloud/[filename]`
4. В зависимости от настроек бакета, публичные ссылки могут иметь ограниченный срок действия

## Советы по оптимизации промптов

1. **Детализация**: Указывайте конкретные детали для лучших результатов
2. **Стиль**: Явно указывайте "flat vector", "minimalistic" для иконок
3. **Цвета**: Указывайте точную цветовую палитру, можно использовать HEX
4. **Запреты**: Используйте "Must not have" для исключения нежелательных элементов
5. **Размеры**: Добавляйте метки размеров `--ar 1:1` для квадратных иконок

## Устранение неполадок

### Общие проблемы:

- **Ошибка авторизации OpenAI**: Проверьте правильность API ключа и достаточность средств на балансе
- **S3 ошибки**: Проверьте правильность настроек S3 и доступ к бакету
- **Temporary URL**: Проверьте настройки бакета на постоянные ссылки

### Специфические ошибки Beget S3:

- **XAmzContentSHA256Mismatch**: Используйте `signature_version='s3'` вместо `s3v4`
- **AccessDenied**: Проверьте правильность ключей и разрешений бакета

## Лицензия

MIT License

## @allgoll8

(c) 26.02.2025

## Планы на будущее

- Добавление поддержки пакетной генерации множественных иконок
- Интеграция с другими облачными хранилищами
- Улучшенный UI с предпросмотром
- Сохранение истории генераций
