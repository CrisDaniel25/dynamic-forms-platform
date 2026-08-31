FROM node:22-alpine

WORKDIR /app

COPY package.json pnpm-workspace.yaml ./
COPY apps/frontend ./apps/frontend
COPY packages ./packages

RUN corepack enable

WORKDIR /app/apps/frontend

CMD ["sh", "-c", "echo Frontend scaffold pending && sleep infinity"]
