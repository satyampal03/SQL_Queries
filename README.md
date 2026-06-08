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


## DNCR_INVALID Leads Querry 

> Want to get the invalid and dncr data from database 
>> You need to change date all there,

```

SELECT 
    s.name AS Sourse_Name,
    sa.number_of_leads AS Total_leads,
    
    COUNT(
        CASE 
            WHEN l.status = 'DNCR'
             AND sa.created_at >= '2026-01-01'
             AND sa.created_at <  '2026-01-31'
            THEN l.phone_number
        END
    ) AS total_dncr_leads,

    COUNT(
        CASE 
            WHEN l.status = 'INVALID'
             AND sa.created_at >= '2026-01-01'
             AND sa.created_at <  '2026-01-31'
            THEN l.phone_number
        END
    ) AS total_invalid_leads,
    sa.created_at

FROM leads l
LEFT JOIN source_allocations sa 
    ON l.source_allocation_id = sa.id
LEFT JOIN sources s
    ON sa.source_id = s.id
WHERE sa.created_at >= '2026-01-01'
             AND sa.created_at <  '2026-01-31'
GROUP BY s.name,sa.id
ORDER BY sa.created_at ;
```


# RM_CLIENTS DETAILS 

```
SELECT 
    rm.id AS RM_Client_ID,
    rm.account_number AS Client_Account_Number,
    rm.name AS Client_Name,
    u.name AS RM_Name, -- This is the Name fetched from the Users table
    rm.phone_number AS Client_Phone_Number,
    rm.email AS Client_Email,
    rm.nationality AS Client_Nationality,
    rm.is_active AS Status,
    rm.initial_amount,
    rm.backfilled_total_redeposit_amount,
    rm.backfilled_total_withdrawal_amount,
    rm.platform_name,
    rm.mt5_account_number,
    rm.created_at,
    rm.updated_at,
    rm.initial_credit
FROM rm_clients rm
-- 1. Connect Client to Organization
JOIN organizations org ON org.id = rm.organization_id
-- 2. Connect Client to Members table using member_id
LEFT JOIN members m ON m.id = rm.member_id
-- 3. Connect Members to Users table to get the RM's Name
LEFT JOIN users u ON u.id = m.user_id
WHERE org.id = 6;

```


# Get user details with specific member type
```
 SELECT 
        u.name As Name,
        u.id AS User_Id,
        u.username AS User_Name,
        m.id AS Member_Id,
        m.type AS Member_Type
    FROM users u
        LEFT JOIN members m
        ON u.id = m.user_id 
    
    WHERE m.type = 'RM'
    ORDER BY u.username ASC;

```


# Get All Active clients with Reffered_by with Level.
```

WITH RECURSIVE
  ReferralHierarchy AS (
    -- Anchor Member: Start with clients who were NOT referred by anyone (Level 0)
    -- Or you can set a specific starting point in the WHERE clause
    SELECT
      id,
      referred_by,
      0 AS LEVEL
    FROM
      rm_clients
    WHERE
      referred_by IS NULL
      AND organization_id = 6
    UNION ALL
    -- Recursive Member: Join the table back to the CTE to increment levels
    SELECT
      child.id,
      child.referred_by,
      parent.level + 1
    FROM
      rm_clients child
      INNER JOIN ReferralHierarchy parent ON child.referred_by = parent.id
  )
SELECT
  rm.account_number AS Client_Account_Number,
  rm.name AS Client_Name,
  u.name AS RM_Name,
  rm.id AS RM_Client_ID,
  rm.phone_number AS Client_Phone_Number,
  rm.email AS Client_Email,
  rm.nationality AS Client_Nationality,
  rm.initial_amount AS initial_amount,
  rm.backfilled_total_redeposit_amount AS BackFilled_Total_redeposit,
  rm.backfilled_total_withdrawal_amount AS BackFilled_Total_withdraw,
  rm.platform_name AS Platform_Name,
  rm.mt5_account_number AS mt5_Account,
  rm.created_at AS Created_At,
  rm.updated_at AS Updated_At,
  rm.initial_credit AS initial_Credit,
  rm.is_active AS Status,
  -- Added Referrer Information
  REF.name AS Referrer_Name,
  REF.account_number AS Referrer_Account_Number,
  rh.level AS Referral_Level
FROM
  rm_clients rm
  -- Join the CTE to get the calculated level
  JOIN ReferralHierarchy rh ON rm.id = rh.id
  JOIN organizations org ON org.id = rm.organization_id
  LEFT JOIN members m ON m.id = rm.member_id
  LEFT JOIN users u ON u.id = m.user_id
  LEFT JOIN rm_clients REF ON rm.referred_by = REF.id
WHERE
  org.id = 6
  AND rm.is_active = 'true'
ORDER BY
  rh.level ASC,
  rm.name ASC;

 ```


 # Get Agent All Saved_Leads
