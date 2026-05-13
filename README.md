# Joshua Bariñan Portfolio

Vue 3 + TypeScript portfolio built with Vite and prepared for Vercel.

## Node version

Use Node 20. This project includes:

- `.nvmrc` with `20`
- `.node-version` with `20.20.2`
- `package.json` engines set to `20.x`

```bash
source ~/.nvm/nvm.sh
nvm use
node --version
```

If `nvm use` does not switch your terminal to Node 20, run:

```bash
source ~/.nvm/nvm.sh
nvm install 20
nvm use 20
```

## Install and run

```bash
source ~/.nvm/nvm.sh
nvm use
npm install
npm run dev
```

## Build

```bash
npm run build
```

## Where to upload portfolio images

Place these files here:

```text
public/portfolio/profile.jpg
public/portfolio/projects/aimhi-dashboard.png
public/portfolio/projects/ai-saas.png
public/portfolio/projects/fintech.png
```

The site has clean fallback artwork, so it will still look presentable before the images are added.

## Deploy to Vercel

1. Push this folder to GitHub.
2. Import the repository in Vercel.
3. Use the default Vite settings:
   - Build command: `npm run build`
   - Output directory: `dist`
4. Make sure Vercel uses Node 20. The `engines` field should handle this automatically.
