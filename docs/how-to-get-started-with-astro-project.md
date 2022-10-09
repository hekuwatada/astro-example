# How to get started with Astro
This doc is to investigate what we can do with Astro. We've follwed:
- https://docs.astronomer.io/astro/cli/install-cli
- https://docs.astronomer.io/astro/create-project


## Step 1. Install Astro CLI
```
brew install astro
```

## Step 2. Create an Astro project
```
mkdir <project>
astro dev init
```

## Step 3. Build the Astro project locally (this will start Airflow locally)
```
astro dev start
```
This will start 4 Docker containers:
  - Postgres
  - Webserver
  - Scheduler
  - Triggerer

Airflow UI
  - https://localhost:8080

### Step 3-1. Troubleshoot restarting containers
For the first time, `astro dev start` resulted in containers restarting themselves until we removed some unused Docker images. We took the following steps to troubleshoot the issus.


#### List all containers
```
astro dev ps
```

#### Check logs from each container
```
astro dev logs --webserver
```

This revealed the diskspace error:
```
OSError: [Errno 28] No space left on device: '/usr/local/airflow/logs/scheduler'
```

#### Solution: removed unused Docker containers and images

We have deleted unused Docker containers and images to reclaim more disk spaces. The 4 running containers were able to recover from the "Restarting" state to "Running".


## Step 4. Run DAGs via Aifrlow UI
We have tried below via the Airflow UI at https://localhost:8080.

- Manually run [example_dag_basic](http://localhost:8080/dags/example_dag_basic/graph)
- Manually run [example_dag_advanced](http://localhost:8080/dags/example_dag_advanced/graph)

## Step 5. Write a DAG (WIP)

https://docs.astronomer.io/astro/astro-python-sdk

### Add Astro Python SDK as dependency
- Added the SDK `astro-sdk-python` to `requirements.txt`
- Updated `.env`
- Restart the local environment
```
astro dev restart
```

## Step 6. Test a DAG (WIP)
#### Parse DAG
```
astro dev parse
```

#### Run tests
```
astro dev pytest
```
This runs [tests/dags/test_dag_integrity.py](../tests/dags/test_dag_integrity.py)

## Misc
### How to run Airflow CLI
```
astro dev run <arguments to Airflow CLI>
```

For example, to run `version`:
```
% astro dev run version
Running: airflow version
2.4.1+astro.1
```

## Questions

- [ ] How do we see logs from Astro?
- [ ] How do we deploy to a remote/Cloud env?
- [ ] What remote/Cloud env can we have? (eg GKE, etc)
- [ ] What are the differences between Astro and bare Airflow?
- [ ] Can we deploy an Astro projct to Cloud Composer? 
- [ ] Can we use a different Airflow version?


