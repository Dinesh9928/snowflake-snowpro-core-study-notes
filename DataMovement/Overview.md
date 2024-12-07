# Data Movement #

Loading and unloading data into Snowflake can be:
* [Bulk Loading](./BulkLoading.md)
* [Continuous Data Loading](./ContinuousDataLoading.md)
* using a Connector

Factors which affect loading times are
* the physical location of the stage,
* GZIP compression efficiency, or
* the number and types of columns

You can both load and unload data into tables with the `COPY` command
* LOAD: copy data from a stage into a Snowflake table 
* UNLOAD: copy data from a table to a stage

## File Formats ##
A schema-level named object which defines the format information required for Snowflake to interpret a file. You can specify different parameters, for example, the file’s delimiter, if you want to skip the header or not, etc.

| Format  | Type            | Load | Unload | Binary Format |
|---------|-----------------|------|--------|---------------|
| CSV     | Structured      | Yes  | Yes    | No            |
| JSON    | Semi-structured | Yes  | Yes    | No            |
| Parquet | Semi-structured | Yes  | Yes    | Yes           |
| XML     | Semi-structured | Yes  | No     | No            |
| AVRO    | Semi-structured | Yes  | No     | Yes           |
| ORC     | Semi-structured | Yes  | No     | Yes           |

* Structured data: CSV
  * It’s the fastest file format to load data
  * May need to specify `FIELD_DELIMITER`, `RECORD_DELIMITER` and whether the file header should be skipped with `SKIP_HEADER`
* Semi-structured data: JSON, Parquet, Avro, ORC, XML
  * Semi-structured data can be loaded as a `VARIANT` type in a Snowflake table
  * The maximum size for the `VARIANT` data type is 16MB
  * it can be queried using JSON notation, see [Query Semi-Structured Data](../SemiStructuredData/QuerySemiStructuredData.md)
* File Format has to be specified for every `COPY INTO` command
  * A File Format can be attached to a stage - automatically associates the File Format for all files stored in the stage
  * A File Format can be attached to a table - automatically associates the File Format for all `COPY INTO` commands targeting the table
  * A File Format can be specified for each `COPY INTO` command which will override the stage or table ones, if specified

## MCQs

### 1. Which of the following is **NOT** a factor that affects data loading time in Snowflake?
- A. The physical location of the stage
- B. The GZIP compression efficiency
- C. The number and types of columns
- D. The size of the Snowflake database
<details><summary>Answer</summary> D. The size of the Snowflake database</details>

### 2. What is the command used to load data into Snowflake from a stage?
- A. LOAD INTO
- B. COPY INTO
- C. INSERT INTO
- D. TRANSFER INTO
<details><summary>Answer</summary> B. COPY INTO</details>

### 3. Which file format can **NOT** be used to unload data from Snowflake?
- A. CSV
- B. JSON
- C. Parquet
- D. AVRO
<details><summary>Answer</summary> B. JSON</details>

### 4. How can you specify the file format when using the `COPY INTO` command in Snowflake?
- A. By specifying a `FILE_FORMAT` clause in the command
- B. By attaching a file format to the stage or table
- C. By modifying the stage or table properties
- D. All of the above
<details><summary>Answer</summary> D. All of the above</details>

### 5. Which file format supports binary data for both loading and unloading in Snowflake?
- A. CSV
- B. JSON
- C. Parquet
- D. ORC
<details><summary>Answer</summary> C. Parquet</details>

### 6. Which file format does **NOT** support unloading data from Snowflake?
- A. CSV
- B. JSON
- C. AVRO
- D. Parquet
<details><summary>Answer</summary> B. JSON</details>

### 7. What is the maximum size for the `VARIANT` data type in Snowflake?
- A. 16KB
- B. 16MB
- C. 64MB
- D. 1GB
<details><summary>Answer</summary> B. 16MB</details>

### 8. Which of the following is a **semi-structured** file format that can be loaded into Snowflake?
- A. CSV
- B. Parquet
- C. ORC
- D. All of the above
<details><summary>Answer</summary> D. All of the above</details>

### 9. When loading data using `COPY INTO` from a stage, which of the following can be overridden by specifying it in the `COPY INTO` command?
- A. Table-level file format
- B. Stage-level file format
- C. File format attached to the warehouse
- D. All of the above
<details><summary>Answer</summary> B. Stage-level file format</details>

