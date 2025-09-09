# Use the official Node.js image with Alpine Linux
FROM node:14.17.0-alpine3.13

# Create a new user and group to run the application
RUN addgroup app && adduser -S -G app app

# Set the working directory for the application
WORKDIR /app

# Copy package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install --no-cache

# Change ownership of the node_modules folder to the app user
RUN chown -R app:app /app/node_modules

# Copy the rest of the application files to the working directory
COPY . .

# Ensure that the app user has ownership of the entire app directory
RUN chown -R app:app /app

# Set the user to use when running this image
USER app

# Set environment variables
ENV API_URL=http://localhost:3000/myapi

# Expose port 3000 to the outside world
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
