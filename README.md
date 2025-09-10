# Overview

**Corporate Climber** is an enterprise-grade Spring Boot application that leverages artificial intelligence and statistical modeling to predict project success probabilities and optimize career development trajectories. The platform utilizes Monte Carlo simulations, machine learning algorithms, and natural language processing to deliver data-driven project recommendations and career guidance.

## Architecture

### Core Technologies

- **Framework**: Spring Boot 3.2
- **Runtime**: Java 17
- **Database**: PostgreSQL (Production), H2 (Development)
- **Build System**: Maven 3.6+

### AI & Analytics Stack

- **AI Platform**: Google Gemini API
- **Natural Language Processing**: Google Cloud NLP
- **Simulation Engine**: Custom Monte Carlo implementation
- **Security**: Spring Security

# Features

## Statistical Analysis Engine
- Monte Carlo simulation with 10,000 trial iterations
- Statistical confidence interval calculations
- Success probability modeling with uncertainty quantification

## AI-Powered Intelligence
- Personalized career recommendation system
- Natural language processing for sentiment analysis
- Team compatibility scoring algorithms
- Skill gap identification and growth path optimization

## Real-Time Analytics
- Interactive dashboard for skill assessment visualization
- Emotional trend analysis and peer interaction metrics
- Project success probability tracking
- Career progression analytics

## Configuration Management
- Customizable priority weighting system
- Configurable simulation parameters
- Environment-specific configuration profiles

## System Requirements

### Prerequisites
- Java Development Kit 17 or higher
- Apache Maven 3.6 or higher
- PostgreSQL 14 or higher

### Hardware Specifications
- Minimum 4GB RAM
- 2 CPU cores
- 10GB available disk space

## Installation Guide

### 1. Repository Setup
```bash
git clone https://github.com/parameshn/corporate-climber.git
cd corporate-climber
```

### 2.Environment Configuration
Create application.yml configuration file:
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/corporate_climber
    username: ${DB_USERNAME:your_username}
    password: ${DB_PASSWORD:your_password}
    driver-class-name: org.postgresql.Driver
  
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: false
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect

server:
  port: 8080

gemini:
  api:
    key: ${GEMINI_API_KEY:your_gemini_api_key}
    base-url: https://generativelanguage.googleapis.com/v1beta
    timeout: 5000

google:
  credentials:
    path: ${GOOGLE_CREDENTIALS_PATH:/path/to/service-account-key.json}
  nlp:
    timeout: 3000

simulation:
  default-trials: 10000
  skill-growth-factor: 0.15
```

### 3. Database Setup

```sql
CREATE DATABASE corporate_climber;
CREATE USER corporate_user WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE corporate_climber TO corporate_user;
```

### 4. Application Deployment
Standard Deployment
```bash
mvn clean install
mvn spring-boot:run
```

## API Reference
### Base URL
```bash
http://localhost:8080/api/v1
```

## Authentication

All API endpoints are secured with Spring Security. Authentication details will be provided in the API documentation.

## Core Endpoints

### User Management

| Method | Endpoint            | Description         |
|------|--------------------|--------------------|
| POST  | `/users`           | Create user profile |
| GET   | `/users/{userId}` | Retrieve user profile |

### Peer Management

| Method | Endpoint                          | Description       |
|------|---------------------------------|----------------|
| GET   | `/peers/department/{department}` | Get peers by department |
| POST  | `/peers/{peerId}/skills`         | Add skill to peer |

### Project Management

| Method | Endpoint                       | Description          |
|------|-------------------------------|-------------------|
| GET   | `/projects`                    | Get all projects   |
| GET   | `/projects/{projectId}`        | Get specific project |
| GET   | `/projects/domain/{domain}`    | Get projects by domain |
| POST  | `/projects`                    | Create new project |

### Project Simulation

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/simulate` | Execute Monte Carlo simulation |
| `GET` | `/users/{userID}/simulations` | Retrieve simulation history |

### Analytics & Insights

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/conversations` | Analyze conversation sentiment |
| `GET` | `/users/{userID}/interactions` | Retrieve peer interaction scores |
| `GET` | `/users/{userID}/recommendations` | Get AI-generated recommendations |
| `GET` | `/recommendations/{id}` | Get specific recommendation |

### Data Analytics

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/users/{userID}/skill-vector` | Get user skill vector |
| `GET` | `/projects/{projectID}/requirement-vector` | Get project requirement vector |

### Health Check

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | Application health status |

---

## Sample Requests

This section provides examples of how to use the API endpoints.

### Project Simulation Request

To start a Monte Carlo simulation, use a `POST` request to the `/api/v1/simulate` endpoint. The body of the request should be a JSON object containing the user ID, a list of candidate projects, and a set of priority weights for the simulation.

```bash
curl -X POST "http://localhost:8080/api/v1/simulate" \
  -H "Content-Type: application/json" \
  -d '{
    "userId": 1,
    "candidateProjectIds": [1, 2, 3],
    "priorityWeights": {
      "SKILL_GROWTH": 0.5,
      "PEER_SYNERGY": 0.3,
      "GOAL_ALIGNMENT": 0.2
    },
    "simulationTrials": 10000
  }'
```
### Conversation Analysis Request
To analyze the sentiment of a conversation, send a POST request to the /api/v1/conversations endpoint. The request body should contain the user and peer IDs, the conversation text, and details about the conversation.
```bash
curl -X POST "http://localhost:8080/api/v1/conversations" \
  -H "Content-Type: application/json" \
  -d '{
    "userId": 1,
    "peerId": 2,
    "conversationText": "Discussion regarding API architecture and platform",
    "conversationDate": "2023-10-15T14:30:00"
  }'
```

### Response Format
#### Simulation Response
```json
{
  "simulationId": "uuid-string",
  "projectId": 1,
  "projectName": "Cloud Migration Initiative",
  "successProbability": 0.847,
  "confidenceInterval": {
    "lower": 0.792,
    "upper": 0.901
  },
  "metrics": {
    "expectedSkillGain": 1.8,
    "peerSynergyScore": 8.7,
    "goalAlignmentScore": 9.2
  },
  "recommendation": "This project demonstrates strong alignment with...",
  "timestamp": "2023-10-15T10:30:00Z"
}
```

## Database Schema

This section provides an overview of the core database tables and their purpose.

### Core Tables

* `users` - Stores user profile information, including personal details and user-specific settings.
* `user_skills` - Contains individual skill assessments and proficiency levels for each user. This table helps in tracking and quantifying a user's skillset.
* `projects` - Holds project metadata and high-level requirements.
* `project_requirements` - Details the specific skills and competency levels required for each project, linking back to the `projects` table.
* `conversation_records` - Logs communication records and associated metadata, serving as the raw data for conversation analysis.
* `emotion_scores` - Stores the results of sentiment and emotion analysis performed on conversation records.
* `simulation_runs` - Catalogs the results from Monte Carlo simulations, including success probabilities and various metrics.
* `recommendations` - Stores AI-generated career guidance and project recommendations for users.
* `peer_interactions` - Contains metrics and data related to team collaboration and peer interactions.
* `skill_growth_tracking` - Provides a historical record of a user's skill development over time.

---
