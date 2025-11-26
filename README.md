# Лабораторно-практична робота №2
## Робота з package.json, залежностями, змінними оточення, семантичним версіонуванням, базовими можливостями TypeScript (type, interface, class).
## Мета
* Ознайомитися зі структурою package.json та призначенням основних полів
* Навчитися відрізняти робочі залежності (dependencies) від залежностей для розробки (devDependencies).
* Розібратися з принципами семантичного версіонування (SemVer) і навчитися оновлювати версії коректно.
* Дослідити використання змінних оточення через .env файли та підключення їх у коді.
* Освоїти базові конструкції TypeScript: типи, інтерфейси, класи, generics.
* Налаштувати інструменти перевірки коду: ESLint, Prettier, Husky, Commitlint.
* Закріпити практику роботи з Git та GitHub (ініціалізація репозиторію, коміти, теги версій).
### Хід роботи:
1. Ініціалізую проєкт  
Створюю репозиторій на GitHub та клоную його локально
У репозиторії натисни Code → вкладка HTTPS → копіюю посилання на свій гіт.  
Відкриваю термінал на комп’ютері та відкриваю Git Bash. За допомогою команди cd переходжу в папку, куди хочу клонувати і виконую команту git clone https://github.com/johuirmbegytm/kpz.git
2. Ініцілазізую npm-проєкт  
Для цього вводжу команду npm init  
Заповнюю name, version (0.0.0), description, license тощо. Це створить базовий package.json.
3. Створюю .gitignore з таким вмістом:
```
node_modules/  
dist/  
.env
```  
4. Встановлюю залежності:  
```
npm i dotenv zod  
  
npm i -D typescript tsup eslint prettier husky commitlint @commitlint/config-conventional @types/node tsx @eslint/js typescript-eslint eslint-config-prettier
```  
5. Ініціалізую TypeScript
```
npx tsc --init
```
6. Створюю файл eslint.config.cjs та заповнюю його
```js
const js = require('@eslint/js');
const tseslint = require('typescript-eslint');
const prettier = require('eslint-config-prettier');

module.exports = [
  { ignores: ['**/*.cjs'] }, // ігноруємо конфігураційні файли у CJS
  js.configs.recommended,    // базові правила JS
  ...tseslint.configs.recommended, // базові правила TS
  prettier,                  // відключення конфліктів з Prettier
  {
    files: ['**/*.ts'],
    languageOptions: {
      parser: tseslint.parser,
      parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module',
      },
    },
    plugins: {
      '@typescript-eslint': tseslint.plugin,
    },

    rules: {
      '@typescript-eslint/no-explicit-any': 'warn',
      'no-unused-vars': 'warn',
    },
  },
];

```
7. Створюю .prettierr.cjs та заповнюю його:  
```js
module.exports = {
  semi: true,
  singleQuote: true,
  printWidth: 100,
  trailingComma: 'all',
};

```
8. Додаю у package.json розділ scripts:  
```json
{
  "scripts": {
    "build": "tsup src/index.ts --format cjs,esm --dts",
    "lint": "eslint . --ext .ts",
    "lint:fix": "eslint . --ext .ts --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "typecheck": "tsc --noEmit",
    "demo": "tsx src/demo.ts"
  }
}
```
9. Створюю файл commitlint.config.cjs:  
```cjs
module.exports = { extends: ['@commitlint/config-conventional'] };
```
10. Налаштовую Husky та додаю git-хуки:  
```bash
npx husky init
```
Команда створить папку .husky/ і додасть постінсталяційний скрипт у package.json.  
Створюю файл .husky/pre-commit:  
```
echo "npm run lint && npm run format:check && npm run typecheck" > .husky/pre-commit
chmod +x .husky/pre-commit
```
Створюю файл .husky/commit-msg:  
```
echo "npx --no-install commitlint --edit \$1" > .husky/commit-msg
chmod +x .husky/commit-msg
```
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/1.png)
11. Створюю папку src і в ньому файл index.ts  
```
mkdir src
echo "" > src/index.ts
```
12. Повторюю перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/2.png)
13. Коммітаю  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/3.png)

