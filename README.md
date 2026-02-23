# FastAPI Beyond CRUD 

This is the source code for the [FastAPI Beyond CRUD](https://youtube.com/playlist?list=PLEt8Tae2spYnHy378vMlPH--87cfeh33P&si=rl-08ktaRjcm2aIQ) course. The course focuses on FastAPI development concepts that go beyond the basic CRUD operations.

For more details, visit the project's [website](https://jod35.github.io/fastapi-beyond-crud-docs/site/).

## Table of Contents

1. [Getting Started](#getting-started)
2. [Prerequisites](#prerequisites)
3. [Project Setup](#project-setup)
4. [Running the Application](#running-the-application)
5. [Running Tests](#running-tests)
6. [Contributing](#contributing)

## Getting Started
Follow the instructions below to set up and run your FastAPI project.

### Prerequisites
Ensure you have the following installed:

- Python >= 3.10
- PostgreSQL
- Redis

### Project Setup
1. Clone the project repository:
    ```bash
    git clone https://github.com/jod35/fastapi-beyond-CRUD.git
    ```
   
2. Navigate to the project directory:
    ```bash
    cd fastapi-beyond-CRUD/
    ```

3. Create and activate a virtual environment:
    ```bash
    python3 -m venv env
    source env/bin/activate
    ```

4. Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```

5. Set up environment variables by copying the example configuration:
    ```bash
    cp .env.example .env
    ```

6. Run database migrations to initialize the database schema:
    ```bash
    alembic upgrade head
    ```

7. Open a new terminal and ensure your virtual environment is active. Start the Celery worker (Linux/Unix shell):
    ```bash
    sh runworker.sh
    ```

## Running the Application
Start the application:

```bash
python -m uvicorn src:app --host 0.0.0.0 --port 8000 --reload
```
Alternatively, you can run the application using Docker:
```bash
docker compose up -d
```
## Running Tests
Run the tests using this command
```bash
pytest
```

## Contributing
I welcome contributions to improve the documentation! You can contribute [here](https://github.com/jod35/fastapi-beyond-crud-docs).

## Assignment Updates

This repository includes updates made to support CI/CD demonstration requirements:

- Docker startup was changed from `fastapi run ...` to `python -m uvicorn src:app ...` to avoid container startup failure when the `fastapi` executable is not available in PATH.
- The nightly workflow now skips semantic-release unless running on `main` and a `package.json` file exists. This prevents Node-specific release tooling from failing in this Python project.
- A temporary failing test can be added to demonstrate failure notifications in GitHub Actions, then removed after recording.
- Conventional Commit format is used for commit messages (for example: `fix(docker): ...`, `ci(workflow): ...`, `test(ci): ...`).

## Items Needed To Make Everything Work

1. Fix Docker runtime command in `Dockerfile`:
   - Use `CMD ["python", "-m", "uvicorn", "src:app", "--host", "0.0.0.0", "--port", "8000"]`.
2. Rebuild containers after Dockerfile changes:
   - `docker compose down`
   - `docker compose build --no-cache`
   - `docker compose up`
3. Fix nightly workflow failure caused by semantic-release:
   - Add a condition so semantic-release only runs on `main` and when `package.json` exists.
4. Ensure required GitHub repository secrets are configured:
   - App/test secrets (for example `DATABASE_URL`, `JWT_SECRET`, `REDIS_URL`)
   - Mail secrets (`EMAIL_USER`, `EMAIL_PASS`)
5. Verify failure-notification path:
   - Push a temporary failing test to a test branch.
   - Trigger `Nightly Build and Test` manually using `workflow_dispatch`.
   - Confirm `Run Tests` fails and `Send email on test failure` executes.
6. Clean up after demo:
   - Remove temporary failing test and push a cleanup commit.
