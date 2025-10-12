# PostgreSQL sessions

## See all sessions:

```sql
SELECT pid, usename, application_name, client_addr, state
  FROM pg_stat_activity
 WHERE datname = 'tfpdb';
```

## Kill all sessions

```sql
SELECT pg_terminate_backend(pid)
  FROM pg_stat_activity
 WHERE datname = 'tfpdb'
   AND pid <> pg_backend_pid();
```
