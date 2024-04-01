# Data Migration Task
---

As an AWS Data Engineer I have been tasked with Migrating an on-premise Postgres 12 database with 20TB of data to AWS. 

Here's an evaluation of options and recommendations:

## AWS Database Migration Service (DMS):
- **Pros:**
  - Supports homogeneous (*PostgreSQL to PostgreSQL*) and heterogeneous (*other databases to PostgreSQL*) migrations.
  - Fully managed service.
  - Continuous data replication (*minimizing downtime & capture ongoing changes*).
  - Suitable for any database size.
  - On-demand payment (*only pay for what is used*).
  - Supports Schema Conversion Tool (SCT) to covert engine types between source DB & target DB.
- **Cons:**
  - Limited to supported databases/database versions (*< Postgresql 10 not supported*).
- **Recommendation:**
  - This method of PostgreSQL migration utilizes a publisher and subscriber model. This model is appropriate for any database size, with an established schema in the destination database, low downtime requirements, and engine compatibility.

## DB Snapshot and restore (pg_dump and pg_restore) + Comprinno (AWS Specialization Partner):
- **Pros:**
  - Straightforward method (*quick & easy*) by leveraging database snapshots.
- **Cons:**
  - Involves significant downtime during the snapshot and restore process.
  - Requires careful planning to ensure consistency during the snapshot.
  - Not suitable for database sizes >100GB.
- **Recommendation:**
  - This method is straightforward utilizing pg_dump & pg_restore (*native PostgreSQL client utility*) for data migration and Comprinno for migration planning, however the downtime involved might not be acceptable for a database of this size.

## Snowball:
- **Pros:**
  - High Capacity (*able to move TB & PB data sizes*).
  - Supports fast online transfers (*10 Gigabit Ethernet and USB 3.0 support rapid data transfer rates*) and offline transfers.
  - On-demand payment or Upfront cost (*1 & 3 years commitment*).
- **Cons:**
  - Requires physical shipment of device, adds time to migration (*2-3 weeks arrival period & 1 week end-end transfer*).
- **Recommendation:**
  - This method is only viable in situations where there's  limited bandwidth or there's a need to migrate large volumes of data relatively quickly.

---

My final recommendation: Given the size of your database, database engine, schema, and the necessity to minimize downtime, AWS DMS is the best option. AWS DMS provides convenience, ease of use, smooth database transition, and overall cost-effectiveness when compared to other transfer options, such as Snowball or Snowmobile.

If the on-premise database contains features that are not supported by native AWS database choices, such as special extensions or custom configurations, the migration method will be considerably impacted. In such scenarios, a hybrid migration method becomes more advantageous, with DMS doing the initial data transfer and then using EC2 instances to perform manual modifications or data transformation to handle unsupported features. This adds more complexity to the migration process.

## Proof-of-concept (POC) plan for DMS solution:
This plan is for testing the AWS Database Migration Service (DMS) solution which involves defining objectives, selecting a suitable environment, setting up the migration, executing tests, and evaluating the results.

1. Objectives:
   - AWS DMS to migrate on-premise PostgreSQL databases (version 10 and above) to AWS with minimal downtime and zero data loss.
   - Leverage AWS DMS's pay-as-you-go model for scalability, making it easy to manage migration resources based on workload.
2. Environment Setup:
   - Set up source (on-premise Postgres 12) and target (RDS Postgres 12) databases, along with replication server and endpoints.
   - Configure migration tasks for full load (20TB data) without schema changes, and establish security measures for DMS access to AWS resources.
3. Migration Plan:
   - Execute full load migration strategy for 20TB data without altering the schema.
4. Data Migration Execution:
   - Initiate DMS migration task, monitor progress, and ensure data consistency and integrity between source and target databases.
   - Validate ongoing data replication (if using CDC) through source database modifications.
5. Performance Testing:
   - Evaluate DMS performance in terms of speed, throughput, and latency, considering scalability under different loads.
   - Assess impact on source database performance during migration.
6. Security & Compliance:
   - Validate DMS security measures (*data encryption in-transit & at rest*) and ensure compliance with regulations for data privacy.
7. Error Handling & Recovery:
   - Implement error handling mechanisms and document troubleshooting procedures for migration errors or failures.
8. Post-Migration Validation:
   - Conduct post-migration validation tests to confirm successful data transfer and assess performance on the target AWS database.
9. Documentation & Reporting:
    - Document any limitations or areas for improvement during the POC, and provide a detailed report summarizing findings and recommendations.
10. Review & Decision:
    - Evaluate whether DMS meets migration requirements and consider alternative solutions if necessary.

# Step-By-Step Implementation Plan
1. Provision AWS account and resources required for the migration project (*VPC, subnets, DB subnets, security groups, NACLs*), and set IAM roles & permisssions for DMS access to other AWS resources.
2. Analyze the source PostgreSQL 12 database schema and data to identify any unsupported features or custom configurations that may require conversion.
3. Set up AWS DMS replication instance and endpoints (*source: PostgreSQL 12 database & target: RDS PostgreSQL 12 database*), and configure replication tasks for initial data load and ongoing replication.
4. Establish a secure connectivity between on-premise database and AWS by leveraging Direct Connect (DX), configure security groups, NACLs, and ensure data encryption during transit and at rest.
5. Initiate the migration task for initial data load, monitor data transfer progress and replication latency via AWS Management console or CLI.
6. Validate data integrity and consistency between source and target databases.
7. Conduct performance tests to assess data transfer speeds, latency. 
8. Validate security & compliance with performance requirements.
9. Optimize database performance on AWS by tuning parameters, indexing, and implementing best practices for database optimization.
10. Implement backup and recovery procedures for the AWS database by leveraging AWS Backup, and test backup & restore processes to ensure data recoverability.
11. Document the migration process, configurations, and best practices for knowledge sharing and future reference.





