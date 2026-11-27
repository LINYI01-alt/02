# Douyin AI Radar Admin

Static Supabase-backed admin for overview, video library, keywords, collection jobs, creative scripts and review queue.

## Deploy
Enable GitHub Pages in Settings → Pages and choose GitHub Actions. A workflow publishes this site.

## Security
The browser uses only the Supabase publishable key. Authentication and Row Level Security protect database operations. Never expose a service-role key in frontend code.

## Integration boundaries
Collection jobs currently create queue records only; connect an authorized Douyin data provider and worker for real collection. AI script generation requires a configured model provider. Medical and compliance review requires human approval.
