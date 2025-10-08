-- Connect to the target database
\c banking

-- Grant privileges on all current tables
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO bank_user;

-- Grant privileges on sequences (for serial/auto-increment columns)
GRANT USAGE, SELECT, UPDATE ON ALL SEQUENCES IN SCHEMA public TO bank_user;

-- Grant future privileges automatically on new tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO bank_user;

ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT USAGE, SELECT, UPDATE ON SEQUENCES TO bank_user;

GRANT CONNECT ON DATABASE banking TO bank_user;
GRANT CREATE ON DATABASE banking TO bank_user;

docker exec -it banking-datapipeline-postgres-1 cat /var/lib/postgresql/data/pg_hba.conf


airflow db init
airflow users create --username abhi --firstname abhi --lastname dobhal --password  --email abhi@email.com --role Admin

docker exec -it kafka /usr/bin/kafka-console-consumer --bootstrap-server kafka:9092 --topic banking_server.public.transactions --from-beginning
