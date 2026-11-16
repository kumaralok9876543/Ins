# Ins

# INSURANCE360 — ENTERPRISE-SCALE SYNTHETIC INSURANCE DATA GENERATION

You are acting as a Senior Data Engineer, Data Architect, Python Engineer, PySpark Engineer and Synthetic Data Engineering Specialist.

Your task is to inspect the COMPLETE existing Insurance360 Enterprise Data Platform repository and implement a production-style, configurable, large-scale synthetic insurance data generation framework.

IMPORTANT: Do not create an unrelated demo project. Everything must follow the existing Insurance360 business requirements, architecture, entity relationships, data dictionary, ERD, source-to-target mapping and project documentation.

==================================================
1. PROJECT CONTEXT
==================================================

Project: Insurance360
Fictional Client: GlobalSure Insurance Group
Industry: Insurance
Insurance domains:
- Health Insurance
- Life Insurance
- Motor Insurance
- Property Insurance

Target architecture:

Synthetic Source Data
        |
        v
Local Parquet Data Files
        |
        v
AWS S3 Raw/Landing Zone
        |
        v
Databricks Auto Loader / Batch Ingestion
        |
        v
Bronze Delta Tables
        |
        v
Silver Standardization + Validation + Deduplication + CDC
        |
        v
Gold Business Data Marts
        |
        v
Power BI / Analytics

The project targets 10M+ total records across the existing 28 core business entities, plus operational and audit datasets.

This is fictional synthetic data only. Never use real customer, employee, bank, hospital or insurance-company data.

==================================================
2. FIRST: AUDIT THE EXISTING REPOSITORY
==================================================

Before writing code, inspect all relevant existing files and folders.

Mandatory documents to inspect:

- README.md
- 01_business_analysis/BRD.md
- 01_business_analysis/FRD.md
- 01_business_analysis/PROJECT_SCOPE.md
- 02_architecture/ARCHITECTURE.md
- 02_architecture/ERD.md
- 02_architecture/DATA_DICTIONARY.md
- 02_architecture/SOURCE_TO_TARGET_MAPPING_TEMPLATE.md
- 03_source_data/
- 04_aws/
- 05_databricks/
- 06_etl_development/
- 07_etl_testing/

If filenames differ, discover their actual locations.

Understand:
1. All source systems.
2. All existing entities and tables.
3. Column names and data types.
4. Primary keys and foreign keys.
5. Business relationships.
6. Source-to-target mappings.
7. Expected data formats.
8. Insurance business rules.
9. Existing synthetic generator code, if any.

Do not invent a new data model when the repository already defines one.

If documentation contains inconsistencies, identify them, follow the most authoritative data dictionary/ERD and document the resolution.

Create a data generation specification before implementation.

==================================================
3. GENERATE ALL EXISTING BUSINESS ENTITIES
==================================================

Implement data generation for EVERY core business entity defined in the existing repository.

Cover all existing business departments and source systems, including:

A. Customer Management
- Customer information
- Customer addresses
- Customer contact details
- Nominees and relationships
- Customer status and lifecycle

B. Policy Management
- Insurance policies
- Policy products
- Policy coverage
- Policy beneficiaries
- Policy renewals
- Policy cancellations
- Policy status history

C. Claims Management
- Insurance claims
- Claim details
- Claim documents
- Claim status history
- Claim assessments
- Claim settlements

D. Premium and Payments
- Premium transactions
- Payment transactions
- Payment methods
- Refunds
- Invoices
- Outstanding balances

E. Agent and Broker Management
- Insurance agents
- Brokers
- Agent assignments
- Agent commissions
- Sales transactions

F. Underwriting and Risk
- Underwriting assessments
- Risk categories
- Risk scores
- Approval decisions
- Rejection reasons

G. Insurance Products
- Health products
- Life products
- Motor products
- Property products
- Coverage definitions

H. Finance and Accounting
- Financial transactions
- General ledger
- Expenses
- Settlements
- Reconciliation records

Also implement every other entity found in the actual project documentation.

Do not omit tables simply because they are difficult to generate.

For every entity, generate all documented columns, correct data types and meaningful business values.

==================================================
4. LARGE-SCALE DATA VOLUME
==================================================

