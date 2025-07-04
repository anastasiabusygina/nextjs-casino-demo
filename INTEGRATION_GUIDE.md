# 🎰 BetSwirl Casino Games - Next.js Integration Guide

> **Полная инструкция интеграции библиотеки casino игр в Next.js проект**

Пошаговое руководство по интеграции библиотеки `@avditte/ui` в существующий Next.js проект.

## 📁 Часть 1: Создание демо Next.js проекта

> Эта часть для демонстрации. Если у вас уже есть Next.js проект, переходите к **Части 2**.

### Шаг 1: Создание папки и Next.js проекта

```bash
# Создаем папку для проекта
mkdir nextjs-casino-demo
cd nextjs-casino-demo

# Создаем Next.js проект в текущей папке
npx create-next-app@latest . --typescript --tailwind --eslint --app --no-src-dir --import-alias "@/*" --use-turbopack
```

### Шаг 2: Проверка установки

```bash
# Запускаем проект чтобы убедиться что все работает
npm run dev
```

Должен открыться стандартный Next.js проект на `http://localhost:3000`

---

## 🎮 Часть 2: Интеграция BetSwirl Casino Games

> **Начинаем интеграцию!** Теперь добавим casino игры в ваш Next.js проект.

### Шаг 1: Установка casino библиотеки

```bash
# Установка библиотеки (теперь доступна в публичном npm реестре)
npm install @avditte/ui@latest
```

> **Что происходит при установке:**
> Библиотека `@avditte/ui` использует технологию "peer dependencies" - это означает, что она автоматически установит все необходимые Web3 библиотеки в правильных версиях:
> - `@tanstack/react-query` - для управления состоянием и кешированием данных
> - `wagmi` - для подключения Web3 кошельков 
> - `viem` - для взаимодействия с блокчейном
> - `@coinbase/onchainkit` - для UI компонентов Coinbase
> - `decimal.js` - для точных математических вычислений с криптовалютами
> 
> Вам не нужно устанавливать эти пакеты отдельно - они установятся автоматически в совместимых версиях.

### Шаг 2: Добавление Web3 провайдеров и импортов

Добавьте необходимые импорты в начало файла `app/page.tsx`:

```tsx
'use client'

import Image from "next/image";
import { createConfig, http } from 'wagmi'
import { base } from 'wagmi/chains'
import { WagmiProvider } from 'wagmi'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { OnchainKitProvider } from '@coinbase/onchainkit'
import { BetSwirlSDKProvider } from '@avditte/ui'
import { CoinTossGame } from '@avditte/ui'
import '@avditte/ui/styles.css'

const config = createConfig({
  chains: [base],
  transports: { [base.id]: http() },
})

const queryClient = new QueryClient()
```

### Шаг 3: Правильная иерархия Web3 провайдеров

**ВАЖНО**: Провайдеры должны быть в строго определенном порядке для корректной работы:

```tsx
export default function Home() {
  return (
    <WagmiProvider config={config}>                    {/* 1. Основа Web3 */}
      <QueryClientProvider client={queryClient}>       {/* 2. Кеширование данных */}
        <OnchainKitProvider chain={base}>              {/* 3. Coinbase UI для Base */}
          <BetSwirlSDKProvider initialChainId={8453}>  {/* 4. Casino функции */}
            
            {/* ВСЕ ваши компоненты должны быть ВНУТРИ всех провайдеров */}
            <div className="grid grid-rows-[20px_1fr_20px] items-center justify-items-center min-h-screen p-8 pb-20 gap-16 sm:p-20 font-[family-name:var(--font-geist-sans)]">
              <main className="flex flex-col gap-[32px] row-start-2 items-center sm:items-start">
                {/* Ваш обычный Next.js контент */}
                <Image src="/next.svg" alt="Next.js logo" width={180} height={38} priority />
                
                {/* Casino игры размещайте внутри main */}
                <div className="mt-8 flex justify-center">
                  <CoinTossGame />
                </div>
              </main>
              
              <footer className="row-start-3 flex gap-[24px] flex-wrap items-center justify-center">
                {/* Ваш футер */}
              </footer>
            </div>
            
          </BetSwirlSDKProvider>
        </OnchainKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

### Почему именно такой порядок провайдеров?

1. **WagmiProvider** (самый внешний)
   - Предоставляет базовые Web3 функции: подключение кошельков, взаимодействие с блокчейном
   - Должен быть первым, так как все остальные провайдеры зависят от него

2. **QueryClientProvider** (второй)
   - Управляет кешированием и синхронизацией данных блокчейна
   - Wagmi v2 требует TanStack Query как обязательную зависимость

3. **OnchainKitProvider** (третий)
   - Предоставляет UI компоненты и утилиты от Coinbase для Base сети
   - Строится поверх Wagmi и React Query

4. **BetSwirlSDKProvider** (самый внутренний)
   - Специфичные для приложения casino функции
   - Зависит от всей Web3 инфраструктуры выше

**КРИТИЧНО**: Компоненты casino игр (CoinTossGame, DiceGame и др.) должны быть размещены ВНУТРИ всех провайдеров!

### Размещение компонента

Casino компонент можно разместить в любом месте вашего приложения:

- **В центре страницы**: `<div className="flex justify-center"><CoinTossGame /></div>`
- **В блоке с отступами**: `<div className="my-8 mx-auto max-w-md"><CoinTossGame /></div>`
- **В карточке**: оберните в div с border и padding по вашему дизайну

Компонент адаптивный и подстроится под ваши стили.

### Шаг 4: Запуск проекта с casino играми

```bash
# Запускаем сервер разработки
npm run dev

