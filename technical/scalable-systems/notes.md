# Scalable Systems

Rough whiteboard sketch summary
**Ask GOOD questions**

## Steps
1. Understand requirements
-- What are you solving for?
-- Inputs? Outputs?
-- Boundary conditions
    * Business
    -- User stories
    * Technical
    -- SLA (Latency)
    -- Uptime
    -- Limits (Api / Entity)
    -- Throughput
    -- Auth

2. Understand constraints
-- Read / Write heavy?
-- Availability?
-- What to optimize for?
-- Try numbers, get a sense of the problem
    * Back of the envelope calculation
    * Complexity
      -- *Data Storage Growth*
      -- *requests/sec*
      -- *network/throughput*
    * Essentially: **Scale, Throughput, Latency**

1. Design
   - Usage patterns
   - Consistency Levels
   - Consistency Levels
   - Sync vs Async
   - Procedure:
     * Figure out service interface
       - What is the MVP? What do we need to do?
       - What are our responsibilities?
       - How do we interface with other teams e.g.
         * Product / Sales / BD
         * Infrastructure / Monitoring
         * Data Science
         * Security / Legal
   - Outline workflows: Call Sequence diagrams
     - This will help build our entity relationship diagram
   - By the end of this you should have a sketch of:
     - Service Interface / External API
     - Call Sequence diagrams
     - Entity Relationship Diagrams (DB Schema too if needed)

4. Scale
   - Caching
   - Single point of failure?
   - Master - slave
   - Async and Sync?
   - Where is it fine to be slow?
   - Product hat!
   - Talk about geography, partitioning, sharding, monitoring, configuration etc

5. Deliver
   - External API
   - Call Sequence Diagrams
   - Entity Relationship Diagram
   - Schema DB
   - Architecture Diagrams
