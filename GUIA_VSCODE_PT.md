# Guia Completo: Executando py-xiaozhi no VSCode

## 📋 Índice

1. [Visão Geral do Projeto](#visão-geral-do-projeto)
2. [Requisitos do Sistema](#requisitos-do-sistema)
3. [Instalação e Configuração](#instalação-e-configuração)
4. [Configurando o VSCode](#configurando-o-vscode)
5. [Executando o Projeto](#executando-o-projeto)
6. [Depuração (Debug)](#depuração-debug)
7. [Tarefas Comuns](#tarefas-comuns)
8. [Solução de Problemas](#solução-de-problemas)

---

## 🎯 Visão Geral do Projeto

### O que é o py-xiaozhi?

O **py-xiaozhi** é um cliente de voz baseado em Python que implementa assistente de IA com capacidades avançadas de interação por voz. É uma porta do projeto [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) para computadores desktop, permitindo experimentar funcionalidades de IA sem necessidade de hardware específico.

### Características Principais

#### 🤖 Funcionalidades de IA
- **Interação por Voz**: Sistema completo de reconhecimento e síntese de voz
- **Visão Computacional**: Reconhecimento e análise de imagens (multimodal)
- **Ativação Inteligente**: Detecção de palavras-chave para ativar o assistente
- **Diálogo Contínuo**: Conversas naturais e fluidas com o assistente

#### 🛠️ Ecossistema de Ferramentas MCP
- **Controle do Sistema**: Monitoramento, gerenciamento de aplicativos, controle de volume
- **Calendário**: Gestão completa de eventos e lembretes
- **Temporizadores**: Contadores regressivos e execução agendada de tarefas
- **Música**: Busca e reprodução de música online com cache local
- **Busca na Web**: Integração com Bing para pesquisas inteligentes
- **Receitas**: Base de dados de receitas com busca e recomendações
- **Mapas**: Serviços de geolocalização, rotas e clima via Amap
- **Câmera**: Captura de imagens e análise com IA

#### 🏠 Integração IoT
- **Controle de Dispositivos**: Arquitetura unificada para gerenciar dispositivos IoT
- **Casa Inteligente**: Controle de luzes, sensores de temperatura, etc.
- **Sincronização em Tempo Real**: Monitoramento de estado dos dispositivos

#### 🎵 Processamento de Áudio Avançado
- **Codecs**: Suporte a Opus e reamostragem em tempo real
- **Detecção de Atividade de Voz (VAD)**: Interrupção inteligente
- **Cancelamento de Eco**: Módulo WebRTC para áudio de alta qualidade
- **Gravação de Áudio do Sistema**: Captura de áudio de loopback

### Arquitetura Técnica

O projeto utiliza uma arquitetura orientada a eventos baseada em `asyncio`:

```
┌─────────────────────────────────────────────────┐
│           Application (Camada Principal)        │
├─────────────────────────────────────────────────┤
│  Protocol Layer    │  Audio Processing Layer   │
│  (WebSocket/MQTT)  │  (Opus, VAD, AEC)        │
├─────────────────────────────────────────────────┤
│  MCP Tools Layer   │  IoT Device Layer         │
│  (Sistema, Música) │  (Thing Manager)          │
├─────────────────────────────────────────────────┤
│  Display Layer (PyQt5 GUI / CLI)               │
└─────────────────────────────────────────────────┘
```

**Componentes Principais**:
- `main.py`: Ponto de entrada da aplicação
- `src/application.py`: Lógica central da aplicação
- `src/protocols/`: Comunicação WebSocket/MQTT
- `src/audio_codecs/`: Processamento de áudio
- `src/mcp/`: Sistema de ferramentas modulares
- `src/views/`: Interface gráfica PyQt5

---

## 💻 Requisitos do Sistema

### Requisitos Básicos

- **Python**: 3.9 - 3.12 (recomendado: 3.10)
- **Sistema Operacional**: 
  - Windows 10+
  - macOS 10.15+
  - Linux (Ubuntu 20.04+, Debian, etc.)
- **Memória RAM**: Mínimo 4GB (recomendado: 8GB+)
- **Espaço em Disco**: 2GB livres
- **Dispositivos de Áudio**: Microfone e alto-falantes funcionais
- **Conexão à Internet**: Necessária para serviços de IA

### Requisitos para Funcionalidades Opcionais

- **Ativação por Voz**: Modelos Sherpa-ONNX (~500MB)
- **Câmera**: Dispositivo de câmera + OpenCV
- **API Keys**: Chave API da Zhipu para visão computacional (opcional)

---

## 🚀 Instalação e Configuração

### Passo 1: Instalar Dependências do Sistema

#### Windows

```powershell
# Instalar Scoop (gerenciador de pacotes)
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex

# Instalar dependências
scoop install git python ffmpeg
```

#### macOS

```bash
# Instalar Homebrew (se ainda não tiver)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instalar dependências
brew install python@3.10 portaudio opus ffmpeg git
xcode-select --install
```

#### Linux (Ubuntu/Debian)

```bash
# Atualizar repositórios
sudo apt-get update

# Instalar dependências principais
sudo apt-get install -y \
    python3.10 python3.10-venv python3-pip \
    portaudio19-dev libportaudio2 \
    ffmpeg libopus0 libopus-dev \
    build-essential libasound2-dev \
    libxcb-xinerama0 libxkbcommon-x11-0 \
    pulseaudio-utils

# Para desenvolvimento
sudo apt-get install -y gcc g++ make cmake pkg-config
```

### Passo 2: Clonar o Repositório

```bash
# Clonar o projeto
git clone https://github.com/huangjunsen0406/py-xiaozhi.git
cd py-xiaozhi
```

### Passo 3: Configurar Ambiente Python

#### Opção A: Usando Miniconda (Recomendado)

```bash
# Baixar e instalar Miniconda
# Windows: https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe
# macOS (Intel): wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
# macOS (Apple Silicon): wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
# Linux: wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Para Linux/macOS, executar:
chmod +x Miniconda3-latest-*.sh
./Miniconda3-latest-*.sh

# Criar ambiente virtual
conda create -n py-xiaozhi python=3.10 -y
conda activate py-xiaozhi

# Instalar PyQt5 via conda (recomendado para evitar problemas)
conda install -c conda-forge pyqt=5.15 -y
```

#### Opção B: Usando venv

```bash
# Criar ambiente virtual
python3 -m venv .venv

# Ativar ambiente virtual
# Windows:
.venv\Scripts\activate
# Linux/macOS:
source .venv/bin/activate
```

### Passo 4: Instalar Dependências Python

```bash
# Atualizar pip
pip install --upgrade pip

# Instalar dependências
# Linux/Windows:
pip install -r requirements.txt

# macOS:
pip install -r requirements_mac.txt
```

### Passo 5: Verificar Instalação

```bash
# Testar importações principais
python -c "import sounddevice; print('✓ SoundDevice OK')"
python -c "import opuslib; print('✓ Opus OK')"
python -c "import PyQt5.QtCore as q; print('✓ PyQt5 OK, versão Qt:', q.QT_VERSION_STR)"
python -c "import websockets; print('✓ WebSockets OK')"
python -c "import asyncio; print('✓ Asyncio OK')"
```

Se todos os testes passarem, a instalação está completa! ✅

---

## 🔧 Configurando o VSCode

### Passo 1: Instalar o Visual Studio Code

Baixe e instale o VSCode em: https://code.visualstudio.com/

### Passo 2: Instalar Extensões Recomendadas

Abra o VSCode e instale as seguintes extensões:

1. **Python** (ms-python.python) - Essencial
2. **Pylance** (ms-python.vscode-pylance) - IntelliSense aprimorado
3. **Python Debugger** (ms-python.debugpy) - Depuração
4. **Black Formatter** (ms-python.black-formatter) - Formatação de código
5. **autoDocstring** (njpwerner.autodocstring) - Geração de docstrings
6. **GitLens** (eamodio.gitlens) - Ferramentas Git avançadas (opcional)
7. **Error Lens** (usernamehw.errorlens) - Visualização de erros (opcional)

Para instalar rapidamente, execute no terminal do VSCode:

```bash
code --install-extension ms-python.python
code --install-extension ms-python.vscode-pylance
code --install-extension ms-python.debugpy
code --install-extension ms-python.black-formatter
code --install-extension njpwerner.autodocstring
```

### Passo 3: Abrir o Projeto no VSCode

```bash
# Na pasta do projeto
code .
```

### Passo 4: Selecionar o Interpretador Python

1. Pressione `Ctrl+Shift+P` (Windows/Linux) ou `Cmd+Shift+P` (macOS)
2. Digite "Python: Select Interpreter"
3. Selecione o ambiente que você criou:
   - Se usou conda: `conda: py-xiaozhi`
   - Se usou venv: `.venv` na pasta do projeto

### Passo 5: Configurar launch.json para Depuração

O projeto já inclui configurações, mas você pode personalizar. Crie/edite `.vscode/launch.json`:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: py-xiaozhi GUI",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "console": "integratedTerminal",
            "justMyCode": false,
            "args": [
                "--mode", "gui",
                "--protocol", "websocket"
            ],
            "env": {
                "PYTHONUNBUFFERED": "1"
            }
        },
        {
            "name": "Python: py-xiaozhi CLI",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "console": "integratedTerminal",
            "justMyCode": false,
            "args": [
                "--mode", "cli",
                "--protocol", "websocket"
            ]
        },
        {
            "name": "Python: py-xiaozhi Skip Activation (Debug)",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "console": "integratedTerminal",
            "justMyCode": false,
            "args": [
                "--mode", "gui",
                "--protocol", "websocket",
                "--skip-activation"
            ]
        },
        {
            "name": "Python: py-xiaozhi MQTT",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "console": "integratedTerminal",
            "justMyCode": false,
            "args": [
                "--mode", "gui",
                "--protocol", "mqtt"
            ]
        }
    ]
}
```

### Passo 6: Configurar tasks.json para Tarefas Comuns

Crie/edite `.vscode/tasks.json`:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Run GUI Mode",
            "type": "shell",
            "command": "${command:python.interpreterPath}",
            "args": ["main.py", "--mode", "gui"],
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "Run CLI Mode",
            "type": "shell",
            "command": "${command:python.interpreterPath}",
            "args": ["main.py", "--mode", "cli"],
            "problemMatcher": []
        },
        {
            "label": "Format Code (Black)",
            "type": "shell",
            "command": "${command:python.interpreterPath}",
            "args": ["-m", "black", "src/", "main.py"],
            "problemMatcher": []
        },
        {
            "label": "Lint Code (Flake8)",
            "type": "shell",
            "command": "${command:python.interpreterPath}",
            "args": ["-m", "flake8", "src/", "main.py"],
            "problemMatcher": []
        },
        {
            "label": "Test Camera",
            "type": "shell",
            "command": "${command:python.interpreterPath}",
            "args": ["scripts/camera_scanner.py"],
            "problemMatcher": []
        }
    ]
}
```

### Passo 7: Configurações do Editor

As configurações já estão em `.vscode/settings.json`, mas aqui está o que elas fazem:

- **Formatação automática**: Código formatado com Black ao salvar
- **Organização de imports**: Imports organizados automaticamente
- **Linting**: Verificação de código com Flake8
- **Docstrings**: Formato Google para documentação

---

## ▶️ Executando o Projeto

### Método 1: Via Terminal Integrado do VSCode

1. Abra o terminal no VSCode: `` Ctrl+` `` (ou View > Terminal)
2. Certifique-se de que o ambiente virtual está ativado
3. Execute:

```bash
# Modo GUI (padrão)
python main.py

# Modo CLI
python main.py --mode cli

# Com protocolo MQTT
python main.py --protocol mqtt

# Pular ativação (para debug)
python main.py --skip-activation
```

### Método 2: Usando Tarefas do VSCode

1. Pressione `Ctrl+Shift+P` (Windows/Linux) ou `Cmd+Shift+P` (macOS)
2. Digite "Tasks: Run Task"
3. Selecione a tarefa desejada:
   - "Run GUI Mode"
   - "Run CLI Mode"

### Método 3: Usando o Botão Play

1. Abra o arquivo `main.py`
2. Clique no botão ▶️ (Play) no canto superior direito
3. Ou pressione `F5` para iniciar com depuração

### Primeira Execução: Ativação do Dispositivo

Na primeira execução, você precisará ativar o dispositivo:

1. O programa abrirá uma janela de ativação
2. Um URL será mostrado (ex: `https://xiaozhi.me/`)
3. Abra o URL em seu navegador
4. Escaneie o QR code ou insira o código manualmente
5. Complete a ativação no site
6. O programa detectará automaticamente a ativação

**Nota**: A ativação é necessária apenas uma vez. As credenciais são salvas em `config/efuse.json`.

---

## 🐛 Depuração (Debug)

### Usando o Debugger do VSCode

1. **Definir Breakpoints**:
   - Clique à esquerda do número da linha no código
   - Um ponto vermelho aparecerá

2. **Iniciar Depuração**:
   - Pressione `F5`
   - Ou vá em "Run > Start Debugging"
   - Selecione a configuração desejada (GUI, CLI, etc.)

3. **Controles de Depuração**:
   - `F5`: Continue
   - `F10`: Step Over (próxima linha)
   - `F11`: Step Into (entrar na função)
   - `Shift+F11`: Step Out (sair da função)
   - `Ctrl+Shift+F5`: Restart
   - `Shift+F5`: Stop

4. **Inspecionar Variáveis**:
   - Painel "Variables" mostra todas as variáveis no escopo
   - Passe o mouse sobre variáveis no código
   - Use o "Debug Console" para avaliar expressões

### Debug Console

Durante a depuração, você pode avaliar expressões Python no Debug Console:

```python
# Verificar valor de uma variável
print(config.get_config("SYSTEM_OPTIONS.CLIENT_ID"))

# Chamar funções
await some_async_function()

# Inspecionar objetos
dir(app)
```

### Logging

O projeto usa logging extensivo. Logs são salvos em:
- Console: Saída colorida
- Arquivo: `logs/xiaozhi.log` (se configurado)

Para ajustar o nível de log, edite `src/utils/logging_config.py`:

```python
# Níveis: DEBUG, INFO, WARNING, ERROR, CRITICAL
setup_logging(level="DEBUG")
```

---

## 📝 Tarefas Comuns

### Formatar Código

```bash
# Via terminal
python -m black src/ main.py

# Via VSCode
# Ctrl+Shift+P > "Format Document"
# Ou salve o arquivo (formatação automática)
```

### Verificar Estilo do Código

```bash
# Via terminal
python -m flake8 src/ main.py

# Via VSCode Task
# Ctrl+Shift+P > "Tasks: Run Task" > "Lint Code (Flake8)"
```

### Testar Câmera

```bash
# Via terminal
python scripts/camera_scanner.py

# Via VSCode Task
# Ctrl+Shift+P > "Tasks: Run Task" > "Test Camera"
```

### Gerar Árvore de Diretórios

```bash
python scripts/dir_tree.py
```

### Verificar Opus

```bash
bash checke_opus.sh
```

### Atualizar Dependências

```bash
# Atualizar pip
pip install --upgrade pip

# Reinstalar requirements
pip install -r requirements.txt --upgrade

# Para macOS
pip install -r requirements_mac.txt --upgrade
```

---

## 🔍 Solução de Problemas

### Problema 1: "ModuleNotFoundError"

**Erro**: `ModuleNotFoundError: No module named 'xxx'`

**Solução**:
```bash
# Verificar se o ambiente virtual está ativado
# Conda:
conda activate py-xiaozhi

# venv:
source .venv/bin/activate  # Linux/macOS
.venv\Scripts\activate     # Windows

# Reinstalar dependências
pip install -r requirements.txt
```

### Problema 2: PyQt5 não funciona

**Erro**: Problemas com interface gráfica

**Solução para Linux**:
```bash
# Instalar dependências do sistema
sudo apt-get install -y libxcb-xinerama0 libxkbcommon-x11-0

# Reinstalar PyQt5
pip uninstall PyQt5 PyQt5-sip PyQt5-Qt5 -y
conda install -c conda-forge pyqt=5.15 -y
```

**Solução para macOS**:
```bash
# Usar conda para PyQt5
conda install -c conda-forge pyqt=5.15
```

### Problema 3: Erro de Áudio (SoundDevice)

**Erro**: `PortAudioError` ou problemas com microfone

**Solução Windows**:
```powershell
# Reinstalar sounddevice
pip uninstall sounddevice -y
pip install sounddevice
```

**Solução Linux**:
```bash
# Verificar PulseAudio
pulseaudio --check
pulseaudio --start

# Adicionar usuário ao grupo audio
sudo usermod -a -G audio $USER
# Fazer logout e login novamente

# Reinstalar dependências de áudio
sudo apt-get install -y portaudio19-dev libportaudio2 pulseaudio-utils
pip install sounddevice --force-reinstall
```

**Solução macOS**:
```bash
# Reinstalar portaudio
brew reinstall portaudio
pip install sounddevice --force-reinstall
```

### Problema 4: Ativação Falha

**Erro**: Não consegue ativar o dispositivo

**Solução**:
```bash
# Limpar configuração de ativação
rm config/efuse.json

# Executar novamente
python main.py
```

### Problema 5: VSCode não encontra o interpretador Python

**Solução**:
1. Pressione `Ctrl+Shift+P`
2. Digite "Python: Select Interpreter"
3. Se o ambiente não aparecer:
   - Para conda: `which python` no terminal conda ativado
   - Para venv: Navegue até `.venv/bin/python` (Linux/macOS) ou `.venv\Scripts\python.exe` (Windows)
4. Cole o caminho completo em "Enter interpreter path"

### Problema 6: Eco ou Áudio Ruim

**Solução**:

Edite `config/config.json`:

```json
{
  "AEC_OPTIONS": {
    "ENABLED": true,
    "FILTER_LENGTH_RATIO": 0.6,
    "BUFFER_MAX_LENGTH": 300,
    "ENABLE_PREPROCESS": true
  }
}
```

Ajuste `FILTER_LENGTH_RATIO`:
- Eco forte: aumentar para 0.6-0.8
- Áudio cortado: diminuir para 0.2-0.4

### Problema 7: Depuração não funciona

**Solução**:

1. Verificar que a extensão Python Debugger está instalada
2. Verificar que o interpretador correto está selecionado
3. Tentar criar um novo `launch.json`:
   - Delete `.vscode/launch.json`
   - Pressione `F5`
   - Selecione "Python Debugger" > "Python File"

### Problema 8: Opus Library não encontrada

**Erro**: `Could not find opus library`

**Solução Windows**:
```powershell
# A DLL já está incluída no projeto em libs/windows/
# Copiar para o diretório do sistema se necessário
copy libs\windows\opus.dll C:\Windows\System32\
```

**Solução macOS**:
```bash
# Instalar via conda (recomendado)
conda install -c conda-forge opus

# Ou via Homebrew
brew install opus

# Configurar DYLD_LIBRARY_PATH se necessário
echo 'export DYLD_LIBRARY_PATH=/opt/homebrew/lib:$DYLD_LIBRARY_PATH' >> ~/.zshrc
source ~/.zshrc
```

**Solução Linux**:
```bash
sudo apt-get install -y libopus0 libopus-dev
```

### Problema 9: Permissões no Linux

**Erro**: Erro de permissão ao acessar dispositivos

**Solução**:
```bash
# Adicionar usuário aos grupos necessários
sudo usermod -a -G audio,video $USER

# Fazer logout e login novamente para aplicar

# Verificar permissões
groups $USER
```

### Problema 10: Wayland no Linux

**Erro**: Interface gráfica não abre em ambientes Wayland

**Solução**: O programa detecta automaticamente Wayland e configura Qt. Se ainda houver problemas:

```bash
# Forçar X11
export QT_QPA_PLATFORM=xcb
python main.py

# Ou forçar Wayland
export QT_QPA_PLATFORM=wayland
python main.py
```

---

## 📚 Recursos Adicionais

### Documentação Completa

- **Site Oficial**: https://huangjunsen0406.github.io/py-xiaozhi/
- **Guia de Configuração**: `documents/docs/guide/配置说明.md`
- **Instalação de Dependências**: `documents/docs/guide/系统依赖安装.md`
- **Ativação de Dispositivo**: `documents/docs/guide/设备激活流程.md`

### Vídeos Tutoriais

- [Vídeo Tutorial Completo](https://www.bilibili.com/video/BV1dWQhYEEmq/)
- [Demonstração do Projeto](https://www.bilibili.com/video/BV1HmPjeSED2/)

### Comunidade e Suporte

- **GitHub Issues**: https://github.com/huangjunsen0406/py-xiaozhi/issues
- **Gitee**: https://gitee.com/huang-jun-sen/py-xiaozhi

### Arquivos de Configuração Úteis

- `config/config.json`: Configuração principal do sistema
- `config/efuse.json`: Dados de ativação do dispositivo
- `models/keywords.txt`: Palavras-chave para ativação por voz
- `.vscode/settings.json`: Configurações do VSCode
- `.vscode/launch.json`: Configurações de depuração

---

## 🎓 Dicas de Desenvolvimento no VSCode

### Atalhos Úteis

| Atalho | Ação |
|--------|------|
| `Ctrl+P` | Abrir arquivo rapidamente |
| `Ctrl+Shift+P` | Paleta de comandos |
| `Ctrl+` ` | Abrir/fechar terminal |
| `F5` | Iniciar depuração |
| `F12` | Ir para definição |
| `Shift+F12` | Encontrar todas as referências |
| `Ctrl+Shift+F` | Buscar em todos os arquivos |
| `Ctrl+/` | Comentar/descomentar linha |

### Snippets Personalizados

Para criar snippets personalizados:
1. File > Preferences > User Snippets
2. Selecione "python.json"
3. Adicione snippets customizados

### Multi-Cursor

- `Alt+Click`: Adicionar cursor
- `Ctrl+Alt+Up/Down`: Adicionar cursor acima/abaixo
- `Ctrl+D`: Selecionar próxima ocorrência

### Zen Mode

Pressione `Ctrl+K Z` para entrar no modo zen (tela cheia sem distrações)

---

## ✅ Checklist de Configuração

- [ ] Python 3.9-3.12 instalado
- [ ] VSCode instalado
- [ ] Extensão Python instalada no VSCode
- [ ] Dependências do sistema instaladas
- [ ] Repositório clonado
- [ ] Ambiente virtual criado e ativado
- [ ] Dependências Python instaladas
- [ ] Interpretador Python selecionado no VSCode
- [ ] `.vscode/launch.json` configurado
- [ ] `.vscode/tasks.json` configurado
- [ ] Teste de importações passou
- [ ] Dispositivo de áudio funcionando
- [ ] Primeira execução e ativação completa
- [ ] Programa executa sem erros

---

## 🎉 Conclusão

Parabéns! Você configurou com sucesso o py-xiaozhi no VSCode. Agora você pode:

✅ Executar o projeto em modo GUI ou CLI
✅ Depurar código com breakpoints
✅ Formatar e verificar código automaticamente
✅ Usar tarefas personalizadas
✅ Desenvolver novos recursos

**Próximos Passos**:
1. Explore as funcionalidades do assistente
2. Personalize as configurações em `config/config.json`
3. Adicione suas próprias palavras de ativação
4. Configure ferramentas MCP adicionais
5. Contribua com o projeto!

**Precisa de Ajuda?**
- Consulte a documentação completa
- Abra uma issue no GitHub
- Assista aos vídeos tutoriais

Boa sorte com o desenvolvimento! 🚀
