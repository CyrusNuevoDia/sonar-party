# DirectorySF first-run setup

Run this once with the operator before the first search. Afterwards the
signed-in browser session is all the skill needs.

## 1. Check for an existing session

Open `https://directorysf.com/searchers` in the in-app browser and wait for the
page to settle. If listing cards render, the operator is signed in — setup is
done.

## 2. Sign in

If the page asks for authentication, send the operator to
`https://directorysf.com` to sign in. The operator completes this themselves in
the in-app browser — never ask for, read, or enter their credentials, and never
copy cookies or inspect session storage to bypass the sign-in wall.

## 3. No account yet

DirectorySF is an invite-based community directory — searcher profiles list who
invited them. If the operator has no account, they need an invite from an
existing member; the skill cannot create access. Suggest they ask a friend who
is already in the directory.

## 4. Verify access

Reload `https://directorysf.com/searchers` and confirm listing cards render
after the `Loading directory listings` status disappears. Unauthenticated
sessions get `401` responses and empty results — if that happens, return to
step 2 rather than falling back to shell HTTP clients.