# Открываем браузер
# http://localhost:3000
```

**🎉 Готово!** У вас работает Next.js приложение с casino играми! 🎮

### Шаг 5: Деплой на Vercel (публикация в интернете)

Vercel - это платформа для хостинга, созданная командой Next.js. Идеально подходит для Next.js проектов.

#### Регистрация и деплой на Vercel

1. **Перейдите на [vercel.com](https://vercel.com)**
2. **Нажмите "Sign Up"** 
3. **Выберите способ входа:**
   - GitHub (рекомендуется)
   - GitLab
   - Bitbucket
   - Email

4. **Деплой проекта:**
   - После входа нажмите **"Add New..."** → **"Project"**
   - Выберите ваш репозиторий из списка
   - Нажмите **"Import"**

3. **Настройки проекта:**
   - **Project Name**: можете оставить `nextjs-casino-demo` или изменить
   - **Framework Preset**: автоматически определится как "Next.js"
   - **Root Directory**: оставьте `./`
   - **Build Command**: автоматически `npm run build`
   - **Output Directory**: автоматически `.next`

4. **Нажмите "Deploy"**

#### Процесс деплоя

Vercel автоматически:
- ✅ Клонирует ваш репозиторий
- ✅ Установит все зависимости (`npm install`)
- ✅ Соберет проект (`npm run build`)
- ✅ Опубликует в интернете

**Время деплоя:** обычно 1-3 минуты.

#### После успешного деплоя

Вы получите:
- **🌐 Публичный URL** - типа `https://nextjs-casino-demo.vercel.app`
- **📱 Автоматические превью** для каждого Pull Request
- **🔄 Автодеплой** при каждом push в main ветку

#### Полезные функции Vercel

- **Custom Domain**: можете подключить свой домен
- **Analytics**: встроенная аналитика производительности  
- **Environment Variables**: для API ключей и настроек
- **Logs**: логи деплоя и выполнения функций

---

## 🎯 Что получилось

- ✅ **Server-Side Rendering** - Быстрая загрузка страниц
- ✅ **App Router** - Современная архитектура Next.js 15
- ✅ **Turbopack** - Сверхбыстрая сборка в dev режиме
- ✅ **TypeScript** - Типизированный код
- ✅ **Tailwind CSS** - Быстрая стилизация
- ✅ **Casino Games** - Готовые к работе игры

## 🎰 Включенные игры

- 🪙 **CoinToss** - Орел/решка с Chainlink VRF
- 🎲 **Dice** - Бросок кубика с настраиваемыми диапазонами
- 💰 **Реальные ставки** - On-chain транзакции
- 🌐 **Base Chain** - Работает на Coinbase L2

## 📦 Установленные пакеты

```json
{
  "dependencies": {
    "@avditte/ui": "^0.0.18",
    "next": "15.3.3",
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  }
}
```

> **Примечание:** Web3 зависимости (`@coinbase/onchainkit`, `@tanstack/react-query`, `viem`, `wagmi`, `decimal.js`) устанавливаются автоматически как peer dependencies библиотеки `@avditte/ui`.


## 💡 Дальнейшее развитие

- **Добавить больше игр** - Roulette, Blackjack, Slots
- **Мультичейн** - Поддержка Ethereum, Polygon, Arbitrum
- **Аутентификация** - Интеграция с Auth.js
- **База данных** - Сохранение статистики игроков
- **Analytics** - Трекинг поведения пользователей

## 🔧 Troubleshooting

### Ошибки установки
```bash
# Очистка кеша npm
npm cache clean --force

# Удаление node_modules и переустановка
rm -rf node_modules package-lock.json
npm install
```

### Проблемы с Web3
- Убедитесь что у вас установлен MetaMask или другой Web3 кошелек
- Проверьте что выбрана сеть Base (Chain ID: 8453)

**Next.js делает casino игры еще мощнее!** 🎰⚡