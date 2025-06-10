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

### Шаг 3: Интеграция casino игр прямо в главную страницу

Замените содержимое файла `app/page.tsx`:

```tsx
'use client'

import Image from "next/image";
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

export default function Home() {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <OnchainKitProvider chain={base}>
          <BetSwirlSDKProvider initialChainId={8453}>
            <div className="grid grid-rows-[20px_1fr_auto_20px] items-center justify-items-center min-h-screen p-8 pb-20 gap-16 sm:p-20 font-[family-name:var(--font-geist-sans)]">
              {/* Существующий контент Next.js */}
              <main className="flex flex-col gap-[32px] row-start-2 items-center sm:items-start">
                <Image
                  className="dark:invert"
                  src="/next.svg"
                  alt="Next.js logo"
                  width={180}
                  height={38}
                  priority
                />
                <ol className="list-inside list-decimal text-sm/6 text-center sm:text-left font-[family-name:var(--font-geist-mono)]">
                  <li className="mb-2 tracking-[-.01em]">
                    Get started by editing{" "}
                    <code className="bg-black/[.05] dark:bg-white/[.06] px-1 py-0.5 rounded font-[family-name:var(--font-geist-mono)] font-semibold">
                      app/page.tsx
                    </code>
                    .
                  </li>
                  <li className="tracking-[-.01em]">
                    Save and see your changes instantly.
                  </li>
                </ol>

                <div className="flex gap-4 items-center flex-col sm:flex-row">
                  <a
                    className="rounded-full border border-solid border-transparent transition-colors flex items-center justify-center bg-foreground text-background gap-2 hover:bg-[#383838] dark:hover:bg-[#ccc] font-medium text-sm sm:text-base h-10 sm:h-12 px-4 sm:px-5 sm:w-auto"
                    href="https://vercel.com/new?utm_source=create-next-app&utm_medium=appdir-template-tw&utm_campaign=create-next-app"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    <Image
                      className="dark:invert"
                      src="/vercel.svg"
                      alt="Vercel logomark"
                      width={20}
                      height={20}
                    />
                    Deploy now
                  </a>
                  <a
                    className="rounded-full border border-solid border-black/[.08] dark:border-white/[.145] transition-colors flex items-center justify-center hover:bg-[#f2f2f2] dark:hover:bg-[#1a1a1a] hover:border-transparent font-medium text-sm sm:text-base h-10 sm:h-12 px-4 sm:px-5 w-full sm:w-auto md:w-[158px]"
                    href="https://nextjs.org/docs?utm_source=create-next-app&utm_medium=appdir-template-tw&utm_campaign=create-next-app"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    Read our docs
                  </a>
                </div>
              </main>

              {/* 🎰 НОВАЯ СЕКЦИЯ: Casino Games */}
              <section className="row-start-3 w-full max-w-4xl">
                <div className="border-t border-gray-200 dark:border-gray-800 pt-8">
                  <h2 className="text-2xl font-bold text-center mb-6">
                    🎰 Try Casino Games!
                  </h2>
                  <div className="bg-white dark:bg-gray-900 rounded-lg border border-gray-200 dark:border-gray-800 p-6">
                    <p className="text-center text-gray-600 dark:text-gray-400 mb-6">
                      🚀 <strong>BetSwirl x Next.js</strong> - Casino игры с SSR! 💰
                    </p>
                    
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                      <div className="space-y-4">
                        <h3 className="text-lg font-semibold text-center">🪙 Coin Toss</h3>
                        <CoinTossGame />
                      </div>
                      
                      <div className="space-y-4">
                        <h3 className="text-lg font-semibold text-center">🎲 Dice Game</h3>
                        <DiceGame />
                      </div>
                    </div>
                  </div>
                </div>
              </section>

              {/* Существующий футер */}
              <footer className="row-start-4 flex gap-[24px] flex-wrap items-center justify-center">
                <a
                  className="flex items-center gap-2 hover:underline hover:underline-offset-4"
                  href="https://nextjs.org/learn?utm_source=create-next-app&utm_medium=appdir-template-tw&utm_campaign=create-next-app"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  <Image
                    aria-hidden
                    src="/file.svg"
                    alt="File icon"
                    width={16}
                    height={16}
                  />
                  Learn
                </a>
                <a
                  className="flex items-center gap-2 hover:underline hover:underline-offset-4"
                  href="https://vercel.com/templates?framework=next.js&utm_source=create-next-app&utm_medium=appdir-template-tw&utm_campaign=create-next-app"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  <Image
                    aria-hidden
                    src="/window.svg"
                    alt="Window icon"
                    width={16}
                    height={16}
                  />
                  Examples
                </a>
                <a
                  className="flex items-center gap-2 hover:underline hover:underline-offset-4"
                  href="https://nextjs.org?utm_source=create-next-app&utm_medium=appdir-template-tw&utm_campaign=create-next-app"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  <Image
                    aria-hidden
                    src="/globe.svg"
                    alt="Globe icon"
                    width={16}
                    height={16}
                  />
                  Go to nextjs.org →
                </a>
              </footer>
            </div>
          </BetSwirlSDKProvider>
        </OnchainKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

### Шаг 4: Запуск проекта с casino играми

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