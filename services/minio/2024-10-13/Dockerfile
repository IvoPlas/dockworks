FROM minio/minio:RELEASE.2024-10-13T13-34-11Z

# Copy MinIO configuration if needed
# COPY config/ /config/

# Expose MinIO ports
# 9000 for API, 9001 for Console
EXPOSE 9000 9001

# Set the entrypoint
ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
CMD ["server", "/data", "--console-address", ":9001"]

