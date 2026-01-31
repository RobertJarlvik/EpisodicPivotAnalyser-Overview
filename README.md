# EpisodicPivotAnalyser

[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![React](https://img.shields.io/badge/React-18%2F19-61DAFB?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript)](https://www.typescriptlang.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](#license)

---

### TL;DR

**EpisodicPivotAnalyser** is an event-driven, machine learning-powered trading analysis platform for identifying episodic market pivots, built with .NET 10 and React.

It ingests earnings events and market data, evaluates **rule-based and quantitative trading logic**, and delivers real-time alerts and sell signals through multiple frontends.  
The system is designed with **clean architecture, shared domain logic, microservice principles, and observability-first tooling**.

> This public repository documents **architecture, system design, and technical decisions**.  
> The full implementation and proprietary trading logic are maintained in **private repositories**.

---

> ⚠️ **Public Overview Repository**  
> This repository serves as a **public technical overview and architectural documentation** of the *EpisodicPivotAnalyser* platform.  
> **The full source code, implementations, and proprietary logic are maintained in private repositories and are not publicly available.**

A sophisticated automated algorithmic trading system designed to identify and analyze stock opportunities based on the **Episodic Pivot** trading strategy. The system automatically scans for stocks with earnings gaps, evaluates multiple technical trading rules, and provides professional trading dashboards with real-time alerts and comprehensive sell signal analysis.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Architecture Summary](#architecture-summary)
- [Features](#features)
- [Roadmap](#Roadme)
- [Acknowledgments](#Acknowledgments)

---

## 🎯 Project Overview

**EpisodicPivotAnalyser** is an end-to-end automated trading analysis platform that combines data collection, algorithmic rule evaluation, and multi-frontend presentation. It is specifically designed for traders following the **Episodic Pivot strategy** (influenced by Qullamaggie's gap-up trading methodology).

### What Problem Does It Solve?

The project addresses several key challenges in algorithmic trading:

1. **Automated Stock Screening**: Continuously monitors earnings calendars and identifies stocks matching specific technical patterns (gap-ups, volume spikes, moving average breakouts)
2. **Rule-Based Analysis**: Evaluates 10+ trading rules automatically to filter high-probability setups
3. **Real-Time Alerts**: Sends email notifications and provides web dashboards for immediate action
4. **Sell Signal Detection**: Monitors positions and alerts traders when predefined exit conditions are met
5. **Multi-Frontend Access**: Provides professional trading dashboards optimized for different workflows (alerts, sell analysis, exploratory research)

### Core Trading Strategy

The **Episodic Pivot** strategy focuses on:
- Stocks gapping up on earnings announcements
- Strong volume confirmation
- Technical breakout patterns (moving averages, relative strength)
- Risk-managed entries with defined stop losses
- R-multiple based profit targets (2R, 3R, 5R)

---

## 🏗️ Architecture Summary

EpisodicPivotAnalyser follows a **hybrid microservices + shared library architecture** with clear separation of concerns:

```
┌────────────────────────────────────────────────────────────────┐
│                     FRONTENDS (User Layer)                     │
├──────────────────┬──────────────────┬──────────────────────────┤
│  FrontEnd        │  SellRulesFrontEnd│  Gui (Next.js)          │
│  (React+TS+Vite) │  (React+TS+Vite) │  (Next.js+Tailwind)      │
└────────┬─────────┴────────┬─────────┴───────────┬──────────────┘
         │                  │                     │ HTTP/REST
         └──────────────────┼─────────────────────┘
                           │
                ┌──────────▼──────────────┐
                │  StockData.Api (ASP.NET)│◄─── Port 5237
                │  ├─ /alerts             │
                │  ├─ /earnings-whisper   │
                │  ├─ /sell-rules         │
                │  └─ /stock-data         │
                └──────────┬──────────────┘
                           │ (Repositories)
         ┌─────────────────┼─────────────────┐
         │                 │                 │
    ┌────▼────────┐  ┌─────▼──────┐  ┌──────▼──────┐
    │  Common Lib │  │  Alerter   │  │  EarningsCsv│
    │  (Core)     │  │ (Service)  │  │  Exporter   │
    │ ├ Rules     │  │ Runs 10min │  └─────────────┘
    │ ├ SellRules │  │ Post-Market│
    │ ├ Services  │  └────┬───────┘
    │ └ Models    │       │
    └────┬────────┘       │ (MassTransit)
         │                │
    ┌────▼────────────────▼─────────┐
    │   SQL Server Database          │
    │  ├ AlertResult                 │
    │  ├ EarningsWhisper             │
    │  ├ EarningsScore               │
    │  └ TrainingData                │
    └────────────────────────────────┘

         Optional Infrastructure:
    ┌─────────────────────────────────┐
    │  RabbitMQ (Message Bus)         │
    │  Redis (Caching)               │
    │  Aspire (Local Orchestration)  │
    └─────────────────────────────────┘
```

### Sub-Applications & Modules

#### 1. **EpisodicPivotAnalyser.Alerter** (Hosted Service)
- **What it does**: Core alert generation engine that runs on a scheduled timer (every 10 minutes during post-market hours: 3:30 PM - 11:59 PM weekdays)
- **Input**: Earnings calendar from Polygon API, stock OHLCV data, trading rules configuration
- **Output**: AlertResult records in database, email notifications to configured recipients
- **How it fits**: This is the **heart of the system** - the single source of truth for trading signals. All business logic resides here.

**Technologies**: .NET 10, Serilog, MassTransit, Selenium WebDriver

#### 2. **EpisodicPivotAnalyser.StockData.Api** (REST API)
- **What it does**: Serves stock alerts, earnings data, and sell signals to frontend applications via REST endpoints
- **Input**: HTTP requests from frontends (GET /alerts, GET /sell-rules/{ticker}, POST /buy-stock, etc.)
- **Output**: JSON responses with alerts, earnings whisper data, sell rule evaluations, historical stock data
- **How it fits**: Acts as the **data gateway** between the backend services/database and frontend UIs. Includes mock data fallback for development.

**Technologies**: ASP.NET Core 10, Swagger/OpenAPI, Skender.Stock.Indicators

**Key Endpoints**:
```
GET  /alerts                           → List all stock alerts
GET  /earnings-whisper/{ticker}        → Earnings surprise data
GET  /earnings-candidates/{ticker}     → Earnings score/percentile
POST /buy-stock                        → Mark alert as buy (user action)
POST /ignore-stock                     → Mark alert as ignore (user action)
GET  /api/sell-rules/{ticker}          → Evaluate all sell rules for a ticker
GET  /api/sell-signals                 → Recent sell signals across all stocks
GET  /api/stock-data/historical        → Historical OHLCV data
```

#### 3. **EpisodicPivotAnalyser.Common** (Shared Library)
- **What it does**: Core domain models, repositories, services, and business logic. **Single source of truth** for all rule evaluations.
- **Input**: Stock data from APIs (Polygon, Stocktwits), database connections
- **Output**: Validated stock data, rule evaluation results, sell signal assessments
- **How it fits**: Shared by all backend components (Alerter, API, ConsoleApp). Contains no UI code, only pure business logic.

**Key Components**:
- **Rules**: 10+ trading rules (GapUpRule, EpisodicPivotRules, VolumeRule, MovingAverageRule, etc.)
- **SellRules**: 7 sell signal detectors (Moving Average Crossover, Climax Volume, Failure to Hold Gap, Reversal Patterns, Profit Targets)
- **Services**: EarningsService, StockDataService, RuleService, EarningsWhisperService (web scraping), MessageService
- **Repositories**: AlertResultRepository, EarningsWhisperRepository, EarningsScoreRepository, FundamentalDataRepository

**Technologies**: .NET 10, Dapper ORM, Selenium WebDriver 4.40, System.Data.SqlClient

#### 4. **EpisodicPivotAnalyser.FrontEnd** (React SPA)
- **What it does**: Professional dark-mode trading dashboard for viewing real-time alerts and taking buy/ignore actions
- **Input**: User interactions (clicks, tab navigation), API responses from StockData.Api
- **Output**: Rendered HTML/CSS/JS dashboard, HTTP POST requests for user actions (buy/ignore)
- **How it fits**: Primary user interface for traders to **view and act on alerts**. Auto-refreshes every 20 seconds.

**Technologies**: React 18, TypeScript, Vite, Material UI v6, Axios

**Features**:
- Dark theme inspired by TradingView/Bloomberg Terminal
- Tab navigation: All Alerts, Buy List, Ignore List
- Stock detail modal with comprehensive earnings data
- Auto-refresh functionality
- Loading states and skeleton screens

#### 5. **EpisodicPivotAnalyser.SellRulesFrontEnd** (React SPA)
- **What it does**: Stock analysis dashboard with interactive charting and sell rules visualization
- **Input**: Ticker symbol input, API /sell-rules endpoint, historical stock data
- **Output**: Interactive candlestick charts, sell rule trigger indicators, visual annotations on charts
- **How it fits**: Focused on **position management** - helps traders monitor exit conditions for existing positions.

**Technologies**: React 19, TypeScript, Vite, Material UI, Lightweight Charts (TradingView library)

**Features**:
- High-performance candlestick charting
- Visual sell signal indicators on charts
- Rule-by-rule breakdown with severity levels
- Support for multiple timeframes

#### 6. **EpisodicPivotAnalyser.Gui** (Next.js App)
- **What it does**: Exploratory GUI with lighter implementation for research and experimentation
- **Input**: User navigation, API calls
- **Output**: Rendered Next.js pages with stock data
- **How it fits**: Alternative frontend for **exploratory analysis** and rapid prototyping.

**Technologies**: Next.js, TypeScript, Tailwind CSS

#### 7. **EpisodicPivotAnalyser.ConsoleApp** (Console Application)
- **What it does**: Statistics collection and batch analysis tool for backtesting and research
- **Input**: Historical stock data, command-line arguments
- **Output**: Console output with statistics, analysis results
- **How it fits**: Used for **backtesting** trading strategies and generating performance reports.

**Technologies**: .NET 10, Serilog

#### 8. **EpisodicPivotAnalyser.Migrations** (Database Migration Tool)
- **What it does**: Manages SQL Server database schema using Grate migration framework
- **Input**: SQL migration scripts in `/Grate` folder
- **Output**: Applied database schema (tables, indexes, stored procedures)
- **How it fits**: Ensures **database schema consistency** across environments (dev, staging, production).

**Technologies**: Grate, SQL Server, PowerShell

#### 9. **EpisodicPivotAnalyser.MassTransit** (Message Bus Library)
- **What it does**: RabbitMQ messaging configuration, health checks, and OpenTelemetry integration
- **Input**: Configuration from appsettings.json
- **Output**: Configured MassTransit bus, telemetry data
- **How it fits**: Enables **asynchronous messaging** between services for scalability (optional).

**Technologies**: MassTransit, RabbitMQ, OpenTelemetry

#### 10. **EpisodicPivotAnalyser.Aspire** (.NET Aspire Orchestration)
- **What it does**: Local development orchestration using .NET Aspire
- **Input**: Service definitions, Docker Compose configurations
- **Output**: Running local environment with Alerter, RabbitMQ, Redis, SQL Server
- **How it fits**: Provides a **one-command local development environment** with full observability.

**Technologies**: .NET 10 Aspire, Docker, OpenTelemetry

#### 11. **EpisodicPivotAnalyser.EarningsCsvExporter** (Console Application)
- **What it does**: Exports earnings data from database to CSV files for external analysis
- **Input**: Database connection, output directory
- **Output**: CSV files with earnings data
- **How it fits**: Utility for **data export and backup**.

**Technologies**: .NET 10

#### 12. **EpisodicPivotAnalyzer.Logging** (Shared Library)
- **What it does**: Logging enrichment, sanitization, and utility functions
- **Input**: Log events from services
- **Output**: Enriched, structured log entries (console, file, Elasticsearch)
- **How it fits**: Provides **centralized logging infrastructure** across all backend services.

**Technologies**: Serilog, Serilog.Sinks.Elasticsearch

#### 13. **Test Projects** (xUnit/NUnit)
- **EpisodicPivotAnalyser.Alerter.Tests**: Unit tests for Alerter service
- **EpisodicPivotAnalyser.Common.Tests**: Unit tests for Common library (rules, services)
- **Integration.Tests**: Integration tests for end-to-end scenarios

**Technologies**: xUnit, NUnit, Moq, FluentAssertions

---

## ✨ Features

### Trading Analysis
- ✅ **Automated Earnings Gap Scanner**: Monitors earnings calendars and identifies gap-up opportunities
- ✅ **10+ Technical Trading Rules**: Comprehensive rule engine (volume, moving averages, ADR, relative strength, low price filter)
- ✅ **Earnings Surprise Data**: Web scraping of EPS and revenue surprises from earnings whisper sources
- ✅ **Machine Learning Scoring**: Proprietary earnings quality score and percentile ranking
- ✅ **Real-Time Alerts**: Email notifications with full stock analysis
- ✅ **Scheduled Execution**: Runs every 10 minutes during post-market hours (3:30 PM - 11:59 PM weekdays)

### Sell Signal Detection
- ✅ **7 Comprehensive Sell Rules**:
  - Failure to Hold Gap-Up Low (momentum breakdown)
  - Moving Average Crossover (10, 20, 50-day)
  - Climax Volume Detection (exhaustion patterns)
  - Momentum Stall (price compression)
  - R-Multiple Profit Targets (2R, 3R, 5R)
  - Bearish Reversal Patterns (Shooting Star, Engulfing, Dark Cloud)
  - Stop Loss Trigger (violation of defined risk levels)

### User Interfaces
- ✅ **Professional Dark-Mode Trading Dashboard**: TradingView-inspired UI with auto-refresh
- ✅ **Interactive Sell Rules Dashboard**: Candlestick charts with visual sell signal indicators
- ✅ **Tab Navigation**: Organized views (All Alerts, Buy List, Ignore List)
- ✅ **Stock Detail Modal**: Comprehensive earnings data, revenue trends, EPS history
- ✅ **Buy/Ignore Actions**: One-click stock list management
- ✅ **Responsive Design**: Works on desktop, tablet, and mobile

### Developer Features
- ✅ **Mock Data Mode**: Full functionality without database for demos/development
- ✅ **Aspire Orchestration**: One-command local environment setup
- ✅ **OpenTelemetry Integration**: Distributed tracing and observability
- ✅ **Swagger API Documentation**: Interactive API explorer at `/swagger`
- ✅ **Comprehensive Logging**: Structured logging with Serilog
- ✅ **Azure Pipelines CI/CD**: Automated build and deployment

### Unique Innovations
- ✅ **Multi-Frontend Strategy**: Different UIs optimized for different workflows
- ✅ **Single Source of Truth Architecture**: All business logic in Common library
- ✅ **Selenium Web Scraping**: Real-time earnings surprise data collection
- ✅ **R-Multiple Risk Management**: Calculates profit targets as multiples of risk unit
- ✅ **Market Hours Awareness**: Smart scheduling that respects market hours

---

## 🎯 Roadmap

Future enhancements under consideration:

- [ ] Add machine learning model for trade probability scoring
- [ ] Implement backtesting framework with historical data
- [ ] Add position sizing calculator based on Kelly Criterion
- [ ] Create mobile app (React Native)
- [ ] Add real-time WebSocket updates instead of polling
- [ ] Implement user authentication and multi-user support
- [ ] Add integration with brokerage APIs for automated trading
- [ ] Add portfolio tracking and performance analytics

---

## 🏆 Acknowledgments

This project is inspired by:
- **Qullamaggie (Kristjan Kullamägi)**: For the Episodic Pivot strategy and gap-up trading methodology
- **Mark Minervini**: For SEPA (Specific Entry Point Analysis) concepts
- **William O'Neil**: For CAN SLIM principles

Built with ❤️ by traders, for traders.
