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

### Шаг 3: Создание компонента с играми

Создайте файл `app/components/BetSwirlGames.tsx`:

```tsx
'use client'

import React from 'react'
import { createConfig, http } from 'wagmi'
import { base } from 'wagmi/chains'
import { WagmiProvider } from 'wagmi'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { OnchainKitProvider } from '@coinbase/onchainkit'
import { BetSwirlSDKProvider } from '@chainhackers/ui'
import { CoinTossGame, DiceGame } from '@chainhackers/ui'
import '@chainhackers/ui/styles.css'

const config = createConfig({
  chains: [base],
  transports: { [base.id]: http() },
})

const queryClient = new QueryClient()

export default function BetSwirlGames() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <OnchainKitProvider chain={base}>
          <BetSwirlSDKProvider initialChainId={8453}>
            <div className="min-h-screen bg-gray-100 p-8">
              <h1 className="text-3xl font-bold mb-8">🎰 BetSwirl + Next.js = 🚀</h1>
              
              <p className="mb-8 text-lg">
                Next.js интеграция работает! Теперь можно делать SSR + casino игры! 💰
              </p>
              
              <div className="space-y-8">
                <section>
                  <h2 className="text-2xl font-semibold mb-4">🪙 Coin Toss Game</h2>
                  <CoinTossGame />
                </section>
                
                <section>
                  <h2 className="text-2xl font-semibold mb-4">🎲 Dice Game</h2>
                  <DiceGame />
                </section>
              </div>
            </div>
          </BetSwirlSDKProvider>
        </OnchainKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  )
}
```

### Шаг 4: Обновление главной страницы

Замените содержимое файла `app/page.tsx`:

```tsx
import BetSwirlGames from './components/BetSwirlGames'

export default function Home() {
  return <BetSwirlGames />
}
```

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
    "@betswirl/sdk-core": "^0.1.6",
    "@betswirl/wagmi-provider": "^0.1.6", 
    "@chainhackers/ui": "^0.0.16",
    "@coinbase/onchainkit": "^0.38.13",
    "@tanstack/react-query": "^5.80.6",
    "decimal.js": "^10.5.0",
    "next": "15.3.3",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "viem": "^2.29.1",
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