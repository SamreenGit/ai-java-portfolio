# RFC 1 — Real-time Anomaly Detection Pipeline

## Overview
Build a **production-grade anomaly detection system** for streaming event data. Incoming events are ingested via Kafka, processed by a Java microservice, scored in real-time using a PyTorch anomaly detection model, and visualized in Grafana dashboards with alerts.

## Problem Statement
Traditional monitoring often fails to catch anomalies quickly or requires manual thresholds. Goal: automatically detect anomalies in **real-time** for high-throughput data streams with low latency (<200 ms median).

## Architecture
- **Data Ingestion:** Kafka producer → Kafka topic (`events.raw`)  
- **Processing:** Kafka Streams/Flink pipeline → preprocess events  
- **Anomaly Scoring Service:** Java Spring Boot → calls PyTorch model (via gRPC/ONNX runtime)  
- **Alerts:** Anomalous events published to Kafka topic (`events.alerts`) → consumed by alerting microservice → Slack/email/Grafana  
- **Observability:** Prometheus + Grafana dashboards for throughput, latency, anomaly rate  

## Milestones
1. Week 1–2: Setup Kafka cluster, Java producer/consumer, Docker Compose local environment  
2. Week 3–4: Implement preprocessing + simple rule-based anomaly detection baseline  
3. Week 5–6: Train and integrate PyTorch model; expose via TorchServe or ONNX + Java client  
4. Week 7: Build anomaly alert microservice; integrate with Grafana + Slack  
5. Week 8: Load testing (10k events/sec), deploy to Kubernetes cluster  

## Test Plan
- **Unit tests:** Kafka producers/consumers, preprocessing, anomaly detection logic  
- **Integration tests:** End-to-end Kafka → scoring → alerts flow  
- **Load tests:** Benchmark throughput, latency at 10k+ events/sec  
- **Chaos tests:** Kill pods/nodes, ensure replay + recovery works  
- **Acceptance criteria:** 95th percentile detection latency < 200 ms, >90% anomaly recall  
