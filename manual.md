# Celonis EMS: Ultimate Step-by-Step Dashboard Setup Guide

This manual provides an ultra-detailed, step-by-step guide on how to take the raw CSV datasets provided in the Summer Analytics Capstone and turn them into a fully functioning interactive process dashboard in your **Celonis Academic EMS (Execution Management System)** environment.

---

## Phase 1: Data Integration & Uploading CSVs

The first step is to bring your raw data into Celonis by creating a **Data Pool**.

1. **Log in to Celonis EMS:** Go to `signup.celonis.com` (or your academic team URL) and log in.
2. **Navigate to Data Integration:** On the left-hand navigation sidebar, click on **Data** -> **Data Integration**.
3. **Create a Data Pool:**
   - Click the **+ New Data Pool** button in the top right corner.
   - Name it `Decarbonizing_Travel_Pool` and click Create.
4. **Upload the Files:**
   - Inside your new Data Pool, click on **Data Connections** -> **+ Add Data Connection**.
   - Select **Upload Files**.
   - Drag and drop the following three files from your `Public Dataset` folder:
     1. `public_trip_data.csv`
     2. `public_trip_event_log.csv`
     3. `public_trip_event_attributes.csv`
   - *Note:* Make sure the delimiter is set correctly to Comma `,` and the system detects the headers.
   - Click **Execute Upload** (the play button) for each table until you see a green checkmark indicating the tables are staged.

---

## Phase 2: Building the Data Model

Celonis needs to know how these tables relate to each other to perform process mining.

1. **Go to Data Models:** Inside your Data Pool, look at the left sidebar under "Output" and click **Data Models**.
2. **Create New Data Model:** Click **+ Add Data Model**. Name it `Travel_Emissions_Model`.
3. **Add Tables:** Click **Add Table** and select the three tables you just uploaded. 
4. **Define Foreign Keys (Relationships):**
   - Click the **Foreign Keys** tab.
   - You need to link the event tables to the main case table (`public_trip_data`).
   - Create a link from `public_trip_event_log` to `public_trip_data` using `TripID` as the joining column.
   - Create a link from `public_trip_event_attributes` to `public_trip_data` using `TripID` as the joining column.
5. **Configure the Activity Table (Crucial for Process Mining):**
   - Go to the **Process Configuration** or **Activity Table** tab.
   - Select `public_trip_event_log` as your Activity Table.
   - Map the required columns:
     - **Case ID:** `TripID`
     - **Activity Name:** `EventName`
     - **Timestamp:** `EventTimestamp`
6. **Load the Data Model:** Click the **Load Data Model** button (top right). Wait until it says "Load Successful". Your data is now ready for Studio!

---

## Phase 3: Celonis Studio (Building the Dashboard)

Now you will build the visual dashboards that you need to screenshot for Slide 9.

1. **Navigate to Studio:** Open the left-hand main navigation sidebar and select **Studio**.
2. **Create a Package & View:**
   - Click **+ Create Package** and name it `Travel Capstone App`.
   - Inside the package, click **+ Create Asset** -> **View**. Name the View `Executive Emissions Dashboard`.
   - Open the View and click **Connect Data Model**. Select the `Travel_Emissions_Model` you built in Phase 2.
3. **Set Up the Tabs:** In the View Editor, click the **+ Tab** icon at the top to create three tabs. Rename them exactly as follows:
   - `Tab 1: Emissions Overview`
   - `Tab 2: Benchmarking`
   - `Tab 3: Emissions Drilldown`

---

### Building Tab 1: Emissions Overview

1. **Total Emissions KPI:**
   - Drag the **Number** component onto the canvas.
   - Set the title to "Total CO2e Footprint (kg)".
   - Set the PQL Formula: `SUM("public_trip_data"."TotalCO2e")`
2. **Total Net Costs KPI:**
   - Drag another **Number** component.
   - Set title to "Total Trip Spend".
   - Set PQL Formula: `SUM("public_trip_data"."NetCosts")`
3. **Trip Volume KPI:**
   - Drag a **Number** component.
   - Set PQL Formula: `COUNT_TABLE("public_trip_data")`
4. **Cost vs Emissions Scatter Plot:**
   - Drag a **Scatter Plot** component.
   - X-Axis (Dimension): `SUM("public_trip_data"."NetCosts")`
   - Y-Axis (KPI): `SUM("public_trip_data"."TotalCO2e")`
   - This visualizes the correlation between expensive and high-emitting trips.

---

### Building Tab 2: Benchmarking

1. **Regional Emissions Column Chart:**
   - Drag a **Column Chart** component onto the canvas.
   - Set the Dimension (X-axis) to: `"public_trip_data"."DepartureLocationCountry"`
   - Set the KPI (Y-axis) to: `SUM("public_trip_data"."TotalCO2e")`
   - Sort descending. You will clearly see Australia (APAC) dominating.
2. **Out-of-Policy Heatmap (Pivot Table):**
   - Drag a **Pivot Table** component.
   - Rows: `"public_trip_data"."DepartureLocationCountry"`
   - Columns: `"public_trip_data"."OutOfPolicy"`
   - Values (KPI): `COUNT_TABLE("public_trip_data")`
   - Go to component settings > Design, and turn on Background Color formatting to create a heatmap effect.

---

### Building Tab 3: Emissions Drilldown

*This is the most critical tab as it proves you can do Process Mining.*

1. **The Process Explorer:**
   - Drag the **Process Explorer** component onto the canvas. It will automatically populate using the Activity Table you configured in Phase 2.
   - Adjust the sliders (Connections and Activities) to reveal the "Unhappy Paths", such as "Trip Extension" or "Mode of Transport Changed".
2. **The Variant Explorer:**
   - Drag the **Variant Explorer** component next to it. This shows the most common standard pathways versus the long-tail irregular paths.
3. **Transport Mode Emissions Breakdown (Bar Chart):**
   - Drag a **Bar Chart** component.
   - Dimension: `"public_trip_data"."ShippingTypeDescription"`
   - KPI: `AVG("public_trip_data"."TotalCO2e")`
   - *Insight:* This will show that Business Class flights emit exponentially more per trip than Economy or Rail.

---

## Final Phase: Exporting to PowerPoint

1. Click **Publish** or **Save** in Celonis Studio.
2. Open the published view and navigate to **Tab 1: Emissions Overview**.
3. Use your computer's screenshot tool (Snipping Tool on Windows, `Cmd+Shift+4` on Mac) to take a clean, full-screen capture of the dashboard.
4. Repeat for **Tab 2** and **Tab 3**.
5. Paste these three screenshots onto Slide 9 (or Slides 9, 10, and 11) of your `Vaibhav_Soni_Capstone.pptx` presentation.

You have now successfully leveraged the Celonis Execution Management System for process mining and secured maximum points for the technical dashboarding requirements!
