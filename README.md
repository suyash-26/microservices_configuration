# Configuration Management in Microservices Architecture

## Overview

This setup uses a centralized configuration approach to manage application settings across multiple microservices. Instead of hardcoding configurations in each service, a dedicated configuration service is used to provide environment-specific properties dynamically.

---

## Configuration Microservice

The **Configuration Microservice** acts as a centralized source of truth for all configuration properties.

### Responsibilities:

* Stores common configurations shared across multiple microservices
* Supports environment-based configurations using profiles such as:

  * `dev`
  * `prod`
  * `default`
* Fetches the appropriate configuration based on the active profile
* Serves configuration data to other microservices on request

### Benefits:

* Eliminates duplication of configuration across services
* Simplifies environment management
* Enables dynamic updates without redeploying services (if configured)

---

## How Other Microservices Use It

Each microservice:

1. Connects to the Configuration Microservice at startup
2. Requests configuration based on:

   * Application name
   * Active profile (e.g., `dev`, `prod`)
3. Loads the received configuration into its runtime environment

This ensures all services remain consistent and environment-aware.

---

## Service Registry

A **Service Registry** is used to manage service discovery across the system.

### Purpose:

* Maintains a registry of all available microservices and their instances
* Each service instance is registered with a unique identifier
* Helps services dynamically discover and communicate with each other

### Why It’s Needed:

Instead of hardcoding:

* Service URLs
* Ports
* Instance details

Microservices can:

* Query the registry
* Resolve service names to actual instances
* Automatically handle scaling (multiple instances)

---

## Overall Flow

1. Microservice starts
2. Registers itself with the Service Registry
3. Fetches configuration from the Configuration Microservice based on its profile
4. Uses Service Registry for discovering and communicating with other services

---

## Key Advantages

* Centralized configuration management
* Environment-specific configurations (dev, prod, etc.)
* No hardcoded service endpoints
* Scalable and dynamic service discovery
* Easier maintenance and deployment

---

## Summary

This architecture combines:

* **Configuration Microservice** → for centralized and environment-based configuration
* **Service Registry** → for dynamic service discovery

Together, they enable a scalable, maintainable, and flexible microservices ecosystem.


                ┌───────────────┐
                │    Client     │
                └──────┬────────┘
                       │
                       ▼
               ┌───────────────┐
               │  API Gateway  │
               └──────┬────────┘
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
 ┌────────────┐ ┌────────────┐ ┌────────────┐
 │ Service A  │ │ Service B  │ │ Service C  │
 └─────┬──────┘ └─────┬──────┘ └─────┬──────┘
       │              │              │
       ▼              ▼              ▼
 ┌────────────┐ ┌────────────┐ ┌────────────┐
 │ Config     │ │ Config     │ │ Config     │
 │ Service    │ │ Service    │ │ Service    │
 └────────────┘ └────────────┘ └────────────┘

(All services also interact with Service Registry)

                ┌────────────────────┐
                │ Service Registry   │
                │ (Service Discovery)│
                └────────────────────┘
