# AI Travel Planner Agent

An AI travel planning assistant built with Dify Agent and LLM.

## Project Overview

AI Travel Planner Agent generates executable travel plans based on user requirements.

Users can provide:

- Destination
- Travel dates
- Number of travelers
- Budget
- Travel style
- Interests

The Agent generates:

- Route planning
- Daily itinerary
- Accommodation suggestions
- Transportation recommendations
- Food recommendations
- Budget feasibility analysis


## Architecture

User Input

↓

Requirement Understanding

↓

Budget Feasibility Analysis

↓

Knowledge Base Retrieval

↓

Travel Plan Generation

↓

Final Validation


## Core Features

### 1. Travel Requirement Understanding

Extracts key travel parameters:

- Destination
- Date
- Duration
- Budget
- Travelers
- Travel preferences


### 2. Budget Feasibility Analysis

The Agent evaluates:

- Whether the budget is realistic
- Budget shortage
- Possible adjustments


### 3. Multi-city Travel Planning

Supports:

- Multi-city routes
- Accommodation allocation
- Transportation planning
- Daily itinerary generation


### 4. Constraint Validation

Checks:

- Date consistency
- Travel duration
- Budget conflicts
- Logical itinerary issues


## Testing

### Normal Case

Input:

Paris → Interlaken → Milan

10 days, 2 travelers, budget 20,000 RMB


Result:

Generated complete travel plan including:

- Route
- Daily itinerary
- Budget analysis
- Accommodation suggestions


### Budget Conflict Case

Input:

Budget: 5,000 RMB


Result:

Detected budget shortage and provided adjustment suggestions.


### Date Conflict Case

Input:

Travel dates conflict with requested duration.


Result:

Detected inconsistency and requested confirmation.


## Tech Stack

- Dify Agent
- Large Language Model (LLM)
- Knowledge Base (RAG)
- API Tools


## Demo

Dify Web App:

(Add your Dify URL here)


## Screenshots

Testing screenshots will be added here.


## Future Improvements

- Real-time flight search API
- Hotel booking API integration
- Weather API integration
- Personalized recommendation system
