# Film & Media Homework Portal — Name + Batch Student Access

This version removes student email/password registration. Students enter their full name and batch code only. Teacher access remains email/password based.

## What changed
- Student login: full name + batch code, no student email/password and no signup confirmation email.
- Names are trimmed and repeated spaces are collapsed. Batch codes are trimmed and converted to uppercase, so `pve 2606`, `Pve 2606`, and `PVE 2606` are treated the same.
- The 29-class homework library, batch unlock limits, class assignment/deadlines, submissions, teacher folders, and red/white design remain.
- Logo can be replaced at `assets/logo.svg`; update `logoPath` in `js/config.js` if using a different filename.
- Teacher access uses the existing teacher email/password account.

## Important: one Supabase setting is required
This still uses your existing Supabase project to share data between devices. It does not use a separate Edge Function or email-based student registration.

1. Open your Supabase project.
2. Go to **Authentication → Sign In / Providers** (the exact label may vary).
3. Enable **Anonymous Sign-Ins**.
4. Save the setting.
5. Confirm `js/config.js` contains your existing project URL and publishable/anon key. Never put a `service_role` or secret key in browser files.
6. Keep the database schema already set up for the current portal. If you have not run it yet, run `supabase/setup.sql` in SQL Editor. If your current portal already uses folders, also run `supabase/folder_migration.sql` only if you have not previously run it.
7. Open the project using VS Code Live Server and test with an existing active batch, for example `PVE 2606`.

## How students use it
1. Type their full name.
2. Type their batch code, for example `PVE 2606`.
3. Click **Open my homework**.

Batch code capitalization is normalized automatically. Names are compared case-insensitively for a current browser session, but spelling differences are not assumed to be the same person.

## How teachers use it
Click **Teacher access** and sign in with the teacher email/password account already assigned the `teacher` role in `public.profiles`. If you have not created/promoted a teacher account yet, follow the teacher setup instructions from your existing setup notes. Do not make a student account a teacher unless it is an account you control.

## Important limitations of name-only student access
- A name and batch code do not prove identity. Someone who knows another student's name and batch code could potentially appear as that student.
- Supabase anonymous sign-in creates a separate anonymous user for a browser session. The portal keeps that session in the browser and hides student sign-out to reduce accidental duplicate records. Clearing browser data or changing devices can create a new student record.
- Do not use name-only access for confidential grades or identity-sensitive records. If reliable identity verification becomes necessary, use student IDs or proper accounts later.
- Supabase anonymous sign-ins may be subject to project limits and abuse protection. If the provider setting is unavailable or anonymous sign-in returns an error, share the exact error; do not create secret keys or deploy an Edge Function.

## Folder structure
- `index.html`
- `assets/logo.svg`
- `css/style.css`
- `js/config.js`
- `js/app.js`
- `supabase/setup.sql`
- `supabase/folder_migration.sql`

## Run locally
Open this folder in VS Code and use the Live Server extension, then open the local URL it shows. For online student access, the website must eventually be hosted on a static host; Supabase remains the shared database/auth service. Both can be used on free tiers subject to their current limits.


## Private keyboard sign-out shortcut

From any portal screen, press **Ctrl + Alt + Shift + L** to sign out of the current Supabase session and return to the student/teacher entry screen. This shortcut is intentionally not shown in the interface. It is a convenience, not a security boundary: anyone who learns the key combination or inspects the JavaScript can discover it. Keep teacher access protected by its teacher login.
