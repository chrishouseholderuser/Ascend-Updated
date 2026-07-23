# Ascend — hosted SAT prep app

This is a standalone, installable PWA for SAT practice. It keeps the original
climb → lesson/practice → results → progress loop and Quiz Lab. Quiz Lab
generates unlimited procedural questions locally, so it has no AI key, usage
cost, or student prompt data to protect.

## What is production-ready

- Real email/password accounts (and optional Google sign-in) through Supabase
  Auth; passwords never pass into app storage.
- Cross-device progress sync in Supabase.
- Row Level Security means an authenticated user can read or change only their
  own progress row.
- PWA installability and offline content cache.
- Security response headers for Netlify/Cloudflare Pages.

## Deploy as a new, separate repository

1. Create a new Supabase project. In **Authentication → Providers**, enable
   Email and (optionally) Google. Turn on email confirmation and add your
   eventual production URL under Authentication → URL Configuration.
2. Run [`supabase/schema.sql`](supabase/schema.sql) in Supabase's SQL Editor.
3. Set the project URL and **publishable/anon** key in `app/config.js`. Do not
   use a `service_role` key. This browser key is intentionally public and may
   be committed with the app.
4. The included GitHub Actions workflow deploys `app/` to GitHub Pages when
   GitHub Pages is set to **GitHub Actions** under repository Settings → Pages.
   The live URL is `https://chrishouseholderuser.github.io/Ascend-Updated/`.
   Use HTTPS; it is required for PWA installation and protects logins in transit.
5. Push this directory to a brand-new GitHub repository (do not add it to an
   existing repository).

### Notes

- The browser-visible Supabase publishable/anon key is designed to be public;
  data protection comes from the RLS policies in `schema.sql`.
- The prior prototype's fake Google sign-in, local password hashing, and saved
  password dropdown are not used by the hosted login flow.
- For a live AI question-authoring assistant, add a server-side function later;
  never put an AI provider key in this static app.