Default target: AT LEAST 10 MILLION TOTAL RECORDS ACROSS THE DATASET.

Use this initial allocation as a planning target, but adjust it to the actual repository's entity model and relationships.

Example allocation:

- Customers: approximately 1,000,000
- Policies: approximately 2,000,000
- Claims: approximately 3,000,000
- Payments and premiums: approximately 2,000,000
- Remaining entities combined: approximately 2,000,000+

The 10 million target means aggregate records across all tables, NOT 10 million in every table.

Requirements:

- Support configurable record counts per table.
- Support configurable total record targets.
- Support 10M, 20M and 50M+ scale profiles.
- Allow individual table counts to be overridden.
- Generate data in configurable chunks/batches.
- Never load the complete dataset into RAM.
- Use streaming/chunked generation.
- Use efficient vectorized or batch operations where appropriate.
- Reuse deterministic ID generation.
- Support restart/resume for interrupted generation.
- Do not silently reduce requested record counts.

Before generating large datasets:
- Check available disk space.
- Estimate expected output size.
- Display the planned row counts per table.
- Display the expected number of files.
- Ask for confirmation only if the environment requires a destructive operation or available storage is insufficient.

==================================================
5. RELATIONAL DATA INTEGRITY — VERY IMPORTANT
==================================================

This must look like a real enterprise insurance database, not unrelated random rows.

Maintain referential integrity across tables.

Examples:

- Every policy must reference a valid customer.
- Every claim must reference a valid policy and customer.
- Every payment must reference a valid policy or invoice as defined by the model.
- Every agent assignment must reference a valid agent.
- Every commission must reference a valid agent and relevant transaction.
- Every claim settlement must reference a valid claim.
- Every nominee must reference an existing customer/policy as appropriate.
- Every underwriting record must reference the appropriate application or policy.

Use the exact relationships defined in the ERD.

Requirements:
- Generate parent entities before dependent entities where needed.
- Generate stable primary keys.
- Generate valid foreign keys.
- Avoid orphan records in the clean historical dataset.
- Avoid duplicate primary keys.
- Ensure date relationships make business sense.
- Ensure amounts and statuses are consistent.
- Maintain consistent customer, policy and claim references across every dataset.

Create automated relationship validation.

==================================================
6. REALISTIC INSURANCE BUSINESS DATA
==================================================

Generate realistic fictional data rather than meaningless random strings.

Examples:

Customer:
- Customer ID
- First name
- Last name
- Date of birth
- Gender where defined
- City
- State
- Postal code
- Email
- Phone
- Customer segment
- Customer creation date
- Customer status

Policy:
- Policy ID
- Customer ID
- Product ID
- Policy type
- Policy start date
- Policy end date
- Premium amount
- Sum insured
- Payment frequency
- Policy status
- Agent ID

Claim:
- Claim ID
- Policy ID
- Customer ID
- Claim date
- Claim amount
- Approved amount
- Claim type
- Claim status
- Settlement date
- Rejection reason where applicable

Payments:
- Payment ID
- Policy ID
- Invoice ID
- Transaction date
- Transaction amount
- Payment status
- Payment method
- Transaction reference

Use actual column definitions from the repository instead of blindly copying these examples.

Business rules:

- Policy end date must normally be after policy start date.
- Claim date must be consistent with policy coverage rules.
- Approved amount must not exceed claimed amount.
- Premium amounts must be non-negative.
- Payment amounts must be consistent with invoices and premium schedules.
- Settlement dates must follow claim processing dates.
- Cancelled policies must have meaningful cancellation dates.
- Renewals must connect to the appropriate previous policy where the model supports this.
- Dates must be distributed across multiple historical years.
- Use realistic status distributions.

Include realistic distributions for:
- Active policies
- Expired policies
- Cancelled policies
- Pending claims
- Approved claims
- Rejected claims
- Settled claims
- Pending payments
- Successful payments
- Failed payments

Use realistic Indian cities, states, PIN codes, phone formats and currency (INR) wherever appropriate for the project's defined geography.

Keep all personal details fictional.

==================================================
7. MULTIPLE SOURCE SYSTEM SIMULATION
==================================================

The project defines 8 logical source systems.

