FROM public.ecr.aws/lambda/python:3.12.2026.05.28.13 AS builder

WORKDIR /build

COPY common-lib ./common-lib
COPY pyproject.toml uv.lock ./

RUN python -m venv .venv && \
    source .venv/bin/activate && \
    pip install --no-cache-dir uv==0.11.30 && \
    uv export --locked --no-dev --no-hashes --no-emit-project --output-file requirements.txt && \
    cd common-lib && \
    uv build --sdist

FROM public.ecr.aws/lambda/python:3.12.2026.05.28.13

WORKDIR /ws

COPY --from=builder /build/common-lib/dist/commonlib-0.1.0.tar.gz .
COPY --from=builder /build/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir commonlib-0.1.0.tar.gz && \
    pip install --no-cache-dir "jaraco.context>=6.1.0" "wheel>=0.46.2"

COPY . .

ENTRYPOINT ["./s3_list_objects.py"]
