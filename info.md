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

## FIND ALL PBX IN A PBX HOW MUCH EXTENTIONS BY a Certain Copany

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
WhERE c.name = 'Al thiqa'
    -- AND pbxs.id = c.id
    -- AND pbxs.PBX_Name = 'Thiqa'
GROUP BY 
    c.id,
    pbxs.id;
```

 ## Find the company and the all extensions in it by using the [PBX_NAME]

 ```
 SELECT 
    pbxs.PBX_Name,
    c.name AS Company_Name,
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
    where pbxs.PBX_Name in ('KCM Ambitions - Location Wings','Thiqa')
GROUP BY 
    c.id,
    pbxs.id
ORDER BY c.name ASC;
```

## Find the [user's id, user's name, member id, member name] By UserID

```
    SELECT 
        u.id AS User_Id,
        u.username AS User_Name,
        m.id AS Member_Id,
        m.type AS Member_Type
    FROM users u
        LEFT JOIN members m
        ON u.id = m.user_id 
    
    WHERE u.username = 'nahuma_nazar'
    ORDER BY u.username ASC;
 
```
## Get All Overview of Companies members list 

```
 SELECT 
    c.name AS Company_Name,
    
     COUNT(
            *
        ) AS All_Members,
    
        COUNT(
        CASE 
            WHEN 
                m.type = 'SUPER_ADMIN'
            THEN  1
            END
        ) AS Super_Admins,
        
    COUNT(
        CASE 
            WHEN 
                m.type = 'ADMIN'
            THEN  1
            END
        ) AS Admins,
    
    COUNT(
        CASE 
            WHEN 
                m.type = 'MANAGER'
            THEN  1
            END
        ) AS Managers,
        
    COUNT(
        CASE 
            WHEN 
                m.type = 'AGENT'
            THEN  1
            END
        ) AS Agents,
    
    COUNT(
        CASE 
            WHEN 
                m.type = 'RM'
            THEN  1
            END
        ) AS RMs,
    
    COUNT(
        CASE 
            WHEN 
                m.type = 'MACHINE'
            THEN 1
            END
        ) AS Machines
    
    FROM companies c
    LEFT JOIN members m
        ON c.id = m.company_id
        
    GROUP BY c.id;

  ```


  ## Get the ORG - Overwiev total list members by its member_type

  ```
  SELECT 
    -- Use COALESCE to replace the NULL rollup row with a "Grand Total" label
    COALESCE(c.name, 'GRAND TOTAL')
    AS Company_Name,
    
    COUNT(*)
    AS All_Members,
    
    COUNT(CASE WHEN m.type = 'SUPER_ADMIN' THEN 1 END)
    AS Super_Admins,
    
    COUNT(CASE WHEN m.type = 'ADMIN' THEN 1 END)
    AS Admins,
    
    COUNT(CASE WHEN m.type = 'MANAGER' THEN 1 END)
    AS Managers,
    
    COUNT(CASE WHEN m.type = 'AGENT' THEN 1 END)
    AS Agents,
    
    COUNT(CASE WHEN m.type = 'RM' THEN 1 END)
    AS RMs,
    
    COUNT(CASE WHEN m.type = 'MACHINE' THEN 1 END)
    AS Machines
    
FROM companies c
LEFT JOIN members m ON c.id = m.company_id
    WHERE m.is_active = 'true'
-- Removed the extra comma and formatted for standard SQL
GROUP BY ROLLUP (c.name); 

```

## Query For Get PBX, COMPANIES, AND TOTAL EXTENSIONS --[Get Extensions Overview by Each company and PBX]

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
--   where pbxs.PBX_Name in ('KCM Ambitions - Location Wings')
GROUP BY 
   c.id,
   pbxs.id
ORDER BY c.name ASC;

```

