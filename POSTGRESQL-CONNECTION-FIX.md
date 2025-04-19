# PostgreSQL Connection Fix Guide

Your application is showing this error:
```
Invalid `prisma.user.findUnique()` invocation in
Can't reach database server at `localhost:5432`
Please make sure your database server is running at `localhost:5432`.
```

This guide will help you fix the PostgreSQL connection issue. I've already added fallback mechanisms in your code to show mock data when the database is down, but here's how to fix the actual connection:

## Quick Fixes

1. **Check PostgreSQL Service Status**
   
   ```powershell
   # Open PowerShell as admin and check PostgreSQL service status
   Get-Service -Name postgresql*
   ```

   If it shows "Stopped" status, start it:
   ```powershell
   Start-Service -Name postgresql*
   ```

2. **Verify PostgreSQL Port**

   Make sure PostgreSQL is listening on port 5432:
   ```powershell
   netstat -ano | findstr :5432
   ```

   If nothing shows up, PostgreSQL might not be running or is using a different port.

## Common Issues and Solutions

### 1. PostgreSQL Service Not Running

1. Open Services (Win+R → type `services.msc`)
2. Find "PostgreSQL" service
3. Right-click and select "Start"
4. Make sure Startup Type is set to "Automatic"

### 2. Incorrect Connection String

Check your connection string in `server/.env`:
```
DATABASE_URL="postgresql://postgres:0000@localhost:5432/chamadigi?schema=public"
```

Make sure:
- Username (`postgres`) is correct
- Password (`0000`) is correct 
- Port (`5432`) matches your PostgreSQL configuration
- Database (`chamadigi`) exists

### 3. PostgreSQL Database Doesn't Exist

Create the database if it doesn't exist:

```powershell
# Connect to PostgreSQL (enter your password when prompted)
psql -U postgres

# Inside PostgreSQL shell, create the database
CREATE DATABASE chamadigi;

# Verify the database exists
\l

# Exit
\q
```

### 4. PostgreSQL Configuration Issues

Check and update the PostgreSQL configuration:

1. Find your PostgreSQL data directory (usually `C:\Program Files\PostgreSQL\<version>\data`)
2. Open `postgresql.conf` in a text editor
3. Make sure these lines are set properly:
   ```
   listen_addresses = 'localhost'    # or '*' for all interfaces
   port = 5432                       # default is 5432
   ```
4. Restart PostgreSQL after changes

### 5. Database Credentials Issues

If you've forgotten your PostgreSQL password:

1. Locate `pg_hba.conf` in your PostgreSQL data directory
2. Temporarily change authentication method to `trust` for local connections
3. Restart PostgreSQL
4. Connect and reset your password:
   ```sql
   ALTER USER postgres WITH PASSWORD 'your_new_password';
   ```
5. Update your `.env` file with the new password
6. Restore `pg_hba.conf` to its original settings and restart PostgreSQL

## Testing Your Connection

Run the database connection test script I've created:

```powershell
cd server
npm install pg --save
node scripts/check-db.js
```

## Application Enhancements

I've already implemented these improvements to your application:

1. **Client-side fallback:** The client now shows mock meeting data when it detects database connection issues
2. **Server-side caching:** Added user session caching to maintain authentication during database outages
3. **API fallback data:** Modified the meetings API to return sample data when the database is unreachable

These changes make your application more resilient against database connectivity issues, but you should still fix the underlying PostgreSQL connection problem.

## Still Having Issues?

If you're still facing connection problems:

1. Check Windows Firewall (might be blocking PostgreSQL)
2. Ensure no other application is using port 5432
3. Try completely uninstalling and reinstalling PostgreSQL
4. Check for PostgreSQL error logs in the data directory's `pg_log` folder

## Additional Resources

- [PostgreSQL Windows Installation Guide](https://www.postgresql.org/docs/current/installation-windows.html)
- [PostgreSQL Connection Troubleshooting](https://www.postgresql.org/docs/current/client-authentication.html)
- [Prisma Database Connection Guide](https://www.prisma.io/docs/concepts/database-connectors/postgresql) 