## Версія 0.1.0 - прості функції з any  
1. Оновлюю src/index.ts - додаємо тип і функцію:  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/4.png)  
Оновлюємо src/demo.ts з навмисною помилкою  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/5.png)
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/6.png)
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/7.png)
4. Оновлюю вміст файлу src/demo.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/8.png)
5. Повторна перевірка  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/9.png)
Помилки зникли, все добре.  
6. Комміт і підняття версії  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/10.png)
## Версія 0.2.0 - ті ж функції, але з базовими типами  
1. Оновлюю src/index.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/11.png)
2. Роблю навмисну помилку в src/demo.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/12.png)  
3. Запускаю перевірку  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/13.png)
Отримуємо очікувані помилки
4. Оновлюю src/demo.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/14.png)
5. Повторюю перевірки та, за потреби, автофікс  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/15.png)
6. Комміт і підняття версії  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/16.png)
## Версія 0.3.0 - нова функція зі складним типом  
1.	Оновлюю src/index.ts – додаємо тип і функцію:  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/17.png)  
2.	Оновлюю src/demo.ts з навмисною помилкою  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/18.png)
3.	Запускаю перевірку  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/19.png)
Отримуємо очікувану помилку при перевірці
4.	Виправляємо код у src/demo.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/20.png)
5.	Повторюю перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/21.png)  
Помилок не знайденно!
6.	Комміт і підняття версії  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/22.png)
1.	Оновлюю src/index.ts – додаю інтерфейс і універсальну функцію groupBy:  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/23.png)  
2. Оновлюю src/demo.ts — навмисно роблю помилку типу для groupBy:  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/24.png)  
3. Запускаю перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/25.png)  
Отримуємо очікувану помилку  
4. Виправляю помилку  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/26.png)  
5. Повторюю перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/27.png)  
Помилок не знайдено!
6. Комітт і підняття версії  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/28.png)  
## Версія 0.5.0 — клас Logger + змінні оточення (.env)
1. Створюю файл src/config.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/29.png)  
2.	Оновлюю src/index.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/30.png)   
3.	Створюю або оновлюю .env  
Створюю .env:  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/31.png)  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/32.png)  
4.	Оновлюю src/demo.ts – спочатку роблю помилковий виклик щоб побачити помилку типів  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/33.png)  
5.	Запускаю перевірку  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/34.png)
Отримуємо очікувані помилки
6.	Виправляю src/demo.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/35.png)
7.	Повторюємо перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/36.png)  
Критичних помилок не знайдено, все добре.  
8.	Комміт і підняття версії  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/37.png)  
## Версія 1.0.0 — стабілізація публічного API + посилення правил
Щоб гарантувати, що в коді більше немає небезпечних типів any, які можуть створювати проблеми для споживачів бібліотеки, потрібно посилити правило в ESLint.  
Відкриваю файл eslint.config.cjs та змінюю правило для @typescript-eslint/no-explicit-any з 'warn' на 'error' у блоці rules.  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/38.png)  
3. Упорядковую публічні експорти (тільки src/index.ts)  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/39.png)  
4. Оновлюю package.json — поле exports і types  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/40.png)  
5.  Збірка й перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/41.png)  
6. Коміт і версія 1.0.0  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/42.png)  
## Версія 2.0.0 — стабілізація публічного API + посилення правил
1. Змінюю сигнатуру add у src/utils/add.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/43.png)  
2.	Роблю навмисно помилковий виклик у src/demo.ts  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/44.png)  
Очікувані помилки  
3.	Виправляю виклик під новий API  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/45.png)  
4.	Повторюю перевірки  
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/46.png)   
5.	Коміт і версія 2.0.0    
![alt text](https://github.com/johuirmbegytm/kpz2/blob/main/images/47.png)      
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
