repository https://github.com/openclaw/openclaw

site https://openclaw.ai/

docs https://docs.openclaw.ai/


 🦞
OpenClaw: The Open-Source Autonomous AI Assistant and Engine Guide
45 источников

OpenClaw представляет собой передовую платформу для создания автономных ИИ-агентов, способных выполнять сложные задачи непосредственно в операционной системе пользователя. В отличие от обычных чат-ботов, эта система обладает модульной архитектурой навыков, позволяющей автоматизировать работу с GitHub, управление Docker-контейнерами и генерацию медиаконтента. Уникальная концепция SOUL MD обеспечивает агентам постоянную память и индивидуальность через текстовые файлы, что превращает их в полноценных цифровых сотрудников.

Для стабильной работы инструмента требуется Node.js версии 22 и выше, а установка возможна как на локальные машины, так и через специализированные облачные хостинги. Важнейшим аспектом эксплуатации является безопасность, поскольку широкие полномочия агентов требуют строгой изоляции в контейнерах и тщательного аудита сторонних расширений. Несмотря на высокую эффективность, пользователи должны учитывать значительный расход API-токенов и необходимость грамотной настройки лимитов для контроля затрат. Проект демонстрирует стремительный рост сообщества, предлагая гибкую альтернативу закрытым облачным решениям в эпоху локального искусственного интеллекта.
Какие 10 навыков OpenClaw наиболее полезны для разработчиков в 2026 году?
Как защитить OpenClaw от уязвимостей и кражи данных?
В чем разница между OpenClaw и Memu.bot в плане памяти и затрат?
Сегодня • 13:00

How to setup it
Because the name OpenClaw refers to two distinct projects—a popular AI assistant framework and a game engine reimplementation for the 1997 classic Captain Claw—the setup process depends on which one you intend to use.
1. Setting up the OpenClaw AI Assistant
OpenClaw is a self-hosted AI agent that runs as a persistent background process (Gateway) on your local hardware or a VPS.
Prerequisites:

    Node.js: You must have Node.js version 22 or higher installed.
    Operating System: Supported natively on macOS and Linux. Windows users are strongly recommended to use WSL2 for a stable Unix-like environment.
    Hardware: At least 2 GB of RAM is the absolute floor, but 8 GB to 16 GB is recommended for stable daily workflows.

Installation Steps:

    Run the Installer Script: This is the fastest method as it handles dependencies and PATH configuration automatically.
        macOS / Linux / WSL2: curl -fsSL https://openclaw.ai/install.sh | bash.
        Windows (PowerShell): iwr -useb https://openclaw.ai/install.ps1 | iex.
    Onboarding: Once installed, run openclaw onboard in your terminal. This interactive wizard will guide you through:
        Configuring your LLM provider (e.g., Anthropic Claude, OpenAI GPT, or local models via Ollama).
        Connecting messaging channels (WhatsApp, Telegram, Discord, etc.).
        Setting up your workspace and initial skills.
    Start the Gateway: To begin using your agent, start the control plane with openclaw gateway start. To ensure it stays running in the background and survives reboots, use openclaw onboard --install-daemon.
    Verify Setup: Access the web dashboard at http://localhost:18789/ to manage your agent and view real-time logs.


--------------------------------------------------------------------------------
2. Setting up the OpenClaw Game Engine
This project is an open-source C++ reimplementation of the engine used for the 1997 game Captain Claw.
Legal Prerequisite: The engine does not include copyrighted game assets. You must legally own a copy of the original Captain Claw to provide the essential CLAW.REZ file.
Installation Steps (Pre-compiled Binaries):

    Download: Navigate to the official OpenClaw GitHub repository (pjasicek/OpenClaw) and download the release archive for your operating system.
    Extract: Unzip the contents into a dedicated project folder (e.g., C:\Games\OpenClaw).
    Integrate Assets: Locate the CLAW.REZ file from your original game CD or digital backup and copy it into the same folder as the OpenClaw executable.
    Launch: Double-click the executable (e.g., OpenClaw.exe on Windows or ./openclaw on Linux) to play.

Building from Source (Advanced): If you prefer to compile the engine yourself, you will need Git, CMake, and the SDL2 library suite.

    Clone the repository: git clone --recursive https://github.com/pjasicek/OpenClaw.git.
    Configure and Build: Use CMake to generate a build directory and then use make (Linux/macOS) or Visual Studio (Windows) to compile the binary.

Как настроить ИИ-ассистента через npx openclaw onboard?
Где взять файл CLAW.REZ для запуска игрового движка?
Как редактировать config.xml для смены разрешения или управления?

