FROM python:3.11-alpine3.22 AS builder
RUN apk update
RUN apk add --no-cache gcc
RUN apk add --no-cache python3-dev
WORKDIR /app
COPY requirements.txt ./
COPY . .
RUN pip3 install -r --prefix=/install requirements.txt

FROM python:3.11-alpine3.22
WORKDIR /app
RUN addgroup -S roboshop
RUN adduser -S roboshop -G roboshop
COPY --from=builder /install /usr/local
COPY --from=builder /app /app
RUN chown -R roboshop:roboshop /app
ENV CART_HOST=cart-cont \
    CART_PORT=8080 \
    USER_HOST=user-cont \
    USER_PORT=8080 \
    AMQP_HOST=rabbitmq-cont \
    AMQP_USER=roboshop \
    AMQP_PASS=roboshop123
USER roboshop
CMD ["/usr/local/bin/uwsgi", "--ini", "payment.ini"]