# Reel Vault automatic fulfilment

Production flow:

1. Customer clicks the hosted storefront CTA.
2. The storefront opens your Razorpay Payment Link.
3. Razorpay sends `payment_link.paid` to the Cloudflare Worker.
4. The Worker validates `X-Razorpay-Signature` against the exact raw webhook body.
5. The Worker verifies the payment is captured/paid, checks amount/currency, and optionally checks one exact Payment Link ID.
6. The buyer email is read from the Razorpay payload.
7. Resend sends the private Google Drive delivery link.
8. Cloudflare KV stores the payment ID to prevent duplicate delivery when Razorpay retries a webhook.

Required live values:

- Public frontend value: `PAYMENT_LINK_URL`
- Worker secret: `RAZORPAY_WEBHOOK_SECRET`
- Worker secret: `RESEND_API_KEY`
- Worker secret: `DRIVE_LINK`
- Worker var: `FROM_EMAIL`
- Worker var: `PRODUCT_NAME`
- Worker var: `EXPECTED_AMOUNT_PAISE=19900`
- Worker var: `RAZORPAY_PAYMENT_LINK_ID`
- KV binding: `FULFILLED`

Razorpay webhook URL:

`https://<worker>.workers.dev/razorpay-webhook`

Subscribe to `payment_link.paid`.

Never put the Drive link, Razorpay webhook secret, or email API key into the static storefront.
