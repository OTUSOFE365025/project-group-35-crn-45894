# Iteration 3 - 7steps 

## Step 1: 

## Step 2: Establish Iteration Goal by Selecting Drivers

Refine the internal architecture of the Sync Subsystem to ensure:

    reliable periodic synchronization
    safe external API access
    retry + circuit-breaker behavior
    consistency with cloud-native deployment

## Step 3: Choose One or More Elements of the System to Refine

Element to refine: 

    Sync Subsystem (consist of Job Scheduler , Syncer Service , Adapters , Retry Handler , Circuit Breaker)

    The part of the architecture we are refining now is the synchronization workflow, since it still has many parts that were not fully defined in the previous iteration. This includes how scheduled jobs trigger sync operations, how failures are handled, and how the system interacts with external APIs without breaking during peak load or outages. Refining this system make sures the system stays reliable, consistent, and stable even when external services behave unpredictably.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

For this iteration, we focus on the sync and reliability subsystem, so the design concepts are chosen to make synchronization reliable, fault-tolerant, and safe when external systems misbehave.

    Scheduled Task Runner: Triggers sync at fixed intervals to avoid random load spikes.
    Structured Sync Pipeline: Clear stages for fetching, transforming, and storing data.
    Retry Pattern: Safely retries temporary failures when external systems are unstable.
    Circuit Breaker: Stops repeated calls to failing APIs to protect the system.
    Bulkhead Pattern: Keeps each external system’s sync isolated so one failure doesn’t block others.
    Sync Logging: Records sync results for monitoring and debugging.

    These concepts directly strengthen UC3 and address concerns around reliability, availability, and stable external integration.

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

    JobScheduler:
    Runs sync tasks at fixed intervals to keep updates consistent.

    SyncManager:
    Coordinates each sync run and routes jobs to the correct handler.

    SyncHandlers (LMS / Registration / Calendar):
    Perform the core sync steps: fetch --> transform --> validate --> store.

    RetryHandle:
    Retries temporary failures safely to improve reliability.

    CircuitBreaker:
    Stops calls to failing external systems until they recover.

    External Adapters:
    Provide a consistent interface to each external API.

    SyncLog Service:
    Tracks sync results for monitoring and troubleshooting.

These components together create a stable, fault-tolerant sync workflow.
