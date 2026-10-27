AI Aircraft Maintenance Application

<img width="1646" height="940" alt="image" src="https://github.com/user-attachments/assets/47427565-461c-49eb-84f6-d8997e078d4c" />

Aircraft engineering analytics and maintenance recommendations, with a React frontend and FastAPI backend.

## Deploy on Render and Vercel

The repository includes a `render.yaml` Blueprint for the API and a Vercel configuration for the frontend. Deploy the backend first so you can set its URL in Vercel.

### 1. Deploy the backend on Render

1. Push this repository to a Git provider connected to Render.
2. In Render, create a new **Blueprint** from the repository and accept the `render.yaml` service configuration.
3. Set `FRONTEND_ORIGIN` to the Vercel site URL after the frontend is deployed. For the first backend deploy, use `http://localhost:3000`; update it after step 2 below. Multiple origins may be comma-separated.
4. Set `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` in the Render service environment to credentials allowed to invoke the configured Amazon Bedrock model. Keep these values in Render secrets; do not commit them.
5. Keep `AWS_REGION=us-east-1` and `BEDROCK_MODEL_ID=amazon.nova-pro-v1:0` unless your Bedrock setup uses different values.

Render builds from `backend/`, installs `backend/requirements.txt`, starts Uvicorn on Render's assigned port, and checks `/health`. The maintenance manual under `backend/data/` is deployed with the backend.

### 2. Deploy the frontend on Vercel

1. Import the repository into Vercel.
2. Set **Root Directory** to `frontend`.
3. Keep the framework preset as **Vite**. Vercel uses `npm run build` and `dist` as the output directory, as configured in `frontend/vercel.json`.
4. Add the environment variable `VITE_API_BASE_URL` with the full Render service URL, such as `https://ai-aircraft-maintenance-api.onrender.com`, without a trailing slash. Add it for Production and Preview if both environments should work.
5. Deploy, then set Render's `FRONTEND_ORIGIN` to the deployed Vercel origin (for example, `https://your-project.vercel.app`). Include any custom domain and preview origins that need API access, separated by commas.

Vite embeds `VITE_API_BASE_URL` at build time, so redeploy the frontend after changing it.

## Local development

- Backend: from `backend/`, install `requirements.txt`, then run `uvicorn app:app --reload --port 8000`.
- Frontend: from `frontend/`, run `npm install`, then `npm run dev`. Set `VITE_API_BASE_URL=http://localhost:8000` in `frontend/.env.local` (see `.env.local.example`).