## Managers and Agents Relations
```
 WITH manager_data AS (
        SELECT 
            m.manager_id,
            u.name AS manager_name,
            COUNT(*) AS total_agents,
            SUM(CASE WHEN m.is_active IS TRUE THEN 1 ELSE 0 END) AS active_agents,
            SUM(CASE WHEN m.is_active IS FALSE THEN 1 ELSE 0 END) AS inactive_agents
        FROM 
            public.members m
        JOIN 
            public.users u ON m.manager_id = u.id
        WHERE 
            m.type = 'AGENT'
            AND m.company_id = '1'
            [[AND u.name = {{manager_name}}]]
            [[AND m.created_at >= {{start_date}}]]
            [[AND m.created_at <= {{end_date}}]]
            [[AND EXISTS (
                SELECT 1
                FROM users agent
                WHERE agent.id = m.user_id
                AND agent.name = {{agent_name}}
            )]]
        GROUP BY 
            m.manager_id, u.name
    )
    SELECT
        ROW_NUMBER() OVER (ORDER BY md.total_agents DESC, md.manager_name) AS "S.No.",
        md.manager_name AS "Manager",
        md.total_agents AS "Total Agents",
        md.active_agents AS "Active Agents",
        md.inactive_agents AS "Inactive Agents",
        CASE 
            WHEN md.total_agents = 0 THEN 0
            ELSE ROUND((md.active_agents::numeric / md.total_agents) * 100, 2)
        END AS "Active Agents (%)"
    FROM
        manager_data md
    ORDER BY
        md.total_agents DESC, md.manager_name;

```

## Get User Details by -- User_name OR name

```
SELECT 
 
 u.username AS username,
 u.password,
 u.name AS Name,
--  u.id AS User_ID,
--  m.id AS Member_ID,
 m.phone_number AS Phone_Number,
 c.name AS Company_Name,
 pbxs.PBX_Name AS PBX_Name,
 e.code AS Extension_Number
 
FROM users u
LEFT JOIN members m
 ON u.id = m.User_ID
LEFT JOIN companies c
 ON c.id = m.company_id
LEFT JOIN member_extension_relationships mer
    ON mer.id = m.id
LEFT JOIN extensions e
    ON e.id = mer.extension_id
    
LEFT JOIN pbx_systems pbxs
    ON mer.extension_id = pbxs.id
    
WHERE u.username = ''
    OR u.name = 'Keerthana Babu'

GROUP BY
 u.id,
 m.id,
 c.id,
 mer.id,
 e.id,
 pbxs.id;

 ```

 ## Get the User Details by User Name [name, username, pass, member_type, phoneNumber, Company, PBX, Extension_Code]
 ```
 

SELECT 
    u.Username AS username,
    u.password AS User_password,
    -- m.User_ID AS user_id,
    u.Name AS User_Name,
    m.Type AS member_type,
    m.Phone_Number AS Phone_Number,
    c.Name AS Company_Name,
    pbxs.Pbx_Name AS PBX_Name,
    e.Code AS Extension_Number
FROM Users u
LEFT JOIN members m 
    ON u.ID = m.User_ID
    
LEFT JOIN Companies c 
    ON c.ID = m.Company_ID
    
LEFT JOIN Member_Extension_Relationships mer 
    ON m.ID = mer.Member_ID
    
LEFT JOIN Extensions e 
    ON mer.Extension_ID = e.ID
LEFT JOIN Pbx_Systems pbxs 
    ON e.Pbx_System_ID = pbxs.ID
WHERE u.Username = '' 
   OR u.Name = 'AKASH'
GROUP BY 
    u.Username, 
    m.User_ID, 
    u.Name, 
    m.Type, 
    m.Phone_Number,
    c.Name, 
    pbxs.Pbx_Name, 
    e.Code,
    u.password
    limit 1;
  ```


  ## GET TOTAL INVAILD LEADS BY SOURCE NAME 

 ```
 SELECT 
    COALESCE(s.name, 'GRAND TOTAL') AS source_name,
    COUNT(l.phone_number) AS total_invalid_leads
FROM leads l
LEFT JOIN source_allocations sa 
    ON l.source_allocation_id = sa.id
LEFT JOIN sources s
    ON sa.source_id = s.id
WHERE l.status = 'INVALID'
    AND l.last_call_datetime >= '2025-12-01' 
    AND l.last_call_datetime < '2026-01-01'
GROUP BY ROLLUP (s.name);
```