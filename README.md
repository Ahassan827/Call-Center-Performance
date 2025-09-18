
# 📊 Call Center Performance Analysis  

## 🧹 Data Cleaning (Power Query)

Applied steps in Power Query for cleaning the dataset:

1. **Source** → Loaded the dataset `01 Call-Center-Dataset`.  
2. **Removed Duplicates** → Eliminated duplicate records to ensure accuracy.  
3. **Changed Data Types** → Adjusted data types for each column (e.g., numbers, dates, text).  
4. **Renamed Columns** → For clarity, e.g.:  
   - `avgtalkduration` → `Talking Duration`.  
5. **Calculated Start of Hour** → Extracted hour from datetime for time-based analysis.  
6. **Inserted Day Name** → Added a column for the day of the week.  
7. **Inserted Day of Week** → Added numeric representation of weekdays.  

---

## 📌 DAX Measures

```DAX
-- Average Satisfaction Rate
Avg Satisfaction Rate = 
AVERAGE('01 Call-Center-Dataset'[satisfaction rating])

-- Average Speed of Answer (ASA)
Avg Speed of call answer = 
AVERAGE('01 Call-Center-Dataset'[speed of answer in seconds])

-- Average Talking Duration (exclude zero durations)
Avg Talking Duration = 
AVERAGEX(
    FILTER(
        '01 Call-Center-Dataset',
        '01 Call-Center-Dataset'[talking duration] > 0
    ),
    '01 Call-Center-Dataset'[talking duration]
)

-- Call Abandoned %
Call Abandoned % = 
VAR abondand =
    CALCULATE(
        COUNTA('01 Call-Center-Dataset'[call id]),
        '01 Call-Center-Dataset'[answered (Y/N)] = "n"
    )
RETURN
    DIVIDE(abondand, [Total Calls], 0)

-- Call Resolved %
Call Resolved % = 
VAR resolvedcalls =
    CALCULATE(
        COUNTA('01 Call-Center-Dataset'[call id]),
        '01 Call-Center-Dataset'[resolved] = "y"
    )
VAR calls = 
    CALCULATE(
        COUNT('01 Call-Center-Dataset'[call id]),
        '01 Call-Center-Dataset'[answered (Y/N)] = "Y"
    )
RETURN
    DIVIDE(resolvedcalls, calls, 0)

-- Total Calls
Total Calls = 
DISTINCTCOUNT('01 Call-Center-Dataset'[call id])

-- Satisfaction Levels Classification
satistfaction levels = 
SWITCH(
    TRUE(),
    ISBLANK('01 Call-Center-Dataset'[satisfaction rating]), "Not Served",
    '01 Call-Center-Dataset'[satisfaction rating] = 1, "Not Satisfied",
    '01 Call-Center-Dataset'[satisfaction rating] = 2, "Not Satisfied",
    '01 Call-Center-Dataset'[satisfaction rating] = 3, "Normal",
    '01 Call-Center-Dataset'[satisfaction rating] = 4, "Satisfied",
    '01 Call-Center-Dataset'[satisfaction rating] = 5, "Very Satisfied"
)


## 📌 Key Performance Indicators (KPIs)  

| KPI                         | Value         |
|------------------------------|---------------|
| Total Calls                 | 5,000         |
| Customer Satisfaction (CSAT) | 3.40 / 5      |
| Average Handle Time (AHT)   | 224.92 sec    |
| Average Speed of Answer (ASA) | 67.52 sec   |
| Abandoned Call Rate         | 18.92%        |
| Resolution Rate (FCR)       | 89.94%        |


## 🔑 Key Insights  

- **Overall Performance:**  
  Call center performance shows a slight decline from **January to February** in customer satisfaction and resolution rate, coupled with an increase in abandoned calls. However, there was a positive recovery in **March**.  

- **Peak Hours & Days:**  
  - Busiest times: **11:00 AM – 5:00 PM**  
  - Busiest days: **Wednesday, Thursday, Friday**  

- **Customer Satisfaction:**  
  - Majority of customers are **"satisfied"** or **"normal"**.  
  - A significant portion expressed **"dissatisfaction"**, indicating a clear area for improvement.  

- **Topic Analysis:**  
  - **Technical Support** and **Streaming** are the most frequent call topics.  
  - **Technical Support** calls have the **highest abandonment rate**, suggesting long wait times or customer frustration.  
  - **Payment-related** issues are resolved **fastest**.  
  - **Contract** and **Admin Support** calls take the **longest**.  

- **Agent Performance:**  
  - **Top Performers:** *Martha, Greg, Dan* → consistently high performance, especially in technical/complex topics.  
  - **Areas for Improvement:** *Joe, Stewart* → lower satisfaction & resolution rates, need targeted training.  
  - **Specialization:**  
    - Martha → excels in **Technical Support**  
    - Jim → strong in **Contract-related issues**  
    - Diane → best in **Admin Support**  

---

## ✅ Recommendations  

1. **Optimize Staffing**  
   - Allocate more agents during **peak hours (11 AM – 5 PM)** and **peak days (Wed–Fri)**.  
   - Focus especially on **Technical Support** to reduce abandoned calls.  

2. **Implement Targeted Training**  
   - Train **Joe** to improve customer service & satisfaction rates.  
   - Coach **Stewart** to address low resolution performance.  

3. **Create a Knowledge Sharing System**  
   - Document best practices from **Martha** and **Dan**.  
   - Share strategies across the team to raise performance standards.  

4. **Explore Skill-Based Routing**  
   - Route calls automatically based on agent specialization.  
   - Example: *Martha → Technical Support, Diane → Admin Support, Jim → Contract*.  

5. **Address the "Dissatisfaction" Root Cause**  
   - Review call recordings & feedback.  
   - Identify systemic service/product issues behind dissatisfaction.  

6. **Focus on High-Abandonment Topics**  
   - Investigate **Technical Support** & **Contract-related** calls.  
   - Possible causes: complex IVR menus, long hold times, poor communication.  

---

