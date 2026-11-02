# 🚀 TuxAssist - Advanced Terminal AI Agent

<!-- INÍCIO: atalhos de download e instalação -->
<div align="center">

<a href="https://github.com/snaidermadilus-debug/MCPServer-iai/archive/refs/heads/main.zip">
  <img src="https://img.shields.io/badge/📦%20Baixar%20Projeto-ZIP-blue?style=for-the-badge" alt="Baixar Projeto ZIP">
</a>
&nbsp;
<a href="https://github.com/snaidermadilus-debug/MCPServer-iai">
  <img src="https://img.shields.io/badge/🐙%20Abrir%20no%20GitHub-black?style=for-the-badge&logo=github" alt="Abrir no GitHub">
</a>

</div>

### ⚡ Instalação rápida das dependências

**Termux / Linux / Windows — Python 3:**

```bash
python -m pip install google-genai rich mcp
```

**ESP32-S3 — Arduino IDE / ArduinoDroid:**

- Instale o pacote de placas **ESP32**.
- Use a placa **ESP32-S3 Dev Module** (ou o modelo correspondente).
- Não é necessário instalar bibliotecas externas ZIP para o firmware descrito neste README.

> 📋 **Copiar código:** os blocos de código abaixo podem ser copiados diretamente pelo botão de copiar exibido pelo GitHub no bloco de código.

<!-- FIM: atalhos de download e instalação -->


O **TuxAssist** é um assistente de Inteligência Artificial de alto desempenho desenvolvido para rodar nativamente no **Termux** (Android). Ele atua como um agente autônomo capaz de ler o sistema, manipular arquivos, executar comandos locais e até interagir com hardware embarcado, como o **ESP32-S3**.

Diferente de wrappers de IA comuns, o TuxAssist possui "mãos e olhos" no seu sistema: ele pode compilar códigos, escanear redes, ler logs e interagir com componentes eletrônicos em tempo real sem depender de um backend externo.

---

## ✨ Principais Funcionalidades

* 🤖 **Arquitetura Multi-IA (Hot-Swap):** Suporte nativo para **Gemini (Google)**, **Groq (Llama 3/Mixtral)** e **OpenAI (ChatGPT)**. Alternância instantânea de modelos e provedores durante a sessão.
* 🛠️ **Integração MCP (Model Context Protocol):** Separação segura entre o cérebro da IA (Cliente) e as ferramentas do sistema (Servidor).
* 📂 **Visão Raio-X de Diretórios:** Escaneamento profundo e recursivo de pastas (ignorando diretórios ocultos e de cache para economia de tokens).
* 💻 **Execução CLI Nativa:** Permissão para executar scripts Bash, Python, Nmap e comandos do sistema operacional diretamente no host.
* 📡 **Integração IoT (ESP32-S3 via TCP Raw):** Comunicação via Socket TCP operando no **Core 0** do ESP32. Permite controle de GPIO, leitura de RAM e sensores em tempo real sem interromper a aplicação principal.
* 🛡️ **Anti-Bloqueio Integrado:** Requisições formatadas com *User-Agent* dinâmico para burlar proteções de Cloudflare (Erro 1010) em APIs gratuitas.
* 🎨 **Renderização Markdown:** Interface visualmente rica no terminal utilizando a biblioteca `rich`, formatando blocos de código, listas e textos automaticamente.

---

## 🏗️ Arquitetura do Sistema

O projeto é dividido em três camadas principais:

1. **`chat.py` (O Cérebro / UI):** 
   Interface interativa. Gerencia as chaves de API, o estado da sessão (via `tuxassist_config.json`) e renderiza a saída em Markdown. Ele inicializa o servidor MCP em segundo plano automaticamente.
    
2. **`servidor_mcp.py` (Os Braços / Ferramentas):** 
    Servidor local que expõe as ferramentas para a IA. Contém os métodos `listar_arquivos`, `ler_arquivo`, `executar_comando` e `comando_remoto_esp32`.

3. **ESP32-S3 TCP Server (O Hardware):** 
    Firmware em C++ que roda uma *Task* isolada (FreeRTOS) no Núcleo 0, ouvindo a porta 2222 para receber injeção de comandos crus (`write()`) do Termux via rede Wi‑Fi local.

---

## ⚙️ Pré-requisitos

Para rodar este projeto, você precisará de:
* Aplicativo **Termux** atualizado (via F-Droid).
* **Python 3** instalado.
* Acesso à mesma rede Wi-Fi que o seu dispositivo ESP32.

