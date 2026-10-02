FROM oven/bun:1.4

WORKDIR /app

COPY . .

RUN if [ -f package.json ]; then bun install; fi

EXPOSE 3000

CMD ["bun", "--hot", "index.ts"]