```
SELECT 
    -- COUNT(DISTINCT l.id) AS Total_Leads
    l.name AS Lead_Name,
  l.phone_number AS Lead_Phone_Number,
  l.last_call_datetime AS Last_Call_DateTime,
  u.name AS User_Name,
  l.Status AS Status
FROM users u
JOIN members m ON u.id = m.user_id
JOIN call_attempts ca ON m.id = ca.member_id
JOIN leads l ON ca.lead_id = l.id
WHERE u.username = 'santosh_aparna' 
  AND l.Status IN ('CANVAS', 'CANVAS_PLUS', 'HOT_LEAD');
```
# Get all redeposite, initial deposite, withrawals, active inactive, clients with, rm, manager, agent and client, floor platform every thing.

```
WITH RECURSIVE
  ReferralHierarchy AS (
    -- Anchor Member: Start with clients who were NOT referred by anyone (Level 0)
    SELECT
      id,
      referred_by,
      0 AS LEVEL
    FROM
      rm_clients
    WHERE
      referred_by IS NULL
      AND organization_id = 1
    UNION ALL
    -- Recursive Member: Join the table back to the CTE to increment levels
    SELECT
      child.id,
      child.referred_by,
      parent.level + 1
    FROM
      rm_clients child
      INNER JOIN ReferralHierarchy parent ON child.referred_by = parent.id
  ),
  
  TransactionAggregates AS (
    SELECT 
      client_id, 
      
      -- Matches 'CREDIT' from your dropdown list
      SUM(CASE WHEN UPPER(type::text) = 'CREDIT' THEN amount ELSE 0 END) AS calculated_initial_deposit,
      
      -- Matches 'RE_DEPOSIT' from your dropdown list
      SUM(CASE WHEN UPPER(type::text) = 'RE_DEPOSIT' THEN amount ELSE 0 END) AS calculated_redeposit,
      
      -- Matches 'WITHDRAWAL' from your dropdown list
      SUM(CASE WHEN UPPER(type::text) = 'WITHDRAWAL' THEN amount ELSE 0 END) AS calculated_withdrawal
    FROM 
      client_entries 
    GROUP BY 
      client_id
  )

SELECT
rm.floor_name,
  rm.sales_agent_name,
  rm.sales_manager_name,
  rm.account_type AS Account_Type,
  rm.mt5_account_number AS mt5_Account,
  rm.account_number AS Client_Account_Number,
  rm.name AS Client_Name,
  u.name AS RM_Name,
  rm.id AS RM_Client_ID,
  rm.phone_number AS Client_Phone_Number,
  rm.email AS Client_Email,
  rm.nationality AS Client_Nationality,
  
  COALESCE(ce.calculated_initial_deposit, 0) AS Initial_Deposit_Amount,
  COALESCE(ce.calculated_redeposit, 0) AS Total_Redeposit_Amount,
  COALESCE(ce.calculated_withdrawal, 0) AS Total_Withdrawal_Amount,
  
  rm.initial_amount AS Legacy_Initial_Amount,
  rm.backfilled_total_redeposit_amount AS BackFilled_Total_redeposit,
  rm.backfilled_total_withdrawal_amount AS BackFilled_Total_withdraw,
  
  rm.platform_name AS Platform_Name,
  rm.created_at AS Created_At,
  rm.updated_at AS Updated_At,
  rm.initial_credit AS initial_Credit,
  rm.is_active AS Status,
  REF.name AS Referrer_Name,
  REF.account_number AS Referrer_Account_Number,
  rh.level AS Referral_Level
FROM
  rm_clients rm
  JOIN ReferralHierarchy rh ON rm.id = rh.id
  JOIN organizations org ON org.id = rm.organization_id
  

  LEFT JOIN TransactionAggregates ce ON rm.id = ce.client_id
  
  LEFT JOIN members m ON m.id = rm.member_id
  LEFT JOIN users u ON u.id = m.user_id
  LEFT JOIN rm_clients REF ON rm.referred_by = REF.id
WHERE
  org.id = 1
  AND rm.platform_name = 'TRADERSHUB'
ORDER BY
  rh.level ASC,
  rm.name ASC;
  ```

  # Get the QC Report For KCM Ambition Call Center and Remote India, 
  ```
  SELECT 
    -- 1. Date & Record Identifiers
    DATE(aud.created_at) AS audit_date,
    aud.id AS audit_id,
    
    -- 2. Company Context
    comp.name AS company_name,
    
    -- 3. Staff Profiles (Names)
    u_agent.name AS agent_name,
    u_qc.name AS qc_member_name,
    COALESCE(u_mgr.name, 'No Action Taken') AS actioning_manager_name,
    
    -- 4. Audit Scoring & Status
    aud.audit_type,
    aud.overall_score,
    aud.has_hard_fail,
    aud.is_flagged,
    aud.flag_type,
    
    -- 5. Manager Action Details
    COALESCE(ma.action_type, 'N/A') AS manager_action,
    COALESCE(ma.note, 'No Notes') AS manager_coaching_note

FROM qc_audits aud

-- Joins
LEFT JOIN companies comp ON aud.company_id = comp.id

LEFT JOIN members m_agent ON aud.audited_member_id = m_agent.id
LEFT JOIN users u_agent ON m_agent.user_id = u_agent.id

LEFT JOIN members m_qc ON aud.qc_member_id = m_qc.id
LEFT JOIN users u_qc ON m_qc.user_id = u_qc.id

LEFT JOIN qc_manager_actions ma ON aud.id = ma.audit_id
LEFT JOIN members m_mgr ON ma.actioned_by_member_id = m_mgr.id
LEFT JOIN users u_mgr ON m_mgr.user_id = u_mgr.id

-- Date & Company Filters
WHERE aud.created_at >= '2026-06-02 00:00:00' 
  AND aud.created_at <= '2026-06-03 23:59:59'
  AND comp.name IN ('KCM ambitions call centers', 'Remote India') -- Fixed operator syntax

-- Grouping
GROUP BY 
    aud.created_at, 
    aud.id, 
    comp.name, 
    u_agent.name, 
    u_qc.name, 
    u_mgr.name, 
    aud.audit_type, 
    aud.overall_score, 
    aud.has_hard_fail, 
    aud.is_flagged, 
    aud.flag_type, 
    ma.action_type, 
    ma.note
ORDER BY 
    aud.id ASC
    ;
```

# Get the list of manager and and their team 

```
SELECT
  companies.name AS Company_Name,
  m_manager.id AS Manager_ID,
  u_mgr.name AS Manager_Name,
  m_manager.type AS Manager_Role,
  m_team.id AS Agent_ID,
  u_agt.name AS Agent_Name,
  m_team.type AS Agent_Role
FROM
  companies
  INNER JOIN members m_manager ON companies.id = m_manager.company_id
  LEFT JOIN users u_mgr ON m_manager.user_id = u_mgr.id
  LEFT JOIN members m_team ON companies.id = m_team.company_id
  AND m_team.id <> m_manager.id
  LEFT JOIN users u_agt ON m_team.user_id = u_agt.id
WHERE
  {{Company_Name}}
  AND m_manager.is_active = 'true'
  AND m_team.is_active = 'true'
  AND m_manager.type = 'MANAGER'
  AND m_team.type = 'AGENT'
ORDER BY
  companies.name ASC,
  u_mgr.name ASC,
  u_agt.name ASC


  ```

