# Battle Brush Studio

A full-stack 3D model-coloring project by Lakota Camp, built with React, Three.js / React Three Fiber, and a Flask / SQLAlchemy backend. The editor lets a user select a model, adjust material colors, and persist model/color selections. Accounts use Flask sessions and Bcrypt password hashing.

This is a clean portfolio snapshot of the original project, not a recreation of its development history. Much of the source retains commented earlier iterations and explanations of the learning process. The original project was largely coded by its author; ChatGPT was used to answer programming questions rather than generate the implementation. This publication adds environment-based configuration, limited security cleanup, and setup documentation.

## Status and limitations

- Frontend production compilation and backend account/session smoke tests have been checked for this snapshot.
- The original repository did not include the `shoe.gltf`, `Donkey.gltf`, or `scene.gltf` files referenced by the editor. Local copies were located later, but Shoe and Donkey redistribution licenses were not established; those models and the Shoe textures are therefore not included. The Space Marine binary is about 227 MB and is also not bundled. Without those assets and their referenced textures/buffers, the corresponding 3D previews cannot render. This is source code, not a claim of a complete deployed demo.
- Model upload is an experimental, unrouted Cloudinary component, not a finished feature. MongoDB migration and broader model-format support are future ideas, not implemented capabilities.
- This is a local learning/portfolio application, **not production-ready**. Authorization/ownership enforcement, input validation, dependency upgrades, and a comprehensive security review are needed before any public deployment with real user data. Do not use real credentials or sensitive information in a demo.

## Local setup

### Backend

Use a Python virtual environment. This snapshot was smoke-tested with Python 3.12.14; the original Pipfile records Python 3.8.13.

```bash
cd server
python -m venv .venv
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
export FLASK_SECRET_KEY="$(python -c 'import secrets; print(secrets.token_hex(32))')"
# PowerShell: $env:FLASK_SECRET_KEY = python -c "import secrets; print(secrets.token_hex(32))"
flask --app app db upgrade
python app.py
```

Use a new randomly generated secret of at least 32 characters. Keep it out of source control. In a persistent installation, securely retain the same secret across restarts; changing it invalidates existing sessions. Never reuse a historic committed key. The server listens locally on port 5555; debug mode is off by default. The SQLite database is created locally and is not distributed.

Create a demo account through the signup screen. The historical `seed.py` is experimental and produces placeholder accounts/model paths; it is not required and is not the recommended startup path.

### Frontend

In a second terminal, at the repository root:

```bash
npm ci
cd client
npm ci
npm run dev
```

Visit `http://localhost:3000`. Vite forwards `/api` requests to the local Flask server. Dependencies exist in **both** root and client manifests, so install both. `npm run build` in `client` produces the static build but does not supply a production backend/proxy.

Clerk imports from an earlier experiment remain, but the active login/signup flow uses the Flask backend and does not require a Clerk key. The unused upload prototype reads `VITE_CLOUDINARY_CLOUD_NAME` and `VITE_CLOUDINARY_UPLOAD_PRESET` from your own optional local client environment. Vite values are public browser configuration, never a place for secret keys. Do not enable an unrestricted unsigned upload preset in production.

## Third-party assets

`client/public/Scene.jsx` includes the original attribution to Matrix (https://sketchfab.com/TUONO), the Space Marine model (https://sketchfab.com/3d-models/space-marine-dffb2975c74c4be8ac4dd991caa03032), and its stated CC-BY-4.0 license. For a local preview, obtain the Space Marine asset from its credited source under the stated license and place its glTF and referenced buffer/texture files under `client/public/`, retaining the original relative paths and attribution. Select Space Marine in the editor. The binary exceeds GitHub's normal single-file limit; it is not bundled here. Preserve third-party attribution and verify asset rights before adding or redistributing models. The React/Vite logos belong to their respective projects. No new license is granted over third-party content.

## Publication hygiene

The public snapshot excludes local environment files, installed dependencies, local databases, and the old course-sync workflow. It contains no original Git history. The private original repository is preserved separately. Never commit secrets, database contents, or local environment files.
