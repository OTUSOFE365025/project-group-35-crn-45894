# Iteration 2

## Step 1 — Review Inputs

### **Use Cases**
- **UC1 – Query Info:** Natural-language academic inquiries.
- **UC2 – Get Notifications:** Upcoming deadlines and updates.
- **UC3 – Sync External Data:** Integrate with LMS, registration, and calendar systems.

### **Concerns**
- **CRN-2:** Maintain consistent performance and availability under peak load.
- **CRN-3:** Ensure reliable synchronization with external university systems.

### **Constraints**
- **CON-1:** Queries must respond within ~2 seconds.
- **CON-2 / CON-4:** Must support cloud-native deployment and scale to 5,000+ users.
- **CON-5:** System must maintain 99.5% uptime.

### **Quality Attributes**
- **QA2 – Scalability**
- **QA3 – Interoperability**
- **QA4 – Availability**


## Step 2 — Establish Iteration Goal by Selecting Drivers

The goal of this iteration is to refine the architecture into more concrete elements that support AIDAP’s primary requirements. This includes defining modules linked to the selected use cases and ensuring consistent performance, reliable integrations, and scalable structure.

### **Selected Drivers**

#### **Use Cases**
- **UC1 – Query Info**
- **UC2 – Get Notifications**
- **UC3 – Sync External Data**

#### **Concerns**
- **CRN-2 – Performance and Availability under load**
- **CRN-3 – Reliable Integration**

#### **Quality Attributes**
- **QA2 – Scalability**
- **QA3 – Interoperability**
- **QA4 – Availability**


## Step 3 — Choose One or More Elements of the System to Refine

### **High-Level Step 3 Diagram**
![Step 3 Diagram](iteration2assets/step3.png)

The refinement focuses on:
- Interaction between students and the system.
- All external connections (LMS, registration, calendar).
- Strengthening scalability, availability, and performance under heavy load.


## Step 4 — Choose Design Concepts That Satisfy the Selected Drivers

A domain model is created first to identify core objects before decomposition.

### **Previous Concepts**
- **Layered Architecture**
- **Adapter Pattern**
- **Cloud-Native Deployment**

### **New Concepts**
- **Message Queue / Event Bus**
- **Caching**
- **Circuit Breaker & Retry Pattern**
- **Background Job Scheduler**


## Step 5 — Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

A basic domain model is used to define the system elements and their relationships.

---

### **Old Components**

#### **Layered Architecture**
- **Presentation Layer:** chat interface
- **Application Layer:** NLU, orchestrator
- **Integration Layer:** adapters for LMS, registration, calendar
- **Data Layer:** configuration, logs, storage  
**Linked Drivers:** UC1, UC2, UC3, CRN-2, **QA2**, **QA4**

#### **Adapter Pattern**
Unifies access to external systems and hides API complexity.  
**Linked Drivers:** UC3, CRN-3, **QA3**

#### **Cloud-Native Deployment**
Enables scaling, availability, and separation of components.  
**Linked Drivers:** CRN-2, **QA2**, **QA4**

---

### **New Components**

#### **Message Queue / Event Bus**
Prevents overload and ensures reliable processing.  
**Location:** Between Application Layer ↔ Integration Layer  
**Linked Drivers:** CRN-2, UC2, **QA2**, **QA4**

#### **Caching**
Stores temporary student conversation data for fast access.  
**Location:** Data Layer → Cache Store  
**Linked Drivers:** CRN-2, UC1, **QA2**

#### **Circuit Breaker & Retry Pattern**
Prevents repeated failing calls and stabilizes external interactions.  
**Location:** Integration Layer (attached to all adapters)  
**Linked Drivers:** CRN-3, UC3, **QA3**, **QA4**

#### **Background Job Scheduler**
Runs scheduled sync tasks and notification jobs.  
**Location:** Application Layer  
**Linked Drivers:** UC2, UC3, CRN-2, **QA2**, **QA4**

---

## Step 6 — Architecture Diagrams

### **Domain Model Diagram**
![Domain Model Diagram](iteration2assets/domain.png)

### **Mapped Domain Model Diagram**
![Mapped Domain Model](iteration2assets/mapped.png)

### **Layered Architecture Diagram**
![Layered Architecture](iteration2assets/layered.png)

### **Sequence Diagram**
![Sequence Diagram](iteration2assets/sequence.png)

---

## Step 7 — Verifying That the Architecture Satisfies the Selected Use Cases and Concerns

### **UC1 – Query Info**
Supported by NLU Engine, Orchestrator, and Query Service.  
Caching improves repeated-query speed → maintains performance.

### **UC2 – Get Notifications**
Notification Service + Message Queue deliver messages reliably.  
Scheduler triggers periodic notifications when needed.

### **UC3 – Sync External Data**
Syncer Service uses LMS/Registration/Calendar adapters.  
Circuit Breaker + Retry ensures stable synchronization during failures.

### **CRN-2 — Performance and Availability**
Handled through caching, message queue, cloud scaling, and separation of responsibilities.

### **CRN-3 — Reliable Integration**
Handled through Adapter Pattern, Circuit Breaker, and Retry logic.

### **QA2 — Scalability**
Cloud-native scaling, caching, and message queue support system growth.

### **QA3 — Interoperability**
Adapters + stable retry logic ensure smooth multi-system integration.

### **QA4 — Availability**
Circuit breaker, retry, and scalable deployment maintain uptime.

The architecture satisfies all selected use cases, concerns, and quality attributes for this iteration.

