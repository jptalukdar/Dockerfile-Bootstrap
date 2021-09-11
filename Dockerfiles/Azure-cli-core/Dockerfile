From python:3.9.7-alpine3.14 as base
## add any packages required for build
RUN apk add gcc \
    musl-dev \
    python3-dev \
    libffi-dev \
    openssl-dev \
    cargo \
    make 

WORKDIR /packages
## must have requirements.txt.
COPY requirements.txt . 
## Build wheel package out of requirements.txt. 
## This would eliminate the need of the above build codes
RUN pip wheel azure-cli-core==2.27.2 --no-deps --wheel-dir=/packages/wheel/pkg
RUN python -m pip wheel -r requirements.txt --wheel-dir=/packages/wheel/pkg


## Must be same as base else it would fail.
## Wheel packages are build dependent
FROM python:3.9.7-alpine3.14 
WORKDIR /code 
COPY --from=base /packages/wheel/ /packages/wheel/
RUN cd /packages/wheel/pkg && pip install * 

CMD ["python3"]