## ⚡ Quick Start
1. Create a Supabase project
2. Create required tables (`employee_contacts`, `sync_logs`)
3. Configure Google Service Account
4. Add secrets to Supabase environment variables
5. Deploy Edge Function
6. Enable `pg_cron` for scheduled sync

# supabase-google-sheets-sync

Supabase – Google Sheets Bi-Directional Sync System

---

## 📋 Project Description
A robust, automated bi-directional synchronization system that connects Google Sheets with a Supabase (PostgreSQL) database for managing employee contact information.

This system ensures:
- Data consistency across both platforms  
- Near real-time synchronization  
- Strong data integrity  
- Comprehensive logging and error handling  

---

## ✨ Key Features

- **Bi-Directional Sync**  
  Automatic synchronization between Google Sheets and Supabase using Edge Functions.

- **Near Real-Time Updates**  
  Data stays consistent with a scheduled sync every 5 minutes.

- **UUID Generation**  
  Each record gets a unique identifier using `gen_random_uuid()`.

- **Error Handling & Logging**  
  Try/catch blocks with full audit logging for failures and retries.

- **Secure Authentication**  
  OAuth 2.0 for Google APIs and JWT for Supabase access.

- **Scheduled Automation**  
  Fully automated execution using `pg_cron`.

- **Data Validation**  
  Input sanitization, null handling, and type validation.

- **Audit Logging**  
  Complete sync history stored in a dedicated logs table.

- **Conflict Resolution**  
  Smart upsert logic using email and timestamps.

---

## 🚀 How It Works (High Level)

1. Read employee data from Google Sheets  
2. Validate and sanitize incoming rows  
3. Upsert records into Supabase using email as the unique key  
4. Sync updated records back to Google Sheets  
5. Log each operation for auditing and debugging  

---

## 🏗️ Tech Stack

### Backend Infrastructure
- PostgreSQL 15+ (Supabase) as the primary database  
- Supabase Edge Functions (Deno) for serverless execution  
- `pg_cron` for scheduled automation  
- Supabase REST API for database interaction  

### APIs & Integrations
- Google Sheets API v4 for spreadsheet read/write  
- Supabase REST API v1 for CRUD operations  

---

## 🔐 Authentication & Security

- Google Service Account using OAuth 2.0  
- Supabase Service Role JWT for backend access  
- PostgreSQL Row Level Security (RLS) disabled  
- Secrets stored securely in Supabase environment variables  

---

## 📊 System Architecture

Data flows in the following order:

Google Sheets  
↓  
Supabase Edge Function (validation + logic)  
↓  
Supabase PostgreSQL  
↓  
Supabase Edge Function  
↓  
Google Sheets  

---

## 🔄 Sync Strategy

### Phase 1: Google Sheets → Supabase
- Read rows from columns A to E  
- Skip the header row  
- Validate required fields  
- Upsert records using email as a unique identifier  
- Update sync metadata  

### Phase 2: Supabase → Google Sheets
- Fetch all employee records from the database  
- Format data into sheet-compatible rows  
- Overwrite the sheet body while preserving headers  

---

## 🧠 Conflict Resolution

- Duplicate email → existing record is updated  
- Sheet data newer than database → Sheet wins  
- Database data newer than Sheet → Database wins  
- Partial failures → Sync continues and logs errors  

---

## 🗄️ Database Design

### employee_contacts
- UUID primary key  
- Unique email constraint  
- Created and updated timestamps  
- Source-aware sync metadata  

### sync_logs
- Full audit trail of every sync run  
- Execution duration and status  
- Detailed error diagnostics  

---

## 🔐 Security Architecture

### Authentication
- Google OAuth 2.0 Service Account  
- Supabase JWT (Service Role Key)  

### Authorization
- PostgreSQL Row Level Security  
- Write access restricted to Edge Functions  

### Data Protection
- TLS 1.2+ encryption in transit  
- Encrypted storage at rest  
- No secrets committed to the repository  

---

## ⏱ Scheduling & Automation

- `pg_cron` triggers the Edge Function every 5 minutes  
- Stateless serverless execution  
- Automatically scales with load  

---

## 👨‍💻 Author
Aniket Gunjal
