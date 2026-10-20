
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