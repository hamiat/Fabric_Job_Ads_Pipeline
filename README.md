# JobTech Job Listings ETL Pipeline (IT Sector, Sweden)

This project connects to Arbetsförmedlingen’s JobSearch API to gather, clean, and analyze IT job market data in Sweden using Microsoft Fabric and Power BI. You can import the pipeline JSON file to reproduce the pipeline in your Fabric workspace.

### Overview
- **Fabric Pipeline** orchestrates the entire workflow 
- Data ingestion using **Copy Activity** with dynamic API parameters  
- Transformation using **Data Flow Gen 2** to clean and shape data into silver tables  
- Storage in a **Lakehouse** as bronze (raw) and silver (cleaned) tables
- Analysis using **Power BI** for reporting and research, as well as the gold table creation (dimensional data modeling) 

### API Details

- Base URL: `https://jobsearch.api.jobtechdev.se`  
- Dynamic query:

```text
@concat(
    'search?occupation-field=', pipeline().parameters.occupation_field,
    '&municipality=', join(pipeline().parameters.municipality,'&municipality='),
    '&offset=', pipeline().parameters.offset,
    '&limit=', pipeline().parameters.limit
)
```
- Municipality takes in taxonomy codes that can be found at here: [Statistiska centralbyrån](https://www.scb.se/hitta-statistik/regional-statistik-och-kartor/regionala-indelningar/lan-och-kommuner/lan-och-kommuner-i-kodnummerordning/)
- Occupation-field taxonomy code "apaJ_2ja_LuF" returns all jobs ads under the IT category






### Scheduling

- The pipeline is scheduled to refresh monthly, keeping data up-to-date for research purposes

### Notes

- Flexible design allows for adding new occupations or municipalities with minimal changes
- Provides a structured dataset ideal for IT labor market analysis in Sweden, ready for Power BI reporting

### Pictures
#### Pipeline
<img width="905" height="730" alt="fabric_pipeline_parameters" src="https://github.com/user-attachments/assets/1a758c7c-afa3-4f1a-aa9d-2206e19637dc" />

- The pipeline is easy to modify for different search parameters, kindly see [the API documentation](https://jobsearch.api.jobtechdev.se)

#### Data Flow Gen 2
<img width="1911" height="911" alt="fabric_dataflowgen2" src="https://github.com/user-attachments/assets/6aea34b6-7030-4095-aa85-e35105f80f41" />

#### Lake House
<img width="1786" height="739" alt="image" src="https://github.com/user-attachments/assets/8fc9f814-c8d1-4c39-a750-078b4fcca955" />

- Bronze table created from copy activity, acting as source for the dataflow
- Silver tables created from Data Flow activity, acting as source for the Power BI report

#### Power BI
<img width="1799" height="990" alt="image" src="https://github.com/user-attachments/assets/b1289fe4-360f-4138-ab23-c743e8b13b6b" />
<img width="1844" height="992" alt="image" src="https://github.com/user-attachments/assets/de64b16d-2e6c-4523-92e0-799113c799dc" />

- Report created with job ads listed between October and December 2025 filtered on Stockholm, Göteborg and Malmö as well as a number of Software Dev skills.
- Dataset excluding job abs under occupations "Drifttekniker" & "Supporttekniker".  


