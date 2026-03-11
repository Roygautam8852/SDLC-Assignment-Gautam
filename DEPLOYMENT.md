# Deployment Guide: Smart-Attend Mobile App
## Step-by-Step Release Process

---

## Pre-Deployment Requirements

### 1. Environment Setup
```bash
# Install dependencies
npm install

# Setup environment variables
cp .env.example .env.production

# Verify API configurations
npm run verify:config
```

### 2. Pre-Release Testing
```bash
# Run full test suite
npm run test:full

# Build both platforms
npm run build:android
npm run build:ios

# Performance check
npm run perf:test
```

---

## Sprint 1 Deployment (Week 2)

### Target: Beta testing with 100 students

**Release Checklist:**
- [ ] All Sprint 1 user stories completed & tested
- [ ] Geo-fencing tested in 3+ real classrooms
- [ ] Teacher dashboard shows real attendance data
- [ ] Notification system working on iOS & Android
- [ ] Offline sync tested
- [ ] Admin can override attendance
- [ ] CI/CD pipeline passing all checks
- [ ] Beta testers identified
- [ ] Support team trained

**Deployment Commands:**
```bash
# Create release branch
git checkout -b release/v0.1-beta

# Tag the release
git tag -a v0.1-beta -m "Sprint 1: MVP - Geo-fencing baseline"

# Push to GitHub (triggers CI/CD)
git push origin release/v0.1-beta
git push origin v0.1-beta

# GitHub Actions automatically:
# - Runs all tests
# - Builds APK/IPA
# - Deploys to TestFlight (iOS beta)
# - Deploys to Google Play internal testing (Android)
```

**Manual Steps:**
- QA: Install on 5 test devices, verify 15 test scenarios
- Invite 100 beta testers from university
- Monitor crash reports & feedback for 1 week

---

## Sprint 2 Deployment (Week 4)

### Target: Full production release to 5,000+ students

**Release Checklist:**
- [ ] Sprint 1 user stories still working (no regression)
- [ ] Biometric verification passes security audit
- [ ] Anti-spoofing tested with 50+ face variations
- [ ] Geo-fencing + Biometric integration tested thoroughly
- [ ] GDPR compliance verified
- [ ] FERPA audit logging enabled
- [ ] Fallback to geo-fencing if biometric fails
- [ ] Teacher dashboard v2 ready
- [ ] All 10 Sprint 2 user stories completed
- [ ] 500 student beta test completed successfully
- [ ] Support team trained on biometric enrollment
- [ ] University IT sign-off
- [ ] CI/CD passing all gates

**Deployment Commands:**
```bash
# Create release branch for production
git checkout -b release/v1.0-prod
git tag -a v1.0-prod -m "Sprint 2: Production Release - Geo-fencing + Biometric"

# Push to GitHub (triggers CI/CD)
git push origin release/v1.0-prod
git push origin v1.0-prod

# GitHub Actions automatically:
# - Runs full test suite (60+ tests)
# - Security scanning
# - Builds optimized APK/IPA
# - Deploys to staging for final QA
# - Waits for Release Manager approval
# - Deploys to Google Play Store (production)
# - Deploys to Apple App Store (production)
# - Notifies all users
# - Monitors system health
```

**Release Manager Tasks (You!):**
1. Review staging build (24 hours before release)
2. Verify no known issues
3. Approve production deployment
4. Watch monitoring dashboard first 4 hours after release
5. Be available for urgent rollback if needed

---

## Rollback Procedure (Emergency Only)

```bash
# If critical issue found in production:
npm run rollback:production

# Or manually:
git checkout v0.1-beta  # Previous stable version
git tag -a v1.0-hotfix-rollback
git push origin v1.0-hotfix-rollback

# GitHub Actions automatically reverts App Store versions
# Users auto-update back to previous version within hours
```

---

## Release Notes Template

**For Sprint 1:**
```markdown
# Smart-Attend v0.1 Beta Release

## New Features
- ✨ Student login via university ID
- ✨ Automatic attendance with geo-fencing (10m radius)
- ✨ Real-time teacher attendance dashboard
- ✨ Instant student notifications
- ✨ Attendance reports (CSV export)
- ✨ Offline-first app sync

## Technical Improvements
- 🔧 CI/CD pipeline fully automated
- 🔧 Enhanced location accuracy
- 🔧 Battery optimization
- 🔧 Database transaction handling

## Known Limitations (Will fix in Sprint 2)
- Biometric authentication coming soon
- Admin override access limited to testing

## System Requirements
- iOS 14.0+
- Android 8.0+
- University network access

## Beta Testing Duration
- 1 week with 100 testers
- Feedback collected for improvements

---
```

