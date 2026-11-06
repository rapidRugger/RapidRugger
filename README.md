# Rapid Rugger — setup guide (about 10 minutes, free)

Files: `index.html` (whole frontend: HTML + CSS + JS) · `Code.gs` (backend for Google Apps Script).

## 1. Backend (Google Sheet + Apps Script)
1. Create a new Google Sheet named **Rapid Rugger DB**. Keep it **private** (never share it; anyone with edit access can bypass the app).
2. In the sheet: **Extensions → Apps Script**. Delete the default code, paste all of `Code.gs`, save.
3. Apps Script → **Project Settings (gear) → Script properties → Add**:
   - `OWNER_EMAIL` = your email
   - `OWNER_NAME` = your name
   - `OWNER_PASSWORD` = a strong password (8+ chars, letters and numbers)
4. In the editor choose function **`setup`** → **Run** → approve permissions (sheet + email access). This creates every tab and your Owner account.
5. **Delete the `OWNER_PASSWORD` script property** (it is only needed once).
6. Optional: run **`seedDemo`** once to add 8 sample products (delete them later from the dashboard).
7. **Deploy → New deployment → Web app**: Execute as **Me**, Who has access **Anyone** → Deploy → copy the URL ending in `/exec`.
   After any later edit to `Code.gs`, use **Deploy → Manage deployments → Edit → New version**.

## 2. Frontend (GitHub Pages)
1. Open `index.html`, find `const API_URL = '';` near the top of the script and paste your `/exec` URL.
2. Create a GitHub repo, upload `index.html`, then **Settings → Pages → Deploy from branch → main / root**.
3. Your site goes live at `https://<user>.github.io/<repo>/`.

## 3. Roles
| Role | How created | Powers |
|---|---|---|
| **Owner** (you) | `setup()` | Everything, including Super Admins, commission %, full activity log. Cannot be edited, suspended or deleted by anyone. |
| **Super Admin** (father, brother) | Owner: Dashboard → Users → Add user → Super Admin | Everything except: touching the Owner, creating/removing/resetting other Super Admins, changing commission, seeing Owner activity. |
| **Seller** | Self-signup ("Start selling"); approved by Owner/Admin | Own analytics, orders, deliveries (status + tracking), earnings, printable receipts, product listing (reviewed before going live unless you turn approval off). |
| **Customer** | Self-signup | Shop, cart, coupons, checkout, order tracking, cancel before packing, receipts, reviews after delivery. |

## 4. Extras included
Coupons · low-stock alerts · CSV export · printable receipts · email notifications (order placed, status changes, approvals, password reset) · seller commission ledger · multi-seller carts (auto-split orders) · dark mode · audit trail · contact inbox · store settings (shipping, free-shipping limit, GSTIN/address on receipts, announcement bar).

## 5. Security built in
Passwords salted + iterated SHA-256 hashing (never stored plain) · 72-hour server-side sessions, revoked on logout/suspend/password reset · every action role-checked on the server (the UI hiding buttons is not what protects data) · prices, stock, totals and commission always computed server-side · login lockout after 5 failures · rate-limited reset/contact · spreadsheet-formula injection blocked · all user text HTML-escaped · only `http(s)` image URLs accepted.

## 6. Know the limits (honest notes)
- **Product images** are URLs (cells can't hold files). Easiest: put photos in your GitHub repo's `images/` folder and use `https://<user>.github.io/<repo>/images/name.jpg`. No image = auto-generated rug pattern.
- **Payments**: COD, or UPI where you share payment details after confirming. For online payments, add Razorpay/Cashfree next (needs a server-side verification step).
- **Email**: free Gmail allows ~100 emails/day from Apps Script.
- **Scale**: Sheets + Apps Script suits a young store (thousands of orders). Each request takes ~1–2 s. Move to Firebase/Supabase when you outgrow it.
- Passwords use SHA-256 (iterated) because Apps Script has no bcrypt. Ask sellers/admins for strong passwords.
- Frontend is public code; secrets (owner password) must stay only in Script properties.
