# Smart-Attend Mobile App: SDLC Assignment
## Course: SDLC & DevOps Fundamentals

**Student Name:** Gautam  
**Assignment Deadline:** 14-03-2026  
**Repository:** SDLC-Assignment-Gautam

---

## 📋 Project Overview

**Smart-Attend** is a university Student Attendance App using geo-fencing technology to automatically mark attendance when students enter a classroom. The project demonstrates the contrast between **Waterfall** (Traditional) and **Agile/Scrum** methodologies, with a mid-development pivot to integrate **Biometric (Face ID) Verification** to prevent proxy attendance.

### Key Features:
- ✅ Student login via university ID
- ✅ Real-time geo-fencing attendance marking
- ✅ Biometric (Face ID) verification for security
- ✅ Teacher dashboard with live attendance data
- ✅ Admin override capabilities
- ✅ Offline-first architecture with sync

---

## 📁 Repository Structure

```
SDLC-Assignment-Gautam/
├── README.md                          # This file
├── WATERFALL_FAILURE_ANALYSIS.md     # Task 1: Why Waterfall fails
├── SPRINT_BACKLOG.csv                # Task 2 & 3: Sprint 1 & 2 Tasks
├── SCRUM_FRAMEWORK_DOCUMENTATION.md  # Task 2: Scrum Sprint planning
├── docs/
│   ├── CI_CD_PIPELINE.md            # CI/CD implementation details
│   ├── ARCHITECTURE.md               # System design overview
│   └── API_SPECIFICATION.md          # API endpoints
├── src/                              # Source code (theoretical)
│   ├── backend/
│   ├── mobile/
│   └── database/
├── .github/
│   └── workflows/
│       ├── ci.yml                    # GitHub Actions CI pipeline
│       └── cd.yml                    # GitHub Actions CD pipeline
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
└── DEPLOYMENT.md                      # Deployment guide

```

---

## 🔄 CI/CD Pipeline: Why & How

### Why CI/CD is Critical for Smart-Attend

**1. Rapid Sprint Iterations**
- Scrum framework requires delivering working increments every 2 weeks
- Manual deployment would create bottlenecks and human error
- Without CI/CD: Each sprint release = 2-3 days of manual testing & deployment
- With CI/CD: Releases are automated; team can focus on feature development

**2. Biometric Integration Requires Continuous Testing**
- Face recognition APIs must be tested on every code push
- Multiple devices (iOS/Android) need simultaneous testing
- CI/CD enables:
  - Automated iOS & Android builds on every commit
  - Biometric security tests run automatically
  - Pre-release validation before production

**3. Preventing Regression with Proxy Attendance**
- Adding biometric verification risks breaking geo-fencing
- CI/CD automated tests ensure:
  - Geo-fencing still works independently
  - Biometric doesn't cause false positives
  - Both systems integrate seamlessly

### CI/CD Architecture for Smart-Attend

```
Developer → Git Push
    ↓
GitHub (repository)
    ↓
Webhook Trigger → GitHub Actions
    ├─ CI Pipeline (Continuous Integration)
    │   ├─ Code checkout
    │   ├─ Unit tests (Jest/React Native)
    │   ├─ Integration tests (Geo-fencing + Biometric)
    │   ├─ Security scan (biometric data handling)
    │   └─ Build APK/IPA (Android/iOS)
    │
    └─ CD Pipeline (Continuous Deployment)
        ├─ Deploy to Staging Environment
        ├─ Smoke tests on staging
        ├─ Performance tests (API latency, battery drain)
        └─ Auto-Deploy to Production (if all tests pass)
                ↓
        App Store / Play Store / Beta testers
```

### Detailed CI/CD Pipeline Stages

#### **Stage 1: Continuous Integration (CI)**

**Trigger:** Every push to main/develop branches

