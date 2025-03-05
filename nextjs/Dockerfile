FROM node:lts-alpine AS base
ENV NEXT_TELEMETRY_DISABLED=1

FROM base AS development
WORKDIR /usr/src/app
COPY . .
CMD ["sh", "-c", "npm install && npm run dev"]

FROM base AS dependencies
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install

FROM base AS build
WORKDIR /usr/src/app
COPY --from=dependencies /usr/src/app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM base AS production
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs
COPY --from=build /usr/src/app/public ./public
COPY --from=build --chown=nextjs:nodejs /usr/src/app/.next/standalone ./
COPY --from=build --chown=nextjs:nodejs /usr/src/app/.next/static ./.next/static
USER nextjs
EXPOSE 3000
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"
CMD ["node", "server.js"]
