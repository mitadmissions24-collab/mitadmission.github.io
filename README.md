.container {
 max-width: 1200px;
 margin: 0 auto;
 padding: 0 16px;
}.btn-primary {
 display: inline-block;
 background: #0a2f5a;
 color: #fff;
 border: none;
 border-radius: 10px;
 font-weight: 600;
 padding: 12px 18px;
 cursor: pointer;
}.btn-secondary {
 display: inline-block;
 background: #fff;
 color: #0a2f5a;
 border: 1px solid #0a2f5a;
 border-radius: 10px;
 font-weight: 600;
 padding: 12px 18px;
 cursor: pointer;
}.field {
 width: 100%;
 padding: 12px 14px;
 border: 1px solid #d1d5db;
 border-radius: 10px;
 background: white;
}oknoFROM node:18-alpine AS runner
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
 runs-on: ubuntu-lateststeps:
  - name: Checkout source
    uses: actions/checkout@v4

  - name: Set up Node.js
    uses: actions/setup-node@v4
    with:
      node-version: 18

  - name: Install dependencies
    run: npm install

  - name: Build app
    run: npm run build

  - name: Authenticate to Google Cloud
    uses: google-github-actions/auth@v2
    with:
      credentials_json: ${{ secrets.GCP_SA_KEY }}

  - name: Set up Cloud SDK
    uses: google-github-actions/setup-gcloud@v2
    with:
      project_id: ${{ secrets.GCP_PROJECT_ID }}
      install_components: "gke-gcloud-auth-plugin"

  - name: Configure Docker for Artifact Registry
    run: |
      gcloud auth configure-docker us-central1-docker.pkg.dev --quiet

  - name: Build and push image to Artifact Registry
    run: |
      docker build -t us-central1-docker.pkg.dev/${{ secrets.GCP_PROJECT_ID }}/cloud-run-source-deploy/ma-admissions-site:${{ github.sha }} .
      docker push us-central1-docker.pkg.dev/${{ secrets.GCP_PROJECT_ID }}/cloud-run-source-deploy/ma-admissions-site:${{ github.sha }}

  - name: Deploy to Cloud Run
    run: |
      gcloud run deploy ma-admissions-site \
        --image=us-central1-docker.pkg.dev/${{ secrets.GCP_PROJECT_ID }}/cloud-run-source-deploy/ma-admissions-site:${{ github.sha }} \
        --region=${{ secrets.GCP_REGION }} \
        --platform=managed \
        --allow-unauthenticated \
        --set-env-vars="GOOGLE_CLOUD_PROJECT=${{ secrets.GCP_PROJECT_ID }},GCS_BUCKET=${{ secrets.GCS_BUCKET }},ADMIN_TOKEN=${{ secrets.ADMIN_TOKEN }},NEXT_PUBLIC_SITE_URL=https://massachusettsadmissions.com"public/robots.txt
User-agent: 
Allow: /https://massachusettsadmissions.com/sitemap.xmlSitemap: https://massachusettsadmissions.com/sitemap.xml<?xml version="1.0" encoding="UTF-8"?><urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://massachusettsadmissions.com/</loc>
    <lastmod>2026-10-16</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://massachusettsadmissions.com/scholarships</loc>
    <lastmod>2026-10-16</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://massachusettsadmissions.com/apply</loc>
    <lastmod>2026-10-16</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.7</priority>
  </url>
</urlset>styles/globals.css
html {
 scroll-behavior: smooth;
}body {
 margin: 0;
 font-family: Arial, Helvetica, sans-serif;
 background: #f7f7f7;
 color: #111827;
}{
 box-sizing: border-box;
}a {
 text-decoration: none;
}input, textarea, select, button {
 font: inherit;
}.page-shell {
 max-width: 1200px;
 margin: 0 auto;
 padding: 24px;
}.card {
 background: #fff;
 border: 1px solid #e5e7eb;
 border-radius: 12px;
 box-shadow: 0 1px 2px rgba(0, 0, 0, 0.04);
}.container {
 max-width: 1200px;
 margin: 0 auto;
 padding: 0 16px;
}