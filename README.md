# EpisodicPivotAnalyser

[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![React](https://img.shields.io/badge/React-18%2F19-61DAFB?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript)](https://www.typescriptlang.org/)
[![SignalR](https://img.shields.io/badge/SignalR-Real--time-blue.svg)](https://dotnet.microsoft.com/apps/aspnet/signalr)

![My GitHub Game](game.gif)

> **Note**: This is a public overview of the private EpisodicPivotAnalyser codebase. The full source code and implementation details are maintained in a private repository.

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

### Data Scale

- **~10 years** of daily OHLC price data per stock
- **100,000+** historical earnings gap-up events analyzed
- **800M+** individual price bars processed
- **100M+** rule evaluations during backtesting
- **5M+** ML training data points

Backtested across multiple market cycles to ensure robustness.

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

## Deterministic Rules + Machine Learning

The system maintains **explainability and auditability** through a hybrid architecture:

**Deterministic Rule Engines** (Primary Logic)
- All buy/sell decisions made by explicit, testable rules
- Each rule implements specific trading patterns or risk management principles
- Composable via Strategy Pattern with full audit trail

**Machine Learning** (Optimization Layer)
- Analyzes historical outcomes to rank rule performance
- Identifies optimal parameter combinations (stop-loss %, volume thresholds, etc.)
- Provides data-driven insights without making direct trading decisions

**Benefits**: Transparency, regulatory compliance, engineer control, and data-driven insights.

### Backtesting

All strategies undergo rigorous historical validation:
1. **Data Collection**: 10 years of OHLC data + earnings announcements
2. **Rule Simulation**: Each rule evaluated against every historical event
3. **Performance Metrics**: Returns, win rates, Sharpe ratios, max drawdown
4. **ML Training**: LightGBM model trained on features + outcomes
5. **Rule Ranking**: Strategies ranked by risk-adjusted returns

---

## Application Portfolio

### EpisodicPivotAnalyser.Alerter (Stock Tipping Engine)

The core production application that generates real-time stock trading signals. Runs as a .NET Hosted Service every 10 minutes (3:30 PM - 11:59 PM weekdays).

**Key Features**:
- Monitors earnings calendar for gap-up events
- Evaluates candidates against 16+ buy rules (gap size, volume, moving averages, etc.)
- Generates alerts for stocks passing all criteria
- Publishes events via RabbitMQ for downstream processing

**Technologies**: .NET 10, Dapper, MassTransit, Serilog

### EpisodicPivotAnalyser.StockData.Api (API Gateway)

Central API gateway and SignalR hub for all frontend applications. Built with ASP.NET Core Minimal APIs.

**Key Features**:
- REST endpoints: alerts, stock data, ML training, rule combinations
- SignalR hubs for real-time updates (sub-100ms latency)
- CORS configuration for multiple frontend origins

**Technologies**: ASP.NET Core 10, SignalR, Swagger/OpenAPI, MassTransit

### EpisodicPivotAnalyser.ML.SellRuleOptimizer

Machine learning service for optimizing sell rule effectiveness using ML.NET and LightGBM.

**Key Features**:
- Extracts 30+ features per earnings event (gap size, volume, ATR, MA distance, etc.)
- Evaluates all 17+ sell rules against historical outcomes
- Trains LightGBM models to predict returns
- Ranks rules by average return, median return, win rate, Sharpe ratio
- Compares time-based exits (5-180 days) vs rule-based exits

**Performance**: Training time ~30-50 seconds (3-4x speedup via parallelization), real-time progress updates via SignalR

**Technologies**: ML.NET 4.0, LightGBM, Parallel Processing

### EpisodicPivotAnalyser.SellRulesFrontEnd

Interactive React 19 web application for analyzing sell rules and ML optimization results.

**Four Primary Views**:

#### 1. Sell Rules Analysis
Visual analysis of historical earnings gap-up events with price charts, performance metrics, and filtering capabilities.

<img width="1445" height="1331" alt="image" src="https://github.com/user-attachments/assets/34b9f733-9ea2-4bcd-a379-ae02c193940d" />

#### 2. ML Optimizer
ML training interface with real-time progress tracking (phases, percentage, ETA) and results display (model metrics, rule rankings, performance comparisons).

<img width="1456" height="1148" alt="image" src="https://github.com/user-attachments/assets/5320785b-9ff6-49de-b004-4733c79fa200" />

#### 3. Rule Combinations
Test combinations of sell rules with performance metrics (returns, win rates, Sharpe ratios), visual summary cards, CSV export, and AI-generated insights.

<img width="1460" height="954" alt="image" src="https://github.com/user-attachments/assets/79ac5986-11f2-437a-8e6d-4fe2b87c0841" />

#### 4. Stock Purchases
Track your stock purchases and receive automatic email alerts when your stocks trigger sell rules.

<img width="1455" height="434" alt="image" src="https://github.com/user-attachments/assets/e984585b-f6be-48a6-8193-020581f3b3b5" />

**Technologies**: React 19, TypeScript 5.7, Vite, Material-UI v6, Lightweight Charts, SignalR

### EpisodicPivotAnalyser.FrontEnd (Primary Dashboard)

Main monitoring dashboard for viewing real-time alerts and system status. Built with React 19 + TypeScript.

<img width="1546" height="1326" alt="image" src="https://github.com/user-attachments/assets/277e9ca1-51ad-4350-9db7-cf8e54af6c5f" />

**Features**: Real-time alert streaming via SignalR (sub-100ms latency), sortable/filterable alert list, stock cards with technical indicators, quick search by symbol

**Technologies**: React 19, TypeScript, Material-UI, SignalR, Axios

### Other Applications

**EpisodicPivotAnalyser.TrainingDashboard**
- Next.js application for manual ML training data annotation
- Side-by-side chart comparison with "Buy"/"Ignore" tagging
- Exports tagged data to JSON for ML training

**EpisodicPivotAnalyser.Common**
- Core business logic shared across all applications
- `IRule` and `ISellRule` interfaces with 16+ buy and 17+ sell rule implementations
- Domain models and technical indicator utilities (leverages Skender.Stock.Indicators)
- Zero framework dependencies (pure domain logic)

**EpisodicPivotAnalyser.Aspire**
- .NET Aspire orchestration for local development
- Starts all services with single `dotnet run` command
- Command-line options: `--all`, `--stockdata`, `--frontends`, `--alerter`, `--verbose`
- Unified observability dashboard with OpenTelemetry

**Supporting Projects**: MassTransit (RabbitMQ integration), Migrations (Fluent Migrator), EarningsCsvExporter, ConsoleApp, Integration/Unit Tests (>80% coverage)

---

## Technology Stack

**Backend**: .NET 10, ASP.NET Core, SignalR, MassTransit, Dapper, ML.NET 4.0, LightGBM, Serilog

**Frontend**: React 18/19, TypeScript 5.7, Next.js, Vite, Material-UI v6, Lightweight Charts, SignalR Client, Axios

**Infrastructure**: SQL Server, Redis, RabbitMQ, .NET Aspire, Polygon.io API, Skender.Stock.Indicators

**DevOps**: OpenTelemetry, Serilog, Azure Pipelines

---

## Key Patterns

- **Strategy Pattern**: Composable rule engines via `IRule` and `IStockPickingStrategy`
- **Repository Pattern**: Data access abstraction
- **Dependency Injection**: Constructor injection throughout
- **Event-Driven Architecture**: Async messaging via MassTransit
- **Ports and Adapters**: Domain logic isolated from frameworks
- **Parallel Processing**: Multi-core CPU utilization for data operations
- **Progress Reporting**: IProgress<T> for long-running operations with real-time UI updates

---

**Inspired by**: Qullamaggie (Episodic Pivot), Mark Minervini (SEPA), William O'Neil (CAN SLIM)

