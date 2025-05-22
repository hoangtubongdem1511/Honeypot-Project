FROM owasp/modsecurity-crs:apache

USER root

# Cài công cụ cần thiết
RUN apt update && apt install -y wget nano curl python3 python3-pip python3-watchdog

# Filebeat setup
RUN wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.9.0-amd64.deb && \
    dpkg -i filebeat-8.9.0-amd64.deb

COPY filebeat.yml /etc/filebeat/filebeat.yml
RUN chmod go-w /etc/filebeat/filebeat.yml

# Fix lỗi thiếu file cấu hình logging
RUN mkdir -p /usr/local/apache2/conf/extra && touch /usr/local/apache2/conf/extra/httpd-logging-after-modsec.conf

# Copy các file cấu hình modsec + web giao diện giả
COPY include.conf /etc/modsecurity.d/
COPY modsecurity-extension.conf /app/modsecurity-extension.conf
COPY preprocess-modsec-log.py /app/preprocess-modsec-log.py
COPY httpd.conf /usr/local/apache2/conf/httpd.conf
COPY robots.txt /usr/local/apache2/htdocs/
COPY index.html /usr/local/apache2/htdocs/
COPY login.html /usr/local/apache2/htdocs/

# Cấu hình log + quyền
RUN touch /var/log/modsec_audit_processed.log && \
    cat /app/modsecurity-extension.conf >> /etc/modsecurity.d/modsecurity.conf

# Entrypoint script
COPY modsec_entry.sh /
RUN chmod +x /modsec_entry.sh

EXPOSE 80 8080 8000 8888 9091
CMD ["/modsec_entry.sh"]