Inspect existing architecture and assign each entity to its documented source system.

Simulate different source-system characteristics:

- Different naming conventions where the source specification requires them.
- Different load frequencies.
- Historical extracts.
- Daily incremental extracts.
- Different source file names.
- Different source timestamps.
- Source system identifiers.
- Different schema versions where appropriate.

Maintain a source-system metadata field wherever defined or required for lineage.

Do not change canonical target table column names just to simulate source differences. Keep source-to-target mapping explicit.

==================================================
8. PARQUET OUTPUT — MANDATORY
==================================================

The primary generated dataset must be Parquet.

Use a reliable Parquet library such as PyArrow or another suitable, documented library. Select the best implementation after inspecting the Codespaces environment.

Requirements:

- Correct Parquet data types.
- Consistent schemas per entity.
- Compression using Snappy or Zstandard where supported.
- Configurable row group sizes.
- Multiple files for large tables.
- Avoid a single massive file per table.
- Avoid excessive tiny files.
- Use sensible file sizes based on available memory and storage.
- Preserve dates, timestamps, decimals and numeric values as correct data types.
- Preserve null values correctly.
- Write data in chunks.
- Do not convert all data into strings.

Use a clear folder layout such as:

03_source_data/
    generated/
        historical/
            customers/
                part-00001.parquet
                part-00002.parquet
            policies/
                part-00001.parquet
            claims/
                part-00001.parquet
            payments/
                part-00001.parquet

            [ALL OTHER DOCUMENTED ENTITY FOLDERS]

        incremental/
            2026/
                2026-01-01/
                2026-01-02/

        cdc/
            customers/
            policies/
            claims/
            payments/

        data_quality_scenarios/
            duplicate_records/
            null_records/
            invalid_foreign_keys/
            invalid_dates/
            invalid_amounts/

        metadata/
            generation_manifest.json
            table_schemas.json
            row_counts.json
            generation_config.json
            validation_report.json

Adjust the structure if existing AWS S3 conventions require it.

Every core entity must have its own clearly named folder.

==================================================
9. HISTORICAL + INCREMENTAL + CDC DATA
==================================================

Generate three separate categories of data.

A. Historical data

- Large historical dataset.
- Multiple historical years.
- Initial full load.
- Valid business relationships.
- Stable primary keys.

B. Incremental data

Generate additional files representing daily or periodic source-system extracts.

Requirements:
- New records.
- Updates to existing records.
- Changed policy statuses.
- Updated customer details.
- New claims.
- New payments.
- Renewals.
- Settlement updates.

The incremental data must reference existing historical primary keys wherever updates are intended.

C. CDC data

Generate CDC event records with the exact structure supported by the project's data model.

Include:
- INSERT
- UPDATE
- DELETE

Where applicable, include:
- Operation type
- Primary key
- Event timestamp
- Source sequence number or ordering field
- Before/after values or change payload
- Source system

Ensure deterministic event ordering and valid update/delete references.

Do not randomly delete historical data during generation.

==================================================
10. CONTROLLED DATA QUALITY SCENARIOS
==================================================

The main historical dataset should be mostly clean and relationally consistent.

Separately generate configurable data-quality scenario datasets for ETL testing.

Include:

- Duplicate records.
- Null mandatory values.
- Invalid foreign keys.
- Invalid date formats or date ranges.
- Negative premium amounts.
- Invalid policy statuses.
- Invalid claim amounts.
- Invalid payment records.
- Unexpected categories.
- Schema evolution examples.

For every quality scenario:
- Keep the records in a separate output folder.
- Record the issue type.
- Record the affected entity.
- Record the affected primary key.
- Record expected ETL handling.
- Record the number of intentionally bad records.

Make bad-record percentages configurable.

Never contaminate the clean historical dataset with accidental errors.

==================================================
11. PYTHON PROJECT STRUCTURE
==================================================

Implement a maintainable generator inside:

03_source_data/

Suggested structure:

