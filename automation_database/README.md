# Automation Database (MySQL)

This container provides a MySQL instance for the automation suite. The backend is responsible for defining the schema via SQLAlchemy models and creating/upgrading tables on startup.

Key points:
- Database is initialized and started via startup.sh
- Connection information is saved to `db_connection.txt` and `db_visualizer/mysql.env`
- Backend must connect using environment variables (do not hardcode credentials)
- Backend should perform auto-creation of tables using SQLAlchemy ORM and migrations (Alembic or `Base.metadata.create_all`)

Quick start:
1) Start the database
   ./startup.sh

2) Verify connection
   cat db_connection.txt
   # Example: mysql -u appuser -pdbuser123 -h localhost -P 5000 myapp

3) Backend connection environment (example)
   # These variables must be set by the backend container (.env)
   MYSQL_URL=mysql://localhost:5000/myapp
   MYSQL_USER=appuser
   MYSQL_PASSWORD=dbuser123
   MYSQL_DB=myapp
   MYSQL_PORT=5000

Schema management:
- The schema (tables: jobs, requirements, artifacts, runs, run_steps, reports) must be defined in the backend code using SQLAlchemy models
- On backend startup, run migrations or `Base.metadata.create_all(engine)` to create missing tables
- No manual SQL files are required or used here

Troubleshooting:
- If MySQL is already running on a different port, the startup script will attempt to stop it and start a fresh instance on the configured port (default 5000)
- Credentials and DB info are printed at the end of the startup script
- A lightweight DB viewer is available under `automation_database/db_visualizer` (optional). To use:
  source automation_database/db_visualizer/mysql.env
  cd automation_database/db_visualizer && npm install && npm start