minimal requirements
The term OpenClaw refers to two unrelated projects: a modern AI assistant framework and a game engine reimplementation for the 1997 classic Captain Claw. The minimal requirements for each are as follows:
1. OpenClaw AI Assistant (Autonomous Agent)
OpenClaw is a persistent background process (Gateway) that orchestrates AI tasks across various messaging channels.

    Node.js: You must have Node.js version 22 or higher (v22.14+ is specifically cited, with Node 24 recommended). Older versions lack the JavaScript features required for the agent to run and will cause syntax errors.
    Memory (RAM):
        Absolute Minimum: 2 GB RAM. Attempting to run with less than 2 GB frequently results in "JavaScript heap out of memory" errors or crashes during the onboarding process.
        Recommended Baseline: 4 GB to 8 GB RAM is suggested for cloud-based automation and light tool usage.
        Production/Local LLM usage: If you intend to run local models (e.g., via Ollama) or heavy automation, 16 GB to 32 GB RAM is considered the realistic floor for responsive execution.
    Processor (CPU): 1 to 2 vCPUs (or cores) are sufficient for general orchestration. CPU power is rarely the bottleneck unless you are running local models instead of cloud APIs.
    Storage: At least 500 MB of free disk space is required for the installation itself, but a minimum of 20 GB SSD is recommended to accommodate logs, skill data, and caches.
    Operating System:
        macOS and Linux are supported natively.
        Windows users are strongly advised to use WSL2 (Windows Subsystem for Linux), as native Windows installation is problematic and officially unsupported.


--------------------------------------------------------------------------------
2. OpenClaw Game Engine (Captain Claw Reimplementation)
This is an open-source C++ reimplementation of the engine used for the 1997 platformer Captain Claw.

    Legal Game Assets: You must legally own a copy of the original 1997 game to provide the CLAW.REZ file. The engine does not include copyrighted assets (graphics, sounds, or levels) and will not function without this file.
    GPU: A graphics card supporting OpenGL 3.3 or higher is required to handle the modernized rendering pipeline.
    Memory (RAM):
        Running the game: Generally requires less than 512 MB RAM.
        Building from source: At least 4 GB RAM is recommended to ensure the compilation and linking processes do not stall.
    CPU: A dual-core processor is recommended to handle the game's logic and audio mixing threads separately.
    Operating System: Native support is available for Windows 10/11, macOS (including Apple Silicon), and Linux (including Raspberry Pi architectures).
    Software Dependencies (for Source Builds): If compiling the engine yourself, you require CMake 3.10+, a C++17 compiler (like Visual Studio 2017+ on Windows or GCC/Clang on Linux), and the SDL2 development library suite (including SDL2_image, SDL2_mixer, and SDL2_ttf).

How do I fix the 'Could not load CLAW.REZ' error?
What is the difference between the AI and game projects?
How can I run OpenClaw using Docker or WSL2?

use cases
The name OpenClaw refers to two distinct projects with different use cases: a versatile AI assistant framework designed to automate developer and personal workflows, and a game engine reimplementation for the 1997 classic Captain Claw.
1. OpenClaw AI Assistant Use Cases
OpenClaw is a "kinetic" agent capable of performing multi-step actions across various digital services. Its modular skills system allows users to tailor the agent to specific professional and personal needs.
Developer and DevOps Automation

    Git and GitHub Management: The agent can clone repositories, create branches, stage changes, push code, and open pull requests conversationally.
    Codebase Understanding: Using skills like AgentLens, it can generate structural overviews of project directories, summarize file purposes, and find where specific modules (like API routes) are defined.
    Infrastructure and CI/CD Monitoring: It can monitor logs from services like Jenkins or Sentry, summarize errors, and notify developers via messaging apps if a server's CPU spikes.
    Container Orchestration: Through the Docker Manager skill, it can build, start, and debug containers, or show the last 50 lines of logs from a database container.
    Automated Testing: It provides a unified interface to run test suites (e.g., PyTest, Jest), parse output, and suggest fixes for failures.

Research and Knowledge Management

    Technical and Web Search: It uses AI-optimized tools like Tavily or Exa to research libraries, find documentation, or see how other projects solved specific technical problems.
    Information Synthesis: It can turn long articles, meeting notes, or research papers into structured summaries.
    Knowledge Base Organization: It integrates with Obsidian to update personal notes or uses the Ontology skill to map complex concepts into knowledge graphs.

Productivity and Business Processes

    Google Workspace Integration: The agent acts as an operator for Gmail, Calendar, Drive, and Sheets, allowing users to check for schedule conflicts or draft emails from the terminal.
    Morning Briefings: It can wake up on a schedule (e.g., 7:00 AM) to pull calendar events, weather, and news, sending a single digest to WhatsApp or Telegram.
    Lead Generation and CRM: Users leverage it to automate prospect research, website auditing, and updating project boards in Linear or Monday.com.
    Complex Negotiations: It has been used to research car prices and inventory to negotiate deals with dealerships, reportedly saving users thousands of dollars.

Media and Creative Workflows

    AI Media Generation: Through the PixVerse skill, it generates text-to-video clips, animates static images, and handles video post-processing like upscaling or AI lip-sync.
    Presentation Creation: It can generate professional, editable PowerPoint decks from text prompts.
    Voice and Communication: It supports text-to-speech for reading responses aloud or sending PDFs as physical letters via the agentic-letters skill.