03_source_data/
    generator/
        __init__.py
        main.py
        config.py
        cli.py

        schemas/
            __init__.py
            entity_schemas.py

        generators/
            customer_generator.py
            policy_generator.py
            claims_generator.py
            payment_generator.py
            agent_generator.py
            underwriting_generator.py
            finance_generator.py
            reference_generator.py

        utilities/
            id_generator.py
            relationship_manager.py
            date_generator.py
            business_rules.py
            parquet_writer.py
            progress_tracker.py
            disk_estimator.py
            logging_config.py

        validation/
            schema_validation.py
            primary_key_validation.py
            foreign_key_validation.py
            business_rule_validation.py
            row_count_validation.py

    generated/
        historical/
        incremental/
        cdc/
        data_quality_scenarios/
        metadata/

    tests/
        test_schemas.py
        test_primary_keys.py
        test_foreign_keys.py
        test_business_rules.py
        test_parquet_output.py
        test_incremental_generation.py
        test_cdc_generation.py

Use the actual repository structure where possible. Do not duplicate existing modules unnecessarily.

==================================================
12. CONFIGURATION AND COMMAND LINE
==================================================

Create a configurable generation system.

Example configuration:

generation:
  seed: 42
  output_format: parquet
  compression: snappy
  batch_size: 100000
  target_total_records: 10000000
  output_directory: 03_source_data/generated

profiles:
  small:
    target_total_records: 100000

  medium:
    target_total_records: 1000000

  large:
    target_total_records: 10000000

  xlarge:
    target_total_records: 20000000

Provide a CLI such as:

python -m generator.main --profile small

python -m generator.main --profile medium

python -m generator.main --profile large

python -m generator.main --profile xlarge

Also support:

python -m generator.main --total-records 10000000

python -m generator.main --entity customers --records 1000000

python -m generator.main --mode historical

python -m generator.main --mode incremental

python -m generator.main --mode cdc

python -m generator.main --mode quality

Adapt command syntax to the actual implementation.

Add:
- Progress bars or progress logs.
- Current entity name.
- Generated row count.
- Number of Parquet files.
- Output size.
- Elapsed time.
- Error handling.
- Resume support.
- Deterministic seed support.

==================================================
13. PERFORMANCE AND MEMORY MANAGEMENT
==================================================

The generator must be suitable for GitHub Codespaces.

Do not assume unlimited RAM, CPU or disk.

Requirements:

- Chunk-based generation.
- Memory-efficient writing.
- Configurable batch size.
- No unnecessary Pandas DataFrame holding millions of rows.
- Avoid generating all entities in one giant in-memory object.
- Avoid unnecessary cross joins.
- Use deterministic ID ranges or other efficient key strategies.
- Use efficient random foreign-key selection.
- Avoid expensive row-by-row Python operations where vectorization is practical.
- Write each completed batch immediately.
- Provide restart/resume without duplicating completed output.
- Never overwrite existing generated data without explicit confirmation.
- Report actual disk usage and generation statistics.

Do not claim that 10M records were generated unless the actual output and validation confirm it.

If Codespaces storage is insufficient, explain the issue and provide a smaller configurable run. Never silently claim success.

==================================================
14. DATA VALIDATION — MANDATORY
==================================================

After generation, run automated validation for every entity.

Check:

1. Expected versus actual row count.
2. Parquet files are readable.
3. Schema matches data dictionary.
4. Primary key uniqueness.
5. Foreign key integrity.
6. Null percentages.
7. Date validity.
8. Amount validity.
9. Business rule compliance.
10. Incremental records reference valid historical keys.
11. CDC event ordering.
12. Quality scenario counts.

Create:

metadata/generation_manifest.json
metadata/row_counts.json
metadata/table_schemas.json
metadata/validation_report.json

The manifest should include:
- Project name.
- Generator version.
- Generation timestamp.
- Random seed.
- Profile.
- Entity names.
- Row counts.
- File counts.
- Parquet file sizes.
- Compression type.
- Schema version.
- Output paths.
- Validation status.

Provide an overall PASS / FAIL result.

Do not mark a dataset as PASS if required validations fail.

==================================================
15. AWS S3 AND DATABRICKS COMPATIBILITY
==================================================

Generated Parquet files must be directly usable by AWS S3 and Databricks.

Prepare compatible folder paths for future upload:

s3://insurance360-data-lake/dev/customers/customers/historical/

s3://insurance360-data-lake/dev/policies/policies/historical/