### Bibliotecas Python:
```bash
python -m pip install google-genai rich


📦 1. Dependências do Projeto Completo (ESP32 + MCP Termux)
Para rodar a versão completa com controle de hardware, o usuário vai precisar preparar os dois ambientes (Celular/PC e a Placa).

🐍 Ambiente Python (Termux / Linux / Windows)
Para o cérebro rodar, o usuário só precisa do Python 3 instalado e baixar 3 pacotes.
Comando de instalação:

python -m pip install google-genai rich mcp


google-genai: A biblioteca oficial mais atualizada do Google para conversar com o Gemini.
rich: Responsável por deixar o terminal bonito, renderizando cores e Markdown (negrito, blocos de código) direto na tela preta.
mcp: O SDK oficial do Model Context Protocol para criar o servidor de ferramentas.
(Nota: O Groq e a OpenAI não precisam de bibliotecas externas neste projeto, usamos o urllib nativo do Python para evitar o travamento do pacote jiter no Android).


⚡ Ambiente Arduino (C++ para o ESP32)

A grande sacada do código ESP32 S3 é que NÃO
É PRECISO BAIXAR NENHUMA BIBLIOTECA EXTERNA (ZIP)
no Arduino IDE ou ArduinoDroid! Escrevemos tudo usando o núcleo padrão da placa.

O que o usuário deve fazer:
Instalar o pacote de placas do ESP32 na IDE.
URL do Gerenciador de Placas: [https://dl.espressif.com/dl/package_esp32_index.json]
(https://dl.espressif.com/dl/package_esp32_index.json)
Selecionar a placa ESP32-S3 Dev Module (ou o modelo correspondente).

Bibliotecas Nativas Utilizadas (Já embutidas no pacote ESP32):
<WiFi.h> (Conexão de rede)
<WebServer.h> (Painel Web UI e API)
<DNSServer.h> (Captive Portal)
<HTTPClient.h> e <WiFiClientSecure.h> (Comunicação com a API do Groq/OpenAI)
<Preferences.h> (Salvar configurações e senhas na memória Flash)
<ESPmDNS.h> (Acesso via navegador por miniia.local)
<Update.h> (Atualização OTA pelo navegador)
Nota: O JSON é processado por uma função customizada no próprio código,
 eliminando a necessidade da pesada biblioteca ArduinoJson.



http://192.168.4.1/

CODIGO DO ESP32 S3 PARA COMPILAR NO ANDROID


// =============================================================================
//  MINI IA ESP32 - ULTRA SERVER
//  Compativel com ESP32 / ESP32-S3 / ESP32-C3
// =============================================================================

// =============================================================================
//  1. INCLUDES
// =============================================================================
#include <WiFi.h>
#include <WebServer.h>
#include <DNSServer.h>
#include <WiFiClientSecure.h>
#include <HTTPClient.h>
#include <Preferences.h>
#include <ESPmDNS.h>
#include <Update.h>
#include <stdlib.h>
#include <time.h>

// FreeRTOS (necessario para xTaskCreate no ArduinoDroid)
#include <freertos/FreeRTOS.h>
#include <freertos/task.h>

// Deep Sleep
#include <esp_sleep.h>

// =============================================================================
//  2. CONFIGURAÇÕES
// =============================================================================

// ---------- Wi-Fi padrão ----------
const char* DEFAULT_WIFI_SSID = "NOME_DO_SEU_WIFI";
const char* DEFAULT_WIFI_PASS = "SENHA_DO_SEU_WIFI";

// ---------- Modo Access Point ----------
const char* AP_SSID = "MiniIA-ESP32";
const char* AP_PASS = "12345678";

// ---------- Groq ----------
const char* GROQ_URL      = "https://api.groq.com/openai/v1/chat/completions";
const char* DEFAULT_MODEL = "openai/gpt-oss-120b";

// ---------- Hardware ----------
const int CONTROL_PIN = 2;

// ---------- Limites e segurança ----------
const size_t        MAX_INPUT_LEN      = 2000;
const unsigned long CHAT_COOLDOWN      = 1500;
const int           MAX_LOGIN_ATTEMPTS = 5;
const unsigned long LOCKOUT_TIME       = 15000;

// =============================================================================
//  3. OBJETOS E VARIÁVEIS GLOBAIS
// =============================================================================

// ---------- Serviços ----------
Preferences prefs;
WebServer   server(80);
DNSServer   dnsServer;
const byte  DNS_PORT = 53;

// Servidor TCP para o TuxAssist (roda em uma task separada)
WiFiServer terminalServer(2222);

// ---------- Estado de sessão / rede ----------
String token  = "";
bool   apMode = false;

// ---------- Controle de login e cooldown ----------
int           failedLogins = 0;
unsigned long lockoutStart = 0;
unsigned long lastAskTime  = 0;

// ---------- Logs e monitoramento ----------
String sysLogs         = "";
float  maxObservedTemp = 0.0;
float  maxTempLimit    = 75.0;

// ---------- Parâmetros da IA ----------
int   httpTimeoutMs = 40000;
float aiTemperature = 0.6;
int   aiMaxTokens   = 1500;

// =============================================================================
//  4. LOGS
// =============================================================================
String getFormattedTime() {
  struct tm timeinfo;
  if (!getLocalTime(&timeinfo)) {
    return String(millis() / 1000) + "s";
  }
  char timeBuff[16];
  snprintf(timeBuff, sizeof(timeBuff), "%02d:%02d:%02d", timeinfo.tm_hour, timeinfo.tm_min, timeinfo.tm_sec);
  return String(timeBuff);
}

void addLog(String msg) {
  sysLogs = "[" + getFormattedTime() + "] " + msg + "\n" + sysLogs;
  if (sysLogs.length() > 3000) sysLogs = sysLogs.substring(0, 3000);
}

// =============================================================================
//  5. PÁGINA WEB (HTML + CSS + JS)
// =============================================================================
const char PAGE[] PROGMEM = R"rawliteral(
<!DOCTYPE html>
<html lang="pt">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Mini IA ESP32 Ultra Server</title>

<!-- ======================= ESTILOS ======================= -->
<style>
/* ----- Temas ----- */
:root{--bg:#0b0d14;--card:#141826;--card2:#1a2033;--line:#252c44;--tx:#e8ecf8;--mut:#8b94b3;--ac:#6c7bff;--ac2:#a06bff;--ok:#3ddc97;--err:#ff6b7a;--warn:#f1c40f}
body.theme-oled{--bg:#000000;--card:#0a0a0a;--card2:#121212;--line:#222222;--tx:#ffffff;--mut:#888888}
body.theme-light{--bg:#f4f6fb;--card:#ffffff;--card2:#eef2f9;--line:#d0d7de;--tx:#1f2328;--mut:#656d76}

/* ----- Base ----- */
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{margin:0;background:var(--bg);color:var(--tx);font:15px/1.5 system-ui,-apple-system,Segoe UI,Roboto,sans-serif;min-height:100vh}
main{max-width:850px;margin:0 auto;padding:14px}
.hidden{display:none!important}

/* ----- Cabeçalho ----- */
header{position:sticky;top:0;z-index:10;display:flex;justify-content:space-between;align-items:center;gap:8px;padding:12px 16px;background:rgba(11,13,20,.9);backdrop-filter:blur(12px);border-bottom:1px solid var(--line)}
.brand{font-weight:700;font-size:17px;display:flex;align-items:center;gap:8px}
.brand small{color:var(--mut);font-weight:500;font-size:11px;border:1px solid var(--line);padding:2px 6px;border-radius:99px}
.dot{width:10px;height:10px;border-radius:50%;background:linear-gradient(135deg,var(--ac),var(--ac2));box-shadow:0 0 10px var(--ac)}
.tools{display:flex;gap:6px;align-items:center}

/* ----- Cards e textos ----- */
.card{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:16px;margin-bottom:14px;box-shadow:0 4px 12px rgba(0,0,0,0.15)}
h2,h3{margin:0 0 6px;font-size:16px;font-weight:700}
p.d{color:var(--mut);margin:0 0 12px;font-size:13px}
.err{color:var(--err);font-size:13px;min-height:18px}
.ok{color:var(--ok)}

/* ----- Formulários e botões ----- */
input,select,textarea{width:100%;background:var(--card2);color:var(--tx);border:1px solid var(--line);border-radius:10px;padding:10px 12px;font:inherit;outline:none;margin-bottom:10px}
input:focus,textarea:focus,select:focus{border-color:var(--ac)}
textarea{resize:none;min-height:48px;max-height:160px;margin-bottom:0}
button{font:inherit;border:1px solid var(--line);background:var(--card2);color:var(--tx);padding:9px 14px;border-radius:10px;cursor:pointer;font-weight:600;font-size:13px;display:inline-flex;align-items:center;justify-content:center;gap:6px;transition:.15s ease}
button:hover{border-color:var(--ac)}
button:active{transform:scale(.97)}
button.primary{background:linear-gradient(135deg,var(--ac),var(--ac2));border:0;color:#fff}
button.ghost{background:transparent}
button.mini{padding:5px 9px;font-size:11px;border-radius:8px}
button:disabled{opacity:.5}

/* ----- Layout ----- */
.row{display:flex;gap:8px;margin-top:10px;justify-content:flex-end;flex-wrap:wrap}
.row.l{justify-content:flex-start}
.stack>*{margin-bottom:10px}

/* ----- Chat ----- */
.log{min-height:40vh;padding-bottom:180px}
.msg{max-width:88%;margin:10px 0;padding:10px 14px;border-radius:14px;white-space:pre-wrap;word-wrap:break-word;position:relative;font-size:14px}
.msg.me{margin-left:auto;background:linear-gradient(135deg,var(--ac),var(--ac2));border-bottom-right-radius:4px}
.msg.ai{background:var(--card);border:1px solid var(--line);border-bottom-left-radius:4px}
.msg.pinned{border:2px solid var(--warn)}
.msg-time{font-size:10px;margin-top:4px;color:var(--mut)}
.msg-actions{display:flex;gap:4px;margin-top:6px;flex-wrap:wrap}
.empty{text-align:center;color:var(--mut);margin-top:4vh}
.empty .big{font-size:40px}
.chips{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;margin-top:10px}
.chip{background:var(--card2);border:1px solid var(--line);padding:5px 10px;border-radius:99px;font-size:11px;cursor:pointer;color:var(--tx)}
.dots span{display:inline-block;width:6px;height:6px;margin:0 2px;border-radius:50%;background:var(--mut);animation:b 1s infinite}
.dots span:nth-child(2){animation-delay:.15s}
.dots span:nth-child(3){animation-delay:.3s}
@keyframes b{0%,80%,100%{opacity:.2}40%{opacity:1}}

/* ----- Compositor (barra de envio) ----- */
.composer{position:fixed;left:0;right:0;bottom:0;background:rgba(11,13,20,.95);backdrop-filter:blur(12px);border-top:1px solid var(--line);padding:10px 14px 14px;z-index:8}
.composer>div{max-width:850px;margin:0 auto;display:flex;flex-direction:column;gap:8px}
.input-group{display:flex;gap:8px;align-items:center}

/* ----- Modal ----- */
.modal{position:fixed;inset:0;background:rgba(0,0,0,.7);display:flex;align-items:center;justify-content:center;padding:16px;z-index:20}
.modal .card{width:100%;max-width:380px;margin:0}

/* ----- Histórico ----- */
.session-card{background:var(--card2);border:1px solid var(--line);border-radius:12px;padding:10px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center;cursor:pointer}
.session-card:hover{border-color:var(--ac)}

/* ----- Admin ----- */
.net{display:flex;justify-content:space-between;gap:8px;padding:10px;border:1px solid var(--line);border-radius:10px;margin-bottom:6px;background:var(--card2);cursor:pointer;font-size:13px}
.net:active{background:var(--line)}
.kv{display:flex;justify-content:space-between;align-items:center;padding:7px 0;border-bottom:1px solid var(--line);font-size:13px}
.kv span:first-child{color:var(--mut)}
.progress-bar{width:100%;height:8px;background:var(--card2);border-radius:4px;overflow:hidden;margin-top:4px;border:1px solid var(--line)}
.progress-fill{height:100%;background:linear-gradient(90deg,var(--ac),var(--ok));transition:width .3s}
.sys-terminal{background:#05070a;color:#3ddc97;font-family:monospace;padding:10px;border-radius:10px;height:140px;overflow-y:auto;font-size:11px;border:1px solid var(--line);white-space:pre-wrap}
.tab-nav{display:flex;gap:6px;overflow-x:auto;padding-bottom:8px;margin-bottom:14px;border-bottom:1px solid var(--line);scrollbar-width:none}
.tab-nav::-webkit-scrollbar{display:none}
.tab-btn{background:var(--card2);border:1px solid var(--line);padding:8px 12px;border-radius:10px;font-size:12px;white-space:nowrap;cursor:pointer;color:var(--mut);font-weight:600}
.tab-btn.active{background:var(--ac);color:#fff;border-color:var(--ac)}
.cmd-shortcuts{display:flex;gap:6px;flex-wrap:wrap;margin-top:8px}
</style>
</head>

<body>

<!-- ======================= CABEÇALHO ======================= -->
<header>
  <div class="brand"><span class="dot"></span>Mini IA <small>ESP32 Server</small></div>
  <div class="tools">
    <button id="bTheme" type="button" class="ghost" style="font-size:13px;">🌙 Dark</button>
    <button id="bTTS" type="button" class="ghost" style="font-size:13px;">🔊 Off</button>
    <button id="bHist" type="button" class="ghost">🗂️ Conversas</button>
    <button id="bAdmin" type="button" class="ghost">⚙️ Admin</button>
  </div>
</header>

<main>

  <!-- ======================= VIEW: SETUP ======================= -->
  <section id="vSetup">
    <div class="card stack" style="margin-top:20px">
      <h2>🔑 Configurar API Groq</h2>
      <p class="d">Cole sua chave API da Groq abaixo para habilitar o Chat.</p>
      <input id="sKey" type="password" placeholder="gsk_..." autocomplete="off">
      <div id="sErr" class="err"></div>
      <div class="row" style="justify-content:space-between; align-items:center;">
        <button id="bSkipSetup" type="button" class="ghost" style="font-size:12px; color:var(--mut);">Pular para o Chat</button>
        <button id="sSave" type="button" class="primary">Salvar e Entrar</button>
      </div>
    </div>
  </section>

  <!-- ======================= VIEW: CHAT ======================= -->
  <section id="vChat" class="hidden">
    <div style="margin-bottom:10px; display:flex; gap:6px;">
      <input id="chatSearch" placeholder="🔍 Filtrar mensagens na tela..." style="margin-bottom:0; font-size:12px; padding:6px 12px;">
    </div>

    <div id="log" class="log">
      <div class="empty" id="empty">
        <div class="big">🤖</div>
        <div>Como posso te ajudar hoje?</div>
        <div class="chips">
          <div class="chip" onclick="quickPrompt('Escreva um código em C++ para ler um sensor no ESP32.')">💻 Código C++ ESP32</div>
          <div class="chip" onclick="quickPrompt('Como calcular o resistor ideal para um LED?')">⚡ Resistor LED</div>
          <div class="chip" onclick="quickPrompt('Me conte uma piada rápida sobre programação.')">🔥 Piada Geek</div>
        </div>
      </div>
    </div>

    <div class="composer"><div>
      <div style="display:flex; justify-content:space-between; align-items:center; gap:6px; flex-wrap:wrap;">
        <select id="modePreset" style="margin-bottom:0; font-size:12px; padding:4px 6px; max-width:45%;">
          <option value="default">🤖 Modo: Padrão</option>
          <option value="turbo">⚡ Modo: Resposta Rápida (Turbo)</option>
          <option value="coder">💻 Modo: Programador</option>
          <option value="tech">⚡ Modo: Eletrônica</option>
          <option value="fun">🔥 Modo: Engraçado</option>
        </select>
        <div style="display:flex; gap:4px;">
          <button id="bClearChatUI" type="button" class="ghost" style="font-size:11px; padding:4px 6px;">🧹 Limpar</button>
          <button id="bExport" type="button" class="ghost" style="font-size:11px; padding:4px 6px;">💾 Exportar</button>
          <button id="bNewChat" type="button" class="ghost" style="font-size:11px; padding:4px 6px;">➕ Novo</button>
        </div>
      </div>
      <div class="input-group">
        <textarea id="q" rows="1" maxlength="2000" placeholder="Digite ou fale... (Ctrl+Enter envia)"></textarea>
        <button id="bMic" type="button" class="ghost" title="Falar">🎤</button>
        <button id="bSend" type="button" class="primary">Enviar</button>
      </div>
    </div></div>
  </section>

  <!-- ======================= VIEW: HISTÓRICO ======================= -->
  <section id="vHistory" class="hidden">
    <div class="card stack">
      <h3>🗂️ Histórico de Conversas</h3>
      <p class="d">Clique em qualquer conversa para reabri-la ou renomeá-la.</p>
      <div id="sessionsList"></div>
      <div class="row">
        <button id="bClearAllSessions" type="button" class="ghost" style="color:var(--err);">Apagar Tudo</button>
        <button id="bBackChat" type="button" class="primary">Voltar ao Chat</button>
      </div>
    </div>
  </section>

  <!-- ======================= VIEW: ADMIN ======================= -->
  <section id="vAdmin" class="hidden">

    <div class="tab-nav">
      <button class="tab-btn active" onclick="switchAdminTab(this, 'tabDash')">📊 Dashboard</button>
      <button class="tab-btn" onclick="switchAdminTab(this, 'tabGPIO')">🎛️ Hardware & CLI</button>
      <button class="tab-btn" onclick="switchAdminTab(this, 'tabNet')">🌐 Rede & IP</button>
      <button class="tab-btn" onclick="switchAdminTab(this, 'tabAI')">🤖 IA & Groq</button>
      <button class="tab-btn" onclick="switchAdminTab(this, 'tabTools')">🛠️ Ferramentas</button>
      <button class="tab-btn" onclick="switchAdminTab(this, 'tabSec')">🔒 Segurança</button>
    </div>

    <!-- ----- Aba: Dashboard ----- -->
    <div id="tabDash" class="admin-tab">
      <div class="card">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
          <h3>📊 Diagnósticos do Hardware</h3>
          <small id="rtPulse" style="color:var(--ok); font-size:11px;">● Ao vivo</small>
        </div>
        <div id="info">Carregando métricas...</div>
        <div class="row l" style="margin-top:12px;">
          <button id="bFlushRAM" type="button" class="mini primary">🗑️ Limpar RAM (Flush)</button>
        </div>
      </div>
      <div class="card stack">
        <h3>📜 Console de Logs (SysLog)</h3>
        <div id="sysTerm" class="sys-terminal">Aguardando logs do sistema...</div>
      </div>
    </div>

    <!-- ----- Aba: Hardware & CLI ----- -->
    <div id="tabGPIO" class="admin-tab hidden">
      <div class="card stack">
        <h3>⚡ Controle do Pino Digital (Relé / LED)</h3>
        <p class="d">Ligue ou desligue o dispositivo físico no GPIO 2 do ESP32.</p>
        <div class="row l" style="gap:8px;">
          <button id="bGpioHigh" type="button" class="primary">🟢 LIGAR (HIGH)</button>
          <button id="bGpioLow" type="button" class="ghost" style="border-color:var(--err); color:var(--err)">🔴 DESLIGAR (LOW)</button>
        </div>
        <div id="gpioStatus" class="d" style="margin-top:8px;">Estado atual: Desconhecido</div>
      </div>

      <div class="card stack">
        <h3>💻 Terminal CLI (Linha de Comando)</h3>
        <p class="d">Digite um comando ou clique nos atalhos abaixo:</p>
        <input id="cliCmd" placeholder="Ex: temp, heap, gpio 2 high, clearlog...">

        <div class="cmd-shortcuts">
          <button type="button" class="mini" onclick="runCliShortcut('temp')">🌡️ Temp</button>
          <button type="button" class="mini" onclick="runCliShortcut('heap')">📊 RAM</button>
          <button type="button" class="mini" onclick="runCliShortcut('gpio 2 high')">🟢 Ligar LED</button>
          <button type="button" class="mini" onclick="runCliShortcut('gpio 2 low')">🔴 Desligar LED</button>
          <button type="button" class="mini" onclick="runCliShortcut('clearlog')">🧹 Limpar Logs</button>
          <button type="button" class="mini" onclick="runCliShortcut('reboot')">🔁 Reiniciar</button>
        </div>

        <div class="row" style="margin-top:10px;"><button id="bRunCli" type="button" class="primary">Executar Comando</button></div>
      </div>
    </div>

    <!-- ----- Aba: Rede & IP ----- -->
    <div id="tabNet" class="admin-tab hidden">
      <div class="card stack">
        <h3>📶 Conectar em Rede Wi-Fi</h3>
        <input id="wifiSsid" placeholder="Nome da rede (SSID)">
        <input id="wifiPass" type="password" placeholder="Senha do Wi‑Fi">
        <div class="row"><button id="bSaveWifi" type="button" class="primary">Salvar e Conectar</button></div>
      </div>

      <div class="card stack">
        <h3>🌐 Configuração de IP Estático (Fixo)</h3>
        <p class="d">Deixe em branco se preferir obter o IP automaticamente (DHCP).</p>
        <input id="stIP" placeholder="IP Estático (ex: 192.168.1.150)">
        <input id="stGW" placeholder="Gateway (ex: 192.168.1.1)">
        <input id="stMask" placeholder="Máscara (ex: 255.255.255.0)">
        <div class="row"><button id="bSaveStaticIP" type="button" class="primary">Fixar IP Estático</button></div>
      </div>

      <div class="card">
        <h3>📡 Escanear Redes Wi-Fi Próximas</h3>
        <div class="row l"><button id="bScan" type="button" class="primary">Escanear Redes</button></div>
        <div id="nets" style="margin-top:10px"></div>
      </div>
    </div>

    <!-- ----- Aba: IA & Groq ----- -->
    <div id="tabAI" class="admin-tab hidden">
      <div class="card stack">
        <h3>🔑 Provedor API & Modelo</h3>
        <input id="aKey" type="password" placeholder="Nova API key (gsk_...)" autocomplete="off">
        <input id="aModel" placeholder="openai/gpt-oss-120b">

        <label style="font-size:12px; color:var(--mut)">Temperatura / Criatividade (0.0 a 1.0):</label>
        <input id="aTemp" type="number" step="0.1" min="0.0" max="1.0" value="0.6">

        <label style="font-size:12px; color:var(--mut)">Limite de Tokens de Resposta (Max Tokens):</label>
        <input id="aTokens" type="number" step="100" min="500" max="4000" value="1500">

        <label style="font-size:12px; color:var(--mut)">Timeout do HTTP em Segundos (10s a 60s):</label>
        <input id="aTimeout" type="number" min="10" max="60" value="40">

        <label style="font-size:12px; color:var(--mut)">Personalidade / System Prompt Customizado:</label>
        <textarea id="aSysPrompt" rows="3" placeholder="Instruções adicionais para a IA..."></textarea>

        <div class="row"><button id="bKey" type="button" class="primary">Salvar Ajustes de IA</button></div>
      </div>
    </div>

    <!-- ----- Aba: Ferramentas ----- -->
    <div id="tabTools" class="admin-tab hidden">
      <div class="card stack">
        <h3>🌐 Teste de Conexão DNS</h3>
        <p class="d">Verifique se o ESP32 consegue resolver endereços da internet.</p>
        <input id="pingHost" placeholder="Host (ex: google.com)">
        <div id="pingRes" class="d" style="color:var(--ok)"></div>
        <div class="row"><button id="bDoPing" type="button" class="primary">Testar Conexão</button></div>
      </div>

      <div class="card stack">
        <h3>⚡ Controle do Clock da CPU</h3>
        <p class="d">Reduza o clock para economizar bateria e esquentar menos. (No ESP32-C3 o máximo é 160 MHz.)</p>
        <select id="cpuFreqSelect">
          <option value="240">240 MHz (Desempenho Máximo)</option>
          <option value="160">160 MHz (Equilibrado)</option>
          <option value="80">80 MHz (Economia / Frio)</option>
        </select>
        <div class="row"><button id="bSetCpu" type="button" class="primary">Alterar Clock CPU</button></div>
      </div>

      <div class="card stack">
        <h3>💤 Agendar Deep Sleep</h3>
        <p class="d">Desliga componentes internos para economia extrema de energia.</p>
        <input id="sleepSec" type="number" placeholder="Tempo em segundos (ex: 60)">
        <div class="row"><button id="bDeepSleep" type="button" class="ghost" style="color:var(--warn); border-color:var(--warn)">Ativar Deep Sleep</button></div>
      </div>

      <div class="card stack">
        <h3>⚡ Gravação de Firmware OTA</h3>
        <input type="file" id="otaFile" accept=".bin">
        <div id="otaProgress" class="d"></div>
        <div class="row"><button id="bUpdateOTA" type="button" class="primary">⚡ Gravar Firmware</button></div>
      </div>
    </div>

    <!-- ----- Aba: Segurança ----- -->
    <div id="tabSec" class="admin-tab hidden">
      <div class="card stack">
        <h3>🔒 Alterar Senha de Admin</h3>
        <input id="aCurrPass" type="password" placeholder="Senha Atual" autocomplete="off">
        <input id="aPass" type="password" placeholder="Nova Senha" autocomplete="off">
        <div class="row"><button id="bPass" class="primary" type="button">Alterar Senha</button></div>
      </div>

      <div class="card stack">
        <h3>🌡️ Teto Máximo de Proteção Térmica (°C)</h3>
        <input id="aMaxTemp" type="number" value="75">
        <div class="row"><button id="bSetMaxTemp" class="primary" type="button">Salvar Limite Térmico</button></div>
      </div>

      <div class="card stack">
        <h3>📦 Backup de Configuração</h3>
        <p class="d">Exporte suas configurações salvas em arquivo JSON.</p>
        <div class="row l"><button id="bBackupExport" class="primary" type="button">📥 Baixar Configurações JSON</button></div>
      </div>

      <div class="card stack" style="border-color:var(--err)">
        <h3 style="color:var(--err)">⚠️ Ações do Sistema</h3>
        <div class="row l" style="gap:6px;">
          <button id="bReboot" class="ghost" style="color:var(--warn); border-color:var(--warn)" type="button">🔁 Reboot ESP32</button>
          <button id="bReset" class="ghost" style="color:var(--err); border-color:var(--err)" type="button">Reset de Fábrica</button>
        </div>
      </div>
    </div>

    <div id="aMsg" class="err"></div>
    <div class="row" style="margin-top:14px;">
      <button id="bBack" type="button">Voltar ao Chat</button>
      <button id="bOut" type="button">Sair</button>
    </div>
  </section>
</main>

<!-- ======================= MODAL DE LOGIN ======================= -->
<div id="modal" class="modal hidden"><div class="card stack">
  <h3 id="modalTitle">🔐 Autenticação Admin</h3>
  <input id="u" value="admin" autocomplete="off" autocapitalize="off">
  <input id="p" type="password" placeholder="Senha (padrão: 1234)" autocomplete="off">
  <div id="lErr" class="err"></div>
  <div class="row">
    <button id="lCancel" type="button">Cancelar</button>
    <button id="lOk" type="button" class="primary">Entrar</button>
  </div>
</div></div>

<!-- ======================= SCRIPT ======================= -->
<script>
/* ---------------------------------------------------------------
   ESTADO GLOBAL E UTILITÁRIOS
--------------------------------------------------------------- */
const $ = id => document.getElementById(id);
let token = sessionStorage.getItem("tk") || "", busy = false;
let autoTTS = false;
let adminInterval = null;
let lastSentText = "";

function vibrate(ms = 30) {
  if (navigator.vibrate) navigator.vibrate(ms);
}

function escHtml(s) {
  return String(s || "").replace(/[&<>\"']/g, c => ({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));
}

function show(v) {
  if (v !== "vAdmin" && adminInterval) {
    clearInterval(adminInterval);
    adminInterval = null;
  }
  ["vSetup", "vChat", "vHistory", "vAdmin"].forEach(x => {
    const el = $(x);
    if (el) el.classList.toggle("hidden", x !== v);
  });
  window.scrollTo(0, 0);
}

function req(path, body, auth, cb) {
  const o = {
    method: body ? "POST" : "GET",
    headers: {"Content-Type": "application/json"}
  };
  if (auth && token) o.headers["X-Token"] = token;
  if (body) o.body = JSON.stringify(body);

  fetch(path, o)
    .then(r => r.json().catch(() => ({})))
    .then(data => cb(null, data))
    .catch(err => cb(err, null));
}

function flash(ok, msg) {
  $("aMsg").className = ok ? "err ok" : "err";
  $("aMsg").textContent = msg;
}

/* ---------------------------------------------------------------
   TEMA
--------------------------------------------------------------- */
let currentTheme = localStorage.getItem("miniia_theme") || "dark";

function applyTheme(t) {
  document.body.classList.remove("theme-oled", "theme-light");
  if (t === "oled") document.body.classList.add("theme-oled");
  else if (t === "light") document.body.classList.add("theme-light");
  localStorage.setItem("miniia_theme", t);
  if (t === "dark") $("bTheme").textContent = "🌙 Dark";
  else if (t === "oled") $("bTheme").textContent = "🖤 OLED";
  else $("bTheme").textContent = "☀️ Light";
}
applyTheme(currentTheme);

$("bTheme").onclick = function() {
  if (currentTheme === "dark") currentTheme = "oled";
  else if (currentTheme === "oled") currentTheme = "light";
  else currentTheme = "dark";
  applyTheme(currentTheme);
};

/* ---------------------------------------------------------------
   SESSÕES DE CONVERSA (localStorage)
--------------------------------------------------------------- */
let sessions = [];
let currentSessionId = null;

try {
  const savedSessions = localStorage.getItem("miniia_sessions_v8");
  if (savedSessions) sessions = JSON.parse(savedSessions);
  cleanOldSessions();
} catch (e) {}

function cleanOldSessions() {
  const thirtyDays = 30 * 24 * 60 * 60 * 1000;
  const now = Date.now();
  sessions = sessions.filter(s => (now - (s.ts || now)) < thirtyDays);
  saveAllSessions();
}

function saveAllSessions() {
  try { localStorage.setItem("miniia_sessions_v8", JSON.stringify(sessions)); } catch (e) {}
}

function getCurrentSession() {
  if (!currentSessionId) return null;
  return sessions.find(s => s.id === currentSessionId);
}

function createNewSession() {
  currentSessionId = "sess_" + Date.now();
  const newSess = {
    id: currentSessionId,
    title: "Nova Conversa",
    ts: Date.now(),
    date: new Date().toLocaleString("pt-BR", {day: '2-digit', month: '2-digit', hour: '2-digit', minute: '2-digit'}),
    messages: []
  };
  sessions.unshift(newSess);
  saveAllSessions();
  restoreChatUI();
}

if (!sessions || sessions.length === 0) {
  createNewSession();
} else {
  currentSessionId = sessions[0].id;
}

function restoreChatUI() {
  $("log").innerHTML = "";
  const sess = getCurrentSession();
  if (!sess || !sess.messages || sess.messages.length === 0) {
    $("log").innerHTML = `
      <div class="empty" id="empty">
        <div class="big">🤖</div>
        <div>Como posso te ajudar hoje?</div>
        <div class="chips">
          <div class="chip" onclick="quickPrompt('Escreva um código em C++ para ler um sensor no ESP32.')">💻 Código C++ ESP32</div>
          <div class="chip" onclick="quickPrompt('Como calcular o resistor ideal para um LED?')">⚡ Resistor LED</div>
          <div class="chip" onclick="quickPrompt('Me conte uma piada rápida sobre programação.')">🔥 Piada Geek</div>
        </div>
      </div>`;
    return;
  }
  for (let i = 0; i < sess.messages.length; i++) {
    let item = sess.messages[i];
    let role = (item.r === "user") ? "me" : "ai";
    addMsgUI(role, item.c, item.t, item.pinned);
  }
}

function quickPrompt(txt) {
  $("q").value = txt;
  $("bSend").click();
}

$("chatSearch").oninput = function() {
  let query = this.value.toLowerCase();
  let msgs = document.querySelectorAll(".msg");
  msgs.forEach(m => {
    let txt = m.textContent.toLowerCase();
    m.style.display = txt.includes(query) ? "block" : "none";
  });
};

/* ---------------------------------------------------------------
   HISTÓRICO
--------------------------------------------------------------- */
function renderHistoryView() {
  const list = $("sessionsList");
  if (!sessions || sessions.length === 0) {
    list.innerHTML = '<p class="d">Nenhuma conversa salva no histórico.</p>';
    return;
  }
  let html = "";
  for (let i = 0; i < sessions.length; i++) {
    let s = sessions[i];
    let active = (s.id === currentSessionId) ? "border-color:var(--ac);" : "";
    html += `
      <div class="session-card" style="${active}" onclick="selectSession('${s.id}')">
        <div>
          <div style="font-weight:600; font-size:14px;">${escHtml(s.title)}</div>
          <div style="font-size:11px; color:var(--mut);">${s.date} • ${s.messages.length} msgs</div>
        </div>
        <div>
          <button class="mini" onclick="event.stopPropagation(); renameSession('${s.id}')">✏️</button>
          <button class="mini" style="color:var(--err)" onclick="event.stopPropagation(); deleteSession('${s.id}')">🗑️</button>
        </div>
      </div>`;
  }
  list.innerHTML = html;
}

function renameSession(id) {
  const s = sessions.find(x => x.id === id);
  if (!s) return;
  const newName = prompt("Digite o novo nome para esta conversa:", s.title);
  if (newName && newName.trim().length > 0) {
    s.title = newName.trim();
    saveAllSessions();
    renderHistoryView();
  }
}

function selectSession(id) {
  currentSessionId = id;
  restoreChatUI();
  show("vChat");
}

function deleteSession(id) {
  sessions = sessions.filter(s => s.id !== id);
  if (currentSessionId === id) {
    if (sessions.length > 0) currentSessionId = sessions[0].id;
    else createNewSession();
  }
  saveAllSessions();
  renderHistoryView();
}

/* ---------------------------------------------------------------
   ÁREA DE COPIAR / VOZ (TTS e reconhecimento)
--------------------------------------------------------------- */
function setCopyButtonState(btnEl, ok) {
  if (!btnEl) return;
  btnEl.textContent = ok ? "✓ Copiado!" : "❌ Erro ao copiar";
  if (ok) vibrate(20);
  setTimeout(() => {
    if (btnEl) btnEl.textContent = "📋 Copiar";
  }, 2000);
}

function copyToClipboard(text, btnEl) {
  const safeText = String(text || "").trim();
  if (!safeText) {
    setCopyButtonState(btnEl, false);
    return;
  }

  if (navigator.clipboard && window.isSecureContext) {
    navigator.clipboard.writeText(safeText)
      .then(() => setCopyButtonState(btnEl, true))
      .catch(() => fallbackCopy(safeText, btnEl));
  } else {
    fallbackCopy(safeText, btnEl);
  }
}

function fallbackCopy(text, btnEl) {
  const textArea = document.createElement("textarea");
  textArea.value = text;
  textArea.setAttribute("readonly", "");
  textArea.style.position = "fixed";
  textArea.style.left = "-9999px";
  document.body.appendChild(textArea);
  textArea.focus();
  textArea.select();
  try {
    const copied = document.execCommand('copy');
    setCopyButtonState(btnEl, copied);
  } catch (err) {
    setCopyButtonState(btnEl, false);
  }
  document.body.removeChild(textArea);
}

function speakText(text) {
  if (!('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  if (!text) return;
  let cleanText = text.replace(/[*_#|`~]/g, '').replace(/[-+]\s+/g, '').trim();
  const utter = new SpeechSynthesisUtterance(cleanText);
  utter.lang = 'pt-BR';
  window.speechSynthesis.speak(utter);
}

