# 1.1 UML – Ticket Triage System

The system design was generated with an AI assistant (Claude) from the prompt in [`00_design_prompt.md`](00_design_prompt.md):
first as PlantUML, then converted to Mermaid. The assistant's answer is in [`01_design_response.md`](01_design_response.md).

| Folder | Content |
|---|---|
| `plantuml/` | PlantUML sources (`.puml`) |
| `png/` | PNG renders of the PlantUML sources (made with PlantUML 1.2024.7) |
| `mermaid/` | The same diagrams converted to Mermaid |


## Use case diagram

![Use case diagram](png/01_use_case.png)

<details><summary>PlantUML source</summary>

```plantuml
@startuml
left to right direction
actor "User / Customer" as U
actor "Support Agent" as S
actor "Gemini API" as AI <<external>>

rectangle "Ticket Triage System" {
  usecase "Submit comment" as UC1
  usecase "View comments" as UC2
  usecase "Analyze comment with AI" as UC3
  usecase "Create ticket" as UC4
  usecase "View tickets" as UC5
}

U --> UC1
S --> UC2
S --> UC5
UC1 ..> UC3 : <<include>>
UC3 ..> UC4 : <<extend>>\n[needs ticket]
UC3 --> AI
@enduml
```
</details>

**Mermaid version**

```mermaid
flowchart LR
  U([User / Customer])
  S([Support Agent])
  AI([Gemini API])
  subgraph SYS[Ticket Triage System]
    UC1(Submit comment)
    UC2(View comments)
    UC3(Analyze comment with AI)
    UC4(Create ticket)
    UC5(View tickets)
  end
  U --> UC1
  S --> UC2
  S --> UC5
  UC1 -. include .-> UC3
  UC3 -. "extend [needs ticket]" .-> UC4
  UC3 --> AI
```


## Component diagram

![Component diagram](png/02_component.png)

<details><summary>PlantUML source</summary>

```plantuml
@startuml
package "Ticket Triage System" {
  [REST API\n(POST /comments, GET /comments,\nGET /tickets, GET /tickets/{id})] as API
  [Triage Service] as TS
  [AI Client] as AIC
  [Repository] as REPO
  database "Comments & Tickets\nDB" as DB
}
cloud "Gemini API" as GEM

API --> TS
TS --> AIC
AIC --> GEM : HTTPS
TS --> REPO
REPO --> DB
@enduml
```
</details>

**Mermaid version**

```mermaid
flowchart LR
  API["REST API<br/>POST /comments, GET /comments<br/>GET /tickets, GET /tickets/{id}"] --> TS[Triage Service]
  TS --> AIC[AI Client]
  AIC -->|HTTPS| GEM[(Gemini API)]
  TS --> REPO[Repository]
  REPO --> DB[(Comments & Tickets DB)]
```


## Class diagram

![Class diagram](png/03_class.png)

<details><summary>PlantUML source</summary>

```plantuml
@startuml
class Comment {
  +id: Long
  +text: String
  +channel: Channel
  +createdAt: DateTime
}
class Ticket {
  +id: Long
  +title: String
  +category: Category
  +priority: Priority
  +summary: String
  +createdAt: DateTime
}
enum Channel {
  APP_REVIEW
  WEB_FORM
  CHAT_WIDGET
  EMAIL
}
enum Category {
  BUG
  FEATURE
  BILLING
  ACCOUNT
  OTHER
}
enum Priority {
  LOW
  MEDIUM
  HIGH
}
class TriageService {
  +process(comment: Comment): Ticket
}
class AiClient {
  +decide(text: String): boolean
  +generateTicket(text: String): TicketDraft
}

Comment "1" -- "0..1" Ticket : generates >
Comment --> Channel
Ticket --> Category
Ticket --> Priority
TriageService --> AiClient
TriageService ..> Comment
TriageService ..> Ticket
@enduml
```
</details>

**Mermaid version**

```mermaid
classDiagram
  class Comment {
    +Long id
    +String text
    +Channel channel
    +DateTime createdAt
  }
  class Ticket {
    +Long id
    +String title
    +Category category
    +Priority priority
    +String summary
    +DateTime createdAt
  }
  class Channel { <<enumeration>> APP_REVIEW WEB_FORM CHAT_WIDGET EMAIL }
  class Category { <<enumeration>> BUG FEATURE BILLING ACCOUNT OTHER }
  class Priority { <<enumeration>> LOW MEDIUM HIGH }
  class TriageService { +process(Comment) Ticket }
  class AiClient { +decide(String) boolean +generateTicket(String) TicketDraft }

  Comment "1" -- "0..1" Ticket : generates
  Comment --> Channel
  Ticket --> Category
  Ticket --> Priority
  TriageService --> AiClient
  TriageService ..> Comment
  TriageService ..> Ticket
```


## Sequence diagram – comment submission with AI triage

![Sequence diagram – comment submission with AI triage](png/04_sequence.png)

<details><summary>PlantUML source</summary>

```plantuml
@startuml
actor User
participant "REST API" as API
participant "Triage Service" as TS
participant "AI Client" as AI
participant "Gemini API" as G
database DB

User -> API : POST /comments {text, channel}
API -> DB : save Comment
API -> TS : process(comment)
TS -> AI : decide(text)
AI -> G : triage prompt
G --> AI : TICKET / NO_TICKET
alt TICKET
  TS -> AI : generateTicket(text)
  AI -> G : ticket generation prompt
  G --> AI : JSON {title, category, priority, summary}
  TS -> DB : save Ticket
end
API --> User : 201 Created (comment + optional ticket)
@enduml
```
</details>

**Mermaid version**

```mermaid
sequenceDiagram
  actor User
  participant API as REST API
  participant TS as Triage Service
  participant AI as AI Client
  participant G as Gemini API
  participant DB

  User->>API: POST /comments {text, channel}
  API->>DB: save Comment
  API->>TS: process(comment)
  TS->>AI: decide(text)
  AI->>G: triage prompt
  G-->>AI: TICKET / NO_TICKET
  alt TICKET
    TS->>AI: generateTicket(text)
    AI->>G: ticket generation prompt
    G-->>AI: JSON {title, category, priority, summary}
    TS->>DB: save Ticket
  end
  API-->>User: 201 Created (comment + optional ticket)
```


## Activity diagram – triage decision flow

![Activity diagram – triage decision flow](png/05_activity.png)

<details><summary>PlantUML source</summary>

```plantuml
@startuml
start
:Receive comment;
:Validate input (length, empty);
:Ask AI: TICKET or NO_TICKET?;
if (Result == TICKET?) then (yes)
  :Ask AI for structured ticket JSON;
  if (JSON valid?) then (yes)
    :Normalize category and priority;
    :Store ticket;
  else (no)
    :Retry once / fallback category = other;
  endif
else (no)
  :Store comment only;
endif
:Return response;
stop
@enduml
```
</details>

**Mermaid version**

```mermaid
flowchart TD
  A([Start]) --> B[Receive comment]
  B --> C[Validate input: length, empty]
  C --> D{"AI: TICKET or NO_TICKET?"}
  D -- TICKET --> E[Ask AI for structured ticket JSON]
  E --> F{JSON valid?}
  F -- yes --> G[Normalize category and priority]
  G --> H[Store ticket]
  F -- no --> I["Retry once / fallback category = other"]
  I --> H
  D -- NO_TICKET --> J[Store comment only]
  H --> K[Return response]
  J --> K
  K --> L([End])
```