### 10. What is the benefit of using semi-structured data formats like JSON, Parquet, and Avro in Snowflake?
- A. They can be stored as a `VARIANT` type
- B. They are not supported for querying
- C. They consume less storage space compared to structured formats
- D. They require no additional compression
<details><summary>Answer</summary> A. They can be stored as a `VARIANT` type</details>

### 11. Which file format requires specifying `FIELD_DELIMITER` and `RECORD_DELIMITER` when loading data into Snowflake?
- A. Parquet
- B. CSV
- C. AVRO
- D. ORC
<details><summary>Answer</summary> B. CSV</details>

### 12. What is the primary purpose of file formats in Snowflake?
- A. To define the structure of the data being loaded or unloaded
- B. To store raw data in a non-compressed form
- C. To provide encryption for data at rest
- D. To limit the size of the file during data load
<details><summary>Answer</summary> A. To define the structure of the data being loaded or unloaded</details>

### 13. Which of the following is **NOT** a semi-structured file format supported by Snowflake?
- A. JSON
- B. CSV
- C. Parquet
- D. ORC
<details><summary>Answer</summary> B. CSV</details>

### 14. What is the compressed size of a file in Snowflake used for when billing customers for storage?
- A. Uncompressed size
- B. Original file size
- C. Compressed size
- D. Storage space used by internal stages
<details><summary>Answer</summary> C. Compressed size</details>

### 15. Which of the following actions is required when unloading data from a Snowflake table into a stage?
- A. Use of the `COPY INTO` command
- B. Define a file format
- C. Specify compression settings
- D. Both A and B
<details><summary>Answer</summary> D. Both A and B</details>

### 16. What is the maximum size of data that can be stored in the VARIANT data type for semi-structured data in Snowflake?
- A. 1 MB
- B. 10 MB
- C. 16 MB
- D. 32 MB
<details><summary>Answer</summary> C. 16 MB</details>

### 17. In which of the following cases can a File Format be specified?
- A. Only in the `COPY INTO` command
- B. Only when creating a table
- C. Only when creating a stage
- D. All of the above
<details><summary>Answer</summary> D. All of the above</details>

### 18. What file format is used for both loading and unloading semi-structured data like JSON or Parquet?
- A. AVRO
- B. CSV
- C. JSON
- D. Parquet
<details><summary>Answer</summary> C. JSON</details>

### 19. Which of the following is a true statement about CSV file format in Snowflake?
- A. It is used for both structured and semi-structured data.
- B. It is typically the slowest file format to load data.
- C. It may require specifying delimiters and record headers.
- D. It can only be used to load data into Snowflake.
<details><summary>Answer</summary> C. It may require specifying delimiters and record headers.</details>

### 20. Which of the following file formats can be used with the VARIANT data type in Snowflake?
- A. CSV
- B. AVRO
- C. JSON
- D. Both B and C
<details><summary>Answer</summary> D. Both B and C</details>

### 21. Which of the following is NOT true about the `COPY INTO` command in Snowflake?
- A. The File Format must be specified for the command.
- B. A File Format can be attached to a table, stage, or used explicitly in the command.
- C. The `COPY INTO` command is only used to load data into Snowflake.
- D. It can be used to unload data from a Snowflake table to a stage.
<details><summary>Answer</summary> C. The `COPY INTO` command is only used to load data into Snowflake.</details>

### 22. What is the effect of using GZIP compression on file formats when loading data into Snowflake?
- A. It will increase the storage costs.
- B. It improves the data load efficiency.
- C. It can prevent pruning during query optimization.
- D. It reduces the compression ratio for larger files.
<details><summary>Answer</summary> B. It improves the data load efficiency.</details>

### 23. Which file format supports binary data in Snowflake?
- A. JSON
- B. Parquet
- C. AVRO
- D. ORC
<details><summary>Answer</summary> B. Parquet</details>

### 24. How does Snowflake store semi-structured data such as JSON or AVRO?
- A. In the VARIANT data type.
- B. In the BINARY data type.
- C. In the STRING data type.
- D. In the ARRAY data type.
<details><summary>Answer</summary> A. In the VARIANT data type.</details>

### 25. Which of the following is a key advantage of using the Parquet file format in Snowflake?
- A. It supports both structured and semi-structured data but is not efficient for binary data.
- B. It is the only file format that supports binary data.
- C. It supports columnar storage, improving query performance for large datasets.
- D. It is not supported for unloading data.
<details><summary>Answer</summary> C. It supports columnar storage, improving query performance for large datasets.</details>