function stopSpeak() {
  if ('speechSynthesis' in window) window.speechSynthesis.cancel();
}

$("bTTS").onclick = function() {
  autoTTS = !autoTTS;
  this.textContent = autoTTS ? "🔊 On" : "🔊 Off";
  if (!autoTTS) stopSpeak();
};

if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
  const SpeechRec = window.SpeechRecognition || window.webkitSpeechRecognition;
  const rec = new SpeechRec();
  rec.lang = 'pt-BR';
  rec.continuous = false;
  rec.onstart = () => { $("bMic").style.color = "var(--ok)"; vibrate(40); };
  rec.onend = () => $("bMic").style.color = "var(--tx)";
  rec.onerror = () => { $("bMic").style.color = "var(--err)"; vibrate(100); };
  rec.onresult = (e) => { $("q").value = e.results[0][0].transcript; };
  $("bMic").onclick = () => { try { rec.start(); } catch (e) { rec.stop(); } };
}

document.addEventListener("keydown", function (e) {
  const isTextField = ['INPUT', 'TEXTAREA'].includes(document.activeElement?.tagName);
  if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 'c' && !isTextField) {
    const activeAI = document.querySelector('.msg.ai');
    if (activeAI) {
      e.preventDefault();
      copyToClipboard(activeAI.dataset.copyText || activeAI.textContent, null);
    }
  }
});

