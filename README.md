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


O **TuxAssist** é um assistente de Inteligência Artificial de alto desempenho desenvolvido para rodar nativamente no **Termux** (Android). Ele atua como um agente autônomo capaz de ler o sistema de arquivos, executar comandos locais e interagir com hardwares remotos (ESP32-S3) através do protocolo MCP (Model Context Protocol).

Diferente de wrappers de IA comuns, o TuxAssist possui "mãos e olhos" no seu sistema: ele pode compilar códigos, escanear redes, ler logs e interagir com componentes eletrônicos em tempo real sem sair da interface do terminal.

---

## ✨ Principais Funcionalidades

* 🤖 **Arquitetura Multi-IA (Hot-Swap):** Suporte nativo para **Gemini (Google)**, **Groq (Llama 3/Mixtral)** e **OpenAI (ChatGPT)**. Alternância instantânea de modelos e provedores durante a conversa sem perder o contexto (comando `/mu`).
* 🛠️ **Integração MCP (Model Context Protocol):** Separação segura entre o cérebro da IA (Cliente) e as ferramentas do sistema (Servidor).
* 📂 **Visão Raio-X de Diretórios:** Escaneamento profundo e recursivo de pastas (ignorando diretórios ocultos e de cache para economia de tokens).
* 💻 **Execução CLI Nativa:** Permissão para executar scripts Bash, Python, Nmap e comandos do sistema operacional diretamente no host.
* 📡 **Integração IoT (ESP32-S3 via TCP Raw):** Comunicação via Socket TCP operando no **Core 0** do ESP32. Permite controle de GPIO, leitura de RAM e sensores em tempo real sem interromper o *main loop* do microcontrolador.
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
   Firmware em C++ que roda uma *Task* isolada (FreeRTOS) no Núcleo 0, ouvindo a porta 2222 para receber injeção de comandos crus (`write()`) do Termux via rede Wi-Fi local.

---

## ⚙️ Pré-requisitos

Para rodar este projeto, você precisará de:
* Aplicativo **Termux** atualizado (via F-Droid).
* **Python 3** instalado.
* Acesso à mesma rede Wi-Fi que o seu dispositivo ESP32.

