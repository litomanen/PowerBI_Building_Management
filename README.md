# PowerBI_Building_Management
Power BI analytics platform monitoring building operations, risk matrices, CO2 emissions, and energy efficiency for smart asset management.
# Dashboard de Gestão Operacional, Risco & Eficiência Energética de Infraestruturas

## Visão Geral do Projeto
Este projeto consiste no desenvolvimento de um painel analítico multinível no Power BI para monitorização de 500 edifícios e infraestruturas em Portugal. O objetivo principal é apoiar gestores de ativos na identificação de edifícios ineficientes, priorização de manutenções com base em matrizes de risco e otimização de custos operacionais.

---

## Navegação & Relatórios

| Home & Mapeamento Geoespacial | Ficha Detalhada do Edifício |
| :---: | :---: |
| ![Home](docs/preview_home.png) | ![Ficha](docs/preview_ficha.png) |

---

## ⚙️ Arquitetura Técnica & Metodologia

### 1. ETL & Power Query
- Limpeza e padronização dos registos de consumos, incidentes e custos de manutenção.
- Tratamento de tipos de dados, valores nulos e criação de atributos derivados (décadas de construção, escalões de consumo).

### 2. Modelagem Relacional
- Implementação de um modelo em estrela (*Star Schema*) garantindo integridade referencial e performance otimizada nas métricas.

### 3. Fórmulas DAX Destacadas

```dax
-- % de Edifícios Ineficientes (Classes D e E)
% Edificios Ineficientes = 
VAR TotalEdificios = COUNTROWS(building_data)
VAR Ineficientes = CALCULATE(COUNTROWS(building_data), building_data[energy_rating] IN {"D", "E"})
RETURN DIVIDE(Ineficientes, TotalEdificios, 0)

-- Controlo de Seleção Dinâmica na Ficha
Cidade_Selecionada = 
IF(HASONEVALUE(building_data[building_id]), SELECTEDVALUE(building_data[city]), "Aguardando Seleção")

# Infrastructure Operational Management, Risk & Energy Efficiency Dashboard

[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-Data_Analysis_Expressions-blue?style=for-the-badge)](https://learn.microsoft.com/dax/)
[![Power Query](https://img.shields.io/badge/Power_Query-ETL_Pipeline-green?style=for-the-badge)](https://learn.microsoft.com/power-query/)

---

## Executive Summary
This project presents an interactive multi-level Power BI analytics platform designed to monitor **500 building assets** across Portugal. 

By integrating geospatial mapping, operational risk assessments, and ESG metrics (CO₂ footprint and energy efficiency classes), the dashboard empowers asset managers and facility leads to transition from static reporting to real-time, data-driven decision-making.

---

## Dashboard Structure & Key Features

### 1. Spatial Mapping & Portfolio Segmentation (Home)
* **Geospatial Insights:** Integrated **Azure Maps** displaying building locations with color-coded classification by asset category[cite: 11].
* **Dynamic Slicing:** Hierarchical filter linkage enabling seamless cross-filtering between building types, geographic regions, and individual asset IDs[cite: 9, 10].

### 2. Individual Asset Deep Dive (Building Sheet)
* **Granular Asset Profile:** Detailed overview featuring built year, floor count, total surface area ($\text{m}^2$), and construction value[cite: 8].
* **Operational KPIs:** Real-time tracking of current **Occupation Rate** (89%), **Annual Maintenance Cost**, and **Hazard Risk Level**[cite: 8].
* **Filter Synchronization:** Multi-page sync rules and drillthrough capabilities ensuring consistent cross-page navigation without visual clutter[cite: 6, 7].

### 3. Risk Assessment & ESG Impact
* **Incident Trends:** 5-year historical breakdown of incident frequencies across regions and risk brackets[cite: 3].
* **Energy Rating & Retrofit Needs:** Identification of inefficient building stock (Classes D–E) requiring targeted renovation capital expenditures[cite: 4, 5].
* **Carbon Footprint Monitoring:** Analysis of total $\text{CO}_2$ emissions and intensity metrics ($\text{CO}_2/\text{m}^2$) by building type.

---

## Data Architecture & Methodology

### Data Modeling (Star Schema)
The underlying architecture leverages a optimized **Star Schema** separating Fact tables (`fBuildingData`, `fIncidents`, `fMaintenance`) from Dimension tables (`dCalendar`, `dLocation`, `dEnergyRating`) to maximize DAX query performance and report scalability.

## Executive Summary & Analytical Insights

### 1. Portfolio Overview & Infrastructure Catalog
The primary objective of this project is to provide a dynamic, interactive infrastructure catalog for managing a portfolio of 500 buildings across Portugal. Users can navigate geographically via Azure Maps or filter directly by `building_id` or `building_type` to access individual building profiles. Each profile aggregates critical operational metrics: spatial location, energy ratings, construction year, occupancy rate, floor count, annual maintenance costs, and energy consumption.

The analytical dashboard is structured into three executive views: **Risk Overview**, **Energy Insights**, and **Environmental Impact**.

---

### 2. Risk & Incident Analysis
* **Perceived vs. Actual Risk:** Data demonstrates that incident frequency does not directly correlate with theoretical hazard risk classifications. Low-risk properties frequently record higher incident volumes than high-risk ones. In the **Centro region**, for instance, low-risk buildings accumulated **36 incidents** over the last 5 years, compared to **25 in high-risk buildings**.
* **Absolute vs. Relative Risk (Incidence Rate):** While the Centro region leads in absolute incident volume, evaluating the custom DAX metric **Incidence Rate** (total incidents normalized by building count per region) reveals **Lisbon as the highest-risk zone at a 64.71% incidence rate**, followed by Centro. The Alentejo region exhibits the lowest operational risk.
* **Asset Class Vulnerability:** Incident counts are distributed fairly evenly across building types. **Commercial assets** recorded the highest number of incidents (**55**), whereas **Industrial assets** recorded the lowest (**44**). This lower rate in industrial facilities likely reflects the impact of strict occupational safety and health regulations (OSH).

---

### 3. Energy Efficiency & Asset Lifecycle
* **Pre-2000 Renovation Urgency:** Building age is the primary driver for structural and energy retrofits. Using a dedicated DAX metric (`% Renovação Pré-2000`), the analysis reveals that **65.55% of all buildings constructed before 2000 require immediate renovation**.
* **The Modern Building Consumption Paradox:** Evaluating energy consumption across construction decades shows that modern buildings exhibit higher average energy demand and maintenance expenditure. This trend is driven by higher technology density, automated systems, and continuous HVAC operational requirements in newer builds.

---

### 4. Environmental Impact & ESG Compliance
* **Carbon Footprint by Asset Class:** Carbon emissions correlate strongly with building usage, with **Commercial properties** generating the highest CO₂ footprint and **Industrial assets** accounting for the lowest.
* **Portfolio Energy Inefficiency:** A portfolio-wide evaluation reveals that **45.80% of properties are energy inefficient (rated D or E)**. This highlights a critical operational vulnerability and underscores the need for targeted capital expenditure (CapEx) in energy efficiency upgrades to meet EU ESG compliance standards.