/* ---------------------------------------------------------------
   CHAT: MENSAGENS E ENVIO
--------------------------------------------------------------- */
$("q").addEventListener("keydown", function(e) {
  if ((e.ctrlKey || e.metaKey) && e.key === "Enter") {
    e.preventDefault();
    $("bSend").click();
  }
});

function addMsgUI(role, text, timeMs, isPinned = false) {
  const empty = $("empty");
  if (empty) empty.classList.add("hidden");
  const d = document.createElement("div");
  d.className = "msg " + role + (isPinned ? " pinned" : "");
  d.dataset.copyText = String(text || "");
  const b = document.createElement("div");
  b.textContent = text;
  d.appendChild(b);

  if (role === 'ai') {
    if (timeMs) {
      const tm = document.createElement("div");
      tm.className = "msg-time";
      let timeSec = (timeMs / 1000).toFixed(2);
      let color = timeMs < 2000 ? "var(--ok)" : (timeMs < 5000 ? "var(--warn)" : "var(--err)");
      let estTokens = Math.round(text.length / 4);
      let estCost = (estTokens * 0.0000002).toFixed(6);
      tm.innerHTML = "Respondido em <b style='color:" + color + "'>" + timeSec + "s</b> • ~" + estTokens + " tokens ($" + estCost + ")";
      d.appendChild(tm);
    }

    const act = document.createElement("div");
    act.className = "msg-actions";

    const btnPlay = document.createElement("button");
    btnPlay.type = "button"; btnPlay.className = "mini"; btnPlay.textContent = "🔊 Ouvir";
    btnPlay.onclick = () => speakText(b.textContent);

    const btnCopy = document.createElement("button");
    btnCopy.type = "button"; btnCopy.className = "mini"; btnCopy.textContent = "📋 Copiar";
    btnCopy.onclick = function() { copyToClipboard(b.textContent, this); };

    const btnPin = document.createElement("button");
    btnPin.type = "button"; btnPin.className = "mini"; btnPin.textContent = isPinned ? "📌 Desfixar" : "📌 Fixar";
    btnPin.onclick = function() {
      d.classList.toggle("pinned");
      let pinnedState = d.classList.contains("pinned");
      btnPin.textContent = pinnedState ? "📌 Desfixar" : "📌 Fixar";
      let sess = getCurrentSession();
      if (sess) {
        let msgObj = sess.messages.find(m => m.c === text);
        if (msgObj) msgObj.pinned = pinnedState;
        saveAllSessions();
      }
    };

    act.appendChild(btnPlay);
    act.appendChild(btnCopy);
    act.appendChild(btnPin);
    d.appendChild(act);
  }

  $("log").appendChild(d);
  d.scrollIntoView({behavior: "smooth", block: "end"});
  return {d, b};
}

