# Sample API

This is a sample api. 

## Technology Used
- [x] [Python](https://www.python.org)
- [x] [FastAPI](https://fastapi.tiangolo.com)
- [x] [SQLAlchemy](https://www.sqlalchemy.org)
- [x] [Poetry](https://python-poetry.org)
- [x] [Flyway](https://www.red-gate.com/products/flyway/community/)
- [x] [Docker Compose](https://docs.docker.com/compose/install/)

## Setup

### Environment variables

The following environment variables are expected:

- ORIGINS: comma separated list of [allowed origins](https://fastapi.tiangolo.com/tutorial/cors/).
- POSTGRES_SERVER: URL of db server
- POSTGRES_USER
- POSTGRES_PASSWORD
- POSTGRES_DB: postgres DB name

### DB adaptor

The [psycopg2](https://www.psycopg.org) adaptor is used. Note the [installation requirements](https://www.psycopg.org/docs/install.html) for the psycopg2 package. The [sample-api dependencies](pyproject.toml) use the psycopg2 package and not the psycopg2-binary package.
    
## Local Development

### Running locally

- Run `docker compose up` command from root directory to start the entire stack. The following will be started: 
  - Postgres server 
  - Flyway - it setups the postgres tables and inserts some dev data.
  - sample-api server
- Environment variables are defined in the docker-compose.yml
- The `sample-api` folder is volume mounted, so any changes to the code will be reflected in the container 
- The API's documentation is available at [http://localhost:3003/docs](http://localhost:3003/docs).


### Unit tests

- Run `docker compose --profile test up` command to run the unit tests from the root directory. This will run the above services as well as the unit tests.
- The folder is volume mounted, so any changes to the code will be reflected in the container and run the unit tests again.


## The API

This is a simple API. It is not production ready. 

The API is based on the table schema defined in [V1.0.0__init.sql](db/migrations/V1.0.0__init.sql) file. Note the foreign key constraint if you want to try the endpoints out.

The [http://localhost:3003/docs](http://localhost:3003/docs) page lists the available endpoints.

## The API

This is a simple API. It is not production ready. 

The API is based on the table schema defined in [V1.0.0__init.sql](db/migrations/V1.0.0__init.sql) file. Note the foreign key constraint if you want to try the endpoints out.

The [http://localhost:3003/docs](http://localhost:3003/docs) page lists the available endpoints.

## Dockerfile Considerations

The Dockerfile for this project is designed with several key considerations in mind:

1. **Multi-stage build**: The Dockerfile uses a multi-stage build process to optimize the final image size and improve security.

2. **Base image**: Python 3.12-alpine is used as the base image for both build and production stages. Alpine Linux is chosen for its small size and security benefits.

3. **Environment variables**: Several environment variables are set to optimize Python and Poetry behavior.

4. **System dependencies**: Necessary system dependencies are installed in the build stage to compile Python packages. Only runtime dependencies are kept in the final stage.

5. **Poetry for dependency management**: Poetry is used to manage Python dependencies, ensuring reproducible builds.

6. **Virtual environment**: Poetry is configured to install packages globally in the Docker container, avoiding the need for a virtual environment.

7. **Copying only necessary files**: Only the required files and installed packages are copied from the build stage to the production stage, reducing the final image size.

8. **Non-root user**: The application runs as a non-root user `appuser` for improved security.

9. **Exposed port**: The Dockerfile exposes port 3000, which is the default port for the FastAPI application.

10. **CMD instruction**: The Dockerfile uses the CMD instruction to specify the command to run the FastAPI application using Uvicorn.

### Reasoning behind these choices:

- **Multi-stage build**: This approach separates the build environment from the runtime environment, resulting in a smaller and more secure final image.

- **Alpine Linux**: Chosen for its small footprint and security benefits, reducing the attack surface of the container.

- **Poetry**: Provides more reliable and reproducible dependency management compared to pip.

- **System dependencies**: Installed only in the build stage to compile Python packages, then discarded to keep the final image lean.

- **Copying strategy**: Only necessary files are copied to the production stage to minimize the final image size and potential security vulnerabilities.

- **Exposed port**: Explicitly declaring the port in the Dockerfile improves clarity and documentation.

- **CMD instruction**: Using CMD allows for easier overriding of the startup command if needed.



### Multi-platform builds:

The Dockerfile now supports multi-platform builds for linux/amd64 and linux/arm64 architectures. This is achieved by using the `--platform` flag in the FROM instructions and utilizing Docker's BuildKit.

To build the image for multiple platforms, use the following command:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t your-image-name:tag .
```

These considerations aim to create a Dockerfile that produces a secure, efficient, and easily maintainable container image for the Sample API, with support for multiple architectures.
