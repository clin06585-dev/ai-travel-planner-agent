# Workflow Design

## 1. Overview

AI Travel Planner Agent is an AI-powered travel planning application built with Dify, LLM, RAG, and tool-calling concepts.

The system converts natural-language travel requirements into structured and executable travel plans.

The workflow considers:

- Destination
- Travel dates
- Trip duration
- Number of travelers
- Total budget
- Whether international airfare is included
- Travel style
- Interest preferences
- Accommodation requirements
- Transportation feasibility
- Route consistency
- Missing or conflicting information

---

## 2. Workflow Architecture

```text
User Input
    ↓
Travel Requirement Extraction
    ↓
Missing Information Detection
    ↓
Knowledge Retrieval (RAG)
    ↓
Travel Planning and Reasoning
    ↓
Date, Route, and Budget Validation
    ↓
Conflict Detection and Correction
    ↓
Final Travel Plan
```

---

## 3. Workflow Steps

### Step 1: User Input

The user provides travel requirements in natural language.

Example:

```text
Plan a five-day trip to Paris for two people.

The total budget is CNY 10,000, excluding international flights.

We prefer a relaxed travel style and are interested in food,
local neighborhoods, museums, and scenic walks.
```

The user may provide:

- Destination or multiple destinations
- Start date and end date
- Number of travel days
- Number of travelers
- Total budget
- Whether international airfare is included
- Travel style
- Interest preferences
- Accommodation preferences
- Special requirements

---

### Step 2: Travel Requirement Extraction

The LLM extracts the user's natural-language request into structured travel parameters.

Example:

```json
{
  "destinations": [
    "Paris"
  ],
  "start_date": null,
  "end_date": null,
  "duration_days": 5,
  "number_of_travelers": 2,
  "total_budget_cny": 10000,
  "international_airfare_included": false,
  "travel_style": "relaxed",
  "interests": [
    "food",
    "local neighborhoods",
    "museums",
    "scenic walks"
  ],
  "accommodation_preference": null,
  "special_requirements": []
}
```

Structured extraction allows the later workflow steps to process the travel request consistently.

---

### Step 3: Missing Information Detection

The agent checks whether essential information is missing.

Important fields include:

- Destination
- Travel date or trip duration
- Number of travelers
- Total budget
- Travel style
- Interest preferences

If critical information is missing, the agent should either:

1. Ask the user for clarification, or
2. Make a clearly stated reasonable assumption when appropriate.

The agent must not silently invent important travel information.

---

### Step 4: Knowledge Retrieval

The agent retrieves relevant information from the travel knowledge base through RAG.

The knowledge base may contain:

- Destination information
- Attraction descriptions
- Local transportation guidance
- Accommodation rules
- Route-planning principles
- Historical climate references
- Budget-planning rules
- Travel feasibility constraints
- Food and neighborhood recommendations

Knowledge retrieval reduces unsupported assumptions and improves the consistency of the final plan.

---

### Step 5: Travel Planning and Reasoning

The LLM combines:

1. User requirements
2. Retrieved knowledge
3. Budget constraints
4. Route feasibility
5. General travel knowledge

The agent then generates:

- Destination sequence
- Accommodation-night allocation
- Daily itinerary
- Attraction grouping
- Transportation suggestions
- Food recommendations
- Estimated budget allocation

For multi-city trips, the agent should minimize unnecessary backtracking and avoid unrealistic transfers.

---

### Step 6: Date and Accommodation Validation

The agent validates the relationship between trip dates, travel days, and accommodation nights.

Example:

```text
Travel dates: September 14 to September 19
Travel duration: 6 calendar days
Required accommodation: 5 nights
```

The agent checks:

- Whether the dates match the stated duration
- Whether accommodation nights are sufficient
- Whether arrival and departure days are handled correctly
- Whether overnight transportation affects accommodation allocation

The final output must not contain inconsistent dates or insufficient accommodation nights.

---

### Step 7: Route Validation

The agent checks whether the proposed route is realistic.

Validation includes:

- Geographic order
- Travel time between destinations
- Transportation availability
- Excessive daily travel
- Repeated backtracking
- Arrival and departure feasibility
- Whether daily attractions are located near each other

The agent should group nearby attractions into the same day whenever possible.

---

### Step 8: Budget Validation

The agent estimates and validates the trip budget.

The budget may include:

- Accommodation
- Intercity transportation
- Local transportation
- Food
- Attraction tickets
- Activities
- Emergency reserve
- International airfare, when included by the user

