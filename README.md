# Load Balancing Rate Limiter

## About Project

This project implements a simple load balancing rate limiter using FastAPI. 
It allows you to control the rate of requests per client/action, manage server load,
and queue requests per tenant when the server is overloaded.

## Requirements
- Python 3.10+
- FastAPI
- Uvicorn
- 
## Local development

Install dependencies with:
```bash
pip install -r requirements.txt
```

## Running the Server
Start the FastAPI server with Uvicorn:
```bash
uvicorn main:app --reload
```

## API Endpoints

### POST /check_and_consume
Checks if a request is allowed and consumes a slot if possible. Handles queuing if the server is overloaded.

**Request JSON:**
```
{
  "tenant_id": "string",
  "client_id": "string",
  "action_type": "string",
  "max_requests": int,
  "window_duration_seconds": int
}
```

**Response:**
- If processed: `{ "allowed": true/false, "status": "processed", ... }`
- If queued: `{ "allowed": false, "status": "queued", ... }`
- If rejected (queue full): `{ "allowed": false, "status": "rejected", ... }`

### GET /status/{tenant_id}/{client_id}/{action_type}
Returns the current rate limit status and queue for the given tenant/client/action.

**Response:**
```
{
  "tenant_id": "string",
  "client_id": "string",
  "action_type": "string",
  "max_requests": int,
  "window_duration_seconds": int,
  "current_requests": int,
  "timestamps": ["ISO8601", ...],
  "queue_size": int,
  "queue_status": [ { "client_id": ..., "action_type": ... }, ... ]
}
```
