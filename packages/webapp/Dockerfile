FROM node:18.16.0-alpine as build

USER root

WORKDIR /app

# Copy application dependency manifests to the container image.
COPY . .

# Install application dependencies
RUN apk update
RUN apk add python3 build-base chromium

# Set PYHTON env
ENV PYTHON=/usr/bin/python3

# Install pnpm packages dependencies
# Pin pnpm 8: it supports Node 18 and matches the repo's lockfileVersion 6.0.
# The latest pnpm (v11) requires Node 22+ and fails on this Node 18 image.
RUN npm install -g pnpm@8
RUN pnpm install --no-frozen-lockfile

# Build webapp package
RUN pnpm run build:webapp

FROM nginx

COPY ./packages/webapp/nginx/sites/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/packages/webapp/dist /usr/share/nginx/html
