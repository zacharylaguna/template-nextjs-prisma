This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

### Prerequisites

- Docker and Docker Compose installed
- Node.js 20.19+ or 22.12+ or 24+ (required for Prisma 7)
- nvm (Node Version Manager) recommended for managing Node.js versions

### Docker Install (Ubuntu24)
```bash
sudo apt install docker.io
sudo usermod -aG docker ubuntu # requires relog
sudo apt install docker-compose-v2 -y
```

### Node.js Setup with nvm

This project uses **Prisma 7**, which requires Node.js 20.19+, 22.12+, or 24+.

**Install nvm:**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

**Load nvm (or restart your terminal):**
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

**Install and use Node.js 20:**
```bash
nvm install 20
nvm use 20
node --version  # Should show v20.x.x
```

### Setup

1. **Create `.env` file:**
```bash
echo 'DATABASE_URL=postgresql://postgres:postgres@localhost:5432/nextjs_db?schema=public' > .env
```

2. **Ensure you're using Node.js 20+:**
```bash
nvm use 20
```

3. **Install dependencies:**
```bash
npm install
npx prisma generate
```

**Note:** This project uses Prisma 7, which stores database configuration in `prisma/prisma.config.ts` instead of the schema file.

### Running with Docker (Production-like)

**Start all services (PostgreSQL + Next.js):**
```bash
sudo docker compose up --build
```

**Stop services:**
Press `Ctrl+C`, then:
```bash
sudo docker compose down
```

**Run database migrations:**
```bash
sudo docker compose exec app npx prisma migrate dev --name init
```

### Running Locally (Development)

This approach is recommended for faster development with hot reload.

**Start only PostgreSQL in Docker:**
```bash
sudo docker compose up db -d
```

The `-d` flag runs PostgreSQL in detached mode (background). Your local Next.js app will connect to PostgreSQL on `localhost:5432`.

**Verify PostgreSQL is running:**
```bash
sudo docker compose ps
```

**Run Next.js locally:**
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

**Stop PostgreSQL when done:**
```bash
sudo docker compose down
```

### After Making Code Changes

- **Frontend/App code changes**: Rebuild and restart containers
  ```bash
  sudo docker compose down
  sudo docker compose up --build
  ```

- **Database schema changes** (`prisma/schema.prisma`):
  ```bash
  sudo docker compose exec app npx prisma migrate dev --name your_migration_name
  ```

- **Dependency changes** (`package.json`):
  ```bash
  sudo docker compose down
  sudo docker compose up --build
  ```

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