```yaml
# .github/workflows/ci.yml (Conceptual)
name: Continuous Integration
on: [push, pull_request]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      # 1. Code checkout
      - uses: actions/checkout@v3
      
      # 2. Setup environment
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18
      
      # 3. Install dependencies
      - run: npm install
      
      # 4. Unit tests (features)
      - name: Run Unit Tests
        run: npm run test:unit
        # Tests: Login validation, token generation, UI components, etc.
      
      # 5. Geo-fencing logic tests
      - name: Test Geo-Fencing Module
        run: npm run test:geofence
        # Validates: 10m radius detection, false-positive prevention, etc.
      
      # 6. Biometric integration tests
      - name: Test Biometric Verification
        run: npm run test:biometric
        # Validates: Face recognition accuracy, liveness detection, anti-spoofing
      
      # 7. Security scanning
      - name: Security Audit
        run: |
          npm audit
          npm run security:scan
        # Checks: Facial data encryption, GDPR compliance, API vulnerabilities
      
      # 8. Code coverage report
      - name: Generate Code Coverage
        run: npm run coverage
        # Must maintain >80% coverage
      
      # 9. Build for multiple platforms
      - name: Build Android APK
        run: npx react-native build-android
      
      - name: Build iOS IPA
        run: npx react-native build-ios
      
      # 10. Artifact upload
      - name: Upload Build Artifacts
        uses: actions/upload-artifact@v3
        with:
          name: builds
          path: |
            dist/*.apk
            dist/*.ipa
```

**What Gets Tested in CI:**
- ✅ Geo-fencing accuracy (10m detection works)
- ✅ Biometric verification flow (face match, liveness check)
- ✅ Student login with university ID
- ✅ Teacher dashboard data binding
- ✅ Admin override functionality
- ✅ Offline-first sync mechanism
- ✅ API integrations (mock servers)
- ✅ Code style & best practices
- ✅ Security: Biometric data encryption
- ✅ Performance: App startup time <3 seconds

**CI Failure = Prevent Merge**
- If any test fails, code doesn't merge into main
- Developer must fix locally and re-push
- This ensures production code is always stable

---

#### **Stage 2: Continuous Deployment (CD)**

**Trigger:** Successful CI build + manual approval (for production)

```yaml
# .github/workflows/cd.yml (Conceptual)
name: Continuous Deployment
on:
  workflow_run:
    workflows: [Continuous Integration]
    types: [completed]

jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    steps:
      # 1. Deploy to staging environment
      - name: Deploy to Staging Server
        run: |
          aws deploy --region us-east-1 \
          --application-name smart-attend \
          --deployment-group staging
      
      # 2. Run smoke tests on staging
      - name: Smoke Tests on Staging
        run: npm run test:smoke
        # Quick checks: App loads, login works, geo-fencing responds
      
      # 3. Performance testing
      - name: Performance Tests
        run: npm run test:performance
        # Measures: API latency, battery drain, face recognition speed
      
      # 4. Notify testing team
      - name: Notify QA Team
        run: |
          curl -X POST https://slack.com/api/chat.postMessage \
          -d "New build ready for testing in staging"
  
  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    if: github.event.workflow_run.conclusion == 'success'
    steps:
      # 1. Manual approval gate
      - name: Wait for Approval
        uses: actions/github-script@v6
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          # Release Manager (you!) must approve before production deploy
      
      # 2. Deploy to production
      - name: Deploy to Production
        run: |
          aws deploy --region us-east-1 \
          --application-name smart-attend \
          --deployment-group production
      
      # 3. Push to App Stores
      - name: Publish to Google Play Store
        run: fastlane android deploy
      
      - name: Publish to Apple App Store
        run: fastlane ios deploy
      
      # 4. Notify users
      - name: Send Push Notification
        run: |
          curl -X POST https://api.smart-attend.com/notify \
          -d "New version deployed: Improved biometric verification"
      
      # 5. Create release notes
      - name: Generate Release Notes
        run: npm run release:notes
        # Automatically documents: New features, bug fixes, security updates
      
      # 6. Monitor post-deployment
      - name: Setup Monitoring Alerts
        run: |
          # Alert on: High API errors, slow biometric processing, crashes
```

**What Happens in CD:**
- ✅ Code from CI → Deployed to staging server
- ✅ QA team tests on real devices (real students + real classrooms)
- ✅ If accepted by Release Manager (you!) → Deploy to production
- ✅ App Store updates (iOS) → Students install automatically
- ✅ Play Store updates (Android) → Students install automatically
- ✅ Monitoring alerts watch for issues in production

---

### Real-World Deployment Flow for Smart-Attend

**Scenario: Biometric verification bug fix in Sprint 2**

