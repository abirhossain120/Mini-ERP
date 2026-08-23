# Mini ERP (শেখার প্রজেক্ট)

Next.js (App Router) + PostgreSQL + Prisma দিয়ে বানানো একটি ছোট ERP সিস্টেম।
মডিউল: Auth, Inventory, Purchase, Sales, CRM, Accounting, HR.

**বর্তমান অবস্থা (Phase 1):** Login/Register, JWT Auth, Role-based User, Dashboard
layout + সব মডিউলের Prisma schema রেডি। বাকি মডিউলের UI ধাপে ধাপে (Phase 2+) যোগ হবে।

---

## ১. লোকাল সেটআপ

### ধাপ ১ — ডিপেন্ডেন্সি ইনস্টল
```bash
npm install
```

### ধাপ ২ — ফ্রি PostgreSQL ডাটাবেজ বানাও
[Neon](https://neon.tech) বা [Supabase](https://supabase.com) এ ফ্রি অ্যাকাউন্ট খুলে
একটা Postgres project বানাও। সেখান থেকে **connection string** কপি করো।

### ধাপ ৩ — Environment variables
```bash
cp .env.example .env
```
`.env` ফাইলে `DATABASE_URL` বসাও (Neon/Supabase থেকে পাওয়া connection string) এবং
`JWT_SECRET` এ যেকোনো লম্বা random string দাও।

### ধাপ ৪ — Database migrate + seed
```bash
npx prisma migrate dev --name init
npm run seed
```
Seed চালালে একটা Admin ইউজার তৈরি হবে:
- Email: `admin@erp.com`
- Password: `admin123`

### ধাপ ৫ — রান করো
```bash
npm run dev
```
[http://localhost:3000](http://localhost:3000) এ ওপেন করো।

---

## ২. GitHub এ আপলোড

```bash
git init
git add .
git commit -m "Initial commit: Mini ERP Phase 1"
git branch -M main
git remote add origin https://github.com/<তোমার-ইউজারনেম>/mini-erp.git
git push -u origin main
```

> `.env` ফাইলটা `.gitignore` এ আছে, তাই এটা GitHub এ যাবে না — এটাই নিরাপদ।

---

## ৩. Vercel এ Deploy

1. [vercel.com](https://vercel.com) এ গিয়ে GitHub দিয়ে লগইন করো।
2. **"Add New Project"** → তোমার `mini-erp` রিপো সিলেক্ট করো।
3. Framework Preset স্বয়ংক্রিয়ভাবে **Next.js** ধরবে — কিছু পরিবর্তনের দরকার নেই।
4. **Environment Variables** সেকশনে দুইটা variable যোগ করো:
   - `DATABASE_URL` → তোমার Neon/Supabase connection string
   - `JWT_SECRET` → তোমার random secret string
5. **Deploy** বাটনে ক্লিক করো।

Deploy হয়ে গেলে Vercel একটা লাইভ URL দিবে (যেমন `mini-erp.vercel.app`)।

> প্রথমবার deploy এর পর, লোকাল থেকে `npx prisma migrate deploy` চালিয়ে
> production database এ টেবিল তৈরি করে নাও (অথবা Neon/Supabase এর SQL editor
> ব্যবহার করেও করা যায়)।

---

## ৪. প্রজেক্ট স্ট্রাকচার

```
mini-erp/
├── prisma/
│   ├── schema.prisma      # সব মডিউলের টেবিল ডিজাইন
│   └── seed.ts            # স্যাম্পল ডেটা
├── src/
│   ├── app/
│   │   ├── api/           # Backend (Route Handlers)
│   │   │   ├── auth/      # login, register, logout
│   │   │   └── me/
│   │   ├── (dashboard)/   # লগইন-প্রোটেক্টেড পেজসমূহ
│   │   │   ├── dashboard/
│   │   │   ├── inventory/
│   │   │   ├── purchase/
│   │   │   ├── sales/
│   │   │   ├── crm/
│   │   │   ├── accounting/
│   │   │   └── hr/
│   │   ├── login/
│   │   ├── register/
│   │   └── layout.tsx
│   ├── components/
│   ├── lib/                # prisma client, JWT helper
│   └── middleware.ts        # route protection
└── package.json
```

## ৫. পরবর্তী ধাপ (Phase 2+)

- [ ] Inventory: Product CRUD + stock movement UI
- [ ] Purchase: Purchase Order তৈরি → stock IN
- [ ] Sales: Sales Order → Invoice → stock OUT
- [ ] CRM: Lead management UI
- [ ] Accounting: Journal entries + ledger view
- [ ] HR: Employee, Attendance, Payroll UI
