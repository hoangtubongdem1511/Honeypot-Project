FROM ubuntu:focal

WORKDIR /app/
USER root
ENV LC_ALL="C.UTF-8"

# Cài công cụ cần thiết
RUN apt-get update && apt-get install -y nano python3-pip
RUN pip3 install pipenv

# Copy code
COPY ./ /app

# Fix version elasticsearch trong Pipfile (nếu dùng repo gốc chưa chỉnh)
RUN sed -i 's/elasticsearch = ".*"/elasticsearch = "==6.8.2"/' Pipfile

# Cài các dependency từ Pipfile (đã bao gồm async-timeout = "==3.0.1")
RUN pipenv install

# Chạy script
CMD ["/bin/bash", "/app/start.sh"]


