[![Stand With Palestine](https://raw.githubusercontent.com/TheBSD/StandWithPalestine/main/banner-no-action.svg)](https://thebsd.github.io/StandWithPalestine)

# Flutter Riverpod Template - 2025 Edition
### (fork ajustado para rodar em GitHub Codespaces)

## Modern Flutter Architecture Template with Riverpod

A production-ready Flutter template built with the latest packages and best practices. This template implements clean architecture principles and provides a robust foundation for building scalable applications.

> 📝 **Nota deste fork:** o README original tinha dois passos desatualizados (pedia um `.env` que não existe mais no projeto, e usava uma versão do pacote `ionicons` incompatível com Flutter 3.44+). Ambos foram corrigidos abaixo. Veja a seção [Correções aplicadas](#correções-aplicadas-neste-fork) no final para o histórico completo.

---

### Key Features

- 🏗️ Clean Architecture with Domain-Driven Design
- 🎯 Riverpod 2.6+ with code generation
- 🔒 Built-in authentication pack with secure storage (Hive CE + AES-256)
- 🌐 Type-safe API integration with Dio 5.8+
- 📱 Responsive UI with adaptive widgets
- 🌍 Internationalization ready with Easy Localization
- 💾 Secure local storage with Hive CE
- 🧪 Pre-configured unit testing for authentication and controller logic
- ⚡ Modern navigation with GoRouter 14.8+
- 🛠️ Custom linting and devtools configuration

---

## Tech Stack

**Core Libraries:**
- State Management: Riverpod 3.1.0, Freezed 3.x (immutable state)
- Network Layer: Dio 5.9+, FPDart 1.1.0 for functional error handling
- Local Storage: Hive CE 2.11+ with AES-256 encryption
- UI & Navigation: GoRouter 17+, Google Fonts, Material 3
- Icons: **Material Icons nativos do Flutter** (`ionicons` foi removido — ver seção de correções)

**Developer Tools:**
- Flutter Lints 6.0+
- Build Runner, code generation
- Custom linting rules (`lint_rules.yaml`)
- Dart & Flutter DevTools support

---

## Project Structure

```
lib/
├── common/            # Shared widgets and components
├── config/            # App configuration (theme etc.)
├── constants/         # App-wide constants (endpoints, assets)
├── core/              # Core functionality, network layer
├── features/          # Feature modules (authentication, home, ...)
│   └── authentication/
│       ├── data/
│       ├── domain/
│       └── presentation/
├── hive/              # Local storage setup and adapters
├── router/            # Navigation & routing
├── utils/             # Utility functions
├── flavors/           # Flavor config (dev/staging/prod)
├── main.dart          # Entry point (sem flavor)
├── main_dev.dart       # Entry point do flavor "dev"
├── main_staging.dart   # Entry point do flavor "staging"
├── main_prod.dart      # Entry point do flavor "prod"
└── my_app.dart         # App configuration
```

---

## Getting Started (GitHub Codespaces)

Esse fluxo assume que você abriu o repositório dentro de um **GitHub Codespace** (terminal com prompt `/workspaces/...`), que não vem com o Flutter SDK pré-instalado.

### 1. Instalar o Flutter SDK no Codespace

```bash
git clone https://github.com/flutter/flutter.git -b stable --depth 1 ~/flutter
export PATH="$PATH:$HOME/flutter/bin"
echo 'export PATH="$PATH:$HOME/flutter/bin"' >> ~/.bashrc
flutter doctor
```

Os avisos `✗ Android toolchain`, `✗ Chrome`, `✗ Linux toolchain` no `flutter doctor` são esperados e **podem ser ignorados** — o Codespace não tem GUI, então vamos rodar via **Flutter Web (web-server)**.

### 2. Instalar as dependências do projeto

```bash
cd Flutter-Riverpod-Quickstart-Template   # ajuste para o nome da sua pasta
flutter pub get
```

### 3. Gerar o código (Freezed, Riverpod, Hive adapters)

```bash
dart run build_runner build --delete-conflicting-outputs
```

> Pode levar de 1 a 3 minutos na primeira vez. É normal parecer travado.

### 4. ~~Configurar o `.env`~~ (não necessário)

O README original pedia `cp .env.example .env`, mas esse arquivo não existe e o projeto **não depende de `flutter_dotenv`** (confirmado no `pubspec.yaml`). Pule esse passo.

### 5. Rodar o app

```bash
flutter run -d web-server --web-hostname=0.0.0.0 --web-port=8080 -t lib/main_dev.dart
```

Quando aparecer a porta `8080` na aba **PORTS** do VS Code, clique no ícone de globo (**Open in Browser**) para abrir o app.

---

## Flavors (app names & package IDs)

Esse template vem com os flavors `dev`, `staging` e `prod`.

### Run commands (via web-server, dentro do Codespace)

- **Dev:**
    ```bash
    flutter run -d web-server --web-hostname=0.0.0.0 --web-port=8080 -t lib/main_dev.dart
    ```
- **Staging:**
    ```bash
    flutter run -d web-server --web-hostname=0.0.0.0 --web-port=8080 -t lib/main_staging.dart
    ```
- **Prod:**
    ```bash
    flutter run -d web-server --web-hostname=0.0.0.0 --web-port=8080 -t lib/main_prod.dart
    ```

> Em uma máquina local com Android/iOS/Chrome instalados, use `--flavor dev` (etc.) junto com `-t lib/main_dev.dart` como no README original.

### Customize names and IDs

- Android flavor names and applicationId suffixes: `android/app/build.gradle.kts`
- iOS bundle IDs and display names: `ios/Flutter/Debug-dev.xcconfig` (e os outros arquivos `.xcconfig` de cada flavor)
- Dart-side flavor labels: `lib/flavors/app_flavor.dart`

---

## Authentication Module

O template inclui um sistema de autenticação completo com armazenamento seguro de credenciais.

### How it works

- **Login & Registration:** usa Dio para fazer POST em `/api/login` e `/api/register`.
- **Credential Caching:** credenciais são armazenadas de forma criptografada no Hive CE (AES-256), com chave derivada por dispositivo.
- **State Management:** toda a UI e lógica de autenticação é gerenciada via notifiers e classes de estado do Riverpod.
- **Error Handling:** erros de rede e validação aparecem na UI.
- **Loading State:** a UI reflete os estados de carregamento e erro.

### Test Credentials (API mock reqres.in)

**Login:**
```json
{
    "email": "eve.holt@reqres.in",
    "password": "cityslicka"
}
```

**Register:**
```json
{
    "email": "eve.holt@reqres.in",
    "password": "pistol"
}
```

---

## Testing

```bash
flutter test
```

---

## Correções aplicadas neste fork

| # | Problema | Causa | Correção |
|---|----------|-------|----------|
| 1 | `cp .env.example .env` falhava com "No such file or directory" | O `pubspec.yaml` não tem `flutter_dotenv` como dependência — o README ficou desatualizado em relação ao código atual do template | Passo removido do fluxo de setup |
| 2 | `flutter: command not found` | GitHub Codespaces não vem com o Flutter SDK pré-instalado | Instalação manual do SDK via `git clone` do repositório oficial do Flutter |
| 3 | `Error: The class 'IconData' can't be extended outside of its library because it's a final class` ao compilar | O pacote `ionicons: ^0.2.2` estende `IconData`, e o Flutter 3.44 tornou essa classe `final` (breaking change oficial) | Dependência `ionicons` removida do `pubspec.yaml`; os 4 usos no código (`home_screen.dart`, `theme_widget.dart`, `social_tile_widget.dart`, `link_card.dart`) foram trocados por ícones nativos `Icons.*` do Material |
| 4 | Tela branca ao abrir a URL do `web-server` | Nenhum problema real — o Codespace só precisa que a porta seja "forwarded" e aberta via aba **PORTS** | Documentado no fluxo de setup acima |

### Mapa de ícones trocados (`ionicons` → `Icons.*`)

| Antes (`Ionicons`) | Depois (`Icons`) |
|---|---|
| `logo_github` | `code` |
| `logo_linkedin` | `business` |
| `logo_medium` | `article` |
| `globe_outline` | `language` |
| `sunny_outline` | `light_mode` |
| `phone_portrait_outline` | `smartphone` |
| `moon_outline` | `dark_mode` |
| `arrow_forward` | `arrow_forward` |
| `open_outline` | `open_in_new` |

---

## Documentation

- Documentação atualizada neste README
- Comentários e exemplos ao longo do código

---

## Contributing

Veja o [Contributing Guide](CONTRIBUTING.md) original do template para detalhes sobre como configurar o ambiente de desenvolvimento, rodar testes e enviar pull requests.

---

## License

Este projeto é licenciado sob a MIT License — veja o arquivo [LICENSE](LICENSE) para detalhes.

---

## Contact (template original)

- [https://www.erengun.dev](https://www.erengun.dev)