### Bibliotecas Python:
```bash
python -m pip install google-genai rich


​📦 1. Dependências do Projeto Completo (ESP32 + MCP Termux)
​Para rodar a versão completa com controle de hardware, o usuário vai precisar preparar os dois ambientes (Celular/PC e a Placa).

​🐍 Ambiente Python (Termux / Linux / Windows)
​Para o cérebro rodar, o usuário só precisa do Python 3 instalado e baixar 3 pacotes.
Comando de instalação:

python -m pip install google-genai rich mcp


​google-genai: A biblioteca oficial mais atualizada do Google para conversar com o Gemini.
​rich: Responsável por deixar o terminal bonito, renderizando cores e Markdown (negrito, blocos de código) direto na tela preta.
​mcp: O SDK oficial do Model Context Protocol para criar o servidor de ferramentas.
​(Nota: O Groq e a OpenAI não precisam de bibliotecas externas neste projeto, usamos o urllib nativo do Python para evitar o travamento do pacote jiter no Android).


​⚡ Ambiente Arduino (C++ para o ESP32)

​A grande sacada do código ESP32 S3 é que NÃO
É PRECISO BAIXAR NENHUMA BIBLIOTECA EXTERNA (ZIP)
 no Arduino IDE ou ArduinoDroid! Escrevemos tudo usando o núcleo padrão da placa.

​O que o usuário deve fazer:
​Instalar o pacote de placas do ESP32 na IDE.
​URL do Gerenciador de Placas: [https://dl.espressif.com/dl/package_esp32_index.json]
(https://dl.espressif.com/dl/package_esp32_index.json)
​Selecionar a placa ESP32-S3 Dev Module (ou o modelo correspondente).

Bibliotecas Nativas Utilizadas (Já embutidas no pacote ESP32):
​<WiFi.h> (Conexão de rede)
​<WebServer.h> (Painel Web UI e API)
​<DNSServer.h> (Captive Portal)
​<HTTPClient.h> e <WiFiClientSecure.h> (Comunicação com a API do Groq/OpenAI)
​<Preferences.h> (Salvar configurações e senhas na memória Flash)
​<ESPmDNS.h> (Acesso via navegador por miniia.local)
​<Update.h> (Atualização OTA pelo navegador)
​Nota: O JSON é processado por uma função customizada no próprio código,
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
button{font:inherit;border:1px solid var(--line);background:var(--card2);color:var(--tx);padding:9px 14px;border-radius:10px;cursor:pointer;font-weight:600;font-size:13px;display:inline-flex;align-items:center;justify-content:center;gap:6px}
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
        <input id="wifiPass" type="password" placeholder="Senha do Wi-Fi">
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
  return String(s || "").replace(/[&<>"']/g, c => ({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));
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
function copyToClipboard(text, btnEl) {
  if (navigator.clipboard && window.isSecureContext) {
    navigator.clipboard.writeText(text).then(() => {
      btnEl.textContent = "✓ Copiado!";
      vibrate(20);
      setTimeout(() => btnEl.textContent = "📋 Copiar", 2000);
    }).catch(() => fallbackCopy(text, btnEl));
  } else {
    fallbackCopy(text, btnEl);
  }
}

function fallbackCopy(text, btnEl) {
  const textArea = document.createElement("textarea");
  textArea.value = text;
  textArea.style.position = "fixed";
  textArea.style.left = "-9999px";
  document.body.appendChild(textArea);
  textArea.focus();
  textArea.select();
  try {
    document.execCommand('copy');
    btnEl.textContent = "✓ Copiado!";
    vibrate(20);
    setTimeout(() => btnEl.textContent = "📋 Copiar", 2000);
  } catch (err) {
    btnEl.textContent = "❌ Erro ao copiar";
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

$("bClearChatUI").onclick = function() {
  const sess = getCurrentSession();
  if (sess) {
    sess.messages = [];
    saveAllSessions();
    restoreChatUI();
  }
};

$("bExport").onclick = function() {
  const sess = getCurrentSession();
  if (!sess || sess.messages.length === 0) { alert("Nenhuma conversa para exportar"); return; }
  let txt = "CONVERSA MINI IA ESP32 - " + sess.title + "\nData: " + sess.date + "\n" + "=".repeat(40) + "\n\n";
  sess.messages.forEach(m => {
    txt += (m.r === "user" ? "VOCÊ: " : "IA: ") + m.c + "\n\n";
  });
  const blob = new Blob([txt], {type: "text/plain;charset=utf-8"});
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = "chat_esp32_" + Date.now() + ".txt";
  a.click();
};

$("bNewChat").onclick = function() { createNewSession(); };
$("bHist").onclick = function() { renderHistoryView(); show("vHistory"); };
$("bBackChat").onclick = function() { restoreChatUI(); show("vChat"); };

$("bClearAllSessions").onclick = function() {
  if (confirm("Tem certeza que deseja apagar todo o histórico de conversas?")) {
    sessions = [];
    localStorage.removeItem("miniia_sessions_v8");
    createNewSession();
    renderHistoryView();
    show("vChat");
  }
};

/* ---------------------------------------------------------------
   SETUP INICIAL (API KEY)
--------------------------------------------------------------- */
$("sSave").onclick = function() {
  const val = $("sKey").value.trim();
  $("sErr").textContent = "";
  if (!val) { show("vChat"); return; }
  req("/api/setup", {key: val}, false, function(err, r) {
    $("sKey").value = "";
    show("vChat");
  });
};

$("bSkipSetup").onclick = function() { restoreChatUI(); show("vChat"); };

/* ---------------------------------------------------------------
   ADMIN: LOGIN / NAVEGAÇÃO
--------------------------------------------------------------- */
function switchAdminTab(btn, tabId) {
  document.querySelectorAll('.admin-tab').forEach(el => el.classList.add('hidden'));
  document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
  $(tabId).classList.remove('hidden');
  if (btn) btn.classList.add('active');
}

$("bAdmin").onclick = function() {
  if (token) {
    startAdminRealtime();
  } else {
    $("lErr").textContent = ""; $("p").value = ""; $("modal").classList.remove("hidden");
  }
};

$("lCancel").onclick = function() { $("modal").classList.add("hidden"); };

$("lOk").onclick = function() {
  $("lErr").textContent = "";
  req("/api/login", {user: $("u").value.trim(), pass: $("p").value}, false, function(err, r) {
    if (r && r.token) {
      token = r.token;
      sessionStorage.setItem("tk", token);
      $("modal").classList.add("hidden");
      startAdminRealtime();
    } else {
      $("lErr").textContent = (r && r.error) ? r.error : "Acesso negado";
    }
  });
};

$("bBack").onclick = function() { restoreChatUI(); show("vChat"); };

$("bOut").onclick = function() {
  token = "";
  sessionStorage.removeItem("tk");
  if (adminInterval) clearInterval(adminInterval);
  restoreChatUI();
  show("vChat");
};

/* ---------------------------------------------------------------
   ADMIN: DASHBOARD EM TEMPO REAL
--------------------------------------------------------------- */
function formatUptime(ms) {
  let sec = Math.floor(ms / 1000);
  let min = Math.floor(sec / 60); sec %= 60;
  let hr = Math.floor(min / 60); min %= 60;
  let day = Math.floor(hr / 24); hr %= 24;
  return (day > 0 ? day + "d " : "") + hr + "h " + min + "m " + sec + "s";
}

function fetchAdminData() {
  req("/api/admin/info", null, true, function(err, i) {
    if (i && !i.error) {
      let tempVal = parseFloat(i.temp);
      let tempColor = "var(--ok)";
      if (!isNaN(tempVal) && tempVal > 70) tempColor = "var(--err)";
      else if (!isNaN(tempVal) && tempVal > 60) tempColor = "var(--warn)";

      let rssiVal = parseInt(i.rssi);
      let rssiQuality = rssiVal > -55 ? "Excelente 📶" : (rssiVal > -70 ? "Bom 📶" : "Fraco");
      let usedHeap = i.total_heap - i.heap;
      let usedPct = Math.round((usedHeap / i.total_heap) * 100);

      const rows = [
        ["IP Local / mDNS", i.ip + " (" + i.mdns + ")"],
        ["Frequência da CPU", i.cpu_freq + " MHz"],
        ["Wi-Fi Conectado", (i.ssid || "-") + " (" + rssiQuality + ")"],
        ["Sinal (RSSI)", i.rssi + " dBm"],
        ["Uptime ESP32", formatUptime(i.uptime || 0)],
        ["Temperatura Atual / Máx", '<b style="color:' + tempColor + '">' + i.temp + ' °C / ' + i.max_temp + ' °C (Limite: ' + i.max_limit + '°C)</b>'],
        ["RAM Livre / Maior Bloco", Math.round(i.heap / 1024) + " KB / " + Math.round(i.max_block / 1024) + " KB"],
        ["API Key", i.key || "—"],
        ["Modelo IA / Temp.", i.model + " (T=" + i.ai_temp + ")"],
        ["Pino GPIO 2 Estado", i.gpio2 ? "🟢 HIGH (LIGADO)" : "🔴 LOW (DESLIGADO)"]
      ];

      let html = rows.map(r => '<div class="kv"><span>' + r[0] + '</span><span>' + r[1] + '</span></div>').join("");
      html += '<div style="margin-top:8px"><span style="font-size:12px; color:var(--mut)">Carga de Memória RAM:</span><div class="progress-bar"><div class="progress-fill" style="width:' + usedPct + '%"></div></div></div>';

      $("info").innerHTML = html;
      $("sysTerm").textContent = i.logs || "Sem eventos.";
      $("gpioStatus").textContent = "Estado atual do Pino 2: " + (i.gpio2 ? "🟢 HIGH (LIGADO)" : "🔴 LOW (DESLIGADO)");

      if (!document.activeElement || document.activeElement.id !== "aModel") $("aModel").value = i.model || "";
      if (!document.activeElement || document.activeElement.id !== "aSysPrompt") $("aSysPrompt").value = i.sys_prompt || "";
    } else {
      $("info").innerHTML = "Sessão expirada ou acesso negado.";
    }
  });
}

function startAdminRealtime() {
  show("vAdmin");
  fetchAdminData();
  if (adminInterval) clearInterval(adminInterval);
  adminInterval = setInterval(fetchAdminData, 2000);
}

$("bFlushRAM").onclick = function() {
  req("/api/admin/flush", {}, true, function(err, r) {
    alert("Memória RAM desfragmentada e limpa!");
    fetchAdminData();
  });
};

/* ---------------------------------------------------------------
   ADMIN: HARDWARE E CLI
--------------------------------------------------------------- */
$("bGpioHigh").onclick = function() { req("/api/admin/gpio", {state: 1}, true, () => fetchAdminData()); };
$("bGpioLow").onclick = function() { req("/api/admin/gpio", {state: 0}, true, () => fetchAdminData()); };

function runCliShortcut(cmdText) {
  $("cliCmd").value = cmdText;
  $("bRunCli").click();
}

$("bRunCli").onclick = function() {
  const cmd = $("cliCmd").value.trim();
  if (!cmd) return;
  req("/api/admin/cli", {cmd: cmd}, true, function(err, r) {
    if (r && r.res) alert("Terminal ESP32:\n" + r.res);
    $("cliCmd").value = "";
    fetchAdminData();
  });
};

/* ---------------------------------------------------------------
   ADMIN: REDE (Wi-Fi, IP estático, scan)
--------------------------------------------------------------- */
$("bSaveWifi").onclick = function() {
  const s = $("wifiSsid").value.trim(), p = $("wifiPass").value;
  if (!s) { flash(false, "Insira o SSID da rede"); return; }
  req("/api/admin/wifi", {ssid: s, pass: p}, true, function(err, r) {
    flash(true, "Wi-Fi salvo! O ESP32 vai conectar na sua rede e sair do modo AP.");
  });
};

$("bSaveStaticIP").onclick = function() {
  const ip = $("stIP").value.trim();
  const gw = $("stGW").value.trim();
  const mask = $("stMask").value.trim();
  req("/api/admin/static_ip", {ip: ip, gw: gw, mask: mask}, true, function(err, r) {
    if (r && r.ok) alert("IP Estático configurado. Reinicie o ESP32 para aplicar.");
    else alert("❌ " + ((r && r.error) ? r.error : "Erro ao salvar IP estático"));
  });
};

function pollScan() {
  req("/api/scan", null, true, function(err, r) {
    if (r && r.status === "busy") {
      setTimeout(pollScan, 1000);
    } else if (r && r.nets) {
      $("bScan").disabled = false;
      let html = "";
      for (let i = 0; i < r.nets.length; i++) {
        let n = r.nets[i];
        html += '<div class="net" data-ssid="' + escHtml(n.ssid || "") + '"><b>' + (n.lock ? "🔒 " : "🔓 ") + escHtml(n.ssid || "(Oculta)") + '</b><span>' + n.rssi + ' dBm</span></div>';
      }
      $("nets").innerHTML = html;
      const list = document.querySelectorAll(".net");
      for (let j = 0; j < list.length; j++) {
        list[j].onclick = function() {
          $("wifiSsid").value = this.getAttribute("data-ssid");
          $("wifiPass").focus();
        };
      }
    }
  });
}

$("bScan").onclick = function() {
  $("bScan").disabled = true;
  $("nets").innerHTML = '<p class="d">Escaneando redes...</p>';
  req("/api/scan_start", null, true, function(err, r) { pollScan(); });
};

/* ---------------------------------------------------------------
   ADMIN: IA & GROQ
--------------------------------------------------------------- */
$("bKey").onclick = function() {
  req("/api/admin/key", {
    key: $("aKey").value.trim(),
    model: $("aModel").value.trim(),
    temp: $("aTemp").value,
    tokens: $("aTokens").value,
    timeout: $("aTimeout").value,
    sys_prompt: $("aSysPrompt").value.trim()
  }, true, function(err, r) {
    $("aKey").value = ""; fetchAdminData(); flash(true, "Ajustes salvos com sucesso!");
  });
};

/* ---------------------------------------------------------------
   ADMIN: FERRAMENTAS (DNS, CPU, Deep Sleep, OTA)
--------------------------------------------------------------- */
$("bDoPing").onclick = function() {
  const target = $("pingHost").value.trim();
  if (!target) return alert("Digite um host para testar");
  $("pingRes").textContent = "Testando resolução DNS do host...";
  req("/api/admin/ping", {host: target}, true, function(err, r) {
    if (r && r.ok) $("pingRes").textContent = "✓ Sucesso! O ESP32 resolveu o IP: " + r.ip;
    else $("pingRes").textContent = "❌ Falha no teste: Não foi possível resolver o host.";
  });
};

$("bSetCpu").onclick = function() {
  const freq = $("cpuFreqSelect").value;
  req("/api/admin/cpu", {freq: freq}, true, function(err, r) {
    if (r && r.ok) alert("Clock do processador alterado para " + freq + " MHz");
    else alert("❌ " + ((r && r.error) ? r.error : "Erro ao alterar o clock"));
    fetchAdminData();
  });
};

$("bDeepSleep").onclick = function() {
  const sec = parseInt($("sleepSec").value);
  if (!sec || sec <= 0) return alert("Digite um tempo válido em segundos");
  if (confirm("Deseja colocar o ESP32 em Deep Sleep por " + sec + " segundos?")) {
    req("/api/admin/deepsleep", {sec: sec}, true, function() { alert("ESP32 entrando em Deep Sleep..."); });
  }
};

$("bUpdateOTA").onclick = function() {
  const fileInput = $("otaFile");
  if (!fileInput.files.length) { alert("Selecione o arquivo .bin compilado primeiro"); return; }
  const file = fileInput.files[0];
  const formData = new FormData();
  formData.append("update", file);

  $("otaProgress").textContent = "⚡ Gravando firmware no ESP32...";
  $("bUpdateOTA").disabled = true;

  fetch("/api/admin/update", {
    method: "POST",
    headers: {"X-Token": token},
    body: formData
  })
  .then(r => r.text())
  .then(txt => {
    $("otaProgress").textContent = " Sucesso! ESP32 reiniciando...";
    setTimeout(() => location.reload(), 6000);
  });
};

/* ---------------------------------------------------------------
   ADMIN: SEGURANÇA E SISTEMA
--------------------------------------------------------------- */
$("bPass").onclick = function() {
  const currP = $("aCurrPass").value;
  const newP = $("aPass").value;
  if (!currP || !newP) { flash(false, "Preencha a senha atual e a nova senha"); return; }
  req("/api/admin/pass", {curr_pass: currP, new_pass: newP}, true, function(err, r) {
    if (r && r.ok) {
      $("aCurrPass").value = ""; $("aPass").value = "";
      flash(true, "Senha do Admin alterada com sucesso!");
    } else {
      flash(false, (r && r.error) ? r.error : "Erro ao alterar senha");
    }
  });
};

$("bSetMaxTemp").onclick = function() {
  const val = $("aMaxTemp").value;
  req("/api/admin/maxtemp", {temp: val}, true, function(err, r) {
    if (r && r.ok) alert("Limite térmico salvo!");
    else alert("❌ " + ((r && r.error) ? r.error : "Erro ao salvar limite"));
  });
};

$("bBackupExport").onclick = function() {
  req("/api/admin/backup", null, true, function(err, r) {
    if (r) {
      const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(r));
      const dlAnchor = document.createElement('a');
      dlAnchor.setAttribute("href", dataStr);
      dlAnchor.setAttribute("download", "esp32_backup_config.json");
      document.body.appendChild(dlAnchor);
      dlAnchor.click();
      dlAnchor.remove();
    }
  });
};

$("bReboot").onclick = function() {
  if (confirm("Reiniciar o ESP32 agora?")) {
    req("/api/admin/reboot", {}, true, function() {
      alert("ESP32 Reiniciando...");
      setTimeout(() => location.reload(), 5000);
    });
  }
};

$("bReset").onclick = function() {
  const confirmPass = prompt("⚠️ ZONA DE PERIGO!\nDigite a senha do Admin para confirmar o RESET DE FÁBRICA:");
  if (confirmPass !== null) {
    if (!confirmPass) { alert("Senha não informada!"); return; }
    req("/api/admin/reset", {pass: confirmPass}, true, function(err, r) {
      if (r && r.ok) {
        alert("ESP32 Resetado com sucesso!");
        location.reload();
      } else {
        alert("❌ Senha incorreta! Reset cancelado.");
      }
    });
  }
};

/* ---------------------------------------------------------------
   INICIALIZAÇÃO
--------------------------------------------------------------- */
req("/api/status", null, false, function(err, s) {
  restoreChatUI();
  if (s && s.configured) show("vChat");
  else show("vSetup");
});
</script>
</body>
</html>
)rawliteral";

// =============================================================================
//  6. UTILITÁRIOS DE JSON / UTF-8
// =============================================================================
uint32_t hexToInt(const String& s) {
  uint32_t val = 0;
  for (unsigned int k = 0; k < s.length(); k++) {
    char c = s[k];
    val <<= 4;
    if (c >= '0' && c <= '9') val |= (c - '0');
    else if (c >= 'a' && c <= 'f') val |= (c - 'a' + 10);
    else if (c >= 'A' && c <= 'F') val |= (c - 'A' + 10);
  }
  return val;
}

void appendUtf8(String& out, uint32_t cp) {
  if (cp < 0x80) {
    out += (char)cp;
  } else if (cp < 0x800) {
    out += (char)(0xC0 | (cp >> 6));
    out += (char)(0x80 | (cp & 0x3F));
  } else if (cp < 0x10000) {
    out += (char)(0xE0 | (cp >> 12));
    out += (char)(0x80 | ((cp >> 6) & 0x3F));
    out += (char)(0x80 | (cp & 0x3F));
  } else {
    out += (char)(0xF0 | (cp >> 18));
    out += (char)(0x80 | ((cp >> 12) & 0x3F));
    out += (char)(0x80 | ((cp >> 6) & 0x3F));
    out += (char)(0x80 | (cp & 0x3F));
  }
}

String readStr(const String& s, int& i) {
  String out;
  int len = s.length();
  for (i++; i < len; i++) {
    char c = s[i];
    if (c == '"') { i++; return out; }
    if (c == '\\') {
      i++;
      if (i >= len) break;
      char e = s[i];
      switch (e) {
        case 'n': out += '\n'; break;
        case 'r': out += '\r'; break;
        case 't': out += '\t'; break;
        case 'b': out += '\b'; break;
        case 'f': out += '\f'; break;
        case 'u': {
          if (i + 4 < len) {
            String hexStr = s.substring(i + 1, i + 5);
            uint32_t cp = hexToInt(hexStr);
            i += 4;
            if (cp >= 0xD800 && cp <= 0xDBFF && i + 6 < len && s[i + 1] == '\\' && s[i + 2] == 'u') {
              String loStr = s.substring(i + 3, i + 7);
              uint32_t lo = hexToInt(loStr);
              if (lo >= 0xDC00 && lo <= 0xDFFF) {
                cp = 0x10000 + ((cp - 0xD800) << 10) + (lo - 0xDC00);
                i += 6;
              }
            }
            appendUtf8(out, cp);
          }
          break;
        }
        default: out += e; break;
      }
    } else {
      out += c;
    }
  }
  return out;
}

String jsonGet(const String& b, const char* key) {
  String pat = String("\"") + key + "\"";
  int p = b.indexOf(pat);
  if (p < 0) return "";
  int i = p + pat.length();
  int len = b.length();
  while (i < len && (b[i] == ' ' || b[i] == ':')) i++;
  if (i >= len) return "";
  if (b[i] == '"') return readStr(b, i);

  // Valor sem aspas (numero/booleano): le ate virgula, chave ou espaco
  String num = "";
  while (i < len && b[i] != ',' && b[i] != '}' && b[i] != ' ' && b[i] != '\n' && b[i] != '\r') {
    num += b[i];
    i++;
  }
  return num;
}

String jsonEscape(const String& s) {
  String o;
  o.reserve(s.length() + 16);
  for (unsigned int k = 0; k < s.length(); k++) {
    unsigned char c = (unsigned char)s[k];
    if (c == '"') o += "\\\"";
    else if (c == '\\') o += "\\\\";
    else if (c == '\n') o += "\\n";
    else if (c == '\r') o += "\\r";
    else if (c == '\t') o += "\\t";
    else if (c < 0x20) {
      char buf[8];
      snprintf(buf, sizeof(buf), "\\u%04x", c);
      o += buf;
    } else o += (char)c;
  }
  return o;
}

String buildHistory(const String& body) {
  String res = "";
  int p = body.indexOf("\"h\"");
  if (p < 0) return res;
  int i = body.indexOf('[', p);
  if (i < 0) return res;
  i++;
  int len = body.length();
  int count = 0;
  while (i < len && count < 8) {
    char c = body[i];
    if (c == ']') break;
    if (c == '{') {
      i++;
      String r = "", cc = "";
      while (i < len && body[i] != '}') {
        if (body[i] == '"') {
          String k = readStr(body, i);
          while (i < len && body[i] != ':') i++;
          i++;
          while (i < len && body[i] == ' ') i++;
          if (i < len && body[i] == '"') {
            String v = readStr(body, i);
            if (k == "r") r = v;
            else if (k == "c") cc = v;
          }
        } else i++;
      }
      i++;
      if ((r == "user" || r == "assistant") && cc.length() > 0) {
        if (cc.length() > 800) cc = cc.substring(0, 800);
        res += "{\"role\":\"" + r + "\",\"content\":\"" + jsonEscape(cc) + "\"},";
        count++;
      }
    } else i++;
  }
  return res;
}

// =============================================================================
//  7. HELPERS HTTP / AUTENTICAÇÃO / PREFERÊNCIAS
// =============================================================================
void sendJson(int code, const String& json) {
  server.sendHeader("Cache-Control", "no-store, no-cache, must-revalidate");
  server.sendHeader("X-Content-Type-Options", "nosniff");
  server.sendHeader("X-Frame-Options", "DENY");
  server.send(code, "application/json; charset=utf-8", json);
}

void sendErr(int code, const String& m) {
  sendJson(code, "{\"error\":\"" + jsonEscape(m) + "\"}");
}

void sendOk() {
  sendJson(200, "{\"ok\":true}");
}

bool authed() {
  return token.length() > 0 && server.header("X-Token") == token;
}

String getModel() {
  return prefs.getString("model", DEFAULT_MODEL);
}

String getAdminPass() {
  return prefs.getString("apass", "1234");
}

String maskKey(const String& k) {
  if (k.length() < 10) return "Não Configurada";
  return k.substring(0, 4) + "****" + k.substring(k.length() - 4);
}

// =============================================================================
//  8. INTEGRAÇÃO COM A GROQ
// =============================================================================
bool callGroq(const String& key, const String& model, const String& sys, const String& histJson,
              const String& user, int maxTokens, String& out, int& httpCode) {
  httpCode = 0;
  if (key.length() < 10) { out = "API key nao configurada no ESP32"; return false; }
  if (WiFi.status() != WL_CONNECTED) { out = "ESP32 sem internet (Wi-Fi desconectado)"; return false; }

  if (temperatureRead() > maxTempLimit) {
    out = "Aviso: O ESP32 superaqueceu acima do limite de segurança. Aguarde o resfriamento.";
    return false;
  }

  String customPrompt = prefs.getString("sys_prompt", "");
  String finalSys = sys;
  if (customPrompt.length() > 0) {
    finalSys += "\nInstrução Adicional do Usuario: " + customPrompt;
  }

  String body = "{\"model\":\"" + jsonEscape(model) + "\",\"max_tokens\":" + String(aiMaxTokens) +
                ",\"temperature\":" + String(aiTemperature, 1) + ",\"messages\":[{\"role\":\"system\",\"content\":\"" + jsonEscape(finalSys) + "\"}," +
                histJson + "{\"role\":\"user\",\"content\":\"" + jsonEscape(user) + "\"}]}";

  HTTPClient http;
  http.setTimeout(httpTimeoutMs);
  if (!http.begin(GROQ_URL)) { out = "Falha ao conectar na Groq"; return false; }
  http.addHeader("Content-Type", "application/json");
  http.addHeader("Authorization", "Bearer " + key);
  httpCode = http.POST(body);
  String resp = (httpCode > 0) ? http.getString() : "";
  http.end();

  if (httpCode <= 0) { out = "Erro de conexao: timeout ou falha na rede"; return false; }
  if (httpCode != 200) {
    String em = jsonGet(resp, "message");
    out = em.length() > 0 ? em : ("Erro HTTP " + String(httpCode));
    return false;
  }

  int targetPos = resp.indexOf("\"content\":");
  if (targetPos >= 0) {
    int startIdx = resp.indexOf("\"", targetPos + 9);
    if (startIdx >= 0) out = readStr(resp, startIdx);
  }
  if (out.length() == 0) out = jsonGet(resp, "content");
  out.trim();
  return true;
}

// =============================================================================
//  9. COMANDOS CLI (compartilhado entre Web UI e Terminal TCP)
// =============================================================================
String executarComandoCLI(String cmd) {
  cmd.trim(); cmd.toLowerCase();
  String res = "";
  if (cmd == "gpio 2 high" || cmd == "led on") { digitalWrite(CONTROL_PIN, HIGH); res = "GPIO 2 ativado"; }
  else if (cmd == "gpio 2 low" || cmd == "led off") { digitalWrite(CONTROL_PIN, LOW); res = "GPIO 2 desativado"; }
  else if (cmd == "heap" || cmd == "ram") { res = "Heap Livre: " + String(ESP.getFreeHeap() / 1024) + " KB"; }
  else if (cmd == "temp") { res = "Temperatura: " + String(temperatureRead(), 1) + " C"; }
  else if (cmd == "clearlog" || cmd == "clear") { sysLogs = ""; res = "Logs limpos"; }
  else if (cmd == "reboot") { delay(500); ESP.restart(); return "Reiniciando..."; }
  else { res = "Comando desconhecido: " + cmd; }
  return res;
}

// =============================================================================
//  10. SERVIDOR TCP (TuxAssist) - TASK FREERTOS
// =============================================================================
void taskTerminalServer(void *pvParameters) {
  terminalServer.begin();
  for (;;) {
    WiFiClient client = terminalServer.available();
    if (client) {
      // Usa String normal, mas injeta via write() (envio de bytes crus imune a erros do ArduinoDroid)
      String msgWelcome = "TuxAssist Socket Server - ESP32-C3\n";
      client.write((const uint8_t*)msgWelcome.c_str(), msgWelcome.length());

      String currentLine = "";
      while (client.connected()) {
        if (client.available()) {
          char c = client.read();
          if (c == '\n') {
            if (currentLine.length() > 0) {
              String resposta = executarComandoCLI(currentLine);
              resposta += "\n";

              client.write((const uint8_t*)resposta.c_str(), resposta.length());

              currentLine = "";
            }
          } else if (c != '\r') {
            currentLine += c;
          }
        }
        vTaskDelay(10 / portTICK_PERIOD_MS); // Evita watchdog travar a CPU
      }
      client.stop();
    }
    vTaskDelay(50 / portTICK_PERIOD_MS); // Respira a CPU
  }
}

// =============================================================================
//  11. HANDLERS: PÁGINA, STATUS, SETUP E CHAT
// =============================================================================
void handleRoot() {
  server.send_P(200, "text/html", PAGE);
}

void handleStatus() {
  bool ok = prefs.getString("gkey", "").length() > 10;
  sendJson(200, ok ? "{\"configured\":true}" : "{\"configured\":false}");
}

void handleSetup() {
  String key = jsonGet(server.arg("plain"), "key");
  key.trim();
  if (key.length() < 10) { sendErr(400, "API key invalida/curta"); return; }
  prefs.putString("gkey", key);
  addLog("API Key reconfigurada.");
  sendOk();
}

void handleAsk() {
  if (millis() - lastAskTime < CHAT_COOLDOWN) {
    sendErr(429, "Aguarde um segundo antes de enviar outra pergunta.");
    return;
  }
  lastAskTime = millis();

  String body = server.arg("plain");
  String q = jsonGet(body, "q");
  String mode = jsonGet(body, "mode");
  q.trim();

  if (q.length() == 0) { sendErr(400, "Pergunta vazia"); return; }
  if (q.length() > MAX_INPUT_LEN) q = q.substring(0, MAX_INPUT_LEN);

  String sys = "Voce e a Mini IA rodando no ESP32. "
               "REGRAS DE RESPOSTA OBRIGATORIAS:\n"
               "1. Responda em texto simples, limpo e direto.\n"
               "2. NUNCA use tabelas em Markdown (nao use o caractere |).\n"
               "3. NUNCA use marcadores de negrito ou italico (nao use ** ou * ou _).\n"
               "4. NUNCA use hashtags (#) para titulos.\n"
               "5. Use apenas letras, numeros, pontuacao comum e quebras de linha para organizar o texto.";

  String out; int code;
  if (!callGroq(prefs.getString("gkey", ""), getModel(), sys, buildHistory(body), q, aiMaxTokens, out, code)) {
    sendErr(code == 429 ? 429 : 502, out);
    return;
  }
  sendJson(200, "{\"a\":\"" + jsonEscape(out) + "\"}");
}

// =============================================================================
//  12. HANDLERS: LOGIN E INFORMAÇÕES DO SISTEMA
// =============================================================================
void handleLogin() {
  if (failedLogins >= MAX_LOGIN_ATTEMPTS) {
    if (millis() - lockoutStart < LOCKOUT_TIME) {
      unsigned long timeLeft = (LOCKOUT_TIME - (millis() - lockoutStart)) / 1000;
      sendErr(429, "Bloqueado por seguranca! Tente novamente em " + String(timeLeft) + "s");
      return;
    } else {
      failedLogins = 0;
    }
  }

  String body = server.arg("plain");
  String u = jsonGet(body, "user");
  String p = jsonGet(body, "pass");

  if (u == "admin" && p == getAdminPass()) {
    failedLogins = 0;
    token = String((uint32_t)esp_random(), HEX) + String((uint32_t)esp_random(), HEX);
    sendJson(200, "{\"token\":\"" + token + "\"}");
  } else {
    failedLogins++;
    if (failedLogins >= MAX_LOGIN_ATTEMPTS) lockoutStart = millis();
    delay(400);
    sendErr(401, "Usuario ou senha incorretos");
  }
}

void handleInfo() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String ip = apMode ? WiFi.softAPIP().toString() : WiFi.localIP().toString();
  String ssid = apMode ? (String(AP_SSID) + " (AP)") : WiFi.SSID();
  int rssi = apMode ? 0 : WiFi.RSSI();

  float tempC = temperatureRead();
  if (tempC > maxObservedTemp) maxObservedTemp = tempC;
  bool gpio2State = digitalRead(CONTROL_PIN);

  String j = "{\"ip\":\"" + ip + "\",\"ssid\":\"" + jsonEscape(ssid) + "\",\"rssi\":" + String(rssi) +
             ",\"temp\":" + String(tempC, 1) +
             ",\"max_temp\":" + String(maxObservedTemp, 1) +
             ",\"max_limit\":" + String(maxTempLimit, 1) +
             ",\"cpu_freq\":" + String(getCpuFrequencyMhz()) +
             ",\"mdns\":\"http://miniia.local\"" +
             ",\"uptime\":" + String(millis()) +
             ",\"ai_temp\":" + String(aiTemperature, 1) +
             ",\"ai_tokens\":" + String(aiMaxTokens) +
             ",\"gpio2\":" + String(gpio2State ? "true" : "false") +
             ",\"sys_prompt\":\"" + jsonEscape(prefs.getString("sys_prompt", "")) + "\"" +
             ",\"key\":\"" + jsonEscape(maskKey(prefs.getString("gkey", ""))) + "\",\"model\":\"" + jsonEscape(getModel()) +
             "\",\"heap\":" + String((unsigned long)ESP.getFreeHeap()) +
             ",\"total_heap\":" + String((unsigned long)ESP.getHeapSize()) +
             ",\"max_block\":" + String((unsigned long)ESP.getMaxAllocHeap()) +
             ",\"logs\":\"" + jsonEscape(sysLogs) + "\"}";
  sendJson(200, j);
}

// =============================================================================
//  13. HANDLERS: ADMIN - HARDWARE E FERRAMENTAS
// =============================================================================
void handleAdminGpio() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int state = jsonGet(server.arg("plain"), "state").toInt();
  digitalWrite(CONTROL_PIN, state ? HIGH : LOW);
  sendOk();
}

void handleAdminCli() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String cmd = jsonGet(server.arg("plain"), "cmd");
  String cmdNorm = cmd;
  cmdNorm.trim(); cmdNorm.toLowerCase();

  // Reboot: responde ANTES de reiniciar, senao o navegador nunca recebe a resposta
  if (cmdNorm == "reboot") {
    sendJson(200, "{\"res\":\"Reiniciando...\"}");
    delay(500);
    ESP.restart();
    return;
  }

  String res = executarComandoCLI(cmd);
  sendJson(200, "{\"res\":\"" + jsonEscape(res) + "\"}");
}

void handleAdminPing() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String host = jsonGet(server.arg("plain"), "host");
  host.trim(); if (host.length() == 0) host = "google.com";
  IPAddress resolvedIP;
  if (WiFi.hostByName(host.c_str(), resolvedIP)) {
    sendJson(200, "{\"ok\":true, \"ip\":\"" + resolvedIP.toString() + "\"}");
  } else {
    sendErr(500, "Falha na resolução");
  }
}

void handleAdminCpu() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int freq = jsonGet(server.arg("plain"), "freq").toInt();
#if CONFIG_IDF_TARGET_ESP32C3
  // ESP32-C3 trabalha no maximo a 160 MHz
  if (freq == 80 || freq == 160) { setCpuFrequencyMhz(freq); sendOk(); }
  else { sendErr(400, "ESP32-C3 aceita apenas 80 ou 160 MHz"); }
#else
  if (freq == 80 || freq == 160 || freq == 240) { setCpuFrequencyMhz(freq); sendOk(); }
  else { sendErr(400, "Frequencia invalida"); }
#endif
}

void handleAdminDeepSleep() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int sec = jsonGet(server.arg("plain"), "sec").toInt();
  if (sec > 0) {
    sendOk();
    delay(1000);
    esp_sleep_enable_timer_wakeup((uint64_t)sec * 1000000ULL);
    esp_deep_sleep_start();
  } else {
    sendErr(400, "Tempo invalido");
  }
}

void handleAdminFlush() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  sendOk();
}

void handleAdminMaxTemp() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  float t = jsonGet(server.arg("plain"), "temp").toFloat();
  if (t >= 50.0 && t <= 90.0) { maxTempLimit = t; sendOk(); }
  else { sendErr(400, "Fora da faixa (50 a 90)"); }
}

void handleAdminBackup() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String j = "{\"model\":\"" + jsonEscape(getModel()) + "\",\"ssid\":\"" + jsonEscape(prefs.getString("wssid", "")) + "\"}";
  sendJson(200, j);
}

// =============================================================================
//  14. HANDLERS: ADMIN - REDE
// =============================================================================
void handleScanStart() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  WiFi.scanDelete();
  WiFi.scanNetworks(true);
  sendOk();
}

void handleScan() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int n = WiFi.scanComplete();
  if (n == WIFI_SCAN_RUNNING) { sendJson(200, "{\"status\":\"busy\"}"); return; }
  if (n < 0) { sendJson(200, "{\"nets\":[]}"); return; }
  String j = "{\"nets\":[";
  for (int i = 0; i < n; i++) {
    if (i > 0) j += ",";
    j += "{\"ssid\":\"" + jsonEscape(WiFi.SSID(i)) + "\",\"rssi\":" + String(WiFi.RSSI(i)) + ",\"lock\":" + (WiFi.encryptionType(i) != WIFI_AUTH_OPEN ? "true" : "false") + "}";
  }
  j += "]}";
  WiFi.scanDelete();
  sendJson(200, j);
}

void handleAdminWifi() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String ssid = jsonGet(server.arg("plain"), "ssid");
  String pass = jsonGet(server.arg("plain"), "pass");
  ssid.trim();
  if (ssid.length() == 0) { sendErr(400, "SSID invalido"); return; }
  prefs.putString("wssid", ssid);
  prefs.putString("wpass", pass);
  sendOk();
  delay(1000);
  ESP.restart();
}

// Salva (ou limpa) o IP estatico. Aplicado no proximo boot em connectWiFi().
void handleAdminStaticIp() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String body = server.arg("plain");
  String ip   = jsonGet(body, "ip");
  String gw   = jsonGet(body, "gw");
  String mask = jsonGet(body, "mask");
  ip.trim(); gw.trim(); mask.trim();

  // Tudo em branco = volta para DHCP
  if (ip.length() == 0 && gw.length() == 0 && mask.length() == 0) {
    prefs.remove("sip");
    prefs.remove("sgw");
    prefs.remove("smask");
    sendOk();
    return;
  }

  IPAddress a, b, c;
  if (!a.fromString(ip) || !b.fromString(gw) || !c.fromString(mask)) {
    sendErr(400, "IP, Gateway ou Mascara invalidos");
    return;
  }
  prefs.putString("sip", ip);
  prefs.putString("sgw", gw);
  prefs.putString("smask", mask);
  sendOk();
}

// =============================================================================
//  15. HANDLERS: ADMIN - IA, SENHA E SISTEMA
// =============================================================================
void handleAdminKey() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String body = server.arg("plain");
  String key = jsonGet(body, "key");
  String model = jsonGet(body, "model");
  key.trim(); model.trim();
  if (key.length() > 0) prefs.putString("gkey", key);
  if (model.length() > 0) prefs.putString("model", model);

  // Parametros extras enviados pela aba "IA & Groq"
  String t   = jsonGet(body, "temp");
  String tk  = jsonGet(body, "tokens");
  String to  = jsonGet(body, "timeout");
  if (t.length() > 0) {
    float v = t.toFloat();
    if (v >= 0.0 && v <= 1.0) aiTemperature = v;
  }
  if (tk.length() > 0) {
    int v = tk.toInt();
    if (v >= 500 && v <= 4000) aiMaxTokens = v;
  }
  if (to.length() > 0) {
    int v = to.toInt();
    if (v >= 10 && v <= 60) httpTimeoutMs = v * 1000;
  }
  prefs.putString("sys_prompt", jsonGet(body, "sys_prompt"));
  sendOk();
}

void handleAdminPass() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String body = server.arg("plain");
  String currP = jsonGet(body, "curr_pass");
  String newP = jsonGet(body, "new_pass");
  if (currP != getAdminPass()) { sendErr(403, "Senha atual incorreta"); return; }
  if (newP.length() < 4) { sendErr(400, "Senha curta"); return; }
  prefs.putString("apass", newP);
  sendOk();
}

void handleAdminReboot() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  sendOk();
  delay(1000);
  ESP.restart();
}

void handleAdminReset() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  if (jsonGet(server.arg("plain"), "pass") != getAdminPass()) { sendErr(403, "Senha incorreta"); return; }
  prefs.clear();
  sendOk();
  delay(1000);
  ESP.restart();
}

// =============================================================================
//  16. HANDLER: ATUALIZAÇÃO OTA (upload do firmware)
// =============================================================================
void handleUpdate() {
  if (!authed()) { return; }
  HTTPUpload& upload = server.upload();
  if (upload.status == UPLOAD_FILE_START) {
    Update.begin(UPDATE_SIZE_UNKNOWN);
  } else if (upload.status == UPLOAD_FILE_WRITE) {
    Update.write(upload.buf, upload.currentSize);
  } else if (upload.status == UPLOAD_FILE_END) {
    Update.end(true);
  }
}

// =============================================================================
//  17. CAPTIVE PORTAL (implementação oficial ESP32)
// =============================================================================
boolean isCaptive() {
  // Se não estiver no modo AP, não intercepta
  if (!apMode) return false;

  // Se o host requisitado for o IP local do ESP32 ou o domínio mDNS, deixa passar
  String hostHeader = server.hostHeader();
  if (hostHeader == "192.168.4.1" || hostHeader == "miniia.local") {
    return false;
  }
  return true;
}

void handleNotFound() {
  if (isCaptive()) {
    // Redireciona rigorosamente para http://192.168.4.1/ com status 302 (Padrão Oficial)
    server.sendHeader("Location", String("http://192.168.4.1/"), true);
    server.send(302, "text/plain", "");
    return;
  }
  sendErr(404, "Nao encontrado");
}

// =============================================================================
//  18. CONEXÃO Wi-Fi
// =============================================================================
void connectWiFi() {
  String savedSSID = prefs.getString("wssid", DEFAULT_WIFI_SSID);
  String savedPASS = prefs.getString("wpass", DEFAULT_WIFI_PASS);

  WiFi.mode(WIFI_STA);

  // IP estatico (se configurado na aba Rede & IP)
  String sip   = prefs.getString("sip", "");
  String sgw   = prefs.getString("sgw", "");
  String smask = prefs.getString("smask", "");
  if (sip.length() > 0 && sgw.length() > 0 && smask.length() > 0) {
    IPAddress ip, gw, mask;
    if (ip.fromString(sip) && gw.fromString(sgw) && mask.fromString(smask)) {
      WiFi.config(ip, gw, mask, gw);  // DNS = gateway
    }
  }

  WiFi.begin(savedSSID.c_str(), savedPASS.c_str());

  unsigned long t0 = millis();
  while (WiFi.status() != WL_CONNECTED && millis() - t0 < 8000) {
    delay(500);
  }

  if (WiFi.status() == WL_CONNECTED) {
    apMode = false;
    configTime(-3 * 3600, 0, "pool.ntp.org", "time.nist.gov");
    addLog("Wi-Fi Conectado. IP: " + WiFi.localIP().toString());
  } else {
    apMode = true;
    WiFi.mode(WIFI_AP_STA);
    WiFi.softAP(AP_SSID, AP_PASS);

    // Inicia o DNS Server interceptando todas as consultas para o IP do AP (Exemplo Oficial)
    dnsServer.start(DNS_PORT, "*", WiFi.softAPIP());

    addLog("Modo AP Ativado IP: " + WiFi.softAPIP().toString());
  }
}

// =============================================================================
//  19. SETUP
// =============================================================================
void setup() {
  Serial.begin(115200);
  pinMode(CONTROL_PIN, OUTPUT);
  digitalWrite(CONTROL_PIN, LOW);

  delay(500);
  prefs.begin("miniia", false);
  connectWiFi();

  if (!apMode && MDNS.begin("miniia")) {
    addLog("mDNS ativo");
  }

  // Inicia a task do servidor TCP para o TuxAssist
  // (xTaskCreate funciona em qualquer ESP32: classico, S3 ou C3 de nucleo unico)
  xTaskCreate(
    taskTerminalServer,  // Funcao da task
    "TerminalTCP",       // Nome
    4096,                // Memoria RAM da task (bytes)
    NULL,                // Parametro
    1,                   // Prioridade
    NULL                 // Handle
  );

  const char* hk[] = {"X-Token"};
  server.collectHeaders(hk, 1);

  // ---------- Rotas: página e chat ----------
  server.on("/",           HTTP_GET,  handleRoot);
  server.on("/api/status", HTTP_GET,  handleStatus);
  server.on("/api/setup",  HTTP_POST, handleSetup);
  server.on("/api/ask",    HTTP_POST, handleAsk);
  server.on("/api/login",  HTTP_POST, handleLogin);

  // ---------- Rotas: admin ----------
  server.on("/api/admin/info",      HTTP_GET,  handleInfo);
  server.on("/api/admin/gpio",      HTTP_POST, handleAdminGpio);
  server.on("/api/admin/cli",       HTTP_POST, handleAdminCli);
  server.on("/api/admin/ping",      HTTP_POST, handleAdminPing);
  server.on("/api/admin/cpu",       HTTP_POST, handleAdminCpu);
  server.on("/api/admin/deepsleep", HTTP_POST, handleAdminDeepSleep);
  server.on("/api/admin/flush",     HTTP_POST, handleAdminFlush);
  server.on("/api/admin/maxtemp",   HTTP_POST, handleAdminMaxTemp);
  server.on("/api/admin/backup",    HTTP_GET,  handleAdminBackup);
  server.on("/api/scan_start",      HTTP_GET,  handleScanStart);
  server.on("/api/scan",            HTTP_GET,  handleScan);
  server.on("/api/admin/wifi",      HTTP_POST, handleAdminWifi);
  server.on("/api/admin/static_ip", HTTP_POST, handleAdminStaticIp);
  server.on("/api/admin/key",       HTTP_POST, handleAdminKey);
  server.on("/api/admin/pass",      HTTP_POST, handleAdminPass);
  server.on("/api/admin/reboot",    HTTP_POST, handleAdminReboot);
  server.on("/api/admin/reset",     HTTP_POST, handleAdminReset);

  // ---------- Rota: OTA ----------
  server.on("/api/admin/update", HTTP_POST, []() {
    if (!authed()) { sendErr(401, "Nao autorizado"); return; }
    server.sendHeader("Connection", "close");
    server.send(200, "text/plain", (Update.hasError()) ? "FAIL" : "OK");
    delay(500);
    ESP.restart();
  }, handleUpdate);

  // Captura todas as rotas desconhecidas e redireciona pelo Captive Portal oficial
  server.onNotFound(handleNotFound);
  server.begin();
}

// =============================================================================
//  20. LOOP
// =============================================================================
void loop() {
  server.handleClient();
  if (apMode) {
    dnsServer.processNextRequest();
  }
}































CODIGO DO ESP32 S3 PARA COMPILAR NO ANDROID



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

// ====== WI-FI PADRÃO ======
const char* DEFAULT_WIFI_SSID = "NOME_DO_SEU_WIFI";
const char* DEFAULT_WIFI_PASS = "SENHA_DO_SEU_WIFI";
// ==========================

const char* AP_SSID = "MiniIA-ESP32";
const char* AP_PASS = "12345678";
const char* GROQ_URL = "https://api.groq.com/openai/v1/chat/completions";
const char* DEFAULT_MODEL = "openai/gpt-oss-120b"; 

const int CONTROL_PIN = 2;

const size_t MAX_INPUT_LEN = 2000;      
const unsigned long CHAT_COOLDOWN = 1500; 
const int MAX_LOGIN_ATTEMPTS = 5;       
const unsigned long LOCKOUT_TIME = 15000; 

Preferences prefs;
WebServer server(80);
DNSServer dnsServer;
const byte DNS_PORT = 53;

// NOVO: Servidor TCP para o TuxAssist (Roda no Core 0)
WiFiServer terminalServer(2222);

String token = "";
bool apMode = false;

int failedLogins = 0;
unsigned long lockoutStart = 0;
unsigned long lastAskTime = 0;

String sysLogs = "";
float maxObservedTemp = 0.0;
float maxTempLimit = 75.0;
int httpTimeoutMs = 40000;
float aiTemperature = 0.6;
int aiMaxTokens = 1500;

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

// ---------- HTML / JS ----------
const char PAGE[] PROGMEM = R"rawliteral(
<!DOCTYPE html><html lang="pt"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Mini IA ESP32 Ultra Server</title>
<style>
:root{--bg:#0b0d14;--card:#141826;--card2:#1a2033;--line:#252c44;--tx:#e8ecf8;--mut:#8b94b3;--ac:#6c7bff;--ac2:#a06bff;--ok:#3ddc97;--err:#ff6b7a;--warn:#f1c40f}
body.theme-oled{--bg:#000000;--card:#0a0a0a;--card2:#121212;--line:#222222;--tx:#ffffff;--mut:#888888}
body.theme-light{--bg:#f4f6fb;--card:#ffffff;--card2:#eef2f9;--line:#d0d7de;--tx:#1f2328;--mut:#656d76}
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
body{margin:0;background:var(--bg);color:var(--tx);font:15px/1.5 system-ui,-apple-system,Segoe UI,Roboto,sans-serif;min-height:100vh}
header{position:sticky;top:0;z-index:10;display:flex;justify-content:space-between;align-items:center;gap:8px;padding:12px 16px;background:rgba(11,13,20,.9);backdrop-filter:blur(12px);border-bottom:1px solid var(--line)}
.brand{font-weight:700;font-size:17px;display:flex;align-items:center;gap:8px}
.brand small{color:var(--mut);font-weight:500;font-size:11px;border:1px solid var(--line);padding:2px 6px;border-radius:99px}
.dot{width:10px;height:10px;border-radius:50%;background:linear-gradient(135deg,var(--ac),var(--ac2));box-shadow:0 0 10px var(--ac)}
.tools{display:flex;gap:6px;align-items:center}
main{max-width:850px;margin:0 auto;padding:14px}
.hidden{display:none!important}
.card{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:16px;margin-bottom:14px;box-shadow:0 4px 12px rgba(0,0,0,0.15)}
h2,h3{margin:0 0 6px;font-size:16px;font-weight:700}
p.d{color:var(--mut);margin:0 0 12px;font-size:13px}
input,select,textarea{width:100%;background:var(--card2);color:var(--tx);border:1px solid var(--line);border-radius:10px;padding:10px 12px;font:inherit;outline:none;margin-bottom:10px}
input:focus,textarea:focus,select:focus{border-color:var(--ac)}
textarea{resize:none;min-height:48px;max-height:160px;margin-bottom:0}
button{font:inherit;border:1px solid var(--line);background:var(--card2);color:var(--tx);padding:9px 14px;border-radius:10px;cursor:pointer;font-weight:600;font-size:13px;display:inline-flex;align-items:center;justify-content:center;gap:6px}
button:active{transform:scale(.97)}
button.primary{background:linear-gradient(135deg,var(--ac),var(--ac2));border:0;color:#fff}
button.ghost{background:transparent}
button.mini{padding:5px 9px;font-size:11px;border-radius:8px}
button:disabled{opacity:.5}
.row{display:flex;gap:8px;margin-top:10px;justify-content:flex-end;flex-wrap:wrap}
.row.l{justify-content:flex-start}
.stack>*{margin-bottom:10px}
.log{min-height:40vh;padding-bottom:180px}
.msg{max-width:88%;margin:10px 0;padding:10px 14px;border-radius:14px;white-space:pre-wrap;word-wrap:break-word;position:relative;font-size:14px}
.msg.me{margin-left:auto;background:linear-gradient(135deg,var(--ac),var(--ac2));border-bottom-right-radius:4px}
.msg.ai{background:var(--card);border:1px solid var(--line);border-bottom-left-radius:4px}
.msg.pinned{border:2px solid var(--warn)}
.msg-time{font-size:10px;margin-top:4px;color:var(--mut)}
.msg-actions{display:flex;gap:4px;margin-top:6px;flex-wrap:wrap}
.empty{text-align:center;color:var(--mut);margin-top:4vh}
.empty .big{font-size:40px}
.composer{position:fixed;left:0;right:0;bottom:0;background:rgba(11,13,20,.95);backdrop-filter:blur(12px);border-top:1px solid var(--line);padding:10px 14px 14px;z-index:8}
.composer>div{max-width:850px;margin:0 auto;display:flex;flex-direction:column;gap:8px}
.input-group{display:flex;gap:8px;align-items:center}
.modal{position:fixed;inset:0;background:rgba(0,0,0,.7);display:flex;align-items:center;justify-content:center;padding:16px;z-index:20}
.modal .card{width:100%;max-width:380px;margin:0}
.err{color:var(--err);font-size:13px;min-height:18px}
.ok{color:var(--ok)}
.net{display:flex;justify-content:space-between;gap:8px;padding:10px;border:1px solid var(--line);border-radius:10px;margin-bottom:6px;background:var(--card2);cursor:pointer;font-size:13px}
.net:active{background:var(--line)}
.kv{display:flex;justify-content:space-between;align-items:center;padding:7px 0;border-bottom:1px solid var(--line);font-size:13px}
.kv span:first-child{color:var(--mut)}
.dots span{display:inline-block;width:6px;height:6px;margin:0 2px;border-radius:50%;background:var(--mut);animation:b 1s infinite}
.dots span:nth-child(2){animation-delay:.15s}.dots span:nth-child(3){animation-delay:.3s}
@keyframes b{0%,80%,100%{opacity:.2}40%{opacity:1}}
.session-card{background:var(--card2);border:1px solid var(--line);border-radius:12px;padding:10px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center;cursor:pointer}
.session-card:hover{border-color:var(--ac)}
.chips{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;margin-top:10px}
.chip{background:var(--card2);border:1px solid var(--line);padding:5px 10px;border-radius:99px;font-size:11px;cursor:pointer;color:var(--tx)}
.progress-bar{width:100%;height:8px;background:var(--card2);border-radius:4px;overflow:hidden;margin-top:4px;border:1px solid var(--line)}
.progress-fill{height:100%;background:linear-gradient(90deg,var(--ac),var(--ok));transition:width .3s}
.sys-terminal{background:#05070a;color:#3ddc97;font-family:monospace;padding:10px;border-radius:10px;height:140px;overflow-y:auto;font-size:11px;border:1px solid var(--line);white-space:pre-wrap}

.tab-nav{display:flex;gap:6px;overflow-x:auto;padding-bottom:8px;margin-bottom:14px;border-bottom:1px solid var(--line);scrollbar-width:none}
.tab-nav::-webkit-scrollbar{display:none}
.tab-btn{background:var(--card2);border:1px solid var(--line);padding:8px 12px;border-radius:10px;font-size:12px;white-space:nowrap;cursor:pointer;color:var(--mut);font-weight:600}
.tab-btn.active{background:var(--ac);color:#fff;border-color:var(--ac)}
.cmd-shortcuts{display:flex;gap:6px;flex-wrap:wrap;margin-top:8px}
</style></head><body>
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

<section id="vAdmin" class="hidden">
  <div class="tab-nav">
    <button class="tab-btn active" onclick="switchAdminTab(this, 'tabDash')">📊 Dashboard</button>
    <button class="tab-btn" onclick="switchAdminTab(this, 'tabGPIO')">🎛️ Hardware & CLI</button>
    <button class="tab-btn" onclick="switchAdminTab(this, 'tabNet')">🌐 Rede & IP</button>
    <button class="tab-btn" onclick="switchAdminTab(this, 'tabAI')">🤖 IA & Groq</button>
    <button class="tab-btn" onclick="switchAdminTab(this, 'tabTools')">🛠️ Ferramentas</button>
    <button class="tab-btn" onclick="switchAdminTab(this, 'tabSec')">🔒 Segurança</button>
  </div>

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

  <div id="tabNet" class="admin-tab hidden">
    <div class="card stack">
      <h3>📶 Conectar em Rede Wi-Fi</h3>
      <input id="wifiSsid" placeholder="Nome da rede (SSID)">
      <input id="wifiPass" type="password" placeholder="Senha do Wi-Fi">
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
      <p class="d">Reduza o clock para economizar bateria e esquentar menos.</p>
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
  <div class="row" style="margin-top:14px;"><button id="bBack" type="button">Voltar ao Chat</button><button id="bOut" type="button">Sair</button></div>
</section>
</main>

<div id="modal" class="modal hidden"><div class="card stack">
  <h3 id="modalTitle">🔐 Autenticação Admin</h3>
  <input id="u" value="admin" autocomplete="off" autocapitalize="off">
  <input id="p" type="password" placeholder="Senha (padrão: 1234)" autocomplete="off">
  <div id="lErr" class="err"></div>
  <div class="row"><button id="lCancel" type="button">Cancelar</button><button id="lOk" type="button" class="primary">Entrar</button></div>
</div></div>

<script>
const $=id=>document.getElementById(id);
let token=sessionStorage.getItem("tk")||"", busy=false;
let autoTTS = false;
let adminInterval = null;
let lastSentText = "";

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

function switchAdminTab(btn, tabId) {
  document.querySelectorAll('.admin-tab').forEach(el => el.classList.add('hidden'));
  document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
  $(tabId).classList.remove('hidden');
  if(btn) btn.classList.add('active');
}

let sessions = [];
let currentSessionId = null;

try {
  const savedSessions = localStorage.getItem("miniia_sessions_v8");
  if(savedSessions) sessions = JSON.parse(savedSessions);
  cleanOldSessions();
} catch(e){}

function cleanOldSessions() {
  const thirtyDays = 30 * 24 * 60 * 60 * 1000;
  const now = Date.now();
  sessions = sessions.filter(s => (now - (s.ts || now)) < thirtyDays);
  saveAllSessions();
}

function saveAllSessions() {
  try { localStorage.setItem("miniia_sessions_v8", JSON.stringify(sessions)); } catch(e){}
}

function getCurrentSession() {
  if(!currentSessionId) return null;
  return sessions.find(s => s.id === currentSessionId);
}

function createNewSession() {
  currentSessionId = "sess_" + Date.now();
  const newSess = {
    id: currentSessionId,
    title: "Nova Conversa",
    ts: Date.now(),
    date: new Date().toLocaleString("pt-BR", {day:'2-digit',month:'2-digit',hour:'2-digit',minute:'2-digit'}),
    messages: []
  };
  sessions.unshift(newSess);
  saveAllSessions();
  restoreChatUI();
}

function vibrate(ms=30) {
  if (navigator.vibrate) navigator.vibrate(ms);
}

if (!sessions || sessions.length === 0) {
  createNewSession();
} else {
  currentSessionId = sessions[0].id;
}

function restoreChatUI() {
  $("log").innerHTML = "";
  const sess = getCurrentSession();
  if(!sess || !sess.messages || sess.messages.length === 0) {
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
  for(let i=0; i<sess.messages.length; i++){
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

function renderHistoryView() {
  const list = $("sessionsList");
  if(!sessions || sessions.length === 0){
    list.innerHTML = '<p class="d">Nenhuma conversa salva no histórico.</p>';
    return;
  }
  let html = "";
  for(let i=0; i<sessions.length; i++){
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
  if(!s) return;
  const newName = prompt("Digite o novo nome para esta conversa:", s.title);
  if(newName && newName.trim().length > 0) {
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
  if(currentSessionId === id) {
    if(sessions.length > 0) currentSessionId = sessions[0].id;
    else createNewSession();
  }
  saveAllSessions();
  renderHistoryView();
}

function escHtml(s){ return String(s||"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c])); }

function copyToClipboard(text, btnEl) {
  if (navigator.clipboard && window.isSecureContext) {
    navigator.clipboard.writeText(text).then(() => {
      btnEl.textContent = "✓ Copiado!";
      vibrate(20);
      setTimeout(() => btnEl.textContent = "📋 Copiar", 2000);
    }).catch(() => fallbackCopy(text, btnEl));
  } else {
    fallbackCopy(text, btnEl);
  }
}

function fallbackCopy(text, btnEl) {
  const textArea = document.createElement("textarea");
  textArea.value = text;
  textArea.style.position = "fixed";
  textArea.style.left = "-9999px";
  document.body.appendChild(textArea);
  textArea.focus();
  textArea.select();
  try {
    document.execCommand('copy');
    btnEl.textContent = "✓ Copiado!";
    vibrate(20);
    setTimeout(() => btnEl.textContent = "📋 Copiar", 2000);
  } catch (err) {
    btnEl.textContent = "❌ Erro ao copiar";
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
  if(!autoTTS) stopSpeak();
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
  $("bMic").onclick = () => { try { rec.start(); } catch(e){ rec.stop(); } };
}

$("q").addEventListener("keydown", function(e) {
  if ((e.ctrlKey || e.metaKey) && e.key === "Enter") {
    e.preventDefault();
    $("bSend").click();
  }
});

function show(v){
  if(v !== "vAdmin" && adminInterval) {
    clearInterval(adminInterval);
    adminInterval = null;
  }
  ["vSetup","vChat","vHistory","vAdmin"].forEach(x=>{
    const el=$(x);
    if(el) el.classList.toggle("hidden", x!==v);
  });
  window.scrollTo(0,0);
}

function req(path, body, auth, cb){
  const o = {
    method: body ? "POST" : "GET",
    headers: {"Content-Type": "application/json"}
  };
  if(auth && token) o.headers["X-Token"] = token;
  if(body) o.body = JSON.stringify(body);
  
  fetch(path, o)
    .then(r => r.json().catch(() => ({})))
    .then(data => cb(null, data))
    .catch(err => cb(err, null));
}

function addMsgUI(role, text, timeMs, isPinned=false){
  const empty = $("empty");
  if(empty) empty.classList.add("hidden");
  const d=document.createElement("div"); d.className="msg "+role + (isPinned ? " pinned" : "");
  const b=document.createElement("div"); b.textContent=text; d.appendChild(b);

  if (role === 'ai') {
    if(timeMs) {
      const tm = document.createElement("div");
      tm.className = "msg-time";
      let timeSec = (timeMs/1000).toFixed(2);
      let color = timeMs < 2000 ? "var(--ok)" : (timeMs < 5000 ? "var(--warn)" : "var(--err)");
      let estTokens = Math.round(text.length / 4);
      let estCost = (estTokens * 0.0000002).toFixed(6);
      tm.innerHTML = "Respondido em <b style='color:"+color+"'>" + timeSec + "s</b> • ~" + estTokens + " tokens ($" + estCost + ")";
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
      if(sess) {
        let msgObj = sess.messages.find(m => m.c === text);
        if(msgObj) msgObj.pinned = pinnedState;
        saveAllSessions();
      }
    };

    act.appendChild(btnPlay);
    act.appendChild(btnCopy);
    act.appendChild(btnPin);
    d.appendChild(act);
  }

  $("log").appendChild(d);
  d.scrollIntoView({behavior:"smooth",block:"end"});
  return {d,b};
}

$("bSend").onclick = function(){
  const q=$("q").value.trim();
  if(!q || busy) return;
  if(q === lastSentText) {
    if(!confirm("Você está enviando a mesma pergunta. Continuar?")) return;
  }
  if(q.length > 2000) {
    alert("⚠️ Pergunta muito longa! Reduza para no máximo 2000 caracteres.");
    return;
  }
  lastSentText = q;
  vibrate(20);
  busy=true; $("bSend").disabled=true; $("q").value="";
  addMsgUI("me",q);
  const m=addMsgUI("ai","");
  m.b.innerHTML='<span class="dots"><span></span><span></span><span></span></span>';
  
  const startTime = Date.now();
  const mode = $("modePreset").value;
  const sess = getCurrentSession();

  if(sess.messages.length === 0) {
    sess.title = q.substring(0, 30) + (q.length > 30 ? "..." : "");
  }

  req("/api/ask", {q:q, h:sess.messages.slice(-6), mode:mode}, false, function(err, r){
    const elapsed = Date.now() - startTime;
    busy=false; $("bSend").disabled=false;
    if(err || !r || r.error){
      vibrate(100);
      m.b.textContent = "⚠️ " + ((r && r.error) ? r.error : "Erro de conexão com o ESP32");
      m.b.style.color = "var(--err)";
    } else {
      vibrate(40);
      m.b.textContent = r.a;
      
      const tm = document.createElement("div");
      tm.className = "msg-time";
      let timeSec = (elapsed/1000).toFixed(2);
      let color = elapsed < 2000 ? "var(--ok)" : (elapsed < 5000 ? "var(--warn)" : "var(--err)");
      let estTokens = Math.round(r.a.length / 4);
      let estCost = (estTokens * 0.0000002).toFixed(6);
      tm.innerHTML = "Respondido em <b style='color:"+color+"'>" + timeSec + "s</b> • ~" + estTokens + " tokens ($" + estCost + ")";
      m.d.appendChild(tm);

      sess.messages.push({r:"user",c:q},{r:"assistant",c:r.a,t:elapsed});
      saveAllSessions();
      if(autoTTS) speakText(r.a);
    }
  });
};

$("bClearChatUI").onclick = function() {
  const sess = getCurrentSession();
  if(sess) {
    sess.messages = [];
    saveAllSessions();
    restoreChatUI();
  }
};

$("bExport").onclick = function() {
  const sess = getCurrentSession();
  if(!sess || sess.messages.length === 0) { alert("Nenhuma conversa para exportar"); return; }
  let txt = "CONVERSA MINI IA ESP32 - " + sess.title + "\nData: " + sess.date + "\n" + "=".repeat(40) + "\n\n";
  sess.messages.forEach(m => {
    txt += (m.r === "user" ? "VOCÊ: " : "IA: ") + m.c + "\n\n";
  });
  const blob = new Blob([txt], {type: "text/plain;charset=utf-8"});
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = "chat_esp32_" + Date.now() + ".txt";
  a.click();
};

$("bNewChat").onclick = function() { createNewSession(); };
$("bHist").onclick = function() { renderHistoryView(); show("vHistory"); };
$("bBackChat").onclick = function() { restoreChatUI(); show("vChat"); };

$("bClearAllSessions").onclick = function() {
  if(confirm("Tem certeza que deseja apagar todo o histórico de conversas?")) {
    sessions = [];
    localStorage.removeItem("miniia_sessions_v8");
    createNewSession();
    renderHistoryView();
    show("vChat");
  }
};

$("sSave").onclick = function(){
  const val = $("sKey").value.trim();
  $("sErr").textContent = "";
  if(!val){ show("vChat"); return; }
  req("/api/setup", {key:val}, false, function(err, r){
    $("sKey").value = "";
    show("vChat");
  });
};

$("bSkipSetup").onclick = function(){ restoreChatUI(); show("vChat"); };

$("bAdmin").onclick = function(){
  if(token){
    startAdminRealtime();
  } else {
    $("lErr").textContent=""; $("p").value=""; $("modal").classList.remove("hidden");
  }
};

function formatUptime(ms) {
  let sec = Math.floor(ms / 1000);
  let min = Math.floor(sec / 60); sec %= 60;
  let hr = Math.floor(min / 60); min %= 60;
  let day = Math.floor(hr / 24); hr %= 24;
  return (day > 0 ? day + "d " : "") + hr + "h " + min + "m " + sec + "s";
}

function fetchAdminData() {
  req("/api/admin/info", null, true, function(err, i){
    if(i && !i.error){
      let tempVal = parseFloat(i.temp);
      let tempColor = "var(--ok)";
      if(!isNaN(tempVal) && tempVal > 70) tempColor = "var(--err)";
      else if(!isNaN(tempVal) && tempVal > 60) tempColor = "var(--warn)";

      let rssiVal = parseInt(i.rssi);
      let rssiQuality = rssiVal > -55 ? "Excelente 📶" : (rssiVal > -70 ? "Bom 📶" : "Fraco");
      let usedHeap = i.total_heap - i.heap;
      let usedPct = Math.round((usedHeap / i.total_heap) * 100);

      const rows=[
        ["IP Local / mDNS", i.ip + " (" + i.mdns + ")"],
        ["Frequência da CPU", i.cpu_freq + " MHz"],
        ["Wi-Fi Conectado", (i.ssid||"-") + " (" + rssiQuality + ")"],
        ["Sinal (RSSI)", i.rssi+" dBm"],
        ["Uptime ESP32", formatUptime(i.uptime||0)],
        ["Temperatura Atual / Máx", '<b style="color:'+tempColor+'">'+i.temp+' °C / '+i.max_temp+' °C (Limite: '+i.max_limit+'°C)</b>'],
        ["RAM Livre / Maior Bloco", Math.round(i.heap/1024)+" KB / "+Math.round(i.max_block/1024)+" KB"],
        ["API Key", i.key||"—"],
        ["Modelo IA / Temp.", i.model + " (T=" + i.ai_temp + ")"],
        ["Pino GPIO 2 Estado", i.gpio2 ? "🟢 HIGH (LIGADO)" : "🔴 LOW (DESLIGADO)"]
      ];

      let html = rows.map(r=>'<div class="kv"><span>'+r[0]+'</span><span>'+r[1]+'</span></div>').join("");
      html += '<div style="margin-top:8px"><span style="font-size:12px; color:var(--mut)">Carga de Memória RAM:</span><div class="progress-bar"><div class="progress-fill" style="width:'+usedPct+'%"></div></div></div>';

      $("info").innerHTML = html;
      $("sysTerm").textContent = i.logs || "Sem eventos.";
      $("gpioStatus").textContent = "Estado atual do Pino 2: " + (i.gpio2 ? "🟢 HIGH (LIGADO)" : "🔴 LOW (DESLIGADO)");
      
      if(!document.activeElement || document.activeElement.id !== "aModel") $("aModel").value = i.model||"";
      if(!document.activeElement || document.activeElement.id !== "aSysPrompt") $("aSysPrompt").value = i.sys_prompt||"";
    } else {
      $("info").innerHTML = "Sessão expirada ou acesso negado.";
    }
  });
}

function startAdminRealtime() {
  show("vAdmin");
  fetchAdminData();
  if(adminInterval) clearInterval(adminInterval);
  adminInterval = setInterval(fetchAdminData, 2000);
}

$("lCancel").onclick = function(){ $("modal").classList.add("hidden"); };
$("lOk").onclick = function(){
  $("lErr").textContent = "";
  req("/api/login", {user:$("u").value.trim(), pass:$("p").value}, false, function(err, r){
    if(r && r.token){
      token = r.token;
      sessionStorage.setItem("tk", token);
      $("modal").classList.add("hidden");
      startAdminRealtime();
    } else {
      $("lErr").textContent = (r && r.error) ? r.error : "Acesso negado";
    }
  });
};

$("bFlushRAM").onclick = function() {
  req("/api/admin/flush", {}, true, function(err, r){
    alert("Memória RAM desfragmentada e limpa!");
    fetchAdminData();
  });
};

$("bGpioHigh").onclick = function() { req("/api/admin/gpio", {state: 1}, true, () => fetchAdminData()); };
$("bGpioLow").onclick = function() { req("/api/admin/gpio", {state: 0}, true, () => fetchAdminData()); };

function runCliShortcut(cmdText) {
  $("cliCmd").value = cmdText;
  $("bRunCli").click();
}

$("bRunCli").onclick = function() {
  const cmd = $("cliCmd").value.trim();
  if(!cmd) return;
  req("/api/admin/cli", {cmd: cmd}, true, function(err, r){
    if(r && r.res) alert("Terminal ESP32:\n" + r.res);
    $("cliCmd").value = "";
    fetchAdminData();
  });
};

$("bDoPing").onclick = function() {
  const target = $("pingHost").value.trim();
  if(!target) return alert("Digite um host para testar");
  $("pingRes").textContent = "Testando resolução DNS do host...";
  req("/api/admin/ping", {host: target}, true, function(err, r){
    if(r && r.ok) $("pingRes").textContent = "✓ Sucesso! O ESP32 resolveu o IP: " + r.ip;
    else $("pingRes").textContent = "❌ Falha no teste: Não foi possível resolver o host.";
  });
};

$("bSetCpu").onclick = function() {
  const freq = $("cpuFreqSelect").value;
  req("/api/admin/cpu", {freq: freq}, true, function(err, r){
    alert("Clock do processador alterado para " + freq + " MHz");
    fetchAdminData();
  });
};

$("bDeepSleep").onclick = function() {
  const sec = parseInt($("sleepSec").value);
  if(!sec || sec <= 0) return alert("Digite um tempo válido em segundos");
  if(confirm("Deseja colocar o ESP32 em Deep Sleep por " + sec + " segundos?")) {
    req("/api/admin/deepsleep", {sec: sec}, true, function(){ alert("ESP32 entrando em Deep Sleep..."); });
  }
};

$("bSaveStaticIP").onclick = function() {
  const ip = $("stIP").value.trim();
  const gw = $("stGW").value.trim();
  const mask = $("stMask").value.trim();
  req("/api/admin/static_ip", {ip:ip, gw:gw, mask:mask}, true, function(err, r){
    alert("IP Estático configurado. Reinicie o ESP32 para aplicar.");
  });
};

$("bSetMaxTemp").onclick = function() {
  const val = $("aMaxTemp").value;
  req("/api/admin/maxtemp", {temp: val}, true, function(err, r){ alert("Limite térmico salvo!"); });
};

$("bBackupExport").onclick = function() {
  req("/api/admin/backup", null, true, function(err, r){
    if(r) {
      const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(r));
      const dlAnchor = document.createElement('a');
      dlAnchor.setAttribute("href", dataStr);
      dlAnchor.setAttribute("download", "esp32_backup_config.json");
      document.body.appendChild(dlAnchor);
      dlAnchor.click();
      dlAnchor.remove();
    }
  });
};

$("bBack").onclick = function(){ restoreChatUI(); show("vChat"); };
$("bOut").onclick = function(){ 
  token=""; 
  sessionStorage.removeItem("tk"); 
  if(adminInterval) clearInterval(adminInterval);
  restoreChatUI(); 
  show("vChat"); 
};

$("bUpdateOTA").onclick = function(){
  const fileInput = $("otaFile");
  if(!fileInput.files.length){ alert("Selecione o arquivo .bin compilado primeiro"); return; }
  const file = fileInput.files[0];
  const formData = new FormData();
  formData.append("update", file);

  $("otaProgress").textContent = "⚡ Gravando firmware no ESP32...";
  $("bUpdateOTA").disabled = true;

  fetch("/api/admin/update", {
    method: "POST",
    headers: {"X-Token": token},
    body: formData
  })
  .then(r => r.text())
  .then(txt => {
    $("otaProgress").textContent = " Sucesso! ESP32 reiniciando...";
    setTimeout(() => location.reload(), 6000);
  });
};

$("bReboot").onclick = function(){
  if(confirm("Reiniciar o ESP32 agora?")) {
    req("/api/admin/reboot", {}, true, function(){
      alert("ESP32 Reiniciando...");
      setTimeout(() => location.reload(), 5000);
    });
  }
};

$("bReset").onclick = function(){
  const confirmPass = prompt("⚠️ ZONA DE PERIGO!\nDigite a senha do Admin para confirmar o RESET DE FÁBRICA:");
  if(confirmPass !== null) {
    if(!confirmPass) { alert("Senha não informada!"); return; }
    req("/api/admin/reset", {pass: confirmPass}, true, function(err, r){
      if(r && r.ok) {
        alert("ESP32 Resetado com sucesso!");
        location.reload();
      } else {
        alert("❌ Senha incorreta! Reset cancelado.");
      }
    });
  }
};

$("bSaveWifi").onclick = function(){
  const s=$("wifiSsid").value.trim(), p=$("wifiPass").value;
  if(!s){ flash(false, "Insira o SSID da rede"); return; }
  req("/api/admin/wifi", {ssid:s, pass:p}, true, function(err, r){
    flash(true, "Wi-Fi salvo! O ESP32 vai conectar na sua rede e sair do modo AP.");
  });
};

function pollScan(){
  req("/api/scan", null, true, function(err, r){
    if(r && r.status === "busy"){
      setTimeout(pollScan, 1000);
    } else if(r && r.nets){
      $("bScan").disabled = false;
      let html = "";
      for(let i=0; i<r.nets.length; i++){
        let n = r.nets[i];
        html += '<div class="net" data-ssid="'+(n.ssid||"")+'"><b>'+(n.lock?"🔒 ":"🔓 ")+(n.ssid||"(Oculta)")+'</b><span>'+n.rssi+' dBm</span></div>';
      }
      $("nets").innerHTML = html;
      const list = document.querySelectorAll(".net");
      for(let j=0; j<list.length; j++){
        list[j].onclick = function(){
          $("wifiSsid").value = this.getAttribute("data-ssid");
          $("wifiPass").focus();
        };
      }
    }
  });
}

$("bScan").onclick = function(){
  $("bScan").disabled = true;
  $("nets").innerHTML = '<p class="d">Escaneando redes...</p>';
  req("/api/scan_start", null, true, function(err, r){ pollScan(); });
};

function flash(ok,msg){ $("aMsg").className=ok?"err ok":"err"; $("aMsg").textContent=msg; }

$("bKey").onclick = function(){
  req("/api/admin/key", {
    key:$("aKey").value.trim(), 
    model:$("aModel").value.trim(),
    temp: $("aTemp").value,
    tokens: $("aTokens").value,
    timeout: $("aTimeout").value,
    sys_prompt: $("aSysPrompt").value.trim()
  }, true, function(err, r){
    $("aKey").value=""; fetchAdminData(); flash(true, "Ajustes salvos com sucesso!");
  });
};

$("bPass").onclick = function(){
  const currP = $("aCurrPass").value;
  const newP = $("aPass").value;
  if(!currP || !newP) { flash(false, "Preencha a senha atual e a nova senha"); return; }
  req("/api/admin/pass", {curr_pass: currP, new_pass: newP}, true, function(err, r){
    if(r && r.ok) {
      $("aCurrPass").value = ""; $("aPass").value = "";
      flash(true, "Senha do Admin alterada com sucesso!");
    } else {
      flash(false, (r && r.error) ? r.error : "Erro ao alterar senha");
    }
  });
};

req("/api/status", null, false, function(err, s){
  restoreChatUI();
  if(s && s.configured) show("vChat");
  else show("vSetup");
});
</script></body></html>
)rawliteral";

uint32_t hexToInt(const String& s) {
  uint32_t val = 0;
  for (unsigned int k = 0; k < s.length(); k++) {
    char c = s[k];
    val <<= 4;
    if (c >= '0' && c <= '9') val |= (c - '0');
    else if (c >= 'a' && c <= 'f') val |= (c - 'a' + 10);
    else if (c >= 'A' && c <= 'F') val |= (c - 'A' + 10);
  }
  return val;
}

void appendUtf8(String& out, uint32_t cp) {
  if (cp < 0x80) {
    out += (char)cp;
  } else if (cp < 0x800) {
    out += (char)(0xC0 | (cp >> 6));
    out += (char)(0x80 | (cp & 0x3F));
  } else if (cp < 0x10000) {
    out += (char)(0xE0 | (cp >> 12));
    out += (char)(0x80 | ((cp >> 6) & 0x3F));
    out += (char)(0x80 | (cp & 0x3F));
  } else {
    out += (char)(0xF0 | (cp >> 18));
    out += (char)(0x80 | ((cp >> 12) & 0x3F));
    out += (char)(0x80 | ((cp >> 6) & 0x3F));
    out += (char)(0x80 | (cp & 0x3F));
  }
}

String readStr(const String& s, int& i) {
  String out;
  int len = s.length();
  for (i++; i < len; i++) {
    char c = s[i];
    if (c == '"') { i++; return out; }
    if (c == '\\') {
      i++;
      if (i >= len) break;
      char e = s[i];
      switch (e) {
        case 'n': out += '\n'; break;
        case 'r': out += '\r'; break;
        case 't': out += '\t'; break;
        case 'b': out += '\b'; break;
        case 'f': out += '\f'; break;
        case 'u': {
          if (i + 4 < len) {
            String hexStr = s.substring(i + 1, i + 5);
            uint32_t cp = hexToInt(hexStr);
            i += 4;
            if (cp >= 0xD800 && cp <= 0xDBFF && i + 6 < len && s[i + 1] == '\\' && s[i + 2] == 'u') {
              String loStr = s.substring(i + 3, i + 7);
              uint32_t lo = hexToInt(loStr);
              if (lo >= 0xDC00 && lo <= 0xDFFF) {
                cp = 0x10000 + ((cp - 0xD800) << 10) + (lo - 0xDC00);
                i += 6;
              }
            }
            appendUtf8(out, cp);
          }
          break;
        }
        default: out += e; break;
      }
    } else {
      out += c;
    }
  }
  return out;
}

String jsonGet(const String& b, const char* key) {
  String pat = String("\"") + key + "\"";
  int p = b.indexOf(pat);
  if (p < 0) return "";
  int i = p + pat.length();
  int len = b.length();
  while (i < len && (b[i] == ' ' || b[i] == ':')) i++;
  if (i >= len || b[i] != '"') return "";
  return readStr(b, i);
}

String jsonEscape(const String& s) {
  String o;
  o.reserve(s.length() + 16);
  for (unsigned int k = 0; k < s.length(); k++) {
    unsigned char c = (unsigned char)s[k];
    if (c == '"') o += "\\\"";
    else if (c == '\\') o += "\\\\";
    else if (c == '\n') o += "\\n";
    else if (c == '\r') o += "\\r";
    else if (c == '\t') o += "\\t";
    else if (c < 0x20) {
      char buf[8];
      snprintf(buf, sizeof(buf), "\\u%04x", c);
      o += buf;
    } else o += (char)c;
  }
  return o;
}

String buildHistory(const String& body) {
  String res = "";
  int p = body.indexOf("\"h\"");
  if (p < 0) return res;
  int i = body.indexOf('[', p);
  if (i < 0) return res;
  i++;
  int len = body.length();
  int count = 0;
  while (i < len && count < 8) {
    char c = body[i];
    if (c == ']') break;
    if (c == '{') {
      i++;
      String r = "", cc = "";
      while (i < len && body[i] != '}') {
        if (body[i] == '"') {
          String k = readStr(body, i);
          while (i < len && body[i] != ':') i++;
          i++;
          while (i < len && body[i] == ' ') i++;
          if (i < len && body[i] == '"') {
            String v = readStr(body, i);
            if (k == "r") r = v;
            else if (k == "c") cc = v;
          }
        } else i++;
      }
      i++;
      if ((r == "user" || r == "assistant") && cc.length() > 0) {
        if (cc.length() > 800) cc = cc.substring(0, 800);
        res += "{\"role\":\"" + r + "\",\"content\":\"" + jsonEscape(cc) + "\"},";
        count++;
      }
    } else i++;
  }
  return res;
}

void sendJson(int code, const String& json) {
  server.sendHeader("Cache-Control", "no-store, no-cache, must-revalidate");
  server.sendHeader("X-Content-Type-Options", "nosniff");
  server.sendHeader("X-Frame-Options", "DENY");
  server.send(code, "application/json; charset=utf-8", json);
}

void sendErr(int code, const String& m) {
  sendJson(code, "{\"error\":\"" + jsonEscape(m) + "\"}");
}

void sendOk() { sendJson(200, "{\"ok\":true}"); }

bool authed() {
  return token.length() > 0 && server.header("X-Token") == token;
}

String getModel() { return prefs.getString("model", DEFAULT_MODEL); }
String getAdminPass() { return prefs.getString("apass", "1234"); }

String maskKey(const String& k) {
  if (k.length() < 10) return "Não Configurada";
  return k.substring(0, 4) + "****" + k.substring(k.length() - 4);
}

bool callGroq(const String& key, const String& model, const String& sys, const String& histJson,
              const String& user, int maxTokens, String& out, int& httpCode) {
  httpCode = 0;
  if (key.length() < 10) { out = "API key nao configurada no ESP32"; return false; }
  if (WiFi.status() != WL_CONNECTED) { out = "ESP32 sem internet (Wi-Fi desconectado)"; return false; }

  if (temperatureRead() > maxTempLimit) {
    out = "Aviso: O ESP32 superaqueceu acima do limite de segurança. Aguarde o resfriamento.";
    return false;
  }

  String customPrompt = prefs.getString("sys_prompt", "");
  String finalSys = sys;
  if(customPrompt.length() > 0) {
    finalSys += "\nInstrução Adicional do Usuario: " + customPrompt;
  }

  String body = "{\"model\":\"" + jsonEscape(model) + "\",\"max_tokens\":" + String(aiMaxTokens) +
                ",\"temperature\":" + String(aiTemperature, 1) + ",\"messages\":[{\"role\":\"system\",\"content\":\"" + jsonEscape(finalSys) + "\"}," +
                histJson + "{\"role\":\"user\",\"content\":\"" + jsonEscape(user) + "\"}]}";

  HTTPClient http;
  http.setTimeout(httpTimeoutMs);
  if (!http.begin(GROQ_URL)) { out = "Falha ao conectar na Groq"; return false; }
  http.addHeader("Content-Type", "application/json");
  http.addHeader("Authorization", "Bearer " + key);
  httpCode = http.POST(body);
  String resp = (httpCode > 0) ? http.getString() : "";
  http.end();

  if (httpCode <= 0) { out = "Erro de conexao: timeout ou falha na rede"; return false; }
  if (httpCode != 200) {
    String em = jsonGet(resp, "message");
    out = em.length() > 0 ? em : ("Erro HTTP " + String(httpCode));
    return false;
  }

  int targetPos = resp.indexOf("\"content\":");
  if (targetPos >= 0) {
    int startIdx = resp.indexOf("\"", targetPos + 9);
    if (startIdx >= 0) out = readStr(resp, startIdx);
  }
  if (out.length() == 0) out = jsonGet(resp, "content");
  out.trim();
  return true;
}

// =========================================================================
// NOVO: Função isolada de Comandos (Compartilhada entre Web UI e Terminal TCP)
// =========================================================================
String executarComandoCLI(String cmd) {
  cmd.trim(); cmd.toLowerCase();
  String res = "";
  if (cmd == "gpio 2 high" || cmd == "led on") { digitalWrite(CONTROL_PIN, HIGH); res = "GPIO 2 ativado"; }
  else if (cmd == "gpio 2 low" || cmd == "led off") { digitalWrite(CONTROL_PIN, LOW); res = "GPIO 2 desativado"; }
  else if (cmd == "heap" || cmd == "ram") { res = "Heap Livre: " + String(ESP.getFreeHeap() / 1024) + " KB"; }
  else if (cmd == "temp") { res = "Temperatura: " + String(temperatureRead(), 1) + " C"; }
  else if (cmd == "clearlog" || cmd == "clear") { sysLogs = ""; res = "Logs limpos"; }
  else if (cmd == "reboot") { delay(500); ESP.restart(); return "Reiniciando..."; }
  else { res = "Comando desconhecido: " + cmd; }
  return res;
}

// =========================================================================
// NOVO: A TASK DO SERVIDOR TCP QUE RODA NO NÚCLEO 0 E USA WRITE()
// =========================================================================
void taskTerminalServer(void *pvParameters) {
  terminalServer.begin();
  for(;;) {
    WiFiClient client = terminalServer.available();
    if (client) {
      // Usa String normal, mas injeta via write() (envio de bytes crus imune a erros do ArduinoDroid)
      String msgWelcome = "TuxAssist Socket Server - ESP32-S3\n";
      client.write((const uint8_t*)msgWelcome.c_str(), msgWelcome.length());
      
      String currentLine = "";
      while (client.connected()) {
        if (client.available()) {
          char c = client.read();
          if (c == '\n') {
            if (currentLine.length() > 0) {
              String resposta = executarComandoCLI(currentLine);
              resposta += "\n";
              
              client.write((const uint8_t*)resposta.c_str(), resposta.length());
              
              currentLine = "";
            }
          } else if (c != '\r') {
            currentLine += c;
          }
        }
        vTaskDelay(10 / portTICK_PERIOD_MS); // Evita watchdog travar a CPU
      }
      client.stop();
    }
    vTaskDelay(50 / portTICK_PERIOD_MS); // Respira o Core 0
  }
}
// =========================================================================

void handleRoot() { server.send_P(200, "text/html", PAGE); }

void handleStatus() {
  bool ok = prefs.getString("gkey", "").length() > 10;
  sendJson(200, ok ? "{\"configured\":true}" : "{\"configured\":false}");
}

void handleSetup() {
  String key = jsonGet(server.arg("plain"), "key");
  key.trim();
  if (key.length() < 10) { sendErr(400, "API key invalida/curta"); return; }
  prefs.putString("gkey", key);
  addLog("API Key reconfigurada.");
  sendOk();
}

void handleAsk() {
  if (millis() - lastAskTime < CHAT_COOLDOWN) {
    sendErr(429, "Aguarde um segundo antes de enviar outra pergunta.");
    return;
  }
  lastAskTime = millis();

  String body = server.arg("plain");
  String q = jsonGet(body, "q");
  String mode = jsonGet(body, "mode");
  q.trim();

  if (q.length() == 0) { sendErr(400, "Pergunta vazia"); return; }
  if (q.length() > MAX_INPUT_LEN) q = q.substring(0, MAX_INPUT_LEN);

  String sys = "Voce e a Mini IA rodando no ESP32. "
               "REGRAS DE RESPOSTA OBRIGATORIAS:\n"
               "1. Responda em texto simples, limpo e direto.\n"
               "2. NUNCA use tabelas em Markdown (nao use o caractere |).\n"
               "3. NUNCA use marcadores de negrito ou italico (nao use ** ou * ou _).\n"
               "4. NUNCA use hashtags (#) para titulos.\n"
               "5. Use apenas letras, numeros, pontuacao comum e quebras de linha para organizar o texto.";

  String out; int code;
  if (!callGroq(prefs.getString("gkey", ""), getModel(), sys, buildHistory(body), q, aiMaxTokens, out, code)) {
    sendErr(code == 429 ? 429 : 502, out);
    return;
  }
  sendJson(200, "{\"a\":\"" + jsonEscape(out) + "\"}");
}

void handleLogin() {
  if (failedLogins >= MAX_LOGIN_ATTEMPTS) {
    if (millis() - lockoutStart < LOCKOUT_TIME) {
      unsigned long timeLeft = (LOCKOUT_TIME - (millis() - lockoutStart)) / 1000;
      sendErr(429, "Bloqueado por seguranca! Tente novamente em " + String(timeLeft) + "s");
      return;
    } else {
      failedLogins = 0;
    }
  }

  String body = server.arg("plain");
  String u = jsonGet(body, "user");
  String p = jsonGet(body, "pass");

  if (u == "admin" && p == getAdminPass()) {
    failedLogins = 0;
    token = String((uint32_t)esp_random(), HEX) + String((uint32_t)esp_random(), HEX);
    sendJson(200, "{\"token\":\"" + token + "\"}");
  } else {
    failedLogins++;
    if (failedLogins >= MAX_LOGIN_ATTEMPTS) lockoutStart = millis();
    delay(400);
    sendErr(401, "Usuario ou senha incorretos");
  }
}

void handleInfo() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String ip = apMode ? WiFi.softAPIP().toString() : WiFi.localIP().toString();
  String ssid = apMode ? (String(AP_SSID) + " (AP)") : WiFi.SSID();
  int rssi = apMode ? 0 : WiFi.RSSI();
  
  float tempC = temperatureRead();
  if (tempC > maxObservedTemp) maxObservedTemp = tempC;
  bool gpio2State = digitalRead(CONTROL_PIN);

  String j = "{\"ip\":\"" + ip + "\",\"ssid\":\"" + jsonEscape(ssid) + "\",\"rssi\":" + String(rssi) +
             ",\"temp\":" + String(tempC, 1) +
             ",\"max_temp\":" + String(maxObservedTemp, 1) +
             ",\"max_limit\":" + String(maxTempLimit, 1) +
             ",\"cpu_freq\":" + String(getCpuFrequencyMhz()) +
             ",\"mdns\":\"http://miniia.local\"" +
             ",\"uptime\":" + String(millis()) +
             ",\"ai_temp\":" + String(aiTemperature, 1) +
             ",\"ai_tokens\":" + String(aiMaxTokens) +
             ",\"gpio2\":" + String(gpio2State ? "true" : "false") +
             ",\"sys_prompt\":\"" + jsonEscape(prefs.getString("sys_prompt", "")) + "\"" +
             ",\"key\":\"" + jsonEscape(maskKey(prefs.getString("gkey", ""))) + "\",\"model\":\"" + jsonEscape(getModel()) +
             "\",\"heap\":" + String((unsigned long)ESP.getFreeHeap()) +
             ",\"total_heap\":" + String((unsigned long)ESP.getHeapSize()) +
             ",\"max_block\":" + String((unsigned long)ESP.getMaxAllocHeap()) +
             ",\"logs\":\"" + jsonEscape(sysLogs) + "\"}";
  sendJson(200, j);
}

void handleAdminGpio() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int state = jsonGet(server.arg("plain"), "state").toInt();
  digitalWrite(CONTROL_PIN, state ? HIGH : LOW);
  sendOk();
}

void handleAdminCli() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String cmd = jsonGet(server.arg("plain"), "cmd");
  String res = executarComandoCLI(cmd);
  if (cmd == "reboot" || cmd == "reboot ") {
    server.send(200, "application/json", "{\"res\":\"Reiniciando...\"}");
    return;
  }
  sendJson(200, "{\"res\":\"" + jsonEscape(res) + "\"}");
}

void handleAdminPing() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String host = jsonGet(server.arg("plain"), "host");
  host.trim(); if(host.length() == 0) host = "google.com";
  IPAddress resolvedIP;
  if (WiFi.hostByName(host.c_str(), resolvedIP)) {
    sendJson(200, "{\"ok\":true, \"ip\":\"" + resolvedIP.toString() + "\"}");
  } else {
    sendErr(500, "Falha na resolução");
  }
}

void handleAdminCpu() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int freq = jsonGet(server.arg("plain"), "freq").toInt();
  if (freq == 80 || freq == 160 || freq == 240) { setCpuFrequencyMhz(freq); sendOk(); }
  else { sendErr(400, "Frequencia invalida"); }
}

void handleAdminDeepSleep() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int sec = jsonGet(server.arg("plain"), "sec").toInt();
  if(sec > 0) { sendOk(); delay(1000); esp_deep_sleep(uint64_t(sec) * 1000000ULL); }
  else { sendErr(400, "Tempo invalido"); }
}

void handleAdminFlush() { if (!authed()) { sendErr(401, "Nao autorizado"); return; } sendOk(); }

void handleAdminMaxTemp() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  float t = jsonGet(server.arg("plain"), "temp").toFloat();
  if (t >= 50.0 && t <= 90.0) { maxTempLimit = t; sendOk(); }
  else { sendErr(400, "Fora da faixa"); }
}

void handleAdminBackup() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String j = "{\"model\":\"" + jsonEscape(getModel()) + "\",\"ssid\":\"" + jsonEscape(prefs.getString("wssid","")) + "\"}";
  sendJson(200, j);
}

void handleScanStart() { if (!authed()) { sendErr(401, "Nao autorizado"); return; } WiFi.scanDelete(); WiFi.scanNetworks(true); sendOk(); }

void handleScan() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  int n = WiFi.scanComplete();
  if (n == WIFI_SCAN_RUNNING) { sendJson(200, "{\"status\":\"busy\"}"); return; }
  if (n < 0) { sendJson(200, "{\"nets\":[]}"); return; }
  String j = "{\"nets\":[";
  for (int i = 0; i < n; i++) {
    if (i > 0) j += ",";
    j += "{\"ssid\":\"" + jsonEscape(WiFi.SSID(i)) + "\",\"rssi\":" + String(WiFi.RSSI(i)) + ",\"lock\":" + (WiFi.encryptionType(i) != WIFI_AUTH_OPEN ? "true" : "false") + "}";
  }
  j += "]}";
  WiFi.scanDelete();
  sendJson(200, j);
}

void handleAdminWifi() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String ssid = jsonGet(server.arg("plain"), "ssid");
  String pass = jsonGet(server.arg("plain"), "pass");
  ssid.trim();
  if (ssid.length() == 0) { sendErr(400, "SSID invalido"); return; }
  prefs.putString("wssid", ssid);
  prefs.putString("wpass", pass);
  sendOk();
  delay(1000);
  ESP.restart();
}

void handleAdminKey() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String body = server.arg("plain");
  String key = jsonGet(body, "key");
  String model = jsonGet(body, "model");
  key.trim(); model.trim();
  if (key.length() > 0) prefs.putString("gkey", key);
  if (model.length() > 0) prefs.putString("model", model);
  sendOk();
}

void handleAdminPass() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  String body = server.arg("plain");
  String currP = jsonGet(body, "curr_pass");
  String newP = jsonGet(body, "new_pass");
  if (currP != getAdminPass()) { sendErr(403, "Senha atual incorreta"); return; }
  if (newP.length() < 4) { sendErr(400, "Senha curta"); return; }
  prefs.putString("apass", newP);
  sendOk();
}

void handleAdminReboot() { if (!authed()) { sendErr(401, "Nao autorizado"); return; } sendOk(); delay(1000); ESP.restart(); }

void handleAdminReset() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  if (jsonGet(server.arg("plain"), "pass") != getAdminPass()) { sendErr(403, "Senha incorreta"); return; }
  prefs.clear(); sendOk(); delay(1000); ESP.restart();
}

void handleUpdate() {
  if (!authed()) { sendErr(401, "Nao autorizado"); return; }
  HTTPUpload& upload = server.upload();
  if (upload.status == UPLOAD_FILE_START) {
    Update.begin(UPDATE_SIZE_UNKNOWN);
  } else if (upload.status == UPLOAD_FILE_WRITE) {
    Update.write(upload.buf, upload.currentSize);
  } else if (upload.status == UPLOAD_FILE_END) {
    Update.end(true);
  }
}

// ----------------------------------------------------
// IMPLEMENTAÇÃO EXATA DO CAPTIVE PORTAL OFICIAL ESP32
// ----------------------------------------------------
boolean isCaptive() {
  // Se não estiver no modo AP, não intercepta
  if (!apMode) return false;
  
  // Se o host requisitado for o IP local do ESP32 ou o domínio mDNS, deixa passar
  String hostHeader = server.hostHeader();
  if (hostHeader == "192.168.4.1" || hostHeader == "miniia.local") {
    return false;
  }
  return true;
}

void handleNotFound() {
  if (isCaptive()) {
    // Redireciona rigorosamente para http://192.168.4.1/ com status 302 (Padrão Oficial)
    server.sendHeader("Location", String("http://192.168.4.1/"), true);
    server.send(302, "text/plain", "");
    return;
  }
  sendErr(404, "Nao encontrado");
}

void connectWiFi() {
  String savedSSID = prefs.getString("wssid", DEFAULT_WIFI_SSID);
  String savedPASS = prefs.getString("wpass", DEFAULT_WIFI_PASS);

  WiFi.mode(WIFI_STA);
  WiFi.begin(savedSSID.c_str(), savedPASS.c_str());
  
  unsigned long t0 = millis();
  while (WiFi.status() != WL_CONNECTED && millis() - t0 < 8000) {
    delay(500);
  }
  
  if (WiFi.status() == WL_CONNECTED) {
    apMode = false;
    configTime(-3 * 3600, 0, "pool.ntp.org", "time.nist.gov");
    addLog("Wi-Fi Conectado. IP: " + WiFi.localIP().toString());
  } else {
    apMode = true;
    WiFi.mode(WIFI_AP_STA);
    WiFi.softAP(AP_SSID, AP_PASS);
    
    // Inicia o DNS Server interceptando todas as consultas para o IP do AP (Exemplo Oficial)
    dnsServer.start(DNS_PORT, "*", WiFi.softAPIP());
    
    addLog("Modo AP Ativado IP: " + WiFi.softAPIP().toString());
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(CONTROL_PIN, OUTPUT);
  digitalWrite(CONTROL_PIN, LOW);

  delay(500);
  prefs.begin("miniia", false);
  connectWiFi();
  
  if (!apMode && MDNS.begin("miniia")) {
    addLog("mDNS ativo");
  }
  
  // INICIA A TASK DO SERVIDOR TCP NO NÚCLEO 0 PARA O TUXASSIST
  xTaskCreatePinnedToCore(
    taskTerminalServer,
    "TerminalTCP",
    4096, // Memória RAM pra task
    NULL,
    1,    // Prioridade
    NULL,
    0     // NÚCLEO 0
  );

  const char* hk[] = {"X-Token"};
  server.collectHeaders(hk, 1);

  // Rotas do painel e API
  server.on("/", HTTP_GET, handleRoot);
  server.on("/api/status", HTTP_GET, handleStatus);
  server.on("/api/setup", HTTP_POST, handleSetup);
  server.on("/api/ask", HTTP_POST, handleAsk);
  server.on("/api/login", HTTP_POST, handleLogin);
  server.on("/api/admin/info", HTTP_GET, handleInfo);
  server.on("/api/admin/gpio", HTTP_POST, handleAdminGpio);
  server.on("/api/admin/cli", HTTP_POST, handleAdminCli);
  server.on("/api/admin/ping", HTTP_GET, handleAdminPing);
  server.on("/api/admin/cpu", HTTP_POST, handleAdminCpu);
  server.on("/api/admin/deepsleep", HTTP_POST, handleAdminDeepSleep);
  server.on("/api/admin/flush", HTTP_GET, handleAdminFlush);
  server.on("/api/admin/maxtemp", HTTP_POST, handleAdminMaxTemp);
  server.on("/api/admin/backup", HTTP_GET, handleAdminBackup);
  server.on("/api/scan_start", HTTP_GET, handleScanStart);
  server.on("/api/scan", HTTP_GET, handleScan);
  server.on("/api/admin/wifi", HTTP_POST, handleAdminWifi);
  server.on("/api/admin/key", HTTP_POST, handleAdminKey);
  server.on("/api/admin/pass", HTTP_POST, handleAdminPass);
  server.on("/api/admin/reboot", HTTP_GET, handleAdminReboot);
  server.on("/api/admin/reset", HTTP_POST, handleAdminReset);
  
  server.on("/api/admin/update", HTTP_POST, []() {
    server.sendHeader("Connection", "close");
    server.send(200, "text/plain", (Update.hasError()) ? "FAIL" : "OK");
    ESP.restart();
  }, handleUpdate);

  // Captura todas as rotas desconhecidas e redireciona pelo Captive Portal oficial
  server.onNotFound(handleNotFound);
  server.begin();
}

void loop() {
  server.handleClient();
  if (apMode) {
    dnsServer.processNextRequest();
  }
}