$("bSend").onclick = function() {
  const q = $("q").value.trim();
  if (!q || busy) return;
  if (q === lastSentText) {
    if (!confirm("Você está enviando a mesma pergunta. Continuar?")) return;
  }
  if (q.length > 2000) {
    alert("⚠️ Pergunta muito longa! Reduza para no máximo 2000 caracteres.");
    return;
  }
  lastSentText = q;
  vibrate(20);
  busy = true; $("bSend").disabled = true; $("q").value = "";
  addMsgUI("me", q);
  const m = addMsgUI("ai", "");
  m.b.innerHTML = '<span class="dots"><span></span><span></span><span></span></span>';

  const startTime = Date.now();
  const mode = $("modePreset").value;
  const sess = getCurrentSession();

  if (sess.messages.length === 0) {
    sess.title = q.substring(0, 30) + (q.length > 30 ? "..." : "");
  }

  req("/api/ask", {q: q, h: sess.messages.slice(-6), mode: mode}, false, function(err, r) {
    const elapsed = Date.now() - startTime;
    busy = false; $("bSend").disabled = false;
    if (err || !r || r.error) {
      vibrate(100);
      m.b.textContent = "⚠️ " + ((r && r.error) ? r.error : "Erro de conexão com o ESP32");
      m.b.style.color = "var(--err)";
    } else {
      vibrate(40);
      m.b.textContent = r.a;
      m.d.dataset.copyText = String(r.a || "");

      const tm = document.createElement("div");
      tm.className = "msg-time";
      let timeSec = (elapsed / 1000).toFixed(2);
      let color = elapsed < 2000 ? "var(--ok)" : (elapsed < 5000 ? "var(--warn)" : "var(--err)");
      let estTokens = Math.round(r.a.length / 4);
      let estCost = (estTokens * 0.0000002).toFixed(6);
      tm.innerHTML = "Respondido em <b style='color:" + color + "'>" + timeSec + "s</b> • ~" + estTokens + " tokens ($" + estCost + ")";
      m.d.appendChild(tm);

      sess.messages.push({r: "user", c: q}, {r: "assistant", c: r.a, t: elapsed});
      saveAllSessions();
      if (autoTTS) speakText(r.a);
    }
  });
};

