# Module 1 — Auth & environments with PAC CLI

Everything in this workshop starts from the terminal in VS Code. The PAC CLI keeps **auth profiles**
(local, on your machine only) and lets you pick the **active environment** for every command.

## 1. Sign in

```bash
pac auth create --url https://yourorg.crm4.dynamics.com
```

A browser window opens — sign in with your work account. Verify:

```bash
pac auth list
```

Auth profiles are stored locally (never commit `.pac/` or `*.paccfg` — they're in `.gitignore`).

## 2. Pick the environment

```bash
pac env list                                   # all environments you can see
pac env select --environment <id-or-url>       # make one the active environment
```

## 3. Verify you're connected

```bash
pac org whoami
```

You should see your user, the organization, and the environment URL.

## 4. The workshop solution

Model-driven apps and most ALM-friendly components live in a **solution**. Create one empty
unmanaged solution in the maker portal (make.powerapps.com → Solutions → New) named
`WorkshopCore` with publisher prefix `wsp` — or ask your workshop lead for an existing one.

On disk, initialize the matching CLI project:

```bash
pac solution init --publisher-name Workshop --publisher-prefix wsp
```

Publisher prefix rules: 2–8 characters, lowercase letters and digits, must start with a letter, and
must be registered for your organization. Everything you create in this workshop gets the `wsp_`
prefix (tables, columns, environment variables).

## 5. Verify solution access

```bash
pac solution list
```

You should see `WorkshopCore` in the list.

## 6. VS Code workflow tip

The **Power Platform Tools** extension shows your environments and solutions in the activity bar —
but for the workshop we'll type the `pac` commands in the integrated terminal so you learn them.
Both do the same thing under the hood.

✅ **Checkpoint:** `pac org whoami` shows your user and environment, and `pac solution list` shows `WorkshopCore`.
