# Bug Fixes

## Fix 1
- **File:** api/main.py, line 8
- **Problem:** Redis host hardcoded as "localhost" — fails inside Docker where services communicate by service name
- **Fix:** Changed to use os.environ.get("REDIS_HOST", "redis")

## Fix 2
- **File:** worker/worker.py, line 4
- **Problem:** Redis host hardcoded as "localhost" — same Docker networking issue
- **Fix:** Changed to use os.environ.get("REDIS_HOST", "redis")

## Fix 3
- **File:** frontend/app.js, line 6
- **Problem:** API URL hardcoded as "http://localhost:8000" — frontend container cannot reach API via localhost
- **Fix:** Changed to use process.env.API_URL || "http://api:8000"

## Fix 4
- **File:** api/main.py
- **Problem:** No /health endpoint — Docker HEALTHCHECK had nothing to ping
- **Fix:** Added GET /health route returning {"status": "ok"}

## Fix 5
- **File:** frontend/app.js
- **Problem:** No /health endpoint for Docker HEALTHCHECK
- **Fix:** Added GET /health route

## Fix 6
- **File:** worker/worker.py
- **Problem:** signal imported but never used — worker cannot shut down gracefully on SIGTERM
- **Fix:** Added signal handlers for SIGTERM and SIGINT with a running flag

## Fix 7
- **File:** api/main.py and worker/worker.py, queue name
- **Problem:** Queue was named inconsistently ("job" vs "jobs" risk) 
- **Fix:** Standardized queue name to "jobs" in both files
