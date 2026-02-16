---
name: stripe
description: Stripe integration patterns — checkout, webhooks, subscription lifecycle, and tier mapping for Yardstick Hockey.
---

# Stripe Skill

Patterns for integrating Stripe subscriptions with the Yardstick Hockey Supabase backend.

## Architecture

```
Mobile App ──► Web checkout page ──► Stripe Checkout ──► Webhook ──► Supabase
                                                          │
                                                          ▼
                                                  profiles.tier = 'paid'
                                                  profiles.stripe_customer_id = 'cus_xxx'
```

## Database Columns

| Column | Table | Purpose |
|--------|-------|---------|
| `tier` | `profiles` | `'free'` or `'paid'`, controls content access |
| `stripe_customer_id` | `profiles` | Links Supabase user to Stripe customer (add when ready) |

## Checkout Flow

1. Mobile app opens web checkout URL with `user_id` param
2. Web page creates Stripe Checkout session via Edge Function
3. User completes payment on Stripe-hosted page
4. Stripe sends `checkout.session.completed` webhook
5. Edge Function updates `profiles.tier = 'paid'` and stores `stripe_customer_id`
6. Mobile app polls or listens for profile change

## Webhook Edge Function

Deploy to `supabase/functions/stripe-webhook`:

```typescript
// Key events to handle:
// - checkout.session.completed → set tier='paid'
// - customer.subscription.deleted → set tier='free'
// - customer.subscription.updated → check status
// - invoice.payment_failed → notify user
```

**Important**: Verify webhook signature with `Stripe-Signature` header.

## Subscription Management

- **Cancel**: Stripe customer portal (hosted by Stripe)
- **Resubscribe**: Same checkout flow
- **Account page**: Show current plan, link to Stripe portal

## Environment Variables

```
STRIPE_SECRET_KEY=sk_live_xxx          # Edge Function secret
STRIPE_WEBHOOK_SECRET=whsec_xxx        # Webhook signature verification
STRIPE_PRICE_ID=price_xxx              # Monthly subscription price
EXPO_PUBLIC_STRIPE_CHECKOUT_URL=...    # Web checkout page URL
```

## Current State (Phase 4)

- ✅ `profiles.tier` column exists (default `'free'`)
- ✅ `isPremium` flag in `AuthContext` checks tier
- ✅ Client-side gating via `PremiumGate` component
- ✅ Admin tier editing in web panel
- ⬜ Stripe API not connected — tiers manually set in Supabase for testing
- ⬜ `stripe_customer_id` column not yet added
- ⬜ Checkout page not built
- ⬜ Webhook Edge Function not deployed
