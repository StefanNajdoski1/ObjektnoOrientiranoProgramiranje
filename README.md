# Create a zip archive with the backend and Angular frontend structure described above.
import os
import zipfile

project_root = "/mnt/data/crypto-exchange-app"
backend_path = os.path.join(project_root, "backend")
frontend_path = os.path.join(project_root, "crypto-app")
os.makedirs(backend_path, exist_ok=True)
os.makedirs(frontend_path, exist_ok=True)

# Create README.md
readme_content = """# 💱 Менувачница на Криптовалути - Full Stack Веб Апликација

Оваа апликација е динамична SPA менувачница за криптовалути со:
- ✅ Корисничка регистрација и најава (JWT)
- ✅ Преглед на цени на криптовалути (CoinGecko API)
- ✅ Конверзија и трансакции (купи/продај)
- ✅ Историја на трансакции по корисник
- ✅ Графикон на цени (ng2-charts)
- ✅ Docker + MongoDB + Angular + Node.js + Express

...

## ✅ Status

🟢 Завршена прва верзија. Подготвено за демонстрација и деплојмент.
"""

with open(os.path.join(project_root, "README.md"), "w") as f:
    f.write(readme_content)

# Create Docker Compose file
docker_compose_content = """
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - MONGO_URI=mongodb://mongo:27017/crypto-db
      - JWT_SECRET=supersecretkey
    depends_on:
      - mongo
    volumes:
      - ./backend:/app

  mongo:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

  frontend:
    build: ./crypto-app
    ports:
      - "4200:80"
    volumes:
      - ./crypto-app:/app

volumes:
  mongo-data:
"""

with open(os.path.join(project_root, "docker-compose.yml"), "w") as f:
    f.write(docker_compose_content)

# Zip the entire project
zip_path = "/mnt/data/crypto-exchange-full-project.zip"
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as zipf:
    for root, _, files in os.walk(project_root):
        for file in files:
            file_path = os.path.join(root, file)
            arcname = os.path.relpath(file_path, project_root)
            zipf.write(file_path, arcname)

zip_path
