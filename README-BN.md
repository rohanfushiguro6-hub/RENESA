# RENESA — বাংলা সেটআপ গাইড

ফাইলসমূহ:
- `index.html` — customer storefront, search, category, cart, COD order form
- `admin.html` — Supabase Auth admin login, product add/edit/delete, image upload, order status
- `supabase-schema.sql` — database tables, RLS policies, product image bucket

## ধাপে ধাপে সেটআপ
1. Supabase-এ নতুন project তৈরি করুন।
2. Supabase Dashboard → SQL Editor → New query খুলে `supabase-schema.sql`-এর সব কোড paste করে Run করুন।
3. Authentication → Users → Add user দিয়ে নিজের admin email ও শক্তিশালী password তৈরি করুন।
4. Users তালিকা থেকে ওই user-এর UUID কপি করুন। SQL Editor-এ নিচের query চালান, UUID বদলে দেবেন:
   `insert into public.admin_users(user_id) values ('PASTE-ADMIN-USER-UUID-HERE');`
5. Supabase Project Settings → API Keys (বা Connect) থেকে Project URL এবং publishable key (`sb_publishable_...`) কপি করুন। পুরোনো project-এ `anon` key দেখালে সেটিও public key হিসেবে ব্যবহার করা যায়।
6. `index.html` ও `admin.html` ফাইল দুটো খুলে `YOUR_SUPABASE_URL` এবং `YOUR_SUPABASE_PUBLISHABLE_KEY`-এর জায়গায় নিজের values বসান। দুই ফাইলেই একই values দিতে হবে।
7. GitHub-এ public repository তৈরি করে এই ফাইলগুলো upload করুন। GitHub Pages-এর নীতিতে বাণিজ্যিক লেনদেনের জন্য তৈরি online shop host করা অনুমোদিত উদ্দেশ্য নয়; তাই GitHub-কে code repository হিসেবে রাখুন এবং e-commerce অনুমোদন করে এমন host বেছে নিন।
8. Website খুলে test product order দিন। Admin panel হবে `admin.html` path-এ।

## নিরাপত্তা
- `sb_secret_...` বা `service_role` key কখনো HTML/JavaScript-এ দেবেন না।
- Public visitors product দেখতে এবং COD order submit করতে পারবে; product edit, order view/status update ও photo upload শুধু admin করতে পারবে (RLS policy অনুসারে)।
- এটি starter/learning project। Public launch করার আগে order total ও item price server-side/Edge Function-এ database থেকে পুনরায় হিসাব করুন, stock validate করুন এবং spam protection যোগ করুন। Card payment এখানে চালু নেই।
