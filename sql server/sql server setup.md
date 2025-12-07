# SQL Server 2025 Developer – Full Local Install Notes

These are the steps I followed to install Microsoft SQL Server 2025 Developer Edition locally with all major features enabled and no Azure billing risk.
Download SQL Server 2025 Developer edition from - https://www.microsoft.com/en-us/sql-server/sql-server-downloads (I downloaded Enterprise Developer Edition)

---

## 1. Start the Installer

1. Run the SQL Server Installation Center.
2. Go to Installation.
3. Select "New SQL Server standalone installation or add features to an existing installation."

---

## 2. Edition Selection

1. Choose "Specify a free edition."
2. Select "Enterprise Developer."
3. Do not enable Azure pay-as-you-go.
4. Click Next.

Just disable or unclick anything to do with Azure.
---

## 3. License, Rules, Updates

1. Accept licence terms → Next.
2. Let Global Rules and Microsoft Update checks run → Next.
3. Install Setup Files → Next.
4. If Windows Firewall shows a warning, ignore it for local development → Next.

---

## 4. Turn Off Azure Extension

On the Azure Extension for SQL Server screen:

1. Untick "Azure Extension for SQL Server."
2. Ensure no Azure features are enabled.
3. Click Next.

This keeps the installation fully local and prevents any Azure resource creation or billing.

---

## 5. Feature Selection

Selected features were:

### Instance Features
- Database Engine Services  
- SQL Server Replication  
- AI Services and Language Extensions  
- Full-Text and Semantic Extractions for Search  
- PolyBase Query Service for External Data  
- Analysis Services  

### Shared Features
- Integration Services  
- Scale Out Master  
- Scale Out Worker  

Disk usage was about 10 GB, which was acceptable.

Leave install directories at defaults, then click Next.

---

## 6. Instance Configuration

1. Select "Default instance."
2. Instance ID will be MSSQLSERVER.
3. Leave directory settings as-is.
4. Click Next.

A default instance allows connecting using simply "localhost."

---

## 7. PolyBase Configuration

When prompted for the PolyBase port range:

1. Leave the default: 16450-16460.
2. Click Next.

---

## 8. Server Configuration (SQL Services)

On the Service Accounts tab:

1. Leave all accounts as their default NT SERVICE virtual accounts.
2. Set Startup Types:
   - SQL Server Agent → Automatic
   - SQL Server Database Engine → Automatic
   - SQL Server Analysis Services → Automatic
   - Integration Services and related Scale Out services → Automatic
   - SQL Server Launchpad → Automatic
   - SQL Full-text Filter Daemon Launcher → Manual
   - SQL Server Browser → Disabled

3. Enable:
   - "Grant Perform Volume Maintenance Tasks privilege to SQL Server Database Engine Service"

Click Next.

---

## 9. Database Engine Configuration

On the Server Configuration tab:

1. Select "Mixed Mode (SQL Server authentication and Windows authentication)."
2. Create and confirm a strong password for the sa login.
3. Click "Add Current User" so your Windows account becomes a SQL Server administrator.
4. Leave Data Directories, TempDB, MaxDOP, Memory, and FILESTREAM as defaults.
5. Click Next.

---

## 10. Analysis Services Configuration

1. Select "Tabular Mode."
2. Click "Add Current User" to add yourself as an SSAS administrator.
3. Leave Data Directories as defaults.
4. Click Next.

---

## 11. Integration Services Scale Out (Master Node)

1. Leave Port Number as default (8391).
2. Keep "Create a new SSL certificate" selected.
3. Leave the CN field as generated (machine name and internal IP).
4. Click Next.

---

## 12. Integration Services Scale Out (Worker Node)

1. Leave the Master Endpoint as the default value (https://<your-machine>:8391).
2. Leave Certificate Path empty.
3. Click Next.

---

## 13. Feature Configuration Rules

Allow the rules to run.  
If all pass or only non-critical warnings appear → Next.

---

## 14. Ready to Install

Review the summary:

- Developer Edition  
- Default instance  
- Mixed Mode authentication  
- Your Windows account added as admin  
- Analysis Services in Tabular mode  
- Integration Services, PolyBase, and ML components configured  
- Azure Extension disabled  

Then click Install.

---

## 15. Post-Installation Steps

1. Install SQL Server Management Studio (SSMS).
   - Download from Microsoft.

2. Test Windows Authentication:
   - Open SSMS.
   - Server type: Database Engine
   - Server name: localhost
   - Authentication: Windows Authentication
   - Connect.

3. Test SQL Authentication:
   - Server name: localhost
   - Login: sa
   - Password: (the one you created)
   - Connect.

4. Begin creating databases, tables, and queries as needed.

---

## Notes

- No Azure features were enabled, so the installation cannot incur cloud charges.
- SQL Server Agent was set to Automatic to allow scheduled jobs.
- All services using NT SERVICE virtual accounts is the recommended approach.
- Defaults for memory and TempDB are fine for a development workstation.
