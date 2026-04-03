https://deepwiki.com/666ghj/MiroFish

DeepWiki

666ghj/MiroFish
Last indexed: 21 March 2026 (1536a7)

    MiroFish Overview
    Getting Started & Configuration
    System Architecture
    Frontend Application
    Home & Project Initialization
    Graph Construction UI (Step 1)
    Environment Setup UI (Step 2)
    Simulation Execution UI (Step 3)
    Report & Interaction UI (Steps 4 & 5)
    Frontend API Client Layer
    Backend Services
    Knowledge Graph Construction (GraphRAG)
    Simulation Preparation & Management
    Simulation Execution Engine
    Zep Memory Integration
    Report Agent & Analysis
    Backend Utilities & Infrastructure
    Simulation State Files
    Agent Profiles & Action Logs
    Glossary

MiroFish Overview
Relevant source files

MiroFish is a next-generation swarm intelligence engine designed to create high-fidelity digital simulations of real-world scenarios. By ingesting "seed" materials—such as news articles, policy drafts, or financial reports—MiroFish automatically constructs a parallel world populated by thousands of autonomous agents. These agents possess independent personalities, long-term memories, and behavioral logic, allowing them to interact and evolve socially to predict future outcomes.
README.md27-32
README-EN.md27-32

The system serves as a "digital sandbox" where users can observe emergent behaviors from a "God's-eye view" and inject variables to test policy risks, public relations strategies, or creative narratives.
README-EN.md38-41
🔄 The Five-Stage Simulation Lifecycle

MiroFish operates through a structured workflow that transitions from raw data to deep analytical insights:

    Graph Building: Extraction of entities and relationships from source documents to build a Knowledge Graph (GraphRAG) and inject collective memory.
    README-EN.md88
    Environment Setup: Generation of agent personas and platform configurations (Twitter/Reddit) based on the extracted ontology.
    README-EN.md89
    Simulation Execution: Parallel execution of multi-agent interactions across simulated social platforms, with dynamic temporal memory updates.
    README-EN.md90
    Report Generation: The ReportAgent uses a specialized toolset to analyze simulation logs and generate comprehensive predictive reports.
    README-EN.md91
    Deep Interaction: A post-simulation phase where users can chat directly with any agent or the ReportAgent to explore specific nuances.
    README-EN.md92

Sources:
README.md86-93
README-EN.md87-92
🛠 Tech Stack

MiroFish utilizes a decoupled architecture combining modern web technologies with advanced AI orchestration:
Component	Technology
Frontend	Vue.js 3, Vite, D3.js (Visualization), Tailwind CSS
Backend	Python 3.11+, Flask (REST API), UV (Package Management)
Simulation Engine	
OASIS (by CAMEL-AI)
Memory Layer	Zep Cloud (GraphRAG & Episode Storage)
LLM Orchestration	OpenAI-compatible SDK (supports GPT-4o, Qwen-plus, etc.)

Sources:
package.json1-21
backend/pyproject.toml11-35
Dockerfile1-11
🏗 System Integration

The system is split into a Node.js-based frontend (port 3000) and a Python/Flask backend (port 5001). The frontend provides a step-by-step wizard to guide the user through the simulation lifecycle, while the backend manages long-running tasks like document processing, LLM-based persona generation, and simulation execution.
package.json9-11
docker-compose.yml9-11
High-Level Component Interaction

This diagram illustrates how the primary code entities bridge the gap between user intent and the underlying simulation engine.

Diagram: System Entity Mapping

Sources:
package.json9-11
Dockerfile26-29
README-EN.md118-128
Simulation Workflow Logic

The following diagram maps the logical stages of the simulation to the specific backend services and configuration entities defined in the code.

Diagram: Workflow to Code Mapping

Sources:
README-EN.md87-92
backend/pyproject.toml20-24
docker-compose.yml13-14
📖 Major Child Sections

For detailed technical documentation, please refer to the following sub-pages:

    Getting Started & Configuration: Step-by-step setup guide covering environment variables like LLM_API_KEY and ZEP_API_KEY, dependency installation via npm and uv, and deployment options including Docker.
    README-EN.md94-178
    System Architecture: Deep dive into the backend architecture, the three primary API blueprints (graph, simulation, report), and the integration of the OASIS engine and Zep Cloud.
    README-EN.md191-193

This wiki is featured in the repository

Refresh this wiki
On this page

    MiroFish Overview
    🔄 The Five-Stage Simulation Lifecycle
    🛠 Tech Stack
    🏗 System Integration
    High-Level Component Interaction
    Simulation Workflow Logic
    📖 Major Child Sections

Ask Devin about 666ghj/MiroFish
Syntax error in textmermaid version 11.12.3


DeepWiki

666ghj/MiroFish
Last indexed: 21 March 2026 (1536a7)

    MiroFish Overview
    Getting Started & Configuration
    System Architecture
    Frontend Application
    Home & Project Initialization
    Graph Construction UI (Step 1)
    Environment Setup UI (Step 2)
    Simulation Execution UI (Step 3)
    Report & Interaction UI (Steps 4 & 5)
    Frontend API Client Layer
    Backend Services
    Knowledge Graph Construction (GraphRAG)
    Simulation Preparation & Management
    Simulation Execution Engine
    Zep Memory Integration
    Report Agent & Analysis
    Backend Utilities & Infrastructure
    Simulation State Files
    Agent Profiles & Action Logs
    Glossary

