---
name: payments-stripe
description: >-
  Integrates Stripe payment flows including checkout, webhooks, and
  subscriptions. Use when adding payments, billing, pricing pages,
  subscription management, or Stripe webhook handlers.
source: antigravity
---

# Stripe Payments

## When to Use

When the track involves adding payment processing, setting up subscriptions, or building pricing/checkout flows. Assumes Stripe as the payment provider.

## Plan Checklist

### Setup

- [ ] Stripe SDK installed (`stripe` for server, `@stripe/stripe-js` for client)
- [ ] API keys stored in environment variables — never in code
- [ ] Separate test and live keys — use test keys during development
- [ ] Stripe webhook endpoint created and registered

### Checkout Flow

- [ ] Use Stripe Checkout (hosted) or Stripe Elements (embedded) — don't build custom card forms
- [ ] Create checkout sessions server-side — never expose secret key to client
- [ ] Include success and cancel URLs
- [ ] Pass metadata (user ID, plan, etc.) to track the purchase

### Webhooks

- [ ] Webhook endpoint verifies Stripe signature — reject unsigned requests
- [ ] Handle key events: `checkout.session.completed`, `invoice.paid`, `customer.subscription.updated`, `customer.subscription.deleted`
- [ ] Webhook handlers are idempotent — same event processed twice should not cause issues
- [ ] Update database in webhook handler, not in the success redirect

### Subscriptions (if applicable)

- [ ] Subscription status tracked in database (synced via webhooks)
- [ ] Handle subscription states: active, past_due, canceled, trialing
- [ ] Customer portal link for self-service management (billing, cancellation)
- [ ] Grace period logic for past_due subscriptions

### Verification

- [ ] Test checkout with Stripe test cards (4242 4242 4242 4242)
- [ ] Webhook receives and processes events correctly (use Stripe CLI for local testing)
- [ ] Database updates reflect payment status
- [ ] User access changes based on subscription status

## Common Mistakes

1. **Trusting the success redirect** — user can manually navigate to success URL without paying; always use webhooks
2. **Exposing secret key** — using the secret key in client-side code
3. **Non-idempotent webhooks** — processing the same event twice creates duplicate records
4. **No webhook signature verification** — anyone can POST to your webhook endpoint
5. **Hardcoding prices** — use Stripe Price IDs from the dashboard, not hardcoded amounts
