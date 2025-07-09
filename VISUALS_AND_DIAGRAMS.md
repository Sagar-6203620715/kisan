# Project Kisan - Visuals & Diagrams

## 1. Process Flow Diagram

```mermaid
flowchart TD
    A[Farmer Opens App] --> B{Selects Feature}
    B -->|Disease Diagnosis| C[Take/Upload Plant Photo]
    C --> D[AI Analyzes Image]
    D --> E[Diagnosis & Advice in Local Language]
    B -->|Market Analysis| F[Ask Market Price]
    F --> G[Fetch Real-Time Data]
    G --> H[AI Analyzes Trends]
    H --> I[Actionable Price Advice]
    B -->|Govt. Schemes| J[Ask About Schemes]
    J --> K[AI Matches Needs to Schemes]
    K --> L[Explains & Links to Apply]
    B -->|Land Advisor| M[Enter Land/Soil Details]
    M --> N[AI + Satellite Analysis]
    N --> O[Personalized Land Use Plan]
    B -->|Voice Interaction| P[Speak Query]
    P --> Q[Speech-to-Text]
    Q --> R[AI Processes Query]
    R --> S[Text-to-Speech Response]
```

---

## 2. Use-Case Diagram

```mermaid
usecase
  actor Farmer
  actor Expert
  actor Government
  Farmer -- (Diagnose Crop Disease)
  Farmer -- (Get Market Prices)
  Farmer -- (Apply for Schemes)
  Farmer -- (Get Land Utilization Advice)
  Farmer -- (Voice Interaction)
  Expert -- (Validate AI Diagnosis)
  Government -- (Provide Scheme Data)
  (Diagnose Crop Disease) ..> (AI System) : <<include>>
  (Get Market Prices) ..> (Market Data API) : <<include>>
  (Apply for Schemes) ..> (Govt. Portal) : <<include>>
  (Get Land Utilization Advice) ..> (Satellite Data API) : <<include>>
```

---

## 3. Architecture Diagram

```mermaid
flowchart TD
    subgraph Mobile App
      A1[React Native App]
      A2[Voice Input/Output]
      A3[Camera/Image Upload]
      A4[Local Language UI]
    end
    subgraph Firebase
      B1[Authentication]
      B2[Firestore DB]
      B3[Cloud Functions]
      B4[Storage]
    end
    subgraph Google Cloud
      C1[Vertex AI Gemini Pro Vision]
      C2[Vertex AI Gemini Pro]
      C3[Speech-to-Text]
      C4[Text-to-Speech]
      C5[Agent Builder]
    end
    subgraph External APIs
      D1[Agmarknet]
      D2[NCDEX]
      D3[Govt. Schemes]
      D4[Earth Engine]
    end
    A1 --> B1
    A1 --> B2
    A1 --> B3
    A1 --> B4
    B3 --> C1
    B3 --> C2
    B3 --> C3
    B3 --> C4
    B3 --> C5
    C2 --> D1
    C2 --> D2
    C2 --> D3
    C1 --> D4
```

---

## 4. Wireframes / Mock Diagrams (Textual Description)

### Home Screen
- Large voice button ("Ask your question")
- Quick access: Diagnose Disease, Market Prices, Schemes, Land Advisor
- Language selector at top

### Disease Diagnosis
- Camera view with "Take Photo" button
- Upload from gallery option
- Result screen: Disease name, solution, local remedies, voice playback

### Market Analysis
- Input: "Which crop?" (voice/text)
- Output: Current prices, trend graph, best time to sell, voice summary

### Government Schemes
- Search bar: "What do you need?"
- List of matched schemes with icons
- Tap for details: eligibility, benefits, apply link, voice explanation

### Land Utilization Advisor
- Form: Land size, soil type, location (voice/text)
- Output: Crop plan, technology suggestions, satellite map, voice advice

---

*For actual presentation, use these diagrams in Mermaid-compatible tools or convert to images for slides. Wireframes can be sketched based on the above descriptions.* 