
# Tableau Project

This repository contains the team's individual Tableau analyses, the shared data warehouse backup, and the integrated dashboard.

## Repository layout

```text
.
├── dashboards/
├── data/
│   └── DataWarehouse.bak
├── workbooks/
│   ├── Gaser_Tableau.twbx
│   ├── Hana_Tableau.twbx
│   ├── Mohamed_Tableau.twbx
│   ├── Mostafa_Tableau.twbx
│   ├── Tableau_Project.twbx
│   └── Yara_Tableau.twbx
└── README.md
```

Each team member's work is in the Tableau workbook named after them:

| Member      | Analysis                                                                    | Workbook location                  |
| ----------- | --------------------------------------------------------------------------- | ---------------------------------- |
| Yara        | Sales Analysis — sales, profit, orders, and quantity                       | `workbooks/Yara_Tableau.twbx`    |
| Mohamed     | Customer Analysis — customers, segments, and top customers                 | `workbooks/Mohamed_Tableau.twbx` |
| Gaser       | Product Analysis — categories, products, and top/bottom products           | `workbooks/Gaser_Tableau.twbx`   |
| Mostafa     | Shipping & Geographic Analysis — ship modes, regions, cities, and delivery | `workbooks/Mostafa_Tableau.twbx` |
| Hana Faisal | Executive Dashboard + Integration — KPIs and the final story/dashboard     | `workbooks/Hana_Tableau.twbx`    |

`workbooks/Tableau_Project.twbx` is the combined project workbook. The `dashboards/` folder contains dashboard-related project files.

## Clone the repository in VS Code

### Using VS Code

1. Install [Git](https://git-scm.com/downloads) and open VS Code.
2. Press `Ctrl+Shift+P` and select **Git: Clone**.
3. Paste the repository URL:

   ```text
   https://github.com/Hanafaisal/tableau-project.git
   ```
4. Select a local folder where you want to save the project.
5. When cloning finishes, select **Open** to open the repository in VS Code.

### Using the VS Code terminal

```powershell
git clone https://github.com/Hanafaisal/tableau-project.git
cd tableau-project
code .
```

Cloning downloads the files committed to the repository's default branch. You need write access to GitHub to push changes; cloning alone only creates a local copy.

## SQL and the data warehouse

**SQL** (Structured Query Language) is used to read and work with data stored in a database. A database contains tables; tables contain rows (records) and columns (fields). For example, a sales table might have columns for order date, product, quantity, and sales amount.

A **data warehouse** is a database organized for combining and analyzing data and building reports. In this project, Tableau uses the `DataWarehouse` database as a source for the team's analyses and dashboards.

The file `data/DataWarehouse.bak` is a SQL Server database backup. It is not a database server and cannot be opened directly in Tableau. Restore it into SQL Server first; then Tableau can connect to the restored database.

## Requirements

- Microsoft SQL Server Database Engine installed and running. SQL Server Express can be used if it is compatible with the backup.
- SQL Server Management Studio (SSMS) installed.
- The repository cloned, so `data/DataWarehouse.bak` is available locally.
- Permission to restore a database on the chosen SQL Server instance.

SSMS is the management tool; installing SSMS alone does not install the SQL Server Database Engine. See [SQL Server downloads](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) and [install SSMS](https://learn.microsoft.com/en-us/ssms/install/install).

## Start SQL Server and restore the data warehouse

### Start the SQL Server service

1. Open **SQL Server Configuration Manager** from the Windows Start menu. If it is not available, open Windows **Services** by searching for `Services` or running `services.msc`.
2. Find the installed database engine service, commonly **SQL Server (SQLEXPRESS)** for SQL Server Express.
3. If its status is **Stopped**, right-click it and choose **Start**. If the service is not listed, the SQL Server Database Engine may not be installed; SSMS by itself is not enough.
4. Open SSMS and connect to **Database Engine**. For a local Express installation, try `localhost\SQLEXPRESS` with **Windows Authentication**. Use the actual instance name if yours is different.

### Restore `DataWarehouse.bak` in SSMS

1. In Object Explorer, right-click **Databases** and select **Restore Database...**.
2. Under **Source**, select **Device**, click `...`, then **Add**.
3. Browse to the cloned project folder and select `data/DataWarehouse.bak`. Click **OK** to return to the restore window.
4. Under **Destination**, set **Database** to `DataWarehouse`. If the name is not listed, type it in.
5. Open the **Files** page and review the data and log file locations. SQL Server usually fills these in automatically. If the restore reports that files already exist, choose a different location or use **Relocate all files to folder** as appropriate.
6. Click **OK** to restore. When it succeeds, refresh **Databases** in Object Explorer and confirm that `DataWarehouse` appears.

Do not select **Overwrite the existing database** unless you intend to replace an existing database. If SQL Server cannot access the backup file, move it to a folder the SQL Server service can read, then select it from that location. For Microsoft's full procedure, see [Restore a database backup using SSMS](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-ver17).

## Connect Tableau to the restored database

1. Open a workbook in Tableau.
2. Choose the **Microsoft SQL Server** connector.
3. Enter the same server or instance name used in SSMS, such as `localhost\SQLEXPRESS`.
4. Choose the authentication method configured for your SQL Server, then select the `DataWarehouse` database.
5. Select the tables or views needed by the workbook. If Tableau reports that the data source is unavailable, check that SQL Server is running and that the server name and authentication match your SSMS connection.

A simple SQL query can check that a table contains data. Replace `dbo.Orders` with an actual table name from the restored database:

```sql
SELECT TOP 10 *
FROM dbo.Orders;
```

## Download and view a workbook

Open the `workbooks/` folder on GitHub, select the workbook you want, and choose **Download raw file**. Open the downloaded `.twbx` file with Tableau Desktop or Tableau Public. The `.twbx` file is a packaged workbook; if its saved data connection is unavailable, connect it to the restored `DataWarehouse` database.
