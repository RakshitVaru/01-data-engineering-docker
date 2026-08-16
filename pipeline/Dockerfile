# base Docker image that we will build on
FROM python:3.13.15-slim

# Copy uv binary from official uv image (multi-stage build pattern)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/
# # set up our image by installing prerequisites; pandas in this case
# RUN pip install pandas pyarrow
WORKDIR /app
ENV PATH="/app/.venv/bin:$PATH"


# Copy dependency files first (better layer caching)
COPY "pyproject.toml" "uv.lock" ".python-version" "src/pipeline/__init__.py" "README.md" ./
COPY src/ ./src/
RUN uv sync --locked


# Install dependencies from lock file (ensures reproducible builds)
# RUN uv sync --locked --verbose


# set up the working directory inside the container

# copy the script to the container. 1st name is source file, 2nd is destination
COPY pipeline.py .
# define what to do first when the container runs
# in this example, we will just run the script
ENTRYPOINT ["python", "pipeline.py"]