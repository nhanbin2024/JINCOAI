
# ChainOpera AI – Q&A Agent (Node.js)

Agent hỏi–đáp dựa trên nội dung **ChainOpera AI White Paper / Roadmap** (RAG tối giản).  
Tính năng:
- Ingest nội dung roadmap thành **embeddings** (OpenAI) và lưu **kb/index.json**.
- API `POST /agent/ask` trả lời câu hỏi bằng tiếng Việt, trích xuất ngữ cảnh liên quan.
- Dễ deploy, dễ mở rộng.

> Yêu cầu: Node 18+ và biến môi trường `OPENAI_API_KEY` (sử dụng Embeddings + Chat Completions).

## 1) Cài đặt
```bash
npm install
cp .env.example .env  # thêm OPENAI_API_KEY vào .env
```

## 2) Ingest dữ liệu
Sửa/giữ file nội dung ở `kb/chainopera_roadmap.md`, rồi chạy:
```bash
npm run ingest
```
Lần đầu sẽ tạo `kb/index.json` (vector store tối giản).

## 3) Chạy server
```bash
npm run dev
# hoặc
npm start
```
Mặc định server ở `http://localhost:3000`.

## 4) Gọi API
```bash
curl -X POST http://localhost:3000/agent/ask   -H "Content-Type: application/json"   -d '{"question":"roadmap này nói về chủ đề gì và khi nào có token/TGE?"}'
```

## 5) Cấu trúc thư mục
```
.
├─ kb/
│  ├─ chainopera_roadmap.md     # nội dung nguồn
│  └─ index.json                 # embeddings (tạo bởi ingest)
├─ scripts/
│  └─ ingest.js                  # tạo embeddings
├─ src/
│  ├─ agent.js                   # logic chọn ngữ cảnh & gọi LLM
│  └─ index.js                   # Express API
├─ .env.example
├─ package.json
└─ README.md
```

## 6) Gợi ý triển khai
- **Bảo mật**: Không commit `.env`. Có thể dùng GitHub Actions Secret hoặc Docker Secret.
- **Mở rộng**: Thêm nhiều file `.md` vào `kb/` rồi cập nhật `scripts/ingest.js` để đọc tất cả.
- **Vector store**: Có thể thay `kb/index.json` bằng Postgres + pgvector, Pinecone, hoặc Weaviate.
- **Model**: Sử dụng `gpt-4o-mini` (rẻ/nhanh). Tùy chọn `gpt-4.1` nếu cần chất lượng cao hơn.
