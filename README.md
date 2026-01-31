# EpisodicPivotAnalyser

[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![React](https://img.shields.io/badge/React-18%2F19-61DAFB?logo=react)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript)](https://www.typescriptlang.org/)

---

### TL;DR

**EpisodicPivotAnalyser** is a sophisticated machine learning-powered trading analysis platform built on **10+ years of historical market data and thousands of hours of quantitative research**, designed to identify high-probability episodic market pivots with statistical edge.

Built with .NET 10 and React, it combines **advanced quantitative models, pattern recognition algorithms, and rule-based trading logic** to analyze earnings events and market data. The system delivers real-time alerts and sell signals through multiple professional frontends, all designed with **clean architecture, shared domain logic, microservice principles, and observability-first tooling**.

> This public repository documents **architecture, system design, and technical decisions**.  
> The full implementation, proprietary machine learning models, and trading logic are maintained in **private repositories**.

---

> ⚠️ **Public Overview Repository**  
> This repository serves as a **public technical overview and architectural documentation** of the *EpisodicPivotAnalyser* platform.  
> **The full source code, implementations, and proprietary logic are maintained in private repositories and are not publicly available.**

A sophisticated automated algorithmic trading system designed to identify and analyze stock opportunities based on the **Episodic Pivot** trading strategy. **Built on a foundation of 10+ years of historical market data and refined through thousands of hours of quantitative research**, the system automatically scans for stocks with earnings gaps, evaluates multiple technical trading rules using statistically-validated patterns, and provides professional trading dashboards with real-time alerts and comprehensive sell signal analysis.

### 🎓 Research Foundation

This platform represents **years of dedicated quantitative research and machine learning development**:
- **10+ years of historical stock market data** analyzed and integrated
- **Thousands of hours** invested in strategy development, backtesting, and validation
- **Statistical edge discovery** through comprehensive historical analysis
- **Machine learning models** trained on extensive historical patterns
- **Quantitative validation** of every trading rule and signal
- **Continuous refinement** based on real-world market performance

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Research Foundation](#-research-foundation)
- [Machine Learning & Data Science](#-machine-learning--data-science)
- [Architecture Summary](#architecture-summary)
- [Features](#features)
- [Roadmap](#roadmap)
- [Acknowledgments](#acknowledgments)

---

## 🎯 Project Overview

**EpisodicPivotAnalyser** is an end-to-end automated trading analysis platform that combines **quantitative research, machine learning, data collection, algorithmic rule evaluation, and multi-frontend presentation**. It is specifically designed for traders following the **Episodic Pivot strategy** (influenced by Qullamaggie's gap-up trading methodology) and is built on a **solid foundation of statistical analysis and historical validation**.

### What Problem Does It Solve?

The project addresses several key challenges in algorithmic trading:

1. **Data-Driven Stock Screening**: Leverages 10+ years of historical data to continuously monitor earnings calendars and identify stocks matching statistically-validated technical patterns (gap-ups, volume spikes, moving average breakouts)
2. **Quantitative Rule-Based Analysis**: Evaluates 10+ trading rules automatically, each validated through extensive backtesting, to filter high-probability setups with proven statistical edge
3. **Machine Learning Integration**: Applies proprietary machine learning models for earnings quality scoring and pattern recognition, trained on years of historical market behavior
4. **Real-Time Alerts**: Sends email notifications and provides web dashboards for immediate action on opportunities identified by the ML-enhanced analysis engine
5. **Sell Signal Detection**: Monitors positions using quantitative indicators and alerts traders when predefined exit conditions are met
6. **Multi-Frontend Access**: Provides professional trading dashboards optimized for different workflows (alerts, sell analysis, exploratory research)

### Core Trading Strategy

The **Episodic Pivot** strategy focuses on:
- Stocks gapping up on earnings announcements
- Strong volume confirmation
- Technical breakout patterns (moving averages, relative strength)
- Risk-managed entries with defined stop losses
- R-multiple based profit targets (2R, 3R, 5R)

---

## 🤖 Machine Learning & Data Science

### Historical Data Foundation

The **EpisodicPivotAnalyser** platform is built on an extensive foundation of historical market data and quantitative research:

- **10+ Years of Market Data**: Complete OHLCV (Open, High, Low, Close, Volume) data spanning over a decade
- **Thousands of Earnings Events**: Comprehensive database of earnings announcements, surprises, and subsequent price movements
- **Millions of Data Points**: Historical technical indicators, volume patterns, and price action across thousands of stocks
- **Backtested Validation**: Every trading rule and signal has been validated against years of historical data

### Machine Learning & Quantitative Models

The system incorporates advanced machine learning and statistical analysis:

#### 1. **Earnings Quality Scoring Model**
- **Training Data**: 10+ years of earnings announcements and subsequent stock performance
- **Features**: EPS surprise percentage, revenue surprise, historical earnings consistency, sector trends
- **Output**: Proprietary earnings score and percentile ranking (0-100)
- **Purpose**: Quantifies the quality and significance of earnings events to prioritize high-probability opportunities
- **Validation**: Continuously validated against out-of-sample data to ensure predictive accuracy

#### 2. **Pattern Recognition Algorithms**
- **Gap-Up Pattern Analysis**: Statistical models identifying successful gap-and-go patterns vs. gap-and-fail scenarios
- **Volume Profile Analysis**: Machine learning classification of volume patterns (climax, accumulation, distribution)
- **Moving Average Dynamics**: Quantitative analysis of moving average interactions and their predictive power
- **Technical Breakout Detection**: Pattern recognition for identifying high-probability technical breakouts

#### 3. **Statistical Edge Validation**
- **Backtesting Framework**: Comprehensive testing infrastructure using historical data
- **Performance Metrics**: Win rate, average R-multiple, expectancy, drawdown analysis
- **Statistical Significance**: All rules tested for statistical significance (p-value < 0.05)
- **Walk-Forward Analysis**: Continuous validation on unseen data to prevent overfitting

#### 4. **Quantitative Rule Engine**
Every trading rule in the system is:
- **Historically Validated**: Tested against 10+ years of market data
- **Statistically Significant**: Proven edge with measurable positive expectancy
- **Continuously Refined**: Rules updated based on ongoing performance analysis
- **Risk-Adjusted**: Incorporates volatility, ATR (Average True Range), and position sizing principles

### Research Methodology

The development of this platform represents **thousands of hours of dedicated research**:

1. **Data Collection & Cleaning** 
   - Aggregation of historical data from multiple sources
   - Data quality validation and cleansing
   - Gap adjustment and split handling
   - Volume normalization

2. **Strategy Development** 
   - Hypothesis generation and testing
   - Rule development and optimization
   - Parameter tuning and sensitivity analysis
   - Risk management integration

3. **Backtesting & Validation**
   - Historical simulation across different market conditions
   - Walk-forward testing and cross-validation
   - Statistical analysis of results
   - Edge verification and robustness testing

4. **Machine Learning Model Development**
   - Feature engineering and selection
   - Model training and hyperparameter optimization
   - Cross-validation and out-of-sample testing
   - Model deployment and monitoring

5. **System Architecture & Engineering** 
   - Microservices design and implementation
   - Database optimization for time-series data
   - Real-time data pipeline development
   - Frontend development and UX optimization

### Continuous Learning & Adaptation

The system is designed for continuous improvement:
- **Performance Monitoring**: Real-time tracking of all signals and their outcomes
- **Model Retraining**: Periodic retraining of ML models with updated data
- **Rule Optimization**: Ongoing refinement based on market conditions
- **A/B Testing**: Systematic testing of rule variations
- **Market Regime Detection**: Adaptive behavior based on current market conditions

This quantitative foundation ensures that **every alert, signal, and recommendation is backed by statistical evidence** and historical validation, not subjective interpretation or guesswork.

---

## 🏗️ Architecture Summary

EpisodicPivotAnalyser follows a **hybrid microservices + shared library architecture** with clear separation of concerns, designed to support **real-time data processing, machine learning inference, and high-performance analytics**:

```
┌─────────────────────────────────────────────────────────────────┐
│                     FRONTENDS (User Layer)                      │
├──────────────────┬───────────────────┬──────────────────────────┤
│  FrontEnd        │  SellRulesFrontEnd│  Training UI (Next.js)   │
│  (React+TS+Vite) │  (React+TS+Vite)  │  (Next.js+Tailwind)      │
└────────┬─────────┴────────┬──────────┴───────────┬──────────────┘
         │                  │                      │ HTTP/REST
         └──────────────────┼──────────────────────┘
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
    ┌────▼────────┐  ┌─────▼──────┐  ┌───────▼──────┐
    │  Common Lib │  │  Alerter   │  │  EarningsCsv │
    │  (Core)     │  │ (Service)  │  │  Exporter    │
    │ ├ Rules     │  │ Runs 10min │  └──────────────┘
    │ ├ SellRules │  │ Post-Market│
    │ ├ Services  │  └────┬───────┘
    │ ├ ML Models │       │
    │ └ Analytics │       │ (MassTransit)
    └────┬────────┘       │
         │                │
    ┌────▼────────────────▼──────────┐
    │   SQL Server Database          │
    │  ├ AlertResult (Current Data)  │
    │  ├ EarningsWhisper (ML Input)  │
    │  ├ EarningsScore (ML Output)   │
    │  ├ TrainingData (10+ yrs)      │
    │  └ Historical OHLCV (10+ yrs)  │
    └────────────────────────────────┘

         Optional Infrastructure:
    ┌─────────────────────────────────┐
    │  RabbitMQ (Message Bus)         │
    │  Redis (Caching)                │
    │  Aspire (Local Orchestration)   │
    └─────────────────────────────────┘
```

### Data Flow & Machine Learning Integration

**1. Data Ingestion Pipeline**
```
External APIs → Alerter Service → Data Validation → Database Storage
    ↓                   ↓                 ↓              ↓
Polygon API     Earnings Calendar    Rule Engine    Historical Data
StockTwits      Price/Volume         ML Models      Training Sets
Web Scraping    Technical Calcs      Analytics      Backtesting
```

**2. Machine Learning Inference Flow**
```
New Earnings Event → Feature Extraction → ML Model → Earnings Score
       ↓                     ↓                ↓            ↓
  EPS Surprise         Historical         Trained      Percentile
  Revenue Data         Patterns           Model        Ranking (0-100)
  Sector Info          Comparisons        Inference    Priority Signal
```

**3. Alert Generation Flow**
```
Market Data → Rule Evaluation → Score Aggregation → Alert Creation
     ↓              ↓                   ↓                  ↓
 OHLCV Data    10+ Rules          ML Scores          Database
 Technical     Statistical        Quantitative       Email Alerts
 Indicators    Validation         Models             Dashboard
```

**4. Real-Time Analysis Flow**
```
User Request → API Endpoint → Data Repository → Response
     ↓              ↓               ↓               ↓
 Ticker Query   Sell Rules     Database Query   JSON Data
 Parameters     Calculation    Cache Check      Charts/UI
 Filters        ML Inference   Historical Data  Visualization
```

### Sub-Applications & Modules

#### 1. **EpisodicPivotAnalyser.Alerter** (Hosted Service)
- **What it does**: Core alert generation engine that runs on a scheduled timer (every 10 minutes during post-market hours: 3:30 PM - 11:59 PM weekdays). This is the **heart of the machine learning inference pipeline**.
- **Input**: Earnings calendar from Polygon API, stock OHLCV data, trading rules configuration, historical pattern database
- **Output**: AlertResult records in database (with ML-generated earnings scores), email notifications to configured recipients
- **How it fits**: This is the **heart of the system** - the single source of truth for trading signals. All business logic, rule evaluation, and **machine learning model inference** happen here. Every alert is enriched with quantitative scores derived from 10+ years of historical data analysis.
- **ML Integration**: 
  - Executes earnings quality scoring models on each event
  - Applies pattern recognition algorithms to identify high-probability setups
  - Uses statistical validation to filter signals
  - Generates percentile rankings based on historical distributions

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
- **What it does**: Core domain models, repositories, services, and business logic. **Single source of truth** for all rule evaluations and **machine learning model implementations**.
- **Input**: Stock data from APIs (Polygon, Stocktwits), database connections, historical training data
- **Output**: Validated stock data, rule evaluation results, sell signal assessments, **ML model predictions and scores**
- **How it fits**: Shared by all backend components (Alerter, API, ConsoleApp). Contains no UI code, only pure business logic, **quantitative models, and ML inference engines**.

**Key Components**:
- **Rules**: 10+ trading rules (GapUpRule, EpisodicPivotRules, VolumeRule, MovingAverageRule, etc.) - all **statistically validated against historical data**
- **SellRules**: 7 sell signal detectors (Moving Average Crossover, Climax Volume, Failure to Hold Gap, Reversal Patterns, Profit Targets) - **backtested on 10+ years of data**
- **ML Models**: Earnings quality scoring, pattern recognition, statistical validation engines
- **Services**: EarningsService, StockDataService, RuleService, EarningsWhisperService (web scraping), MessageService, **QuantitativeAnalysisService**
- **Repositories**: AlertResultRepository, EarningsWhisperRepository, EarningsScoreRepository, FundamentalDataRepository, **HistoricalDataRepository (10+ years)**

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
- **What it does**: Statistics collection and batch analysis tool for **backtesting, research, and ML model validation**
- **Input**: Historical stock data (10+ years), command-line arguments, backtesting parameters
- **Output**: Console output with statistics, analysis results, **model performance metrics, backtest reports**
- **How it fits**: Used for **backtesting** trading strategies, generating performance reports, **training and validating ML models**, and conducting quantitative research on historical data.

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

### Machine Learning & Quantitative Analysis
- ✅ **10+ Years Historical Database**: Complete market data repository with OHLCV, earnings, and technical indicators
- ✅ **Earnings Quality ML Model**: Proprietary machine learning model scoring earnings events based on historical performance patterns
- ✅ **Pattern Recognition Engine**: Statistical algorithms identifying gap-and-go patterns vs. gap-and-fail scenarios
- ✅ **Quantitative Backtesting**: Comprehensive framework for validating strategies on historical data
- ✅ **Statistical Edge Validation**: Every rule tested for statistical significance (p < 0.05) and positive expectancy
- ✅ **Performance Analytics**: Real-time tracking and analysis of signal outcomes and model accuracy

### Trading Analysis
- ✅ **Automated Earnings Gap Scanner**: Monitors earnings calendars and identifies gap-up opportunities using ML-enhanced filtering
- ✅ **10+ Technical Trading Rules**: Comprehensive rule engine (volume, moving averages, ADR, relative strength, low price filter) - **all statistically validated**
- ✅ **Earnings Surprise Data**: Web scraping of EPS and revenue surprises from earnings whisper sources
- ✅ **Machine Learning Scoring**: Proprietary earnings quality score and percentile ranking (0-100)
- ✅ **Real-Time Alerts**: Email notifications with full stock analysis and quantitative scores
- ✅ **Scheduled Execution**: Runs every 10 minutes during post-market hours (3:30 PM - 11:59 PM weekdays)

### Sell Signal Detection
- ✅ **7 Comprehensive Sell Rules** (all backtested on historical data):
  - Failure to Hold Gap-Up Low (momentum breakdown)
  - Moving Average Crossover (10, 20, 50-day)
  - Climax Volume Detection (exhaustion patterns)
  - Momentum Stall (price compression)
  - R-Multiple Profit Targets (2R, 3R, 5R) - statistically optimal exit levels
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
- ✅ **Single Source of Truth Architecture**: All business logic and ML models in Common library
- ✅ **Selenium Web Scraping**: Real-time earnings surprise data collection
- ✅ **R-Multiple Risk Management**: Calculates profit targets as multiples of risk unit
- ✅ **Market Hours Awareness**: Smart scheduling that respects market hours
- ✅ **Quantitative Foundation**: Every signal backed by 10+ years of statistical validation
- ✅ **Continuous Learning**: System designed for ongoing model retraining and optimization
- ✅ **Historical Data Repository**: Complete 10+ year database for backtesting and research

---

## 🎯 Roadmap

Future enhancements under consideration:

### Machine Learning & AI
- [ ] Implement deep learning models (LSTM/Transformer) for price prediction
- [ ] Add reinforcement learning for dynamic position sizing
- [ ] Develop ensemble models combining multiple ML approaches
- [ ] Create automated feature engineering pipeline
- [ ] Add neural network-based pattern recognition

### Quantitative Analysis
- [ ] Expand backtesting framework with walk-forward optimization
- [ ] Add Monte Carlo simulation for risk analysis
- [ ] Implement factor analysis and multi-factor models
- [ ] Create correlation analysis tools for portfolio construction
- [ ] Add sentiment analysis integration (news, social media)

### Trading & Risk Management
- [ ] Add advanced position sizing calculator based on Kelly Criterion
- [ ] Implement portfolio-level risk management
- [ ] Add integration with brokerage APIs for automated trading
- [ ] Create dynamic stop-loss adjustment based on volatility
- [ ] Add portfolio tracking and performance analytics

### Platform Enhancements
- [ ] Add real-time WebSocket updates instead of polling
- [ ] Implement user authentication and multi-user support
- [ ] Create mobile app (React Native)
- [ ] Add advanced visualization tools (heat maps, correlation matrices)
- [ ] Implement distributed computing for faster backtesting

---

## 🏆 Acknowledgments

### Research & Development

This platform represents a **significant investment in quantitative research and software engineering**:
- **10+ years** of historical market data collected, cleaned, and validated
- **4000+ hours** of dedicated development and research time
- **Thousands of backtests** conducted to validate strategies
- **Continuous refinement** based on real-world performance

The system embodies a deep commitment to **data-driven trading**, where every decision is backed by statistical evidence and historical validation rather than subjective interpretation.

### Inspirations

This project is inspired by:
- **Qullamaggie (Kristjan Kullamägi)**: For the Episodic Pivot strategy and gap-up trading methodology
- **Mark Minervini**: For SEPA (Specific Entry Point Analysis) concepts
- **William O'Neil**: For CAN SLIM principles

Built with ❤️ by traders and data scientists, for traders.