--------------------------------------------------------------------------------
2. OpenClaw Game Engine Use Cases
This project is an open-source C++ reimplementation of the engine used for the 1997 game Captain Claw.

    Modern Playability: It allows the game to run natively on Windows 10/11, macOS (including Apple Silicon), and Linux without the compatibility issues of 90s-era code.
    High-Resolution and Widescreen Support: The engine enables playing at arbitrary resolutions, including 4K and ultra-wide monitors, and integrates improved rendering for sharper pixel art.
    Modern Controller Support: It uses the SDL2 library to provide native support for modern gamepads like the Xbox Series X or DualSense.
    Custom Content and Modding: It is designed to be compatible with community level editors like WapMap, allowing users to load and play custom .WWD level files.
    Cross-Platform Portability: It can be compiled for Raspberry Pi for compact retro setups or into WebAssembly (WASM) to be played directly in a modern web browser.
    Infrastructure Integration: Interestingly, the AI assistant project can be used in tandem with the game engine to automate the deployment of game servers or generate custom levels based on XML schemas.

Как подключить каналы связи вроде Telegram или WhatsApp?
Какие преимущества дает использование локальных моделей через Ollama?
Как настроить автоматизацию через Heartbeats и Cron?

best practices
Best practices for OpenClaw differ significantly between the AI assistant framework and the game engine reimplementation.
1. OpenClaw AI Assistant (Autonomous Agent)
Given that this framework has system-level access to your machine, security and resource management are the primary focuses.
Security and Network Hardening

    Never expose the Gateway to the public internet: Bind the Gateway to localhost (127.0.0.1) by default. If you need remote access, use a secure private tunnel like Tailscale or SSH port forwarding instead of opening ports.
    Patch immediately: Ensure you are running version 2026.1.29 or higher to mitigate critical vulnerabilities like CVE-2026-25253, which allowed for remote code execution via malicious links.
    Treat skills as untrusted code: Review the source code of any third-party skill from ClawHub before installation, as researchers found a significant percentage contained malware or credential-leaking flaws. Use tools like SkillCheck or Snyk to scan them.
    Isolate the environment: Run OpenClaw in a dedicated container (Docker) or on a separate machine (like a Mac Mini or VPS) rather than your primary workstation.

Configuration and Efficiency

    Use the Primary + Fallback model: Configure a primary model (e.g., Claude 3.5 Sonnet) and at least one fallback (e.g., GPT-4o) in openclaw.json to ensure the agent remains operational if one provider hits rate limits or outages.
    Optimize token costs: Use the cheapest capable models (like GPT-4o-mini) for routine tasks such as heartbeats and cron jobs. Enable context pruning or compaction to prevent conversation histories from growing too large and expensive.
    Protect sensitive keys: Never store API keys in plain text within openclaw.json or commit them to Git. Use a dedicated .env file or a secrets manager like HashiCorp Vault.

Identity and Behavioral Guardrails

    Be specific in SOUL.md: Instead of vague instructions, define a clear role, tone, and decision-making framework (e.g., "Atlas, a senior engineer who values clean architecture").
    Enforce strict boundaries: Explicitly instruct the agent in its configuration to never execute destructive commands (like rm -rf or DROP TABLE) without manual human-in-the-loop confirmation.


--------------------------------------------------------------------------------
2. OpenClaw Game Engine (Captain Claw Reimplementation)
Best practices for the game engine focus on asset integrity and modern hardware optimization.
Asset Management

    Verify file integrity: Ensure your CLAW.REZ file is not corrupted, as this is the leading cause of segmentation faults and crashes.
    Maintain directory hierarchy: Place the CLAW.REZ file in the same folder as the OpenClaw executable for the engine to locate it correctly during the default initialization sequence.

Technical Optimization

    Fix blurriness on Windows: If using a 4K monitor, override High-DPI scaling behavior in the executable properties and set it to "Application" to keep the pixel art sharp.
    Prevent screen tearing: Enable V-Sync in the config.xml file to synchronize the frame rate with your monitor's refresh rate.
    Restore background music: On Linux, you must manually install a MIDI synthesizer like Timidity++ or FluidSynth to hear the game's soundtrack.

System Safety

    Manage antivirus flags: Because OpenClaw is an unsigned open-source binary, it may be flagged as a false positive. If you downloaded it from the official GitHub, it is safe to add an exception in your security software.

Как настроить OpenClaw через установщик?
Какие системные требования нужны для OpenClaw?
Как подключить LLM-провайдера к помощнику?
41 источник
Студия
Аудиопересказ
Презентация
Видеопересказ
Ментальная карта
Отчеты
Карточки
Тест
Инфографика
Таблица данных
Выходные данные из студии будут сохраняться здесьДобавив источники, используйте кнопки для создания аудиопересказа, методички, ментальной карты и т. д.
Ответы NotebookLM могут быть неточны. Обязательно проверяйте их.