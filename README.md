# EpisodicPivotAnalyser

[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![React](https://img.shields.io/badge/React-18%2F19-61DAFB?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript)](https://www.typescriptlang.org/)
[![SignalR](https://img.shields.io/badge/SignalR-Real--time-blue.svg)](https://dotnet.microsoft.com/apps/aspnet/signalr)

A production-grade algorithmic trading system that combines deterministic rule engines with machine learning to identify and evaluate stock trading opportunities. The system analyzes earnings gap-up events using 16+ buy rules and 17+ sell rules, all backtested against up to 10 years of historical market data.

---

## System Overview

### Core Architecture

The EpisodicPivotAnalyser is a hybrid trading system that leverages both **deterministic rule-based logic** and **machine learning optimization**. The architecture separates concerns across multiple specialized applications, with the **EpisodicPivotAnalyser.Alerter** serving as the primary stock-tipping engine that continuously monitors the market and generates actionable trading signals.

**Key Architectural Principles:**
- **Deterministic Rule Engines**: All trading strategies are implemented as composable, testable rule sets
- **ML-Enhanced Optimization**: Machine learning ranks rule effectiveness but doesn't replace rule logic
- **Historical Backtesting**: All rules and strategies are validated against up to 10 years of historical data
- **Event-Driven Communication**: Asynchronous message passing via RabbitMQ for loose coupling
- **Real-Time Streaming**: SignalR WebSockets for sub-100ms latency updates to frontends

### Data Scale & Processing

The system processes substantial historical market data to ensure statistical significance and robustness:

- **~10 years** of daily OHLC (Open, High, Low, Close) price data per stock
- **100,000+** historical earnings gap-up events analyzed
- **800,000,000+** individual price bars processed
- **100,000,000+** rule evaluations performed during backtesting
- **5,000,000+** ML training data points for sell rule optimization

This scale ensures that backtested strategies are validated across multiple market cycles, including bull markets, bear markets, and high-volatility periods.

### Architecture Diagram

```
┌───────────────────────────────────────────────────────────────────┐
│                    Frontend Applications                          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐     │
│  │   Primary    │  │ Sell Rules   │  │    Training          │     │
│  │   Dashboard  │  │   Frontend   │  │    Dashboard         │     │
│  │  (React 19)  │  │  (React 19)  │  │    (Next.js)         │     │
│  └──────┬───────┘  └───────┬───────┘ └───────────┬──────────┘     │
└─────────┼──────────────────┼─────────────────────┼────────────────┘
          │                  │                     │
          │                  └─────────┬───────────┘
          │                            │ REST + SignalR (WebSockets)
          │                            │
┌─────────┴────────────────────────────▼────────────────────────────┐
│                  EpisodicPivotAnalyser.StockData.Api              │
│                     (API Gateway & Hub)                           │
│  • REST Endpoints         • SignalR Hubs      • Request Routing   │
└────────┬─────────────────────────────┬────────────────────────────┘
         │                             │
         │                             │
    ┌────▼──────────────┐    ┌─────────▼──────────────┐
    │                   │    │                        │
    │  Alerter          │    │  ML.SellRuleOptimizer  │
    │  (Stock Tipping   │    │  (ML Training &        │
    │   Engine)         │    │   Rule Analysis)       │
    │                   │    │                        │
    │ • Scheduled runs  │    │ • LightGBM training    │
    │ • Rule evaluation │    │ • Feature engineering  │
    │ • Signal gen      │    │ • Performance ranking  │
    └────────┬──────────┘    └─────────┬──────────────┘
             │                         │
             └──────────┬──────────────┘
                        │
              ┌─────────▼──────────┐
              │                    │
              │  Common Library    │
              │                    │
              │ • IRule interface  │
              │ • ISellRule        │
              │ • 16+ Buy Rules    │
              │ • 17+ Sell Rules   │
              │ • Strategy Pattern │
              └─────────┬──────────┘
                        │
         ┌──────────────┴───────────────────┐
         │                                  │
    ┌────▼────────┐   ┌────────────┐   ┌────▼────────┐
    │ SQL Server  │   │  RabbitMQ  │   │   Redis     │
    │             │   │            │   │             │
    │ • Alerts    │   │ • Events   │   │ • Caching   │
    │ • Prices    │   │ • Messages │   │ • Sessions  │
    │ • Training  │   │            │   │             │
    └─────────────┘   └────────────┘   └─────────────┘
```

---

## Deterministic Rules + Machine Learning Hybrid

### The Hybrid Approach

Unlike pure ML "black box" systems, EpisodicPivotAnalyser maintains **explainability and auditability** through a hybrid architecture:

**Deterministic Rule Engines (Primary Logic)**
- All buy and sell decisions are made by explicit, testable rules
- Each rule implements a specific trading pattern or risk management principle
- Rules are composable via the Strategy Pattern
- Every trade decision can be traced to specific rule violations/triggers

**Machine Learning (Optimization Layer)**
- ML analyzes historical outcomes to rank which rules perform best
- Identifies optimal parameter combinations (e.g., stop-loss percentages, volume thresholds)
- Provides data-driven insights for strategy refinement
- Does NOT make trading decisions directly

This approach ensures:
- **Transparency**: Every decision has a clear explanation
- **Regulatory Compliance**: Audit trail for all signals
- **Engineer Control**: Strategies can be refined based on domain expertise
- **Data-Driven Insights**: ML identifies patterns humans might miss

### Backtesting Framework

All strategies undergo rigorous historical validation:

1. **Data Collection**: 10 years of daily OHLC data + earnings announcements
2. **Rule Simulation**: Each rule evaluated against every historical event
3. **Performance Calculation**: Returns computed for multiple holding periods (5-180 days)
4. **Statistical Analysis**: Win rates, Sharpe ratios, max drawdown calculated
5. **ML Training**: LightGBM model trained on features + outcomes
6. **Rule Ranking**: Strategies ranked by risk-adjusted returns

The backtesting engine processes millions of historical data points to ensure statistical significance across bull markets, bear markets, and sideways markets.

---

## Application Portfolio

### EpisodicPivotAnalyser.Alerter (Primary Stock Tipping Engine)

**Role**: The core production application that generates real-time stock trading signals.

**Architecture**: Background service (.NET Hosted Service) that runs on a scheduled basis (every 10 minutes from 3:30 PM to 11:59 PM on weekdays).

**Responsibilities**:
- Monitors earnings calendar for gap-up events (stocks opening significantly higher after earnings)
- Evaluates each candidate against 16+ buy rules (gap size, volume, moving averages, technical indicators)
- Generates alerts for stocks that pass all buy criteria
- Persists alerts to database for consumption by frontends
- Publishes events via RabbitMQ for downstream processing

**Key Technologies**: .NET 10, Hosted Services, Dapper ORM, MassTransit, Serilog

**Rule Examples**:
- `GapUpRule`: Validates minimum gap percentage
- `PricePercentChangeRule`: Checks price momentum
- `AverageDailyRangeRule`: Ensures sufficient volatility
- `MovingAverageCrossRule`: Confirms trend alignment

### EpisodicPivotAnalyser.StockData.Api (API Gateway)

**Role**: Central API gateway and SignalR hub for all frontend applications.

**Architecture**: ASP.NET Core Web API with minimal API endpoints and SignalR hubs.

**Responsibilities**:
- REST endpoints for alert history, stock data, ML training, rule combinations
- SignalR hubs for real-time alert broadcasting and ML training progress
- Request routing to backend services (Alerter, ML Optimizer)
- CORS configuration for multiple frontend origins

**Key Endpoints**:
- `/api/v1/alerts` - Alert history and search
- `/api/v1/ml/sell-rule-optimizer` - ML training and rule rankings
- `/api/v1/rule-combinations` - Backtest rule combinations
- `/api/v1/stocks` - Stock price and technical data
- `/hubs/training-progress` - Real-time ML training updates (SignalR)

**Key Technologies**: ASP.NET Core 10, SignalR, Swagger/OpenAPI, MassTransit

### EpisodicPivotAnalyser.ML.SellRuleOptimizer

**Role**: Machine learning service for optimizing sell rule effectiveness.

**Architecture**: ML.NET library with LightGBM regression models.

**Responsibilities**:
- Loads historical earnings gap-up data from JSON files
- Extracts 30+ features per event (gap size, volume ratio, ATR, MA distance, etc.)
- Evaluates all 17+ sell rules against historical outcomes
- Trains LightGBM models to predict returns based on features
- Ranks rules by average return, median return, win rate, Sharpe ratio
- Compares time-based exits (5, 10, 15... 180 days) vs rule-based exits

**ML Pipeline**:
1. **Data Preparation** (parallel processing): Feature engineering for ~100k+ events
2. **Training** (LightGBM): Gradient boosting with 100 iterations
3. **Evaluation**: Test set performance metrics (R², RMSE, MAE)
4. **Ranking**: Sort rules by risk-adjusted performance

**Key Technologies**: ML.NET 4.0, LightGBM, Parallel Processing, IProgress<T> for real-time updates

**Performance Metrics**:
- Training time: ~30-50 seconds (down from 90-120s via parallelization)
- Feature engineering: 3-4x speedup using all CPU cores
- Real-time progress updates via SignalR (phase, percentage, ETA)

### EpisodicPivotAnalyser.SellRulesFrontEnd (Sell Rules Analysis & ML Optimizer)

**Role**: Interactive web application for analyzing sell rules and ML optimization results.

**Architecture**: React 19 + TypeScript SPA with Material-UI components and Lightweight Charts.

**Three Primary Views**:

#### 1. Sell Rules Analysis
Visual analysis of historical earnings gap-up events and their outcomes.

![Sell Rules Analysis](src/EpisodicPivotAnalyser.SellRulesFrontEnd/screenshots/sell-rules-analysis.png)

**Features**:
- List of historical gap-up events with stock cards
- Price charts showing entry points and sell signals
- Performance metrics: realized returns, holding days, rule violations
- Filter by symbol, date range, rule violations

#### 2. ML Optimizer
Machine learning training interface with real-time progress tracking.

![ML Optimizer](src/EpisodicPivotAnalyser.SellRulesFrontEnd/screenshots/ml-optimizer.png)

**Features**:
- "Train Model" button to initiate ML training
- **Real-time progress dialog** with:
  - Current phase indicator (Data Loading → Feature Engineering → Training → Evaluation → Ranking)
  - Linear progress bar with percentage
  - Estimated time remaining (ETA)
  - Step counters (e.g., "453 of 1,000 gaps processed")
  - Warning/error display
- **Results display**:
  - Model performance metrics (R², RMSE, MAE)
  - Rule rankings table sorted by predicted return
  - Performance comparison: time-based vs rule-based exits
  - Win rate percentages and risk metrics

The progress dialog provides complete visibility into the ML training process, updating every 0.5-1 seconds via SignalR WebSocket connection.

#### 3. Rule Combinations
Interactive tool for testing combinations of sell rules and analyzing synergistic effects.

![Rule Combinations](src/EpisodicPivotAnalyser.SellRulesFrontEnd/screenshots/rule-combinations.png)

**Features**:
- **Rule Selection Interface**: 
  - Checkboxes for 16+ available rules
  - Stop Loss Rule (4%) automatically included
  - Create multiple test combinations
- **Performance Metrics**:
  - Average return, median return, win rate
  - Sharpe ratio, standard deviation, max drawdown
  - Max/min returns, total trades
  - Average holding days
  - Stop loss trigger count and percentage
- **Visual Summary Cards**: Highlight best performers
- **CSV Export**: Download results for further analysis
- **Insights Engine**: AI-generated insights on rule effectiveness and synergies

**Key Technologies**: React 19, TypeScript 5.7, Vite, Material-UI v6, Lightweight Charts, SignalR Client, Axios

### EpisodicPivotAnalyser.FrontEnd (Primary Dashboard)

**Role**: Main monitoring dashboard for viewing real-time alerts and system status.

**Architecture**: React 19 + TypeScript SPA with Material-UI.

**Features**:
- Real-time alert streaming via SignalR (sub-100ms latency)
- Alert list with sorting, filtering, pagination
- Stock cards with technical indicators
- Quick search by symbol
- Market status indicators

**Key Technologies**: React 19, TypeScript, Material-UI, SignalR, Axios

### EpisodicPivotAnalyser.TrainingDashboard

**Role**: Manual annotation tool for creating ML training datasets.

**Architecture**: Next.js application with server-side rendering.

**Features**:
- Side-by-side chart comparison for buy candidates
- Manual "Buy" / "Ignore" tagging interface
- Exports tagged data to JSON for ML training
- Enables human-in-the-loop dataset curation

**Key Technologies**: Next.js, React, TypeScript, Chart.js

### EpisodicPivotAnalyser.Common (Shared Domain Library)

**Role**: Core business logic shared across all applications.

**Architecture**: Class library with zero framework dependencies (pure domain logic).

**Contains**:
- `IRule` and `ISellRule` interfaces
- 16+ buy rule implementations (e.g., `GapUpRule`, `MovingAverageCrossRule`)
- 17+ sell rule implementations (e.g., `StopLossRule`, `TrailingStopRule`, `ProfitTargetRule`)
- `IStockPickingStrategy` for rule composition
- Domain models (Stock, Alert, PriceBar, etc.)
- Service interfaces (repository abstractions)
- Technical indicator utilities (leveraging Skender.Stock.Indicators)

**Design Patterns**: Strategy Pattern, Repository Pattern, Dependency Inversion

### EpisodicPivotAnalyser.Aspire (Orchestration)

**Role**: .NET Aspire orchestration host for local development.

**Architecture**: .NET Aspire AppHost project.

**Responsibilities**:
- Starts all services with a single `dotnet run` command
- Manages dependencies (SQL Server, Redis, RabbitMQ containers)
- Configures service discovery and environment variables
- Provides unified observability dashboard (OpenTelemetry)
- Aggregates logs from all services (Serilog integration)

**Command-Line Options**:
- `--all`: Start entire stack (API + 3 frontends + Alerter + infrastructure)
- `--stockdata`: API only
- `--frontends`: All 3 frontends only
- `--alerter`: Alerter with dependencies
- `--verbose`: Enable debug logging

### Supporting Projects

**EpisodicPivotAnalyser.MassTransit**
- RabbitMQ integration via MassTransit
- Event consumers and publishers
- Message contract definitions

**EpisodicPivotAnalyser.Migrations**
- Database migration scripts (SQL Server)
- Schema versioning with Fluent Migrator

**EpisodicPivotAnalyser.EarningsCsvExporter**
- Utility for exporting earnings data to CSV
- Used for external analysis and reporting

**EpisodicPivotAnalyser.ConsoleApp**
- CLI tools for testing and data operations
- Contains historical JSON training data

**Integration.Tests / EpisodicPivotAnalyser.Common.Tests / EpisodicPivotAnalyser.Alerter.Tests**
- Comprehensive test suites (>80% coverage on core logic)
- Unit tests for individual rules
- Integration tests for API contracts (OpenAPI validation)
- End-to-end tests for rule engine workflows

---

## Technology Stack

### Backend (.NET)
- **.NET 10**: Modern cross-platform runtime with C# 13
- **ASP.NET Core**: Minimal APIs with endpoint routing
- **SignalR**: Real-time WebSocket communication
- **MassTransit**: Message bus abstraction over RabbitMQ
- **Dapper**: High-performance micro-ORM for SQL Server
- **ML.NET 4.0**: Cross-platform machine learning framework
- **LightGBM**: Fast gradient boosting for structured data
- **Serilog**: Structured logging with enrichers and sinks

### Frontend (JavaScript/TypeScript)
- **React 18/19**: Modern hooks-based UI framework
- **TypeScript 5.7**: Type-safe JavaScript with strict mode
- **Next.js**: React framework with SSR for TrainingDashboard
- **Vite**: Fast HMR and optimized production builds
- **Material-UI v6**: Professional component library
- **Lightweight Charts**: High-performance financial charting
- **SignalR Client**: WebSocket client library
- **Axios**: HTTP client for REST APIs

### Infrastructure & Data
- **SQL Server**: Relational database for alerts, prices, training data
- **Redis**: In-memory cache for frequently accessed reference data
- **RabbitMQ**: Message broker for async event distribution
- **.NET Aspire**: Local orchestration with Docker Compose-like experience
- **Polygon.io API**: Market data feed (OHLC, earnings, fundamentals)
- **Skender.Stock.Indicators**: 50+ technical analysis indicators

### DevOps & Observability
- **OpenTelemetry**: Distributed tracing and metrics collection
- **Serilog**: Structured logging with correlation IDs
- **Azure Pipelines**: CI/CD automation (azure-pipelines.yml)
- **.NET Aspire Dashboard**: Real-time service health monitoring

---

## Key Architectural Patterns

- **Strategy Pattern**: Composable rule engines via `IRule` and `IStockPickingStrategy`
- **Repository Pattern**: Data access abstraction with interface-based contracts
- **Dependency Injection**: Constructor injection throughout (avoiding service locator anti-pattern)
- **CQRS-lite**: Separate read models (dashboards) from write models (Alerter state)
- **Event-Driven Architecture**: Async messaging via MassTransit for loose coupling
- **Ports and Adapters (Hexagonal)**: Domain logic in Common library, isolated from frameworks
- **Parallel Processing**: Multi-core CPU utilization for data-intensive operations
- **Progress Reporting**: IProgress<T> pattern for long-running operations with real-time UI updates

---

**Inspired by**: Qullamaggie (Episodic Pivot), Mark Minervini (SEPA), William O'Neil (CAN SLIM)

