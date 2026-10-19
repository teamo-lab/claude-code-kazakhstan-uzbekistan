# Claude Code и Codex в Казахстане, Узбекистане, Грузии, Армении и Кыргызстане: подключение и оплата в USDT

> Репозиторий ведёт команда TeamoRouter. Шлюз, который используется в инструкции, — наш.

## Короткий ответ

Claude Code и Codex ставятся из npm и умеют работать через сторонний API-шлюз: для Claude Code задаются адрес шлюза и ключ в переменных окружения, для Codex — файл `~/.codex/config.toml`. Входить в аккаунт Anthropic или ChatGPT не нужно. Один ключ [TeamoRouter](https://teamorouter.com/ru?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan) работает с Claude, GPT-6, Gemini, DeepSeek и другими моделями, оплата — только за токены, без подписки. Баланс пополняется в USDT (сеть X Layer, от $5), а если у вас остался российский банк — в рублях.

## Кому это нужно

Anthropic и OpenAI не принимают карты российских банков. Если после переезда у вас осталась только российская карта, оплатить подписку Claude или ChatGPT или их API напрямую не получится. Если у вас есть карта, которую эти сервисы принимают, можно пользоваться и официальным путём; эта инструкция — для остальных случаев.

## Что понадобится

- **Node.js 22 или новее** ([nodejs.org](https://nodejs.org/en/download/)); на Windows для Claude Code ещё [Git for Windows](https://git-scm.com/download/win).
- **Аккаунт TeamoRouter** и API-ключ из [консоли](https://teamorouter.com/dashboard?tab=api-keys&utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan).

## Claude Code

```bash
npm install -g @anthropic-ai/claude-code
claude --version
```

**macOS и Linux** (скрипт сам выберет `~/.zshrc` или `~/.bashrc`):

```bash
SHELL_RC="$HOME/.zshrc"; case "$SHELL" in *bash*) SHELL_RC="$HOME/.bashrc";; esac
cat >> "$SHELL_RC" <<'EOF'
export ANTHROPIC_BASE_URL=https://api.teamorouter.com
export ANTHROPIC_AUTH_TOKEN="ваш ключ TeamoRouter"
export ANTHROPIC_MODEL=claude-opus-5-5
export ANTHROPIC_DEFAULT_FABLE_MODEL=claude-fable-5-1
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-5-5
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-5
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5
export CLAUDE_CODE_SUBAGENT_MODEL=claude-sonnet-5
export CLAUDE_CODE_EFFORT_LEVEL=xhigh
EOF
source "$SHELL_RC"
```

**Windows (PowerShell)** — те же переменные через `setx`, после этого откройте терминал заново:

```powershell
setx ANTHROPIC_BASE_URL "https://api.teamorouter.com"
setx ANTHROPIC_AUTH_TOKEN "ваш ключ TeamoRouter"
setx ANTHROPIC_MODEL "claude-opus-5-5"
```

Остальные строки для Windows и скриншоты — в [руководстве по Claude Code](https://teamorouter.com/ru/docs/install-claude-code?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan). Запуск: `claude` в папке проекта.

- `ANTHROPIC_BASE_URL` — **без** `/v1` на конце.
- Видите `Not logged in · Please run /login`? Не входите в аккаунт: в этом терминале не задан `ANTHROPIC_AUTH_TOKEN`. Выполните `source ~/.zshrc` или откройте новое окно после `setx`.

## Codex

```bash
npm install -g @openai/codex
```

**macOS и Linux:**

```bash
mkdir -p ~/.codex && cat > ~/.codex/config.toml <<'EOF'
model_provider = "teamorouter"
model = "gpt-6-sol"
model_reasoning_effort = "high"
[model_providers.teamorouter]
name = "TeamoRouter"
base_url = "https://api.teamorouter.com/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
EOF
echo 'export OPENAI_API_KEY="ваш ключ TeamoRouter"' >> ~/.zshrc   # на Linux — ~/.bashrc
source ~/.zshrc
```

**Windows (PowerShell):** тот же `config.toml` кладётся в `%USERPROFILE%\.codex\`, ключ задаётся командой `[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "ваш ключ TeamoRouter", "User")`. Готовые команды — в [руководстве по Codex CLI](https://teamorouter.com/ru/docs/install-codex?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan).

- `base_url` — **с** `/v1` на конце (у Claude Code наоборот).
- Модели: `gpt-6-sol` по умолчанию, `gpt-6-luna` для простых задач (в разы дешевле), `gpt-6-astra` для самых сложных (самая дорогая). Переключение в сессии — `/model`, быстрый режим — `/fast` (дороже).

## Оплата в USDT

Нужны браузер на компьютере с расширением [OKX Wallet](https://chromewebstore.google.com/detail/okx-wallet/mcohilncbfahbmgdjkbpemcciiolgcge) и USDT в сети **X Layer** в этом кошельке (в блокчейне отображается как USD₮0, 1:1 к USDT). Баланс на бирже не считается, пока вы не выведете его в кошелёк.

**Как перевести USDT в X Layer:**

| Где USDT | Что делать |
|---|---|
| OKX | «Активы → Вывод» → USDT → сеть **X Layer** → адрес вашего OKX Wallet |
| Binance | Вывод в сети **Arbitrum** на адрес OKX Wallet (это USDT0), затем перевод в X Layer на [usdt0.to](https://usdt0.to); нужно немного ETH в Arbitrum на газ |
| Другая биржа | Сначала на аккаунт OKX по общей сети (например, TRC20), затем вывод в X Layer |
| Нет аккаунта OKX или OKX недоступна в вашей стране | Раздел Transfer на [usdt0.to](https://usdt0.to): исходная сеть — где лежат USDT, целевая — X Layer; газ платится в исходной сети |

Сначала отправьте небольшую сумму: самая частая ошибка — неверная сеть.

**Пополнение:**

1. Откройте окно пополнения в консоли, выберите сумму (минимум $5) и нажмите **«Оплатить криптовалютой»**.
2. Подтвердите подключение кошелька и переключение на X Layer (в первый раз — добавление сети).
3. Подпишите запрос. Это разрешение EIP-3009 только на эту сумму, одноразовое и с ограниченным сроком; транзакцию в сеть отправляем мы, поэтому OKB на газ не нужен. Сумма может показываться в единицах с 6 знаками: `5000000` — это 5 USDT.

Ошибки и подробности — в статье [«Как оплатить Claude API в USDT»](https://teamorouter.com/ru/blogs/pay-claude-with-usdt?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan).

**В рублях.** Если у вас остался российский банк или кошелёк ЮMoney, в [русской версии сайта](https://teamorouter.com/ru?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan) в окне пополнения есть СБП, Т-Банк, ЮMoney и SberPay (платёж проводит Payssion, минимум $5).

## Частые вопросы

**Нужен ли VPN?** Нет: Claude Code и Codex обращаются к `api.teamorouter.com`, а не к серверам Anthropic и OpenAI, вход в их аккаунты не нужен.

**Это те же модели Claude?** Да, TeamoRouter их не подменяет; идентификаторы те же, что в документации Anthropic.

**Один ключ на Claude Code и Codex?** Да, ключ TeamoRouter один на все модели.

**Есть ли лимиты, как в подписке?** Нет: лимиты, которые обнуляются раз в пять часов или неделю, относятся к подпискам. По ключу вы платите за токены, пока есть баланс.

**Сколько стоит?** Цены по каждой модели — на [странице цен](https://teamorouter.com/ru?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan#pricing).

**Можно оплатить с телефона?** Для USDT нужен браузер на компьютере с OKX Wallet: в мобильных браузерах расширения кошелька обычно нет.

**Куда писать по оплате?** Telegram [@teamorouter](https://t.me/teamorouter).

## Ссылки

- [Claude Code в России: установка, ошибки, оплата](https://teamorouter.com/ru/blogs/claude-code-in-russia?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan)
- [Codex в России: config.toml и частые ошибки](https://teamorouter.com/ru/blogs/codex-in-russia?utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan)
- [Создать API-ключ](https://teamorouter.com/dashboard?tab=api-keys&utm_source=github&utm_medium=readme&utm_campaign=claude-code-kazakhstan-uzbekistan)