### 26. Which of the following file formats is used for loading data into Snowflake as the VARIANT data type?
- A. CSV
- B. JSON
- C. ORC
- D. XML
<details><summary>Answer</summary> B. JSON</details>

### 27. What does Snowflake do when loading semi-structured data?
- A. Automatically converts it into a relational format.
- B. Loads it into a VARIANT data type.
- C. Stores it in a BINARY format.
- D. Requires manual conversion before loading.
<details><summary>Answer</summary> B. Loads it into a VARIANT data type.</details>

### 28. When using the `COPY INTO` command, how can you override the default file format?
- A. By specifying the `FILE_FORMAT` parameter in the command.
- B. By using the `STAGE_FILE_FORMAT` option.
- C. By using the `OVERWRITE` option.
- D. By changing the table schema.
<details><summary>Answer</summary> A. By specifying the `FILE_FORMAT` parameter in the command.</details>

### 29. What is the primary benefit of using the ORC file format in Snowflake?
- A. It is a compressed format suitable for structured data.
- B. It supports binary storage for better data transfer speed.
- C. It is optimized for large-scale, high-throughput data processing.
- D. It can store data in a row-based format for quicker queries.
<details><summary>Answer</summary> C. It is optimized for large-scale, high-throughput data processing.</details>

### 30. Which of the following statements is true regarding data unloading in Snowflake?
- A. Data can only be unloaded from a Snowflake table to a CSV file.
- B. Semi-structured data such as JSON cannot be unloaded.
- C. The `COPY INTO` command can be used for both loading and unloading data.
- D. Snowflake does not support unloading data into external stages.
<details><summary>Answer</summary> C. The `COPY INTO` command can be used for both loading and unloading data.</details>

### 31. Which file format does Snowflake support for unloading data?
- A. CSV
- B. JSON
- C. XML
- D. AVRO
<details><summary>Answer</summary> A. CSV</details>

### 32. Which of the following file formats supports binary format for unloading data from Snowflake?
- A. CSV
- B. JSON
- C. Parquet
- D. ORC
<details><summary>Answer</summary> C. Parquet</details>

### 33. What does Snowflake use to store metadata for semi-structured data?
- A. The VARIANT data type.
- B. The JSON data type.
- C. The BINARY data type.
- D. The STRUCTURED data type.
<details><summary>Answer</summary> A. The VARIANT data type.</details>

### 34. What file format is most suitable for structured data in Snowflake?
- A. JSON
- B. CSV
- C. Parquet
- D. AVRO
<details><summary>Answer</summary> B. CSV</details>

### 35. In Snowflake, which file format is used to store compressed data in a columnar format for efficient querying?
- A. CSV
- B. Parquet
- C. ORC
- D. JSON
<details><summary>Answer</summary> B. Parquet</details>

### 36. What is the maximum size allowed for a VARIANT data type in Snowflake?
- A. 1 MB
- B. 5 MB
- C. 8 MB
- D. 16 MB
<details><summary>Answer</summary> D. 16 MB</details>

### 37. Which of the following file formats is NOT supported for unloading data in Snowflake?
- A. CSV
- B. Parquet
- C. XML
- D. AVRO
<details><summary>Answer</summary> C. XML</details>

### 38. What happens when a file format is specified for a `COPY INTO` command in Snowflake?
- A. It overrides the file format attached to the table or stage.
- B. It only affects loading operations.
- C. It automatically sets the compression level for the file.
- D. It disables the automatic schema detection.
<details><summary>Answer</summary> A. It overrides the file format attached to the table or stage.</details>

### 39. When loading data into Snowflake, what happens if the header is not skipped in CSV format?
- A. Snowflake will automatically infer the column names.
- B. The first row of data will be treated as column headers.
- C. An error will occur during loading.
- D. The first row of data will be skipped.
<details><summary>Answer</summary> B. The first row of data will be treated as column headers.</details>

### 40. Which statement is true about the `COPY INTO` command in Snowflake?
- A. It can only be used to load data from an external stage.
- B. It allows unloading data into external stages.
- C. It supports only structured data formats.
- D. It only works with internal Snowflake stages.
<details><summary>Answer</summary> B. It allows unloading data into external stages.</details>

