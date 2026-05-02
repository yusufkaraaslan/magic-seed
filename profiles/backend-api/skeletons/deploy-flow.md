# Flow: Deploy

> **Profile:** Backend API

## Phases

### Phase 1: CHECKLIST
- [ ] Tests pass
- [ ] Database migrations ready
- [ ] Environment vars set
- [ ] Docker image builds

### Phase 2: VERSION
Bump version.

### Phase 3: BUILD
```bash
docker build -t myapp:{version} .
```

### Phase 4: TEST BUILD
Run smoke tests.

### Phase 5: DEPLOY
Deploy to:
- Kubernetes
- AWS ECS/Fargate
- Google Cloud Run
- Azure Container Instances
- Heroku
- VPS

### Phase 6: VERIFY
- [ ] Health check passes
- [ ] API responds
- [ ] Database connected
- [ ] Logs normal

### Phase 7: ANNOUNCE
Notify team.

## Platform Notes
- **Blue/Green:** Zero-downtime deployment
- **Database:** Run migrations before app start
- **Rollback:** Keep previous version ready
