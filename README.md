# Alpha Account Registration

A small, static registration page for Alpha Accounts. It uses the same Supabase Auth project as AlphaLauncher and does not modify the launcher repository.

## Current files

- `index.html` — complete responsive registration page and Supabase sign-up flow.
- No build step or package installation is required.

## Deploy with Cloudflare Pages

1. Open [Cloudflare Dashboard](https://dash.cloudflare.com/) and select **Workers & Pages**.
2. Choose **Create** / **Create application**, then select **Pages** and connect to Git.
3. Select `MsAlexTsuki/alpha-account-registration`.
4. Use the `main` branch.
5. For a plain static site, set the build command to **None** (or leave it empty if Cloudflare allows that), and set the output directory to `/` or the repository root, as offered by the current UI.
6. Deploy. Cloudflare will give the site a `.pages.dev` URL.

## Required Supabase configuration

This page uses the existing Alpha Supabase project URL and public publishable key already used by AlphaLauncher. A publishable key is intended for client-side use; never add a `service_role` or secret key to this repository.

After Cloudflare gives you the final public URL:

1. Open the Supabase project dashboard.
2. Go to **Authentication → URL Configuration**.
3. Set **Site URL** to the final `https://...pages.dev` URL.
4. Add that same URL to **Redirect URLs** if needed.
5. Check **Authentication → Providers → Email** to confirm whether email confirmation is enabled. If enabled, users must confirm their email before signing in.

## Important profile/permissions checks

The registration form sends `username` and `display_name` as Supabase Auth user metadata. The existing Alpha database function `public.handle_new_auth_user()` was found in the AlphaLauncher repository. Before accepting public registrations, verify in the Supabase database that an `auth.users` trigger actually invokes that function and creates `public.profiles` rows. The function definition by itself does not install a trigger.

The form does not offer a rank selector. Rank assignment must remain enforced by database policies/functions, never by trusting browser-submitted metadata. The page does not set `rank_key`.

## Local preview

Open `index.html` in a modern browser, or serve the repository with any static HTTP server. Some browser configurations restrict JavaScript modules on `file://`, so an HTTP preview is preferred.

## Before sharing the link

- Test one new account with an email address you control.
- Verify email confirmation and sign-in behavior.
- Verify a profile row is created in `public.profiles`.
- Verify a newly registered user cannot change their rank.
- Review Supabase Auth rate limits and abuse protection before public launch.
