ELAN AFRIQUE — WEBSITE + SUPABASE SETUP

FILES
- index.html: storefront, customer sign-up/login, admin product manager.
- supabase/schema.sql: database tables, security policies and image storage bucket.

IMPORTANT
The site is a frontend that connects to Supabase. It is not connected to a live backend until you create a Supabase project and add its URL and anon/public key to index.html. Do NOT put a Supabase service_role key, database password, or payment secret in index.html.

SETUP
1. Create a Supabase project at https://supabase.com/dashboard.
2. Open SQL Editor in the project, paste all of supabase/schema.sql, and run it.
3. In Supabase Project Settings > API, copy the Project URL and anon/public key.
4. In index.html, replace YOUR_SUPABASE_PROJECT_URL and YOUR_SUPABASE_ANON_KEY with those values.
5. In Supabase Authentication > URL Configuration, add your Vercel site URL to Site URL and Redirect URLs. For local testing, also add http://localhost:3000/**.
6. Upload index.html to your GitHub repository and deploy it with Vercel. Keep supabase/schema.sql and this README in the repository.
7. Open the deployed site, create your owner account using your email, name and password, and confirm the email if confirmation is enabled.
8. In Supabase SQL Editor, run the following after replacing the email with the exact owner email:

update public.profiles p
set role = 'admin'
from auth.users u
where p.id = u.id
  and lower(u.email) = lower('YOUR-OWNER-EMAIL@example.com');

9. Sign out and sign back in on the website. The Admin button will appear. Open it to upload product photos, add products, edit details, and remove products. Saved products and images are published to the storefront.

AUTHENTICATION
Customer accounts use Supabase Auth. Passwords are handled by Supabase and are never stored in this website's database as plain text. New accounts are assigned the customer role by the database trigger. Only an account explicitly promoted by the database owner can administer products.

SECURITY / SCOPE
- The browser uses only the Supabase anon/public key, protected by Row Level Security policies.
- Never expose service_role keys.
- This build covers sign-up/login and product/image administration. It does not yet implement payment processing, order management, customer order history, or a production checkout. Those require separate server-side workflows and payment-provider credentials.
- Image uploads accept JPG, PNG or WebP up to 5 MB.
