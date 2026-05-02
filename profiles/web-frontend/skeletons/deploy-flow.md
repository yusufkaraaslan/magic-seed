# Flow: Deploy

> **Profile:** Web Frontend

## Phases

### Phase 1: CHECKLIST
- [ ] Build succeeds
- [ ] Tests pass
- [ ] Environment variables set

### Phase 2: VERSION
Bump version.

### Phase 3: BUILD
```bash
npm run build
# or
yarn build
# or
pnpm build
```

### Phase 4: TEST BUILD
Run smoke tests.

### Phase 5: DEPLOY
Deploy to:
- Vercel
- Netlify
- AWS S3 + CloudFront
- GitHub Pages
- Firebase Hosting

### Phase 6: VERIFY
- [ ] Site loads
- [ ] Routes work
- [ ] API calls succeed
- [ ] Assets load

### Phase 7: ANNOUNCE
Notify team.

## Platform Notes
- **SPA:** Configure redirect rules
- **SSR:** Check server startup
- **Static:** Verify CDN cache invalidation
