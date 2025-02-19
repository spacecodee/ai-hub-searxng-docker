# Use a more specific and stable Node.js version
FROM node:20-alpine

WORKDIR /app

# Clone the repository
RUN apk add --no-cache git && \
    git clone https://github.com/stackblitz-labs/bolt.diy.git . && \
    apk del git

# Install dependencies
RUN npm install

# Build the application
RUN npm run build

# Expose the port
EXPOSE 3000

# Start the application
CMD ["npm", "start"]