Example:

```json
{
  "accommodation": 3500,
  "intercity_transportation": 1200,
  "local_transportation": 500,
  "food": 2200,
  "attractions_and_activities": 1000,
  "emergency_reserve": 800,
  "total": 9200
}
```

The estimated total must not exceed the user's stated budget.

If the original request is not feasible within the budget, the agent should adjust:

- Accommodation level
- Number of destinations
- Transportation method
- Paid attractions
- Dining assumptions
- Trip duration

The agent must not knowingly output an over-budget plan.

---

### Step 9: Conflict Detection and Correction

Before generating the final answer, the agent checks for conflicts.

Possible conflicts include:

- Dates that do not match the trip duration
- Accommodation nights that do not match the itinerary
- Transportation routes that are not executable
- Budget totals that exceed the user's limit
- Activities that conflict with the user's travel style
- Missing destinations
- Repeated or geographically inconsistent routes
- Assumptions that contradict user requirements

When a conflict is detected, the agent must correct it before producing the final output.

---

### Step 10: Final Output Generation

The agent generates a structured and executable travel plan.

The final output includes:

1. Trip summary
2. Assumptions
3. Route overview
4. Accommodation allocation
5. Daily itinerary
6. Transportation suggestions
7. Food recommendations
8. Budget breakdown
9. Feasibility notes
10. Important reminders

---

## 4. Decision Priority

When information or constraints conflict, the agent follows this priority:

```text
1. Explicit user requirements
2. Knowledge base information
3. Budget constraints
4. Travel feasibility
5. General model knowledge
```

Higher-priority rules override lower-priority rules.

The system must not output:

- An uncorrected error
- An over-budget plan
- Insufficient accommodation
- Contradictory dates
- An unrealistic route
- Unsupported certainty about real-time information

---

## 5. Real-Time Information Handling

The agent distinguishes between historical reference information and real-time information.

Historical reference information may include:

- Typical climate
- Average transportation cost
- Common travel duration
- General attraction information

Real-time information may include:

- Current ticket prices
- Hotel availability
- Live transportation schedules
- Temporary closures
- Weather forecasts
- Entry-policy changes

When real-time tools are unavailable, the agent should clearly label estimates and recommend verification before booking.

---

## 6. Error Handling

### Missing Required Information

The agent requests clarification or states a reasonable assumption.

### Invalid Dates

The agent identifies the inconsistency and corrects the itinerary.

### Budget Too Low

The agent reduces costs or proposes a more feasible alternative.

### Unsupported Destination Information

The agent avoids fabricating precise facts and clearly states limitations.

### Knowledge Retrieval Failure

The agent falls back to general knowledge while marking uncertain information.

### Tool Call Failure

The agent continues with available information and identifies which results require manual verification.

---

## 7. Example Workflow

### User Request

```text
Plan a five-day Paris trip for two travelers.

The budget is CNY 10,000, excluding international flights.

We prefer a relaxed schedule and enjoy food, museums,
local neighborhoods, and scenic walks.
```

### Extracted Requirements

```json
{
  "destinations": [
    "Paris"
  ],
  "duration_days": 5,
  "number_of_travelers": 2,
  "total_budget_cny": 10000,
  "international_airfare_included": false,
  "travel_style": "relaxed",
  "interests": [
    "food",
    "museums",
    "local neighborhoods",
    "scenic walks"
  ]
}
```

### Planning Logic

```text
Requirements extracted
    ↓
Paris knowledge retrieved
    ↓
Nearby attractions grouped
    ↓
Four accommodation nights allocated
    ↓
Daily itinerary generated
    ↓
Budget calculated
    ↓
Route and budget validated
    ↓
Final travel plan returned
```

---

## 8. Output Quality Checklist

Before returning the final travel plan, the agent verifies:

- [ ] All explicit user requirements are included
- [ ] Dates and duration are consistent
- [ ] Accommodation nights are sufficient
- [ ] The route is geographically reasonable
- [ ] Daily schedules are not overloaded
- [ ] Transportation is executable
- [ ] The budget does not exceed the limit
- [ ] Assumptions are clearly stated
- [ ] Real-time information is properly labeled
- [ ] The final output contains no unresolved contradictions

---

## 9. Workflow Result

This workflow enables the AI Travel Planner Agent to produce travel plans that are:

- Structured
- Personalized
- Budget-aware
- Route-consistent
- Date-consistent
- Explainable
- Realistic
- Executable
