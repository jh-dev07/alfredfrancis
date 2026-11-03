## Offline run
Docker and Compose v2 must already be installed. In the copied directory:

```sh
docker load -i chatbot-images.tar
docker compose -f compose.offline.yml up -d --pull never
docker compose -f compose.offline.yml logs -f backend
```

Wait for startup training to finish, then open http://localhost:8080 . Readiness: http://localhost:8080/api/ready . Gateway can return 502 until backend is ready; refresh after training finishes. Disconnect host Internet to verify. Internal container network blocks outbound Internet, and the UI binds only to localhost. Initial training may take minutes and repeats on restart. Models and MongoDB data persist in volumes.

Create intents/examples/responses through the UI and use its build/train action after changes. Default mode uses local spaCy/scikit-learn/CRF models and predefined responses; it is not a general generative assistant. Starter data covers greetings, cancel and fallback.

Stop: `docker compose -f compose.offline.yml down`
Restart: `docker compose -f compose.offline.yml up -d --pull never`
Do not use down -v unless you intend to erase data. Use the supplied offline compose, not upstream compose.
