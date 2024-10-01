# Respuestas

## Respuesta de la **Actividad 1.1**

```bash
docker volume create boston-data
```

## Respuesta de la **Actividad 1.2**

```bash
docker run --name boston-db -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=BOSTON -v boston-data:/var/lib/postgresql/data -p 5432:5432 -d --rm postgres:latest
```

## Respuesta de la **Actividad 1.3**

```bash
docker exec -i boston-db psql -U postgres -d BOSTON < db.sql
```

## Respuesta de la **Actividad 3.1**

```Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

EXPOSE 8080

CMD ["python", "app.py"]
```

## Respuesta de la **Actividad 3.2**

```bash
docker build -t tarea2_material-app:v1.0 .
```

## Respuesta de la **Actividad 3.3**

```bash
docker run --name boston-app -p 8080:8080 -e DB_HOST=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' boston-db) -d --rm tarea2_material-app:v1.0
```

## Respuesta de la **Actividad 4.1**

```bash
docker tag tarea2_material-app:latest miguel201121/tarea2_material-app:v1.0
docker push miguel201121/tarea2_material-app:v1.0
```

## Respuesta de la **Actividad 5.1**

```yaml
version: '3.9'
services:
  db:
    image: postgres:latest
    container_name: boston-db
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
    volumes:
      - boston-data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  app:
    build: .
    container_name: boston-app
    ports:
      - "8080:8080"
    depends_on:
      - db
    environment:
      DB_HOST: db
      DB_USER: ${DB_USER}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_NAME: ${DB_NAME}

volumes:
  boston-data:
```

## Respuesta de la **Actividad 5.2**

```bash
docker-compose up -d
```