s3://insurance360-data-lake/dev/claims/claims/historical/

s3://insurance360-data-lake/dev/payments/payments/historical/

Follow the existing repository's actual S3 naming convention.

Requirements:
- Preserve entity folder structure.
- Do not include local absolute paths in data.
- Do not use

- Requirements:
- Preserve entity folder structure.
- Do not include local absolute paths in data.
- Do not use machine-specific file paths in the generated dataset.
- Keep schemas consistent.
- Ensure Spark can read the generated Parquet files.
- Add a small PySpark read-validation example.
- Provide a future AWS CLI upload script.
- Provide an S3 upload configuration template.
- Provide Databricks Bronze ingestion instructions

Do not automatically create an AWS bucket.
Do not upload anything to AWS without explicit configuration and authorization.
Do not store AWS credentials in source code.
Do not commit credentials, tokens or secrets.

==================================================
16. TESTING
==================================================

Write tests for:



Write tests for:

- Every entity schema.
- Data type correctness.
- Primary key uniqueness.
- Foreign key relationships.
- Business rules.
- Deterministic generation.
- Parquet read/write.
- Batch processing.
- Resume/restart behavior.
- Incremental generation.
- CDC generation.
- Data-quality scenario generation.
- Manifest correctness.

Run tests on a small dataset first.

Run actual validation on generated data.

Do not claim tests passed unless they were executed.

==================================================
17. DOCUMENTATION
==================================================

Create or update:

03_source_data/GENERATOR_README.md
03_source_data/DATA_GENERATION_SPEC.md
03_source_data/ENTITY_VOLUME_PLAN.md
03_source_data/PARQUET_LAYOUT.md
03_source_data/GENERATION_EXECUTION_GUIDE.md

Document:

- All entities generated.
- All columns and types.
- Record counts per entity.
- Relationships.
- Generation logic.
- Business rules.
- Parquet layout.
- Historical/incremental/CDC strategy.
- Data-quality scenarios.
- Commands to run.
- Memory and disk requirements.
- Validation results.
- AWS S3 upload process.
- Databricks Bronze ingestion process.

18. IMPLEMENTATION EXECUTION PLAN
==================================================

Work in this order:

PHASE 1:
Audit the repository and understand the complete data model.

PHASE 2:
Create entity schemas and relationships.

PHASE 3:
Implement realistic business data generation.

PHASE 4:
Implement chunked Parquet output.

PHASE 5:
Implement historical data generation.

PHASE 6:
Implement incremental and CDC datasets.

PHASE 7:
Implement separate data-quality scenarios.

PHASE 8:
Implement validations, manifests and tests.

PHASE 9:
Run a small test generation and validate it.

PHASE 10:
Run the large profile with at least 10M total records if storage permits.

PHASE 11:
Update documentation and provide exact execution instructions.

Do not stop after creating empty Python files or documentation. Implement working code.

==================================================
19. FINAL ACCEPTANCE CRITERIA
==================================================

The implementation is complete only when:


All existing core entities are supported.
- All documented columns are represented.
- Relationships are valid.
- At least 10M aggregate records can be generated through the large profile.
- All generated business datasets are in Parquet format.
- Generation is chunked and memory-efficient.
- Historical data exists.
- Incremental data can be generated.
- CDC events can be generated.
- Separate data-quality scenarios exist.
- Every entity has automated validation.
- Manifest and row-count reports are produced.
- Databricks can read the Parquet files.
- S3-compatible folder structure is available.
- Tests have been executed.
- Documentation has been updated.

==================================================
20. FINAL RESPONSE FROM YOU
==================================================

After implementation, provide:

Repository audit summary.
2. List of all supported entities.
3. Table-wise record count plan.
4. Exact files created and modified.
5. Exact commands to generate 100K, 1M and 10M records.
6. Actual number of records generated.
7. Actual disk space consumed.
8. Actual number of Parquet files.
9. Validation and test results.
10. Location of all generated Parquet folders.
11. AWS S3 upload command.
12. Databricks Bronze ingestion next steps.
13. Any remaining limitations.

Be honest about actual execution results.

Do not fabricate generated files, record counts, validation results or performance metrics.

START NOW BY AUDITING THE EXISTING REPOSITORY.
