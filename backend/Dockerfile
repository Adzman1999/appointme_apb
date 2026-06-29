FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --no-cache

COPY . .

RUN npm run build

FROM node:20-alpine

USER node

WORKDIR /app

COPY --chown=node:node package*.json ./

RUN npm ci --only=production --no-cache

COPY --chown=node:node --from=builder /app/dist ./dist

EXPOSE 5000

HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
  CMD node -e "require('http').get('http://localhost:5000/health', (r) => {if (r.statusCode !== 200) throw new Error(r.statusCode)})"

CMD ["node", "dist/server.js"]