# Tripay E-ATM — Web

> ASP.NET Core MVC frontend for a multi-service digital wallet and E-ATM system.

![ASP.NET Core](https://img.shields.io/badge/ASP.NET%20Core-MVC-512BD4?logo=dotnet)
![C#](https://img.shields.io/badge/C%23-.NET%2010-239120?logo=csharp)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Bootstrap](https://img.shields.io/badge/UI-Bootstrap-7952B3?logo=bootstrap&logoColor=white)

This repository contains the web application and database setup for Tripay E-ATM. Its Node.js REST, gRPC and SOAP services are maintained in the companion [Tripay-E-ATM-Services repository](https://github.com/omer-damar/Tripay-E-ATM-Services).

## Features

- Registration and login flow
- Cookie-based web session with JWT-backed service access
- User and administrator views
- Multi-currency account management
- Deposit and withdrawal workflows
- Transaction summaries and recent-transaction ViewComponent
- Exchange-rate screen backed by the Node.js service
- Account, user and transaction administration
- PostgreSQL-based data layer

## Architecture

```text
Browser
   |
   v
ASP.NET Core MVC web app
   |
   +--> PostgreSQL
   |
   +--> Node.js REST API --> auth, accounts, exchange rates
                           +--> gRPC WalletService
                           +--> SOAP WalletService
```

## Tech Stack

| Area | Technologies |
|---|---|
| Web | ASP.NET Core MVC, C#, Razor Views |
| UI | Bootstrap, CSS, JavaScript |
| Authentication | Cookie authentication + JWT service token |
| Data | PostgreSQL, Npgsql |
| Integration | HttpClient, REST, gRPC, SOAP |

## Getting Started

### Prerequisites

- .NET 10 SDK
- PostgreSQL
- Node.js 18+ for the companion services

### 1. Clone both repositories

```bash
git clone https://github.com/omer-damar/Tripay-E-ATM-Web.git
git clone https://github.com/omer-damar/Tripay-E-ATM-Services.git
```

### 2. Prepare PostgreSQL

Create a `digital_payment` database, then run:

```bash
psql -U postgres -d digital_payment -f database_setup.sql
```

The setup script contains demonstration users and balances. Review or remove seed data before using the project outside a local development environment.

### 3. Configure the web application

Update `appsettings.json` or use environment-specific configuration:

```json
{
  "ConnectionStrings": {
    "PostgreSQL": "Host=localhost;Port=5432;Database=digital_payment;Username=postgres;Password=your_password"
  },
  "NodeApiSettings": {
    "BaseUrl": "http://localhost:3000",
    "GrpcUrl": "http://localhost:50051",
    "SoapUrl": "http://localhost:3001"
  }
}
```

For local development, keep the committed value as a placeholder and provide the real connection string through an environment variable:

```bash
# PowerShell
$env:ConnectionStrings__PostgreSQL="Host=localhost;Port=5432;Database=digital_payment;Username=postgres;Password=your_password"
```

ASP.NET Core maps the double underscore to `ConnectionStrings:PostgreSQL`. Do not commit real database passwords or production secrets.

### 4. Start the backend services

Follow the instructions in [Tripay-E-ATM-Services](https://github.com/omer-damar/Tripay-E-ATM-Services) and start the REST service. Start gRPC and SOAP as needed for those integration scenarios.

### 5. Run the web application

```bash
cd Tripay-E-ATM-Web
dotnet restore
dotnet run --project digitalpayment3.csproj
```

Use the local URL printed by ASP.NET Core.

## Main Areas

| Area | Purpose |
|---|---|
| `Auth` | Registration and login |
| `Account` | Account CRUD and account details |
| `Transaction` | Deposit, withdrawal and summaries |
| `Admin` | User, account, transaction and report views |
| `NodeApi` | Screens that exercise the Node.js service layer |

## Project Structure

```text
Tripay-E-ATM-Web/
├── Controllers/             # MVC controllers
├── Data/                    # PostgreSQL repositories
├── Models/                  # View models
├── Services/                # Node.js API client and settings
├── ViewComponents/          # Reusable server-rendered components
├── Views/                   # Razor views
├── wwwroot/                 # CSS, JavaScript and vendor assets
├── database_setup.sql       # Local database setup and seed data
└── Program.cs               # Application bootstrap
```

## Related Documentation

- [Detailed project criteria report](PROJE_KRITER_RAPORU_DETAYLI.md)
- [Project criteria summary](KRITER_RAPORU_OZET.md)
- [Implemented features](YENI_OZELLIKLER.md)

