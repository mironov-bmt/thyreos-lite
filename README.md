<div align="center">
  <img src="assets/banner.svg" alt="Thyreos Lite Banner" width="800">
  <br><br>

  ![License](https://img.shields.io/badge/License-MIT-4FC3F7?style=flat-square&logo=opensourceinitiative&logoColor=white)
  ![Crypto](https://img.shields.io/badge/Crypto-PBKDF2%20%7C%20AES--256--GCM-69F0AE?style=flat-square&logo=datadog&logoColor=white)
  ![Browser](https://img.shields.io/badge/Browser-Client--Side%20Only-FFD740?style=flat-square&logo=googlechrome&logoColor=white)
  ![Status](https://img.shields.io/badge/Status-Educational%20%F0%9F%93%9A-FF5370?style=flat-square)

  <h3>🛡️ Локальное шифрование файлов прямо в браузере</h3>
  <p><em>Все криптографические операции выполняются локально. Файлы не покидают устройство.</em></p>
</div>

---

> ⚠️ **ОБРАЗОВАТЕЛЬНЫЙ ДИСКЛЕЙМЕР**
>
> **Данный проект создан исключительно в образовательных и демонстрационных целях.** Это экспериментальный инструмент для демонстрации реализации криптографических примитивов (PBKDF2, AES-256-GCM) в браузере с использованием Web Crypto API.
>
> 🚫 **НЕ ДЛЯ ПРОИЗВОДСТВЕННОГО ИСПОЛЬЗОВАНИЯ.** Автор **не несёт ответственности** за любой ущерб, потерю данных или нарушения безопасности, возникшие в результате использования данного ПО. Пользователь берёт на себя **полную ответственность** за все действия, совершённые с помощью этого инструмента.
>
> 🛡️ **Без гарантий:** Данное ПО предоставляется "как есть", без каких-либо гарантий, явных или подразумеваемых. Если вам необходимо защитить конфиденциальные данные, используйте проверенные инструменты промышленного уровня, такие как GnuPG, OpenSSL или VeraCrypt.

---

## 📋 Содержание

- [Обзор](#обзор)
- [Как это работает](#как-это-работает)
- [Возможности](#возможности)
- [Быстрый старт](#быстрый-старт)
- [Инструкция по использованию](#инструкция-по-использованию)
- [Технические спецификации](#технические-спецификации)
- [Квантовая стойкость](#квантовая-стойкость)
- [Научные источники](#научные-источники)
- [Предупреждение безопасности](#предупреждение-безопасности)
- [English Version](#english-version)

---

## Обзор

**Thyreos-lite** — это легковесный инструмент для шифрования файлов на стороне клиента, использующий парольное шифрование через PBKDF2 и AES-256-GCM. Все операции выполняются локально в браузере, файлы никуда не отправляются.

Проект служит живой демонстрацией для студентов, разработчиков и энтузиастов безопасности, которые хотят понять:

- Как PBKDF2 превращает пароль в криптографический ключ
- Как AES-256-GCM обеспечивает аутентифицированное шифрование
- Как Web Crypto API позволяет выполнять безопасные операции в браузере
- Как AAD (Additional Authenticated Data) защищает метаданные

## Как это работает

```
┌─────────────────────────────────────────────────────────────────────┐
│                    СХЕМА ШИФРОВАНИЯ                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Пароль (пользователь)                                             │
│        │                                                             │
│        ▼                                                             │
│   ┌──────────┐           ┌──────────┐                                │
│   │ PBKDF2   │──────────▶│  Ключ    │                                │
│   │SHA-256  │  Соль     │ 256 бит  │                                │
│   │600k iter│ (16 байт) │          │                                │
│   └──────────┘           └──────────┘                                │
│                              │                                       │
│                              ▼                                       │
│   ┌──────────┐           ┌──────────────┐                           │
│   │ Заголовок│           │ AES-256-GCM  │                           │
│   │ .thy     │──────────▶│  Encrypt     │                           │
│   │ (AAD)    │           │  File Data   │                           │
│   └──────────┘           └──────────────┘                           │
│                              │                                       │
│                              ▼                                       │
│                    ┌─────────────────┐                                │
│                    │  Пакет .thy     │                                │
│                    │ [MAGIC][SALT]   │                                │
│                    │ [IV][NAME][CT]  │                                │
│                    └─────────────────┘                                │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Криптографическая архитектура

| Уровень | Алгоритм | Назначение | Стандарт |
|---------|----------|------------|----------|
| **Ключевое преобразование** | PBKDF2 (SHA-256, 600 000 итераций) | Превращение пароля в 256-битный ключ | PKCS #5 v2.1 (RFC 8018) |
| **Шифрование данных** | AES-256-GCM | Шифрование содержимого файла | NIST SP 800-38D |
| **Аутентификация** | GCM Auth Tag | Целостность и аутентичность | NIST SP 800-38D |
| **AAD** | Весь заголовок .thy | Защита от подмены любого метаданных | NIST SP 800-38D |

## Возможности

- 🔒 **Настоящее клиентское шифрование** — Все операции через Web Crypto API; данные никуда не передаются
- 🧪 **Образовательный дизайн** — Чистая структура кода с подробным логированием для обучения
- 🔑 **Парольное шифрование** — Простая модель: один пароль = один ключ, никаких файлов ключей
- 🛡️ **Аутентифицированное шифрование** — AES-256-GCM с AAD (весь заголовок .thy) защищает от подмены метаданных
- 📦 **Автономный пакет** — Зашифрованные файлы `.thy` содержат всё необходимое для дешифрования
- 🎨 **Современный интерфейс** — Тёмная тема, адаптивный дизайн, индикаторы прогресса
- ⚡ **Квантовая стойкость AES-256** — AES-256 устойчив к квантовым атакам (см. раздел [Квантовая стойкость](#квантовая-стойкость))

## Быстрый старт

### Вариант 1: Открыть напрямую

1. Клонируйте или скачайте репозиторий
2. Откройте `index.html` в любом современном браузере (Chrome, Firefox, Edge, Safari)
3. Никакой сборки, никакого сервера, никаких зависимостей

```bash
git clone https://github.com/mironov-bmt/thyreos-lite.git
cd thyreos-lite
# Откройте index.html в браузере
```

### Вариант 2: Статический хостинг

Разверните на GitHub Pages, Netlify, Vercel или любом статическом хостинге:

```bash
# Пример GitHub Pages
git push origin main
# Затем включите Pages в настройках репозитория
```

## Инструкция по использованию

### 🔐 Шифрование файла

1. **Перейдите на вкладку "Шифровать"**
2. **Введите пароль** — Минимум 8 символов. Рекомендуется 12+. Повторите для подтверждения
3. **Выберите файл** — Перетащите или нажмите для выбора (лимит: 100 MB)
4. **Нажмите "Зашифровать файл"** — Файл `.thy` скачается автоматически

> ⚠️ **Важно:** Пароль не восстанавливается. Забудешь — файл не открыть никогда.

### 🔓 Дешифрование файла

1. **Перейдите на вкладку "Дешифровать"**
2. **Выберите файл `.thy`** — Зашифрованный пакет
3. **Введите пароль** — Тот же пароль, что использовался при шифровании
4. **Нажмите "Дешифровать файл"** — Оригинальный файл восстановится и скачается

> Если пароль неверный или файл повреждён — дешифрование прервётся с ошибкой. AES-GCM auth-tag защищает от подмены.

## Технические спецификации

### Формат файла (`.thy`)

| Поле | Размер | Описание |
|------|--------|----------|
| `MAGIC` | 4 байта | `0x54 0x48 0x59 0x03` — Сигнатура файла (v3) |
| `salt_len` | 2 байта | Длина соли |
| `salt` | 16 байт | Случайная соль для PBKDF2 |
| `iv` | 12 байт | Нонс AES-GCM |
| `name_len` | 2 байта | Длина оригинального имени файла |
| `filename` | Переменный | Оригинальное имя файла в UTF-8 |
| `aes_ciphertext` | Переменный | Данные файла, зашифрованные AES-256-GCM (включает auth-tag 16 байт) |

### Параметры PBKDF2

| Параметр | Значение |
|----------|----------|
| Алгоритм | PBKDF2 |
| Хеш-функция | SHA-256 |
| Итерации | 600 000 |
| Длина ключа | 256 бит (32 байта) |
| Длина соли | 128 бит (16 байт) |

### Параметры AES-GCM

| Параметр | Значение |
|----------|----------|
| Алгоритм | AES-256-GCM |
| Длина ключа | 256 бит |
| Длина IV | 96 бит (12 байт) |
| Длина auth-tag | 128 бит (16 байт) |
| AAD | Весь заголовок .thy (MAGIC + salt_len + salt + iv + name_len + filename) |

### Поддерживаемые браузеры

| Браузер | Минимальная версия | Примечания |
|---------|-------------------|------------|
| Chrome | 60+ | Полная поддержка |
| Firefox | 55+ | Полная поддержка |
| Edge | 79+ | Полная поддержка |
| Safari | 14+ | Полная поддержка |
| Opera | 47+ | Полная поддержка |

### Ограничения

- Максимальный размер файла: **100 MB**
- Минимальная длина пароля: **8 символов** (рекомендуется 12+)
- Пароль не хранится нигде — восстановление невозможно

## Квантовая стойкость

### AES-256 и алгоритм Гровера

AES-256 устойчив к квантовым атакам благодаря структуре симметричного шифрования. В отличие от асимметричных алгоритмов (RSA, ECC), которые уязвимы к алгоритму Шора и могут быть взломаны за полиномиальное время на квантовом компьютере, симметричное шифрование подвержено только алгоритму Гровера.

Алгоритм Гровера обеспечивает лишь **квадратичное ускорение** при переборе ключей: эффективная сложность падает с 2^256 до 2^128 операций. Это остаётся вычислительно неосуществимым. NIST прямо заявляет: *«весьма вероятно, что алгоритм Гровера даст мало или не даст вообще преимущества при атаке на AES, и AES-128 останется безопасным на десятилетия вперёд»*. AES-256 при этом сохраняет ~128 бит квантовой безопасности — запас, который превышает текущие рекомендации NIST.

### PBKDF2 в квантовую эпоху

PBKDF2 как KDF (Key Derivation Function) не подвержен прямым квантовым атакам, поскольку его безопасность опирается на сложность пароля, а не на математическую задачу. Квантовый компьютер может ускорить перебор пароля через Гровера, но это линейное ускорение (в √N раз), которое компенсируется увеличением длины пароля. 600 000 итераций PBKDF2 делают каждую попытку дорогой даже для квантового компьютера.

> **Итог:** AES-256 и PBKDF2 с достаточно сложным паролем остаются безопасными в квантовую эпоху без необходимости замены алгоритмов. Угроза касается прежде всего асимметричной криптографии (RSA, ECC, ECDSA), которую NIST уже заменяет на постквантовые стандарты (ML-KEM, ML-DSA).

## Научные источники

### Основные стандарты

1. **NIST SP 800-38D** — *Recommendation for Block Cipher Modes of Operation: Galois/Counter Mode (GCM) and GMAC* (2007)  
   [https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-38d.pdf](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-38d.pdf)

2. **NIST SP 800-132** — *Recommendation for Password-Based Key Derivation* (2010)  
   [https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-132.pdf](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-132.pdf)

3. **RFC 8018** — *PKCS #5: Password-Based Cryptography Specification Version 2.1* (2017)  
   [https://datatracker.ietf.org/doc/html/rfc8018](https://datatracker.ietf.org/doc/html/rfc8018)

4. **W3C Web Cryptography API** — *Recommendation* (2017)  
   [https://www.w3.org/TR/WebCryptoAPI/](https://www.w3.org/TR/WebCryptoAPI/)

### Квантовая стойкость

5. **NIST Post-Quantum Cryptography FAQs** — *Official NIST guidance on AES and Grover's algorithm* (обновлено 16.06.2026)  
   [https://csrc.nist.gov/projects/post-quantum-cryptography/faqs](https://csrc.nist.gov/projects/post-quantum-cryptography/faqs)

6. **NIST Research Paper** — *On the practical cost of Grover for AES key recovery* (2024)  
   [https://csrc.nist.gov/csrc/media/Events/2024/fifth-pqc-standardization-conference/documents/papers/on-practical-cost-of-grover.pdf](https://csrc.nist.gov/csrc/media/Events/2024/fifth-pqc-standardization-conference/documents/papers/on-practical-cost-of-grover.pdf)

7. **arXiv** — *Quantum-enabled framework for the Advanced Encryption Standard in the post-quantum era* (2025)  
   [https://arxiv.org/html/2502.02445v1](https://arxiv.org/html/2502.02445v1)

### Дополнительные источники

8. **NIST FIPS 180-4** — *Secure Hash Standard (SHS)* — Спецификация SHA-256  
   [https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)

9. **NIST FIPS 197** — *Advanced Encryption Standard (AES)* — Спецификация AES  
   [https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf)

### Образовательные ресурсы

10. **Boneh, D. & Shoup, V.** — *A Graduate Course in Applied Cryptography* (Бесплатный онлайн-учебник)  
    [https://toc.cryptobook.us/](https://toc.cryptobook.us/)

11. **MDN Web Docs** — *Документация Web Crypto API*  
    [https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

## Предупреждение безопасности

### Что демонстрирует этот инструмент

- ✅ Парольное ключевое преобразование (PBKDF2)
- ✅ Аутентифицированное шифрование с привязанными данными (AEAD через AES-GCM)
- ✅ Защита целостности метаданных через AAD (весь заголовок .thy)
- ✅ Клиентское шифрование с помощью Web Crypto API
- ✅ Обработка файлов с нулевым доверием (нет загрузки на сервер)

### Что этот инструмент НЕ гарантирует

- ❌ Защиту от скомпрометированных браузеров или вредоносного ПО
- ❌ Устойчивость к атакам по сторонним каналам (timing, cache и т.д.)
- ❌ Защиту от слабых паролей (используйте парольные менеджеры)
- ❌ Формальный аудит безопасности или сертификацию
- ❌ Соответствие нормативным требованиям (HIPAA, GDPR и т.д.)

---

<div align="center">
  <h1>🇬🇧 English Version</h1>
</div>

<div align="center">

  ![License](https://img.shields.io/badge/License-MIT-4FC3F7?style=flat-square&logo=opensourceinitiative&logoColor=white)
  ![Crypto](https://img.shields.io/badge/Crypto-PBKDF2%20%7C%20AES--256--GCM-69F0AE?style=flat-square&logo=datadog&logoColor=white)
  ![Browser](https://img.shields.io/badge/Browser-Client--Side%20Only-FFD740?style=flat-square&logo=googlechrome&logoColor=white)
  ![Status](https://img.shields.io/badge/Status-Educational%20%F0%9F%93%9A-FF5370?style=flat-square)

  <h3>🛡️ Browser-Based File Encryption for Educational Purposes</h3>
  <p><em>All cryptographic operations run locally in your browser. No files ever leave your device.</em></p>
</div>

---

> ⚠️ **EDUCATIONAL DISCLAIMER**
>
> **This project is created strictly for educational and demonstration purposes.** It is an experimental tool designed to showcase how modern cryptographic primitives (PBKDF2, AES-256-GCM) can be implemented in a browser environment using the Web Crypto API.
>
> 🚫 **NOT FOR PRODUCTION USE.** The author assumes **no liability** for any damage, data loss, or security breaches resulting from the use of this software. Users assume **full responsibility** for all actions taken with this tool.
>
> 🛡️ **No Warranty:** This software is provided "as is", without warranty of any kind, express or implied. If you need to protect sensitive data, please use audited, production-grade tools such as GnuPG, OpenSSL, or VeraCrypt.

---

## 📋 Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Features](#features)
- [Quick Start](#quick-start)
- [Usage Guide](#usage-guide)
- [Technical Specifications](#technical-specifications)
- [Quantum Resistance](#quantum-resistance)
- [Academic References](#academic-references)
- [Security Notice](#security-notice)

---

## Overview

**Thyreos-lite** is a lightweight, client-side file encryption tool that uses password-based encryption via PBKDF2 and AES-256-GCM. All operations run locally in the browser — no files ever leave your device.

This project serves as a **living demonstration** for students, developers, and security enthusiasts who want to understand:

- How PBKDF2 transforms a password into a cryptographic key
- How AES-256-GCM provides authenticated encryption
- How the Web Crypto API enables secure operations in modern browsers
- How AAD (Additional Authenticated Data) protects metadata

## How It Works

```
┌─────────────────────────────────────────────────────────────────────┐
│                         ENCRYPTION FLOW                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   User Password                                                      │
│        │                                                             │
│        ▼                                                             │
│   ┌──────────┐            ┌──────────┐                             │
│   │ PBKDF2   │───────────▶│   Key    │                             │
│   │SHA-256  │   Salt     │ 256-bit  │                             │
│   │600k iter│  (16 B)    │          │                             │
│   └──────────┘            └──────────┘                             │
│                              │                                       │
│                              ▼                                       │
│   ┌──────────┐           ┌──────────────┐                           │
│   │ .thy     │           │ AES-256-GCM  │                           │
│   │ Header   │──────────▶│   Encrypt    │                           │
│   │ (AAD)    │           │  File Data   │                           │
│   └──────────┘           └──────────────┘                           │
│                              │                                       │
│                              ▼                                       │
│                    ┌─────────────────┐                                │
│                    │   .thy Package    │                                │
│                    │ [MAGIC][SALT]     │                                │
│                    │ [IV][NAME][CT]    │                                │
│                    └─────────────────┘                                │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Cryptographic Architecture

| Layer | Algorithm | Purpose | Standard |
|-------|-----------|---------|----------|
| **Key Derivation** | PBKDF2 (SHA-256, 600,000 iterations) | Transform password into 256-bit key | PKCS #5 v2.1 (RFC 8018) |
| **Data Encryption** | AES-256-GCM | Encrypt file contents | NIST SP 800-38D |
| **Authentication** | GCM Auth Tag | Integrity and authenticity | NIST SP 800-38D |
| **AAD** | Entire .thy header | Protect against metadata tampering | NIST SP 800-38D |

## Features

- 🔒 **True Client-Side Encryption** — All operations use the Web Crypto API; no data is transmitted
- 🧪 **Educational Design** — Clean code structure with detailed logging for learning
- 🔑 **Password-Based Encryption** — Simple model: one password = one key, no key files
- 🛡️ **Authenticated Encryption** — AES-256-GCM with AAD (entire .thy header) protects against tampering
- 📦 **Self-Contained Package** — Encrypted `.thy` files include all metadata needed for decryption
- 🎨 **Modern UI** — Dark-themed, responsive interface with real-time progress indicators
- ⚡ **Quantum-Resistant AES-256** — AES-256 remains secure against quantum attacks (see [Quantum Resistance](#quantum-resistance))

## Quick Start

### Option 1: Open Directly

1. Clone or download this repository
2. Open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari)
3. No build step, no server, no dependencies

```bash
git clone https://github.com/mironov-bmt/thyreos-lite.git
cd thyreos-lite
# Open index.html in your browser
```

### Option 2: Static Hosting

Deploy to GitHub Pages, Netlify, Vercel, or any static host:

```bash
# GitHub Pages example
git push origin main
# Then enable Pages in repository settings
```

## Usage Guide

### 🔐 Encrypting a File

1. **Switch to the "Encrypt" tab**
2. **Enter a password** — Minimum 8 characters. 12+ recommended. Confirm it
3. **Select a file** — Drag & drop or click to choose (limit: 100 MB)
4. **Click "Encrypt File"** — The `.thy` file will download automatically

> ⚠️ **Important:** The password cannot be recovered. If you forget it, the file will be impossible to open.

### 🔓 Decrypting a File

1. **Switch to the "Decrypt" tab**
2. **Select the `.thy` file** — The encrypted package
3. **Enter the password** — The same password used during encryption
4. **Click "Decrypt File"** — The original file is restored and downloaded

> If the password is wrong or the file is corrupted — decryption will abort with an error. The AES-GCM auth-tag protects against tampering.

## Technical Specifications

### File Format (`.thy`)

| Field | Size | Description |
|-------|------|-------------|
| `MAGIC` | 4 bytes | `0x54 0x48 0x59 0x03` — File signature (v3) |
| `salt_len` | 2 bytes | Salt length |
| `salt` | 16 bytes | Random salt for PBKDF2 |
| `iv` | 12 bytes | AES-GCM nonce |
| `name_len` | 2 bytes | Original filename length |
| `filename` | Variable | Original filename in UTF-8 |
| `aes_ciphertext` | Variable | File data encrypted with AES-256-GCM (includes 16-byte auth-tag) |

### PBKDF2 Parameters

| Parameter | Value |
|-----------|-------|
| Algorithm | PBKDF2 |
| Hash Function | SHA-256 |
| Iterations | 600,000 |
| Key Length | 256 bits (32 bytes) |
| Salt Length | 128 bits (16 bytes) |

### AES-GCM Parameters

| Parameter | Value |
|-----------|-------|
| Algorithm | AES-256-GCM |
| Key Length | 256 bits |
| IV Length | 96 bits (12 bytes) |
| Auth-Tag Length | 128 bits (16 bytes) |
| AAD | Entire .thy header (MAGIC + salt_len + salt + iv + name_len + filename) |

### Supported Browsers

| Browser | Minimum Version | Notes |
|---------|-----------------|-------|
| Chrome | 60+ | Full support |
| Firefox | 55+ | Full support |
| Edge | 79+ | Full support |
| Safari | 14+ | Full support |
| Opera | 47+ | Full support |

### Limitations

- Maximum file size: **100 MB**
- Minimum password length: **8 characters** (12+ recommended)
- Password is never stored anywhere — recovery is impossible

## Quantum Resistance

### AES-256 and Grover's Algorithm

AES-256 is resistant to quantum attacks due to the nature of symmetric encryption. Unlike asymmetric algorithms (RSA, ECC), which are vulnerable to Shor's algorithm and can be broken in polynomial time on a quantum computer, symmetric encryption is only affected by Grover's algorithm.

Grover's algorithm provides only a **quadratic speedup** for key search: effective complexity drops from 2^256 to 2^128 operations. This remains computationally infeasible. NIST explicitly states: *"it is quite likely that Grover's algorithm will provide little or no advantage in attacking AES, and AES-128 will remain secure for decades to come."* AES-256 maintains ~128 bits of quantum security — a margin that exceeds current NIST recommendations.

### PBKDF2 in the Quantum Era

PBKDF2 as a KDF (Key Derivation Function) is not directly vulnerable to quantum attacks, as its security relies on password complexity rather than a mathematical problem. A quantum computer could speed up password guessing via Grover, but this is a linear speedup (√N), which is mitigated by increasing password length. 600,000 PBKDF2 iterations make each attempt expensive even for a quantum computer.

> **Conclusion:** AES-256 and PBKDF2 with a sufficiently strong password remain secure in the quantum era without requiring algorithm replacement. The threat primarily concerns asymmetric cryptography (RSA, ECC, ECDSA), which NIST is already replacing with post-quantum standards (ML-KEM, ML-DSA).

## Academic References

### Primary Standards

1. **NIST SP 800-38D** — *Recommendation for Block Cipher Modes of Operation: Galois/Counter Mode (GCM) and GMAC* (2007)  
   [https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-38d.pdf](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-38d.pdf)

2. **NIST SP 800-132** — *Recommendation for Password-Based Key Derivation* (2010)  
   [https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-132.pdf](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-132.pdf)

3. **RFC 8018** — *PKCS #5: Password-Based Cryptography Specification Version 2.1* (2017)  
   [https://datatracker.ietf.org/doc/html/rfc8018](https://datatracker.ietf.org/doc/html/rfc8018)

4. **W3C Web Cryptography API** — *Recommendation* (2017)  
   [https://www.w3.org/TR/WebCryptoAPI/](https://www.w3.org/TR/WebCryptoAPI/)

### Quantum Resistance

5. **NIST Post-Quantum Cryptography FAQs** — *Official NIST guidance on AES and Grover's algorithm* (updated 2026-06-16)  
   [https://csrc.nist.gov/projects/post-quantum-cryptography/faqs](https://csrc.nist.gov/projects/post-quantum-cryptography/faqs)

6. **NIST Research Paper** — *On the practical cost of Grover for AES key recovery* (2024)  
   [https://csrc.nist.gov/csrc/media/Events/2024/fifth-pqc-standardization-conference/documents/papers/on-practical-cost-of-grover.pdf](https://csrc.nist.gov/csrc/media/Events/2024/fifth-pqc-standardization-conference/documents/papers/on-practical-cost-of-grover.pdf)

7. **arXiv** — *Quantum-enabled framework for the Advanced Encryption Standard in the post-quantum era* (2025)  
   [https://arxiv.org/html/2502.02445v1](https://arxiv.org/html/2502.02445v1)

### Supporting References

8. **NIST FIPS 180-4** — *Secure Hash Standard (SHS)* — SHA-256 specification  
   [https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)

9. **NIST FIPS 197** — *Advanced Encryption Standard (AES)* — AES specification  
   [https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf)

### Educational Resources

10. **Boneh, D. & Shoup, V.** — *A Graduate Course in Applied Cryptography* (Free online textbook)  
    [https://toc.cryptobook.us/](https://toc.cryptobook.us/)

11. **MDN Web Docs** — *Web Crypto API documentation*  
    [https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)

## Security Notice

### What This Tool Demonstrates

- ✅ Password-based key derivation (PBKDF2)
- ✅ Authenticated encryption with associated data (AEAD via AES-GCM)
- ✅ Metadata integrity protection via AAD (entire .thy header)
- ✅ Client-side encryption using Web Crypto API
- ✅ Zero-trust file handling (no server upload)

### What This Tool Does NOT Guarantee

- ❌ Protection against compromised browsers or malware
- ❌ Side-channel attack resistance (timing, cache, etc.)
- ❌ Protection against weak passwords (use password managers)
- ❌ Formal security audit or certification
- ❌ Compliance with regulatory requirements (HIPAA, GDPR, etc.)

---

<div align="center">
  <br>
  <img src="assets/banner.svg" width="32" alt="Shield">
  <br><br>
  <b>THYREOS-LITE</b><br>
  <sub>PBKDF2 + AES-256-GCM · MIT License · 2026</sub>
  <br><br>
  <sub>🛡️ Created with security education in mind. Use responsibly.</sub>
</div>
