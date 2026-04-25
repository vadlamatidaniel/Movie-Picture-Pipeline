# ---------- Build Stage ----------
FROM node:18 AS build

WORKDIR /app

# Accept build arg from CI
ARG REACT_APP_MOVIE_API_URL

# Set env for React build
ENV REACT_APP_MOVIE_API_URL=${REACT_APP_MOVIE_API_URL}

# Install dependencies
COPY package*.json ./
RUN npm ci

# Copy source code
COPY . .

# Build React app
RUN npm run build


# ---------- Production Stage ----------
FROM nginx:alpine

# Remove default nginx static files (cleaner)
RUN rm -rf /usr/share/nginx/html/*

# Copy build output
COPY --from=build /app/build /usr/share/nginx/html

# Expose port
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]