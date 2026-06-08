## Managed vs Self-Hosted (TCO Analysis)

### Cost Comparison

| Aspect | Pinecone (Serverless) | Self-Hosted (Qdrant/Milvus) |
|--------|-----------------------|-----------------------------|
| **Ops Overhead** | Zero | High (Requires K8s + SRE) |
| **Scaling** | Instant (Scale to zero) | Manual (Node provisioning) |
| **Cost (Small)** | $0 - $100/mo | $50/mo (Minimum instance) |
| **Cost (Scale)** | High per token/vector | Low unit cost |

### Managed Service Pricing (indicative, always verify on provider pages)

| Provider | Model | Example: 10M vectors, 1536 dims |
|----------|-------|--------------------------------|
| Pinecone | Pod-based or Serverless | ~$70-150/month serverless |
| Qdrant Cloud | Per GB | ~$50/month (20GB) |
| Weaviate Cloud | Per dimensions | ~$100/month |
| Zilliz (Milvus) | Per CU | ~$75/month |


## Selection Framework

### Decision Tree

```
Need < 100K vectors?
+-- Yes -> pgvector (if already using PostgreSQL)
|          +-- Chroma (for prototyping)
|
+-- No -> Need managed service?
          +-- Yes -> Cloud-first?
          |          +-- Yes -> Pinecone (easiest)
          |          +-- No -> Qdrant Cloud or Zilliz
          |
          +-- No -> Need enterprise features?
                    +-- Yes -> Milvus on Kubernetes
                    +-- No -> Qdrant or Weaviate self-hosted
```

### Evaluation Criteria

| Criterion | Weight | Questions to Ask |
|-----------|--------|------------------|
| Scale | High | How many vectors now? In 1 year? |
| Latency | High | What are p99 requirements? |
| Ops capacity | High | Can we operate this? |
| Cost | Medium | Budget constraints? |
| Features | Medium | Hybrid search? Multimodal? |
| Lock-in risk | Low-Medium | Open source preferred? |

