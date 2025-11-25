# KPZ Utils Library (v2.0.0)

Ця бібліотека розроблена як частина лабораторної роботи для демонстрації принципів **семантичного версіонування (SemVer)**, роботи з **TypeScript** (типи, інтерфейси, класи, generics), а також налаштування сучасного JS/TS-проєкту з використанням **ESLint**, **Prettier** та **змінних оточення** (`.env` / Zod).

Бібліотека пропонує набір простих, але типізованих утиліт, які можна використовувати в будь-якому Node.js-застосунку для базових математичних операцій, форматування чисел, маніпуляцій з рядками та логування.

---

## Встановлення та Запуск

### Встановлення

```bash
npm install
```

### Скрипти

| Скрипт            | Призначення                                                                 |
|-------------------|------------------------------------------------------------------------------|
| npm run demo      | Запуск демонстраційного файлу `src/demo.ts` для перевірки функціоналу.       |
| npm run build     | Компіляція вихідного коду (`src/`) у формат CommonJS та ESM в папці `dist/`. |
| npm run typecheck | Перевірка TypeScript на наявність помилок типізації.                         |
| npm run lint      | Запуск ESLint для перевірки стилю та правил (включно із забороною `any`).    |
| npm run format    | Форматування коду за допомогою Prettier.                                     |

---

## Приклади Використання

Всі публічні сутності експортуються з основного модуля **`index.ts`**.

### Імпорт

```ts
import { add, capitalize, formatNumber, groupBy, Logger, config } from 'kpz';
```

### Функції та Класи

#### 1. `add`  
**BREAKING CHANGE v2.0.0:** приймає масив чисел через rest-параметри  

```ts
const sum = add(1, 2, 3, 4);
console.log(sum); // Вивід: 10
```

#### 2. `capitalize`  

```ts
const capitalized = capitalize('hello world');
console.log(capitalized); // Вивід: Hello world
```

#### 3. `formatNumber`  
Використовує конфігурацію (наприклад, `APP_PRECISION` з `.env`)  

```ts
const formatted = formatNumber(123.45678, { precision: 2 });
console.log(formatted); // Вивід: 123.46
```

#### 4. `groupBy`  
Використовує **Generics**  

```ts
const items = [{ id: 1, type: 'A' }, { id: 2, type: 'B' }];
const grouped = groupBy(items, 'type');
console.log(grouped); // Вивід: { A: [ ... ], B: [ ... ] }
```

#### 5. `Logger`  
Використовує `LogLevel` з `.env`  

```ts
const logger = new Logger(config.LOG_LEVEL);

logger.info('Застосунок працює.'); // Вивід, якщо LOG_LEVEL не 'silent'
logger.debug('Деталі (для розробки).'); // Вивід, якщо LOG_LEVEL = 'debug'
```

---

## ⚙️ Конфігурація через .env

Бібліотека використовує файл `.env` у корені проєкту для налаштування логування та точності чисел.  
Валідація змінних виконується за допомогою Zod у `src/config.ts`.

| Ключ         | Призначення                                      | Очікувані значення        |
|--------------|--------------------------------------------------|----------------------------|
| APP_PRECISION| Точність за замовчуванням для `formatNumber`.    | Ціле число (0 до 10).      |
| LOG_LEVEL    | Рівень логування для класу `Logger`.             | `silent`, `info`, `debug` |

### Приклад `.env`

```ini
APP_PRECISION=3
LOG_LEVEL=debug
```

---

## Еволюція Проєкту (SemVer)

| Версія  | Що додано / Чому змінилась версія                                         | Тип зміни |
|---------|---------------------------------------------------------------------------|-----------|
| 0.1.0   | Додані початкові функції (`add`, `capitalize`) з використанням `any`.     | MINOR     |
| 0.2.0   | Впроваджено базові суворі типи (`number`, `string`) у функціях.           | PATCH/MINOR |
| 0.3.0   | Додана функція `formatNumber` зі складним типом `NumberFormatOptions`.    | MINOR     |
| 0.4.0   | Впроваджено інтерфейси (`IObjectWithKey`) та функцію `groupBy` з Generics.| MINOR     |
| 0.5.0   | Додано клас `Logger`, dotenv та Zod для валідації конфігурації з `.env`.  | MINOR     |
| 1.0.0   | Стабілізація API: фіналізація експортів, ESLint (заборона `any`).         | MAJOR     |
| 2.0.0   | Breaking Change: зміна сигнатури `add` з `(a, b)` на `(...nums: number[])`| MAJOR     |

---

## Теги Релізів

Ви можете переглянути історію всіх релізів за посиланнями на теги Git:

- [`v2.0.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v2.0.0)
- [`v1.0.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v1.0.0)
- [`v0.5.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v0.5.0)
- [`v0.4.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v0.4.0)
- [`v0.3.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v0.3.0)
- [`v0.2.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v0.2.0)
- [`v0.1.0`](https://github.com/johuirmbegytm/kpz/releases/tag/v0.1.0)
