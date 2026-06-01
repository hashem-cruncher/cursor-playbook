# Pre-Deployment / Release Checklist

## Purpose
To provide a rigorous, standardized checklist that must be completed before deploying code to staging or production environments. This ensures system stability, data integrity, and prevents configuration-related outages.

## 1. Environment & Configuration Check
- [ ] **Environment Variables:** Are all required `.env` variables set in the target environment (e.g., Azure App Settings, Vercel, AWS Secrets Manager)?
- [ ] **API Keys & Webhooks:** Are production API keys active (e.g., Stripe, SendGrid)? Ensure no test/sandbox keys are used in the production environment.
- [ ] **Feature Flags:** Are experimental features disabled or appropriately gated behind feature flags?

## 2. Database & Migrations
- [ ] **Migration Scripts:** Are all new database migrations tested against a snapshot of production data (Staging environment)?
- [ ] **Backups:** Has a fresh, automated backup of the production database been verified before applying schema alterations?
- [ ] **Seed Data:** Ensure seed scripts used for local development will NOT execute in the production environment.

## 3. Performance & Optimization
- [ ] **Frontend Build:** Did the frontend build successfully without warnings (`npm run build`)? 
- [ ] **Bundle Size:** Verify no unnecessary large dependencies were introduced (e.g., importing the entire `lodash` library instead of specific functions).
- [ ] **Backend Logging:** Is the application set to `Production` mode (e.g., debug mode disabled in FastAPI/Django, detailed stack traces hidden)?

## 4. Security & Compliance
- [ ] **CORS Settings:** Are Cross-Origin Resource Sharing (CORS) policies strictly configured to allow only trusted production domains?
- [ ] **Rate Limiting:** Is rate limiting active on critical endpoints (e.g., Authentication, Password Reset) to prevent brute-force attacks?
- [ ] **SSL/TLS:** Are all endpoints forcing HTTPS?

## 5. Deployment Execution (The "Go Live" Phase)
- [ ] **Zero-Downtime:** If applicable, ensure the deployment strategy (e.g., Blue-Green, Rolling Updates) is configured to prevent user downtime.
- [ ] **Monitoring:** Are application monitoring tools (e.g., Sentry, Datadog) active and configured to alert the engineering team of post-deployment spikes in 5xx errors?
- [ ] **Rollback Plan:** Is there a clear, documented strategy to revert the deployment instantly if critical systems fail upon release?

## Post-Deployment Verification (Smoke Testing)
Once deployed, perform manual verification in the live environment:
- [ ] Verify the main application loads without console errors.
- [ ] Test the primary user flow (e.g., Sign up -> Login -> Core Action -> Logout).
- [ ] Monitor the server logs for the first 15 minutes for unexpected error rates.