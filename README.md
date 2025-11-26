# 🌤️ Zip Code Temperature Service – GO + OTEL + Zipkin

This project is composed of **two Go microservices** that communicate to fetch the current temperature of a city based on a **Brazilian Zip Code (CEP)**.

It also implements **distributed observability** with **OpenTelemetry** and **Zipkin**, and is ready to run with Docker and Docker Compose.

---

## 🧱 Architecture

```
[User]
   ↓ POST /cep
[Service A - Input]
   ↓ GET /weather?cep=...
[Service B - Weather]
   ↙            ↘
[ViaCEP API]   [WeatherAPI]
```

---

## 🧰 Technologies Used

- **Go 1.21+**
- **Docker + Docker Compose**
- **OpenTelemetry (OTEL)**
- **Zipkin** for distributed tracing
- **ViaCEP API** for Zip Code location
- **WeatherAPI** for current weather

---

## 📦 Folder Structure

```
deploy-com-cloud-run/
├── handler/
│   ├── input.go           # Service A
│   └── weather.go         # Service B
├── service/
│   ├── cep.go
│   └── weather.go
├── otelsetup/
│   └── otel.go            # OTEL/Zipkin Configuration
├── main.go
├── Dockerfile
├── docker-compose.yml
├── go.mod
└── README.md
```

---

## 🚀 How to Run Locally (without Docker)

### 1. Install Go (>= 1.21)

### 2. Run Zipkin with Docker:
```bash
docker run -d -p 9411:9411 openzipkin/zipkin
```

### 3. Run Service B (Weather)
```bash
export SERVICE_MODE=weather
export WEATHER_API_KEY=YOUR_API_KEY
export PORT=8082

go run .
```

### 4. Run Service A (Input)
```bash
export SERVICE_MODE=input
export SERVICE_B_URL=http://localhost:8082
export PORT=8081

go run .
```

### 5. Test the API
```bash
curl -X POST http://localhost:8081/cep \
  -H "Content-Type: application/json" \
  -d '{"cep": "29902555"}'
```

---

## 🐳 How to Run with Docker Compose

```bash
# Add your WeatherAPI key to docker-compose.yml
docker-compose up --build
```

- Service A (Input): http://localhost:8081/cep
- Service B (Weather): http://localhost:8082/weather?cep=29902555
- Zipkin Dashboard: http://localhost:9411

---

## 🧪 Tests

In the `tests/` folder, you will find basic tests to check the API behavior in Service B. To run:

```bash
go test ./tests
```

---

## 📈 Observability with Zipkin

This project implements **OTEL spans** to track:

- HTTP requests between Service A → B
- Time spent on the ViaCEP API call
- Time spent on the WeatherAPI call

Access the Zipkin dashboard:

👉 http://localhost:9411

---

## ✅ Validation Rules

- CEP must contain **8 digits** and be a **string**.
- Errors return:
  - `422 Unprocessable Entity` if CEP is invalid
  - `404 Not Found` if the CEP is not found

---

## 🧼 Success Response Example

```json
{
  "city": "São Paulo",
  "temp_C": 27.3,
  "temp_F": 81.14,
  "temp_K": 300.3
}
```

---

## 👨‍💻 Author

Developed by [@vitorlrrcamargo](https://github.com/vitorlrrcamargo) 💚
