markdown
#SQL Querying for Security Incident Investigation
## Objective
The objective of this lab was to utilize structured SQL queries to filter and analyze database log infrastructure. By querying user log-in attempts and machine asset tables, I identified security anomalies, tracked down potential brute force indicators, and mapped unauthorized system access.
----
## Scenario Overview
As a security analyst, Iinvestigated a suspected security incident involving multiple failed log-in attempts and unauthorized access to restricted organizational asssets.
My task was to query the database tables ('log_in_attempts' and 'employees') to:
1. Locate log-in attempts occurring outside of normal business hours.
2. Filter for failed log-in attempts that could point to brute-forc attacks.
3. Identify the origin of malicious connection attempts by filtering IP addresses and geographic locations.
----
## Database Queries and Methodology
### 1. Identifying After-Hours Log-in Attempts
To investigate whether malicious actors were attempting to access systems during unmonitored hours, I filtered the 'log_in_attempts' table by timestamp metrics using basic filtering clauses:
'''sql
SELECT *
FROM log_in_attempts
WHERE login_time > '18:00:00' OR login_time < '07:00:00';
.Query Breakdown:
.SELECT *: Selects all columns from the target rows.
.FROM log_in_attempts:Specifies the database table stroring the logs.
.WHERE: Filters the results to only include rows matching our criteria.
OR:Combines multiple conditions, pulling records from either late evening or early morning.
2. Hunting for Brute-Force Attacks (Failed Logins)
A high volume of failed logins is a classic signature of password-spraying or brute-force attack. I used the logical operator AND to isolate failed connections originating from outside the local network:
sql
SELECT user_id,ip_address,login_time
FROM log_in_attempts
WHERE success = FALSE AND ip_address NOT LIKE '192.168.%;
.Query Breakdown:
.success = FALSE: Filters exclusively for failed authentication attempts.
.NOT LIKE '192.168.%': Uses the wildcard character (%) to exclude any standard internal local network IP addresses, forcing the query to highlight external, public connection attempts.
3. Tracking Specific Suspicious IP Networks.
During the inestigation, a specific malicious IP range starting with 203.0.113. was flagged by threat intelligence. I queried the database to find every instance of an employee account interacting with external network:
sql
SELECT *
FROM log_in_attempts
WHERE ip_address LIKE '203.0,113,%';
4. Investigating Rogue Devices by Country
To correlate network indicators with employee profiles, I ran conditional queries on the employee database to locate assets assigned to offices where the unauthorized traffic allegedly originated:
sql
SELECT employee_id,department,country
FROM employees
WHERE country = 'Mexico' or country = 'Canada';
Investigation Findings & Summary
By structuring and executing these SQL queries, I successfully achieved the following security goals:
. Attack Pattern Identification:Isolated a cluster of 50+ failed login attempts from an external IP range,confirmeing an ongoing brute-force attack.
.Impact Mitigation:Generated a clean list of targeted user_id accounts, allowing the system administrators to proactively temporarily lock those accounts and reset credentials.
.Audit Readiness: Provided a structured , query-driven timeline of events to include in the official incident response report.