```
You (Release Manager) → Make decision to deploy
        ↓
Developer → Fixes facial recognition accuracy bug
        ↓
Push to GitHub → Automatic CI runs
        ↓
CI Results:
  ✅ Unit tests pass
  ✅ Biometric tests pass
  ✅ Integration tests pass (geo-fencing + biometric)
  ✅ Security scan passed (facial data encrypted)
  ✅ APK & IPA built successfully
        ↓
Auto-Deploy to Staging → QA tests on 50 real university students
        ↓
QA Results:
  ✅ Face recognition now works with glasses (bug fixed!)
  ✅ False positives reduced from 2% to 0.1%
  ✅ Performance impact: +50ms (acceptable)
        ↓
You (Release Manager) → Click "APPROVE FOR PRODUCTION"
        ↓
Auto-Deploy to Production:
  - Latest APK pushed to Google Play Store
  - Latest IPA pushed to Apple App Store
  - 5,000+ university students auto-update within 24 hours
  - Release notes sent to university admin
        ↓
Monitoring alerts:
  - API error rate: 0.01% (normal)
  - Biometric processing time: 2.3 seconds (optimal)
  - App crashes: 0
        ↓
✅ DEPLOYMENT SUCCESS in ~30 minutes!
(Without CI/CD, this would take 2-3 days + manual risks)
```

---

### Benefits of CI/CD for This Project

| Benefit | Impact | Example |
|---------|--------|---------|
| **Automated Testing** | Catch bugs before production | Biometric API timeout detected in CI, not production |
| **Rapid Feedback** | Developers know test results in 10 minutes | Can fix & re-push same day |
| **Reduced Human Error** | Fewer manual deployment mistakes | No accidental configuration errors |
| **Frequent Releases** | Scrum 2-week sprints feasible | Deploy new features every 2 weeks to all students |
| **Quick Rollback** | If production issue found, revert in minutes | Previous version redeployed automatically |
| **Audit Trail** | Every deployment logged | Compliance with university regulations |
| **Security** | Automated security scanning on every push | Biometric data encryption validated before deployment |

---

## 📊 Sprint Structure

### Sprint 1: MVP (Geo-fencing Only) - 2 Weeks
**Goals:** Deliver basic attendance system
- User login functionality
- Real-time geo-fencing logic
- Teacher dashboard
- Student notifications
- Admin override

**User Stories:** US01 - US10 (40 hours total)

**Deployment:** 
- Week 2 release to beta testing group (100 students)
- Collect feedback from real classroom environment
- Fix critical bugs before Sprint 2

### Sprint 2: Biometric Integration - 2 Weeks
**Goals:** Add Face ID verification + prevent proxy attendance
- Biometric profile management
- Face recognition API integration
- Anti-spoofing measures
- Secure facial data storage
- Integration with existing geo-fencing

**User Stories:** US11 - US20 (55 hours total)

**Deployment:**
- Week 4 release to all students
- Gradual rollout with monitoring
- Support team ready for biometric enrollment help

---

## 🚀 How CI/CD Enables Scrum Success

**Problem:** Traditional deployment takes 1 week per sprint cycle
- Scrum sprint = 2 weeks
- Testing & deployment = 1 week
- Development time = only 1 week (not enough!)

**Solution:** CI/CD automation
- Continuous integration = Immediate feedback
- Continuous deployment = Deploy in <1 hour
- Development time reclaimed = Full 2 weeks for features!

**Result:** Scrum becomes practical & productive

---

## 📋 Assignment Submission Checklist

- ✅ **Repository Created:** SDLC-Assignment-Gautam (GitHub)
- ✅ **Waterfall Failure Analysis:** WATERFALL_FAILURE_ANALYSIS.md (3 specific reasons)
- ✅ **Sprint Backlog:** SPRINT_BACKLOG.csv (20 user stories across 2 sprints)
- ✅ **CI/CD Documentation:** README.md (this file)
- ✅ **Scrum Framework Planning:** SCRUM_FRAMEWORK_DOCUMENTATION.md
- ✅ **Deployment Guide:** DEPLOYMENT.md

**Deadline:** 14-03-2026

---

## 🔗 References

- **Scrum Guide:** https://www.scrumguides.org/scrum-guide.html
- **GitHub Actions:** https://docs.github.com/en/actions
- **Waterfall vs Agile:** https://www.atlassian.com/agile/waterfall-vs-agile
- **CI/CD Best Practices:** https://cloud.google.com/architecture/continuous-delivery

---

## 📧 Contact & Questions

**Assignment:** SDLC & DevOps Fundamentals  
**Student:** Gautam  
**Submission Date:** On or before 14-03-2026  

---

*This is a theoretical assignment demonstrating SDLC concepts and DevOps practices in a realistic university project context.*
