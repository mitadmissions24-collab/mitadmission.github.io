FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app ./
EXPOSE 8080
CMD ["npm", "start"]
next.config.jspackage.json
{
 "name": "massachusettsadmissions-site",
 "version": "1.0.0",
 "private": true,
 "scripts": {
 "dev": "next dev -p 3000",
 "build": "next build",
 "start": "next start -p 8080"
 },
 "dependencies": {
 "@google-cloud/firestore": "^7.1.0",
 "@google-cloud/storage": "^7.6.0",
 "formidable": "^3.6.0",
 "next": "13.4.12",
 "react": "18.2.0",
 "react-dom": "18.2.0"
 }
}next.config.js
module.exports = {
 reactStrictMode: true,
};next.config.js
module.exports = {
 reactStrictMode: true,
};.env.example
ADMIN_TOKEN=replace_with_a_strong_token
GOOGLE_CLOUD_PROJECT=your-project-id
GCS_BUCKET=your-storage-bucket-name
NEXT_PUBLIC_SITE_URL=https://massachusettsadmissions.com
.gitignore
node_modules/
.next/
.env
.env.local
npm-debug.log
.DS_Store
uploads/
.log.gcloudignore
node_modules/
.env
.env.local
.next
.git
.gitignore
uploads
.logDockerfile
FROM node:18-alpine AS deps
WORKDIR /app
COPY package.json ./
RUN npm installFROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app ./
EXPOSE 8080
CMD ["npm", "start"].github/workflows/deploy.yml
name: deploy-to-cloud-runon:
 push:
 branches: [ main ]jobs:
 deploy:
 runs-on: ubuntu-latest