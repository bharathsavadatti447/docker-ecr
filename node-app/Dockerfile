# Use Node official image
FROM node:18-alpine

# Set working directory
WORKDIR /node-app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install --production

# Copy rest of the app code
COPY . .

# Expose port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
