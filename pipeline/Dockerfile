# base Docker image that we will build on
FROM python:3.13.11-slim

# Copy uv binary from official uv image (multi-stage build pattern)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# set up our image by installing prerequisites; pandas in this case
# commented out because we are using uv to run the script instead of python
# RUN pip install pandas pyarrow

# set up the working directory inside the container
WORKDIR /app

# Add virtual environment to PATH so we can use installed packages
# This allows us to use the packages installed in the virtual environment without needing to activate it
# In this case, we are using uv to run the script, which will automatically use the virtual environment
# And you don't need to activate the virtual environment manually ("uv run" will automatically use the virtual environment)
ENV PATH="/app/.venv/bin:$PATH"
# Copy dependency files first (better layer caching)
COPY "pyproject.toml" "uv.lock" ".python-version" ./
# Install dependencies from lock file (ensures reproducible builds)
RUN uv sync --locked

# copy the script to the container. 1st name is source file, 2nd is destination
COPY pipeline.py pipeline.py

# define what to do first when the container runs
# in this example, we will just run the script
# ENTRYPOINT ["uv", "run", "pipeline.py"]  # This is also OK
ENTRYPOINT ["python", "pipeline.py"]