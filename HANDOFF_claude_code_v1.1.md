# Rocksbox — HANDOFF v1.1 (2026-06-21)
_Внедрена архитектура типов: разделение на БИБЛИОТЕКУ и НОВЫЕ ТИПЫ._

## Что изменилось vs v1.0:

### ✅ Реализовано:
1. **Архитектура типов разделена:**
   - `libConstructionTypes`, `libPanelTypes`, `libMatTypes`, `libElementTypes` — базовая библиотека (неменяется)
   - `newConstructionTypes`, `newPanelTypes`, `newMatTypes`, `newElementTypes` — типы, добавленные в текущем проекте (сохраняются)
   - `constructionTypes`, `panelTypes`, `matTypes`, `elementTypes` — комбинация (библиотека + новые)

2. **CSV-файлы библиотеки созданы:**
   - `library-construction-types.csv` — конструкции (name;carrier;finish;note)
   - `library-panel-types.csv` — панели (name;description)
   - `library-mat-types.csv` — маты (name;description)
   - `library-element-types.csv` — типовые элементы (name;description)

3. **Функции импорта/экспорта CSV:**
   - `parseCSV()` — парсит CSV с разделителем `;`
   - `importLibraryCSV(kind, csvText)` — загружает CSV в library*Types
   - `uploadLibraryCSV(input, kind)` — обрабатывает файл input
   - `exportLibraryCSV(kind)` — скачивает CSV из текущей библиотеки

4. **Раздел управления библиотекой в форме:**
   - 4 пары кнопок (импорт/экспорт для каждого типа)
   - Hidden file inputs для загрузки CSV
   - Workflow: экспорт → редактирование в Excel/Sheets → импорт

5. **Функции добавления типов обновлены:**
   - `promptAddType(kind)` — добавляет в `new*Types`, обновляет комбинированный массив
   - `promptAddConstruction(location)` — добавляет конструкцию по месту крепления

6. **Export/Import:**
   - Сохранение: экспортируем только `_newTypes` (новые типы)
   - Загрузка: восстанавливаем `newTypes` из `_newTypes`, комбинируем с библиотекой

### 📝 Логика workflow:

```
Проект 1:
  Библиотека (lib) + Новые типы из проекта 1 → Export для Claude
  ↓
Web-Claude обрабатывает, очищает дубли, создаёт новый HANDOFF
  ↓
Я вшиваю обновленную библиотеку в форму (обновляю library-*.json)

Проект 2:
  Обновленная библиотека (lib) + Новые типы из проекта 2 → Export для Claude
```

### ⚠️ Еще не реализовано:
- Загрузка `library-*.json` из файлов (сейчас типы зашиты в коде)
- Система версионирования HANDOFF (_v1.0, _v1.1 и т.д.)
- localStorage autosave
- .docx генерация

---

# ПОЛНЫЙ КОНТЕКСТ (из v1.0)

[см. HANDOFF_claude_code_v1.0.md]

Дополнения к архитектуре:

## Версионирование архитектуры

Сохраняем снимки состояния:
- `HANDOFF_claude_code_v1.0.md` — исходная версия с монолитной библиотекой
- `HANDOFF_claude_code_v1.1.md` — текущая (разделённая архитектура)
- При откате → берём нужную версию, я восстанавливаюсь из её контекста

## Текущее состояние формы (v4)

Файл `rocksbox_tz_form.html`:
- Архитектура типов: разделена на lib* и new*
- JSON export: `_newTypes` вместо `_catalogs`
- JSON import: восстанавливает новые типы, комбинирует с библиотекой
- Остальное как в v3

## Что делать дальше (приоритет):

1. ✅ Архитектура типов разделена (DONE)
2. ✅ CSV файлы и система импорта/экспорта (DONE 2026-06-21)
3. ⏳ **localStorage autosave** (автосохранение проекта по названию)
4. **Раздел "Условия выполнения работ"** (раздел 6)
5. **Раздел "Этапы"** (раздел 7)
6. **Раздел "Гарантия"** (раздел 9)
7. Генерация .docx
