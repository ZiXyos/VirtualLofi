FROM node:lts as dependencies
WORKDIR /virtual-lofi
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

FROM node:lts as builder
WORKDIR /virtual-lofi
COPY . .
COPY --from=dependencies /virtual-lofi/node_modules .node_nodules
RUN yarn build

FROM node:lts as runner
WORKDIR /virtual-lofi
ENV NODE_ENV production
COPY --from=builder /virtual-lofi/public ./public
COPY --from=builder /virtual-lofi/.next ./.next
COPY --from=builder /virtual-lofi/node_modules ./node_modules
COPY --from=builder /virtual-lofi/package.json ./package.json

EXPOSE 3000
CMD ["yarn", "start"]