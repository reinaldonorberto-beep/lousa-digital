# Lousa Digital — UNEMAT Cáceres
## Como gerar o APK

### OPÇÃO 1 — Android Studio (mais fácil) ✅

1. Instale o **Android Studio**: https://developer.android.com/studio
2. Abra o Android Studio → "Open an existing project"
3. Selecione a pasta `lousa-apk`
4. Aguarde o Gradle sincronizar (baixa dependências automaticamente)
5. Menu **Build → Build Bundle(s)/APK(s) → Build APK(s)**
6. O APK estará em: `app/build/outputs/apk/debug/app-debug.apk`

Para instalar no dispositivo:
- Habilite "Fontes desconhecidas" nas configurações do Android
- Copie o APK e abra no dispositivo

---

### OPÇÃO 2 — Linha de comando (Linux/Mac/Chrome OS)

```bash
# 1. Instale o Android SDK Command Line Tools
# https://developer.android.com/studio#command-tools

# 2. Configure variáveis de ambiente
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools

# 3. Instale as ferramentas necessárias
sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

# 4. Na pasta do projeto, compile
cd lousa-apk
chmod +x gradlew
./gradlew assembleDebug

# APK gerado em:
# app/build/outputs/apk/debug/app-debug.apk

# 5. Instalar direto no dispositivo conectado via USB
adb install app/build/outputs/apk/debug/app-debug.apk
```

---

### OPÇÃO 3 — Compilação online gratuita ✅ (sem instalar nada)

**GitHub Actions** (gratuito):

1. Crie uma conta em https://github.com
2. Crie um repositório novo
3. Faça upload de todos os arquivos desta pasta
4. Crie o arquivo `.github/workflows/build.yml` com o conteúdo abaixo
5. O GitHub compila e disponibiliza o APK para download

```yaml
# .github/workflows/build.yml
name: Build APK
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
      - name: Build APK
        run: |
          chmod +x gradlew
          ./gradlew assembleDebug
      - uses: actions/upload-artifact@v3
        with:
          name: lousa-digital-apk
          path: app/build/outputs/apk/debug/app-debug.apk
```

---

### OPÇÃO 4 — APKOnline.net (sem instalar nada, só browser) ✅

1. Acesse https://www.apkonline.net/compileandroid/
2. Faça upload de todos os arquivos do projeto
3. Clique em Compile → Download APK

---

## Estrutura do projeto

```
lousa-apk/
├── app/
│   ├── src/main/
│   │   ├── java/br/edu/unemat/caceres/lousa/
│   │   │   └── MainActivity.java     ← Activity principal (WebView)
│   │   ├── assets/
│   │   │   └── lousa.html            ← App completo (HTML/JS/CSS)
│   │   ├── res/
│   │   │   ├── layout/activity_main.xml
│   │   │   ├── values/styles.xml
│   │   │   ├── xml/network_security_config.xml
│   │   │   └── mipmap-*/ic_launcher.png
│   │   └── AndroidManifest.xml
│   ├── build.gradle
│   └── proguard-rules.pro
├── gradle/wrapper/
│   └── gradle-wrapper.properties
├── build.gradle
├── settings.gradle
├── gradlew          ← Linux/Mac/ChromeOS
├── gradlew.bat      ← Windows
└── local.properties ← Aponte para seu Android SDK
```

## Especificações do APK

- **Package**: br.edu.unemat.caceres.lousa
- **Min Android**: 7.0 (API 24)
- **Target Android**: 14 (API 34)
- **Orientação**: Paisagem (landscape)
- **Modo**: Tela cheia imersiva (sem barras do sistema)
- **Tamanho estimado**: ~150 KB