**For Sprint 2:**
```markdown
# Smart-Attend v1.0 Production Release

## Major Features (Production Ready)
- ✨ **NEW**: Face ID / Biometric verifi cation
- ✨ **NEW**: Anti-spoofing protection (detects printed photos)
- ✨ Geo-fencing attendance (refined from beta feedback)
- ✨ Teacher attendance dashboard
- ✨ Student notifications
- ✨ PDF/CSV report export
- ✨ Offline-first app with sync

## Security & Compliance
- 🔒 AES-256 encryption for facial data
- 🔒 GDPR compliance for PII handling
- 🔒 FERPA audit logging for all biometric attempts
- 🔒 Rate limiting on authentication attempts

## Performance
- ⚡ Face recognition: 2-3 seconds average
- ⚡ App startup: <3 seconds
- ⚡ Battery impact: <2% per authentication

## Bug Fixes (From Beta Testing)
- 🐛 Fixed geo-fencing false positives in high-RF areas
- 🐛 Fixed database transaction conflicts during high usage
- 🐛 Fixed notification delivery on slow networks

## Support
- 📞 Help center: help.smartattend.edu
- 📧 Email: support@smartattend.edu
- 💬 Chat support: Available during university hours

---
```

---

## Monitoring Post-Deployment

### Metrics to Watch (First 24 Hours)

```
Critical Metrics:
  - Crash rate: Should be <0.01%
  - API error rate: Should be <0.1%
  - Biometric success rate: Should be >98%
  - Geo-fencing accuracy: Should be within 10m

Warning Thresholds:
  - If crash rate > 0.05% → Investigate immediately
  - If API errors > 1% → Check backend servers
  - If biometric success < 95% → Check API reliability
  - If geo-fencing accuracy > 15m → Check GPS libraries

Automatic Alerts:
  - Slack notification if any metric exceeds threshold
  - SMS to Release Manager if critical (>0.1% crash rate or >2% API errors)
  - Auto-rollback initiated if crash rate > 0.5%
```

### Daily Monitoring Week 1

```bash
# Check deployment health
npm run monitoring:status

# View error logs
npm run logs:production | grep -i error

# Monitor user feedback
npm run feedback:summary

# Performance dashboard
npm run metrics:dashboard
```

---

## Post-Deployment Communication

### Day 1 (Release Day)
- ✓ Release notes published to app stores
- ✓ Email: All students & teachers
- ✓ Slack: #announcements for university IT
- ✓ Dashboard: Status page updated

### Day 2-3 (Monitor Phase)
- ✓ Daily monitoring reports
- ✓ Respond to support tickets
- ✓ Collect feedback from early adopters

### End of Week 1 (Review)
- ✓ Release retrospective
- ✓ Success metrics analysis
- ✓ User feedback summary
- ✓ Plan for next sprint

---

## Troubleshooting

### Students Report: "App crashes on startup"
```bash
# Immediate action:
- Check crash logs: npm run logs:production | grep "crash"
- Check recent changes: git log -5 --oneline
- If critical: npm run rollback:production
- If minor: Deploy hotfix with CI/CD pipeline
```

### Teachers Report: "Attendance not updating"
```bash
# Investigate:
- Check database connections: npm run db:status
- Check API response times: npm run api:perf
- Verify geo-fencing service running
- Check if students are in 10m radius
```

### Biometric Report: "Face detection timing out"
```bash
# Investigate:
- Check Face Recognition API status
- Monitor API rate limiting: npm run api:rate-limits
- Check network latency
- Increase timeout or add queue (deploy fix via CI/CD)
```

---

## Success Criteria

**Sprint 1 Deployment (Beta):**
- ✅ 100 students can use app in real classrooms
- ✅ Attendance marked correctly 99%+ of the time
- ✅ No critical crashes
- ✅ Teachers can see real-time attendance
- ✅ Positive feedback from beta testers

**Sprint 2 Deployment (Production):**
- ✅ 5,000+ students successfully update
- ✅ Biometric enrollment successful for 95%+
- ✅ Attendance marked correctly with geo-fencing OR biometric
- ✅ Zero security breaches
- ✅ User satisfaction > 4.5/5 stars
- ✅ Ready for next features in future sprints

---

*This guide ensures Smart-Attend launches smoothly and scales to all university students.*
