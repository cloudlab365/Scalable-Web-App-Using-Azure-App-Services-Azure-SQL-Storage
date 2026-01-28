# 🚀 Full Deployment Guide  
## Scalable Web App Using Azure App Services, Azure SQL & Azure Storage

---

## 1️⃣ **Prerequisites**
Before deploying, ensure you have:

- An Azure subscription with **Contributor** or **Owner** permissions  
- Application code (API, frontend, or full-stack) ready for deployment  
- Azure CLI or Azure Portal access  
- Optional: GitHub or Azure DevOps for CI/CD  
- Networking requirements (if using private endpoints)

---

## 2️⃣ **Create the Resource Group**
Organize all resources into a single logical group.

### Azure Portal  
1. Go to **Resource Groups**  
2. Select **Create**  
3. Provide:  
   - Name: `rg-webapp-prod`  
   - Region: closest to your users  
4. Click **Review + Create**

---

## 3️⃣ **Deploy Azure App Service Plan**
This defines the compute resources for your web app.

### Steps  
1. Go to **App Service Plans**  
2. Select **Create**  
3. Configure:  
   - Resource Group: `rg-webapp-prod`  
   - Name: `asp-webapp-prod`  
   - Region: same as resource group  
   - Pricing Tier:  
     - **B1** for dev/test  
     - **S1** or **P1v3** for production  
4. Create the plan

---

## 4️⃣ **Deploy Azure App Service (Web App)**
This is where your application code will run.

### Steps  
1. Go to **App Services**  
2. Select **Create**  
3. Configure:  
   - Resource Group: `rg-webapp-prod`  
   - Name: `webapp-prod-001`  
   - Runtime stack: `.NET / Node.js / Python / Java`  
   - Region: same as plan  
   - App Service Plan: `asp-webapp-prod`  
4. Create the Web App

---

## 5️⃣ **Create Azure SQL Server + Database**
This will host your relational data.

### Steps  
1. Go to **Azure SQL**  
2. Select **Create SQL Database**  
3. Configure:  
   - Resource Group: `rg-webapp-prod`  
   - Database Name: `sqldb-webapp-prod`  
   - Server: Create new  
     - Name: `sqlserver-webapp-prod`  
     - Authentication:  
       - Use **Azure AD** (recommended)  
   - Compute + Storage:  
     - **General Purpose** (vCore) or **DTU S2**  
4. Create the database

### Configure Firewall  
1. Open the SQL Server  
2. Go to **Networking**  
3. Enable:  
   - Allow Azure services  
4. Add your IP if needed

---

## 6️⃣ **Create Azure Storage Account**
Used for static assets, images, documents, logs, etc.

### Steps  
1. Go to **Storage Accounts**  
2. Select **Create**  
3. Configure:  
   - Resource Group: `rg-webapp-prod`  
   - Name: `stwebappprod001`  
   - Region: same as others  
   - Redundancy: LRS or GRS  
4. Create the storage account

### Create Blob Containers  
1. Open the storage account  
2. Go to **Containers**  
3. Add containers:  
   - `assets`  
   - `uploads`  
   - `logs`  

---

## 7️⃣ **Configure Application Settings**
Your App Service needs connection strings and environment variables.

### Steps  
1. Open **App Service**  
2. Go to **Configuration**  
3. Add:  
   - **Connection Strings**  
     - Name: `DefaultConnection`  
     - Type: SQLAzure  
     - Value:  
       ```
       Server=tcp:sqlserver-webapp-prod.database.windows.net,1433;
       Initial Catalog=sqldb-webapp-prod;
       Authentication=Active Directory Default;
       ```
   - **App Settings**  
     - `StorageAccountUrl`  
     - `BlobContainerName`  
     - `UseManagedIdentity = true`

### Enable Managed Identity  
1. Go to **Identity**  
2. Turn **System Assigned** ON  
3. Save

### Assign Permissions  
- On SQL Server → Add **Azure AD Admin**  
- On Storage Account → Assign role:  
  - **Storage Blob Data Contributor**

---

## 8️⃣ **Deploy Application Code**
Choose your deployment method:

### Option A — ZIP Deploy  
1. Go to **Deployment Center**  
2. Upload ZIP package  
3. Deploy

### Option B — GitHub Actions  
1. Connect GitHub repo  
2. Auto‑generate workflow  
3. Commit and push

### Option C — Azure DevOps  
1. Create pipeline  
2. Add App Service deploy task  
3. Run pipeline

---

## 9️⃣ **Enable Monitoring & Logging**
### Application Insights  
1. Go to **App Service**  
2. Select **Application Insights**  
3. Enable and link to workspace

### Diagnostic Logs  
1. Go to **App Service → Diagnostic Settings**  
2. Send logs to:  
   - Log Analytics  
   - Storage Account  
   - Event Hub (optional)

---

## 🔟 **Configure Autoscaling**
### Steps  
1. Open **App Service Plan**  
2. Select **Scale Out (Autoscale)**  
3. Add rules:  
   - CPU > 70% → Add 1 instance  
   - CPU < 40% → Remove 1 instance  
4. Set instance limits:  
   - Min: 1  
   - Max: 5  

---

## 1️⃣1️⃣ **Secure the Application**
### Recommended  
- Enable **HTTPS Only**  
- Add **Custom Domain + SSL**  
- Use **Private Endpoints** for SQL & Storage  
- Store secrets in **Azure Key Vault**  
- Enable **Defender for Cloud** recommendations  

---

## 1️⃣2️⃣ **Validation & Testing**
### Validate  
- Web app loads successfully  
- Database connectivity works  
- Blob uploads/downloads work  
- Scaling triggers under load  
- Logs appear in Application Insights  

---

## 1️⃣3️⃣ **Operational Handover**
Document:  
- Architecture  
- Connection strings  
- Scaling rules  
- Backup policies  
- Deployment process  
- Monitoring dashboards  
- Incident response steps  

---

# ✅ Deployment Complete  
You now have a **fully deployed, scalable, secure, cloud‑native web application** running on:

- Azure App Services  
- Azure SQL Database  
- Azure Storage  
- Managed Identity  
- Application Insights  
- Autoscaling  

---