/* ... restante do script igual ao original ... */
</script>
</body>
</html>
)rawliteral";

// =============================================================================
//  6. FUNÇÃO PARA USAR A PÁGINA WEB
// =============================================================================
void servePage() {
  server.send(200, "text/html", PAGE);
}

// =============================================================================
//  7. SETUP DO SERVIDOR
// =============================================================================
void setup() {
  Serial.begin(115200);
  delay(500);

  WiFi.mode(WIFI_STA);
  WiFi.setSleep(false);

  // Carregar dados salvos ou entrar em AP mode
  prefs.begin("miniia", false);
  String savedSsid = prefs.getString("wifi_ssid", "");
  String savedPass = prefs.getString("wifi_pass", "");
  prefs.end();

  if (savedSsid.length() > 0 && savedPass.length() > 0) {
    WiFi.begin(savedSsid.c_str(), savedPass.c_str());
    Serial.println("Conectando à rede salva...");
    for (int i = 0; i < 30 && WiFi.status() != WL_CONNECTED; i++) {
      delay(500);
    }
  }

  if (WiFi.status() != WL_CONNECTED) {
    WiFi.mode(WIFI_AP);
    WiFi.softAP(AP_SSID, AP_PASS);
    apMode = true;
  }

  // Iniciar servidor HTTP / WebSocket / DNS
  server.on("/", servePage);
  server.begin();

  terminalServer.begin();
  Serial.println("Servidor pronto.");
}

void loop() {
  server.handleClient();
}
