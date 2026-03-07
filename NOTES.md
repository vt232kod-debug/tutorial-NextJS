# Next.js Dashboard Tutorial — Notes & Screenshots

Проходження офіційного туторіалу Next.js (Dashboard App).  
Сертифікат: https://nextjs.org/learn/certificate?course=dashboard-app&user=150855&certId=dashboard-app-150855-1772877087670

---

## 1. Створення проекту

Ініціалізував проект за допомогою `create-next-app` з офіційного стартового шаблону від Vercel.

```bash
npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
```

![Створення проекту](images/Pasted%20image%2020260304100852.png)

**Питання:** Чому Next.js рекомендує pnpm замість npm? — pnpm ефективніше використовує дисковий простір через жорсткі посилання на пакети.

---

## 2. Запуск проекту на локальному сервері

Запустив dev-сервер командою `pnpm run dev`.

![Термінал — запуск dev сервера](images/Pasted%20image%2020260304102423.png)
![Браузер — стартова сторінка](images/Pasted%20image%2020260304102357.png)

**Думки:** Next.js 16 використовує Turbopack замість Webpack за замовчуванням — він значно швидший при hot-reload завдяки інкрементальній компіляції.

---

## 3. CSS Модулі

Попрацював з CSS Modules — кожен компонент має свій локальний `.module.css` файл.

![CSS Modules — код](images/Pasted%20image%2020260304110641.png)
![CSS Modules — результат у браузері](images/Pasted%20image%2020260304110619.png)

**Питання:** Яка різниця між CSS Modules і Tailwind CSS? — CSS Modules дають scoped-стилі через унікальні класи, Tailwind — утилітарний підхід без написання власного CSS. Next.js підтримує обидва підходи одночасно.

---

## 4. Підключення зображень та шрифтів

Використав компонент `<Image>` з `next/image` та `next/font/google` для завантаження шрифтів без layout shift.

![Шрифти — код](images/Pasted%20image%2020260304122350.png)
![Зображення — код](images/Pasted%20image%2020260304122410.png)
![Сторінка з логотипом і шрифтом](images/Pasted%20image%2020260304122454.png)
![Fonts файл](images/Pasted%20image%2020260304122609.png)

**Думки:** `next/image` автоматично оптимізує зображення (WebP, lazy loading, правильний розмір для viewport). Шрифти за допомогою `next/font` завантажуються разом з білдом і не спричиняють зайвих мережевих запитів.

---

## 5. Створення нових сторінок та вкладених маршрутів

В Next.js App Router файл `page.tsx` у певній папці = маршрут. Додав сторінки `/dashboard`, `/dashboard/invoices`, `/dashboard/customers`.

![Структура папок — маршрути](images/Pasted%20image%2020260304123920.png)
![Нова сторінка в браузері](images/Pasted%20image%2020260304123934.png)
![Layout файл](images/Pasted%20image%2020260304125347.png)

**Питання:** Як працює `layout.tsx`? — Layout-файл обгортає всі дочірні сторінки в межах своєї папки. Він не перерендерується при навігації між дочірніми сторінками — лише `page.tsx` змінюється.

---

## 6. Деплой на Vercel та підключення бази даних

Задеплоїв проект на Vercel через GitHub. Підключив Postgres базу даних через Vercel Storage.

![Vercel деплой](images/Pasted%20image%2020260305100238.png)
![База даних підключена](images/Pasted%20image%2020260305100326.png)

**Думки:** Vercel автоматично деплоїть кожен push у main. Postgres від Vercel використовує `@vercel/postgres` — це serverless PostgreSQL, яка добре інтегрується з Next.js Server Actions.

---

## 7. Seeding бази даних та відображення на дашборді

Запустив `/seed` ендпоінт для заповнення бази тестовими даними. Підключив Server Components для отримання даних напряму з БД.

![Seed результат](images/Pasted%20image%2020260305115440.png)
![Дані в dashboard](images/Pasted%20image%2020260305115451.png)
![Dashboard сторінка повністю](images/Pasted%20image%2020260305125219.png)

