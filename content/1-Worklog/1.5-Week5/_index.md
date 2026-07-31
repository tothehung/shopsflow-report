---
title: "Worklog Week 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

- **Start Date:** June 29, 2026
- **Completion Date:** July 04, 2026

### Objectives for Week 5:
- Theoretical research on RDS PostgreSQL relational database (Multi-AZ, Read Replicas) & Amazon ElastiCache Redis Caching
- Study automated database schema migration version control using Flyway Migration tools
- Kick off Shopsflow E-Commerce project: Provision RDS PostgreSQL in Private Subnets, configure Redis cluster, and execute Flyway SQL Migration scripts

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study Amazon RDS PostgreSQL theory: Multi-AZ deployment for high availability, DB Subnet Groups, Parameter Groups, Automated Backups, and Read Replicas. |
| 3 | Research Amazon ElastiCache for Redis caching: In-Memory data structures, Redis Clusters, eviction policies, and Flyway SQL Migration version control mechanisms. |
| 4 | Hands-on AWS Lab (RDS PostgreSQL & Redis Setup): Create DB Subnet Groups, provision RDS PostgreSQL 16 in Private DB Subnets, and initialize ElastiCache Redis Cluster. |
| 5 | Integrate Spring Data JPA & Redis Caching into Shopsflow Spring Boot backend; draft Flyway migration SQL scripts (`V1__init_schema.sql`, `V2__seed_demo_data.sql`). |
| 6 | Execute Flyway migrations auto-creating tables (Users, Categories, Products, Orders, Reviews), verify DB connectivity, and update Week 5 progress report. |

### Results achieved in Week 5:
* Completed Week 5 tasks on schedule (RDS, Redis Labs & Shopsflow Database Setup).
* Successfully initialized RDS PostgreSQL relational database and Redis caching for Shopsflow.

### References & Study Materials:
- [Amazon RDS PostgreSQL User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
- [Amazon ElastiCache for Redis Guide](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/)
- [Flyway Database Migration Tool Documentation](https://documentation.red-gate.com/flyway)
