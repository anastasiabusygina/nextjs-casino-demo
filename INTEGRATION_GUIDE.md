# 🎰 BetSwirl Casino Games - Next.js Integration Guide

> **Полная инструкция интеграции библиотеки casino игр в Next.js проект**

Пошаговое руководство по интеграции библиотеки `@chainhackers/ui` в существующий Next.js проект.

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

### Шаг 1: Подключение к приватному реестру

```bash
# Создайте .npmrc файл в корне проекта:
cat > .npmrc << EOF
@chainhackers:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
EOF
```

> **Примечание:** Замените `YOUR_GITHUB_TOKEN` на ваш GitHub Personal Access Token с правами `read:packages`

### Шаг 2: Установка casino библиотеки

```bash
# Установка основной библиотеки
npm install @chainhackers/ui@latest

# Peer dependencies (требуются для работы библиотеки)
npm install @tanstack/react-query wagmi viem @coinbase/onchainkit decimal.js
```

### Шаг 3: Добавление Web3 провайдеров и импортов

Добавьте необходимые импорты в начало файла `app/page.tsx`:

```tsx
'use client'

import Image from "next/image";
import { createConfig, http } from 'wagmi'
import { base } from 'wagmi/chains'
import { WagmiProvider } from 'wagmi'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { OnchainKitProvider } from '@coinbase/onchainkit'
import { BetSwirlSDKProvider } from '@chainhackers/ui'
import { CoinTossGame } from '@chainhackers/ui'
import '@chainhackers/ui/styles.css'

const config = createConfig({
  chains: [base],
  transports: { [base.id]: http() },
})

const queryClient = new QueryClient()
```

### Шаг 4: Оборачивание в провайдеры

Оберните весь JSX в необходимые Web3 провайдеры:

```tsx
export default function Home() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <OnchainKitProvider chain={base}>
          <BetSwirlSDKProvider initialChainId={8453}>
            {/* Ваш существующий JSX */}
            
            {/* Добавьте casino компонент где нужно */}
            <div className="mt-8 flex justify-center">
              <CoinTossGame />
            </div>
            
          </BetSwirlSDKProvider>
        </OnchainKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

### Размещение компонента

Casino компонент можно разместить в любом месте вашего приложения:

- **В центре страницы**: `<div className="flex justify-center"><CoinTossGame /></div>`
- **В блоке с отступами**: `<div className="my-8 mx-auto max-w-md"><CoinTossGame /></div>`
- **В карточке**: оберните в div с border и padding по вашему дизайну

Компонент адаптивный и подстроится под ваши стили.

### Шаг 5: Запуск проекта с casino играми

```bash
# Запускаем сервер разработки
npm run dev

# Открываем браузер
# http://localhost:3000
```

**🎉 Готово!** У вас работает Next.js приложение с casino играми! 🎮

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
    "@chainhackers/ui": "^0.0.16",
    "@coinbase/onchainkit": "^0.38.13",
    "@tanstack/react-query": "^5.80.6",
    "decimal.js": "^10.5.0",
    "next": "15.3.3",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "viem": "^2.31.0",
    "wagmi": "^2.15.6"
  }
}
```

## 🚀 Деплой на Vercel

```bash
# Подключить к Vercel
npx vercel

# Или использовать GitHub integration
# https://vercel.com/new
```

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