**Питання:** Чому Server Components можуть звертатись до БД напряму? — Вони виконуються тільки на сервері, тому секрети (DB credentials) не потрапляють на клієнт. Це одна з ключових переваг App Router.

---

## 8. Форма створення інвойсу та Server Actions

Додав сторінку `/dashboard/invoices/create` з формою. Дані відправляються через Server Action — функцію з директивою `'use server'`.

![Форма створення інвойсу в браузері](images/Pasted%20image%2020260306102258.png)

**Думки:** Server Actions — це mutation-функції, які виконуються на сервері, але викликаються з клієнтського коду. Це замінює окремий API-ендпоінт — не потрібно писати `POST /api/invoices`.

---

## 9. Функціонал пошуку (Search)

Реалізував пошук через URL search params (`?query=...`). Використав хуки `useSearchParams`, `usePathname`, `useRouter`.

![Пошук — компонент](images/Pasted%20image%2020260306103935.png)
![Пошук у браузері](images/Pasted%20image%2020260306103954.png)

**Питання:** Чому пошук через URL params, а не через state? — URL-based пошук дозволяє зберігати стан пошуку при оновленні сторінки, копіюванні URL та роботі кнопок назад/вперед у браузері.

---

## 10. Пагінація, редагування та видалення

Додав пагінацію для списку інвойсів. Реалізував редагування (`/invoices/[id]/edit`) та видалення записів через Server Actions.

![Пагінація і таблиця](images/Pasted%20image%2020260306134945.png)
![Редагування інвойсу](images/Pasted%20image%2020260306134954.png)

**Думки:** Dynamic routes в Next.js — папки з `[id]` в назві. Значення `id` доступне через параметр `params` у сторінці.

---

## 11. Валідація форм (Zod)

Додав серверну валідацію полів за допомогою бібліотеки Zod. При помилках користувач бачить повідомлення без оновлення сторінки.

![Валідація полів форми](images/Pasted%20image%2020260306143900.png)

**Питання:** Чому валідація на сервері, якщо є валідація на клієнті? — Клієнтська валідація може бути обійдена. Серверна валідація — обов'язковий захист. Next.js дозволяє робити обидві без зайвого дублювання коду через `useActionState`.

---

## 12. Аутентифікація (NextAuth.js)

Налаштував аутентифікацію через NextAuth.js v5. Додав login-форму, захистив маршрути через middleware.

![Login сторінка](images/Pasted%20image%2020260307092545.png)
![Middleware захист маршрутів](images/Pasted%20image%2020260307094314.png)
![auth.config.ts](images/Pasted%20image%2020260307094333.png)
![Успішний вхід у dashboard](images/Pasted%20image%2020260307094908.png)
![Auth файл](images/Pasted%20image%2020260307095028.png)

**Думки:** NextAuth middleware (`auth.ts`) перехоплює кожен запит і перевіряє сесію before rendering. Це набагато ефективніше ніж перевірка авторизації всередині кожної сторінки окремо.

---

## 13. Metadata

Додав metadata для сторінок через `export const metadata` та `generateMetadata()`. Це покращує SEO та вигляд при розшаренні у соцмережах.

![Metadata — код і результат](images/Pasted%20image%2020260307095116.png)

**Питання:** Яка різниця між статичним `metadata` і `generateMetadata()`? — Статичний `metadata` об'єкт підходить для сторінок з фіксованим заголовком. `generateMetadata()` потрібен коли metadata залежить від даних (наприклад, назва конкретного інвойсу при динамічному маршруті).

---

## Загальні висновки

- **App Router vs Pages Router** — App Router (з папкою `app/`) є сучасним підходом з підтримкою Server Components, streaming, nested layouts.
- **Server Components** виконуються тільки на сервері — менше JS на клієнті, прямий доступ до БД.
- **Server Actions** замінюють REST API для mutations — менше коду, вбудована безпека.
- **Streaming** через `loading.tsx` і `<Suspense>` дозволяє показувати частини сторінки до завершення завантаження всіх даних.
- **next/image** та **next/font** автоматично оптимізують ресурси.

Сертифікат про завершення: https://nextjs.org/learn/certificate?course=dashboard-app&user=150855&certId=dashboard-app-150855-1772877087670
