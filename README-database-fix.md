# Database Connection Troubleshooting Guide

Your application is having trouble connecting to the PostgreSQL database. Based on the error logs, the server can't reach the database at `localhost:5432`. Here are steps to fix this issue:

## 1. Verify PostgreSQL is Running

First, check if PostgreSQL is running on your system:

- **Windows:** 
  - Press `Win + R`, type `services.msc` and press Enter
  - Look for a service named "PostgreSQL" and make sure it's running
  - If it's not running, right-click and select "Start"

- **Or check using command line:**
  ```powershell
  pg_isready -h localhost -p 5432
  ```

## 2. Check Database Connection Settings

The connection string in your `.env` file is:
```
DATABASE_URL="postgresql://postgres:0000@localhost:5432/chamadigi?schema=public"
```

Make sure this matches your actual PostgreSQL configuration:
- Username: `postgres`
- Password: `0000`
- Host: `localhost`
- Port: `5432`
- Database: `chamadigi`

## 3. Check if the Database Exists

If the PostgreSQL server is running but the database doesn't exist, you'll need to create it:

```powershell
# Connect to PostgreSQL
psql -U postgres

# Once connected, create the database
CREATE DATABASE chamadigi;

# Exit PostgreSQL
\q
```

## 4. Test the Database Connection

Run the database test script:

```powershell
cd server
npm install pg --save
node scripts/check-db.js
```

## 5. Database Connection Workaround

While you're fixing the database connection, your client application has been modified to show mock data when it detects database connection issues. This allows you to continue using the application even when the database is not available.

## 6. Check PostgreSQL Port Configuration

1. Make sure PostgreSQL is configured to listen on port 5432:
   - Find and open `postgresql.conf` (usually in the PostgreSQL data directory)
   - Check that `port = 5432` is set
   - Check that `listen_addresses = 'localhost'` or `listen_addresses = '*'` is set

2. Make sure no other service is using port 5432:
   ```powershell
   netstat -ano | findstr :5432
   ```

## 7. Rebuild Prisma Client

If your database structure has changed:

```powershell
cd server
npx prisma generate
```

## 8. Restart Your Application

After fixing the database issues, restart your application:

```powershell
npm run dev
```

## Additional Resources

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Prisma Database Connection Guide](https://www.prisma.io/docs/concepts/database-connectors/postgresql) 