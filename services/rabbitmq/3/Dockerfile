FROM rabbitmq:3-management

# Copy RabbitMQ configuration if needed
# COPY config/rabbitmq.conf /etc/rabbitmq/rabbitmq.conf

# Enable management plugin (already enabled in 3-management image)
# RUN rabbitmq-plugins enable rabbitmq_management

# Expose RabbitMQ ports
# 5672 for AMQP, 15672 for Management UI
EXPOSE 5672 15672

