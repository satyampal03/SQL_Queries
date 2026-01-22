# SQL Queries db management

## Getting the Invalid Leads_Queries by Source

> Will Select all the source_name and will count the all leads those are the invalid leads
>> there are the 3 tables are the connected

```
    SELECT 
    s.name,
    COUNT(l.phone_number) AS total_invalid_Leads
    
FROM leads l
LEFT JOIN source_allocations sa 
    ON l.source_allocation_id = sa.id
LEFT JOIN sources s
    ON sa.source_id = s.id
WHERE l.status = ('INVALID')
    AND l.last_call_datetime >= '2025-12-01' 
    AND l.last_call_datetime < '2026-01-01'
GROUP BY s.id

```
## Getting the DNCR Leads_Queries [by date - sa.created_at]

> Will Select all the source_name and will count the all leads those are the DNCR leads
>> there are the 3 tables are the connected
 
>>[ AND s.name LIKE '%wasim%'] --> this is the Like word If we want to select like word

```
SELECT 
    s.name,
    COUNT(l.phone_number) AS total_invalid_Leads,
    s.created_at
FROM leads l
LEFT JOIN source_allocations sa 
    ON l.source_allocation_id = sa.id
LEFT JOIN sources s
    ON sa.source_id = s.id
WHERE l.status = ('DNCR')
    AND sa.created_at >= '2025-12-01' 
    AND sa.created_at < '2026-01-01'
GROUP BY s.id
```

## Getting all DND and INVALID  BY Source

> Will Select all the DNCR and invalid  All__Leads  from (source_allotion , source, leads) Tables,

```
SELECT 
    s.name,
    
    COUNT(
        CASE 
            WHEN l.status = 'DNCR'
             AND sa.created_at >= '2025-12-01'
             AND sa.created_at <  '2026-01-01'
            THEN l.phone_number
        END
    ) AS total_dncr_leads,

    COUNT(
        CASE 
            WHEN l.status = 'INVALID'
             AND l.last_call_datetime >= '2025-12-01'
             AND l.last_call_datetime <  '2026-01-01'
            THEN l.phone_number
        END
    ) AS total_invalid_leads

FROM leads l
LEFT JOIN source_allocations sa 
    ON l.source_allocation_id = sa.id
LEFT JOIN sources s
    ON sa.source_id = s.id
GROUP BY s.name;
```


## Getting  Details about PBX AND EXTENTIONS BY COMPANY

>  [total companies-- > Each Company Has how much PBS's AND Each PBX has how muxh --> Extensions]

```
    SELECT 
    c.name AS Company_Name,
    COUNT(DISTINCT pbxs.id) AS Total_PBX,
    COUNT(e.id) AS Total_Extensions
FROM companies c
LEFT JOIN pbx_systems pbxs
    ON c.id = pbxs.company_id
LEFT JOIN extensions e
    ON pbxs.id = e.pbx_system_id
GROUP BY 
    c.id;
    
```

## Getting the all detals about extensions and bpx by company

> Each Company hs how much pbx, with all pbx name, number of pbx, also each pxb has how much Tootal Extensions in each of PBX and how much are in assigned and non-assigned

```
 SELECT 
    c.name AS Company_Name,
    pbxs.PBX_Name,
    COUNT(DISTINCT pbxs.id) AS Total_PBX,
    COUNT(e.id) AS Total_Extensions,
    COUNT (
        CASE
            WHEN e.state = 'ASSIGNED'
            THEN e.id
            end
        )AS Total_Assigned_Extensions,
    COUNT (
        CASE
            WHEN e.state = 'UNASSIGNED'
            THEN e.id
            end
        ) AS Total_Unassigned_Extensions 
FROM companies c
LEFT JOIN pbx_systems pbxs
    ON c.id = pbxs.company_id
LEFT JOIN extensions e
    ON pbxs.id = e.pbx_system_id
GROUP BY 
    c.id,
    pbxs.id;
    
```

## Getting All Invalid Leads

> With the help of this querry we will get the all data_with phone number with source also within a certain date and time

```
  SELECT 
    s.name ,
     sa.created_at as Source_allocation_Date,
    l.phone_number AS total_invalid_Leads
    
FROM leads l
LEFT JOIN source_allocations sa 
    ON l.source_allocation_id = sa.id
LEFT JOIN sources s
    ON sa.source_id = s.id
WHERE l.status = ('INVALID')
    AND sa.created_at >= '2025-12-01' 
    AND sa.created_at < '2026-01-01'
GROUP BY s.id, l.id, sa.id
ORDER BY s.name ASC; 

```