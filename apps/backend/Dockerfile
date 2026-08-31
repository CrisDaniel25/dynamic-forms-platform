FROM node:22-alpine

WORKDIR /app

COPY package.json pnpm-workspace.yaml ./
COPY apps/backend ./apps/backend
COPY packages ./packages

RUN corepack enable

WORKDIR /app/apps/backend

CMD ["sh", "-c", "echo Backend scaffold pending && sleep infinity"]