Getting Started & Configuration
Relevant source files

This page provides a comprehensive technical guide for setting up the MiroFish development environment and deploying the application using Docker. MiroFish is a decoupled full-stack application consisting of a Vue.js frontend and a Python Flask backend, integrated via a unified task management system.
Environment Requirements

Before installation, ensure your system meets the following version requirements:
Component	Requirement	Role
Node.js	>= 18.0.0	Powers the frontend build system and concurrently runner
package.json15-18
Python	>= 3.11	Required for the Flask backend and OASIS simulation engine
backend/pyproject.toml5
uv	Latest	Fast Python package installer and resolver used in the backend
package.json10
npm	Latest	Package manager for the root and frontend directories
package.json6
Installation Steps

The project uses a root-level package.json to orchestrate dependencies across both the frontend and backend.
1. Clone the Repository

git clone https://github.com/666ghj/MiroFish.git
cd MiroFish

2. Install Dependencies

MiroFish provides helper scripts to automate the installation of Node modules and Python virtual environments.

    Complete Setup: Runs npm install in the root and frontend, and uv sync in the backend
    package.json6-8

    npm run setup:all

    Manual Backend Setup: Uses uv to create a synchronized environment based on pyproject.toml
    backend/pyproject.toml1-35

    cd backend && uv sync

    Manual Frontend Setup:

    cd frontend && npm install

Dependency Data Flow

The following diagram illustrates how dependencies are managed across the system boundaries.

Dependency Management Architecture

Sources:
package.json1-21
backend/pyproject.toml1-35
Dockerfile13-21
Configuration (.env)

MiroFish requires external API keys for LLM reasoning and the Zep Cloud memory layer. Create a .env file in the root directory by copying .env.example
.env.example1-16
Required Variables
Variable	Description	Source/Recommendation
LLM_API_KEY	Primary API key for LLM operations.	Alibaba Bailian (qwen-plus)
.env.example2-4
LLM_BASE_URL	OpenAI-compatible endpoint URL.	https://dashscope.aliyuncs.com/compatible-mode/v1
.env.example5
LLM_MODEL_NAME	The specific model ID to use.	qwen-plus
.env.example6
ZEP_API_KEY	Key for Zep Cloud Knowledge Graph.	getzep.com
.env.example10
Optional Acceleration

To speed up parallel operations (like persona generation), you can define a "Boost" LLM configuration
.env.example12-16 If these keys are not present, the system defaults to the primary LLM_API_KEY.

Sources:
.env.example1-16
Running the Application
Development Mode

The npm run dev command uses concurrently to start both the Flask backend and the Vite-powered frontend simultaneously
package.json9

npm run dev

    Frontend: http://localhost:3000
    Dockerfile26
    Backend API: http://localhost:5001
    Dockerfile26

Execution Flow

The following diagram maps the startup command to the specific code entities responsible for the runtime.

Runtime Execution Map

Sources:
package.json9-11
Dockerfile28-29
Docker Deployment

MiroFish provides a Dockerfile and docker-compose.yml for containerized deployment.
Dockerfile Specification

The Dockerfile uses a multi-stage-like approach within a Python 3.11 base:

    Node.js Installation: Installs Node.js >= 18 for frontend building
    Dockerfile3-6
    uv Integration: Copies the uv binary from the official Astral image for fast Python dependency resolution
    Dockerfile9
    Dependency Caching: Copies package.json and pyproject.toml files first to leverage Docker layer caching
    Dockerfile14-16
    Sync: Runs npm ci and uv sync --frozen to ensure reproducible builds
    Dockerfile19-21

Docker Compose

To deploy the entire stack with a single command:

docker-compose up -d

Configuration in docker-compose.yml:

    Image: ghcr.io/666ghj/mirofish:latest
    docker-compose.yml3
    Environment: Loads the .env file from the host
    docker-compose.yml7-8
    Persistence: Mounts ./backend/uploads to /app/backend/uploads to persist uploaded documents and simulation logs
    docker-compose.yml13-14
    Ports: Maps 3000 (Web UI) and 5001 (API)
    docker-compose.yml10-11

Sources:
Dockerfile1-29
docker-compose.yml1-14
.dockerignore1-24

This wiki is featured in the repository

Refresh this wiki
On this page

    Getting Started & Configuration
    Environment Requirements
    Installation Steps
    1. Clone the Repository
    2. Install Dependencies
    Dependency Data Flow
    Configuration (.env)
    Required Variables
    Optional Acceleration
    Running the Application
    Development Mode
    Execution Flow
    Docker Deployment
    Dockerfile Specification
    Docker Compose

Ask Devin about 666ghj/MiroFish
Getting Started & Configuration | 666ghj/MiroFish | DeepWiki
Syntax error in textmermaid version 11.12.3
Syntax error in textmermaid version 11.12.3