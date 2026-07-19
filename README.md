# AI Code Reviewer

An AI-powered code review tool with a split-pane interface: write or paste code on the left, click **Review**, and get an instant, structured code review — complete with issues found and a recommended fix — on the right, powered by Google's Gemini model.

## ✨ Features

- 🖊️ **Live code editor** with syntax highlighting (via `react-simple-code-editor` + Prism)
- 🤖 **AI-generated reviews** using Google Gemini (`gemini-2.0-flash`)
- 📋 **Structured feedback** — Bad Code → Issues → Recommended Fix → Improvements
- 🎨 **Markdown-rendered output** with syntax-highlighted code blocks (`react-markdown` + `rehype-highlight`)
- ⚡ **Fast dev experience** with Vite + React 19

## 🧱 Tech Stack

| Layer      | Technology |
|------------|------------|
| Frontend   | React 19, Vite, Axios, Prism.js, react-markdown, rehype-highlight |
| Backend    | Node.js, Express |
| AI         | Google Generative AI SDK (`@google/generative-ai`), Gemini 2.0 Flash |
| Other      | CORS, dotenv |

## 📁 Project Structure

```
Code-review-main/
├── BackEnd/
│   ├── server.js                     # App entry point
│   ├── package.json
│   └── src/
│       ├── app.js                    # Express app + middleware setup
│       ├── routes/
│       │   └── ai.routes.js          # POST /ai/get-review route
│       ├── controllers/
│       │   └── ai.controller.js      # Request handling / validation
│       └── services/
│           └── ai.service.js         # Gemini API integration & system prompt
│
└── Frontend/
    ├── index.html
    ├── package.json
    └── src/
        ├── App.jsx                   # Main UI: editor + review panes
        ├── App.css
        ├── main.jsx
        └── index.css
```

## ⚙️ How It Works

1. The user types or pastes code into the editor on the **left**.
2. Clicking **Review** sends a `POST` request to `http://localhost:3000/ai/get-review` with the code in the request body.
3. The backend controller (`ai.controller.js`) validates the request and forwards the code to `ai.service.js`.
4. `ai.service.js` calls the Gemini API with a detailed **system instruction** that tells the model to act as a senior code reviewer, evaluating code quality, best practices, performance, security, and test coverage.
5. Gemini's markdown-formatted response (issues found + recommended fix) is returned to the frontend and rendered on the **right** pane.

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18+ recommended)
- A [Google Gemini API key](https://ai.google.dev/)

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd Code-review-main
```

### 2. Backend Setup

```bash
cd BackEnd
npm install
```

Create a `.env` file inside `BackEnd/` with your Gemini API key:

```env
GOOGLE_GEMINI_KEY=your_gemini_api_key_here
```

Start the backend server:

```bash
node server.js
```

The server will run at **http://localhost:3000**.

### 3. Frontend Setup

Open a new terminal:

```bash
cd Frontend
npm install
npm run dev
```

The frontend will run at **http://localhost:5173** (default Vite port).

## 🔌 API Reference

### `POST /ai/get-review`

Submits code for AI review.

**Request Body**

```json
{
  "code": "function sum() {\n  return 1 + 1\n}"
}
```

**Response**

A markdown-formatted string containing the AI's review (Bad Code, Issues, Recommended Fix, Improvements).

**Errors**

| Status | Reason |
|--------|--------|
| `400`  | `code` field missing from request body |


## 🗺️ Roadmap / Possible Improvements

- [ ] Move the frontend API URL into an environment variable instead of hardcoding `http://localhost:3000`
- [ ] Add loading/error states in the UI while waiting for the AI response
- [ ] Support multiple languages beyond JavaScript in the editor's syntax highlighting
- [ ] Add authentication/rate-limiting to protect the Gemini API key from abuse
- [ ] Add automated tests for backend routes and services
- [ ] Deploy backend and frontend (e.g. Render/Vercel) with CORS restricted to the deployed frontend origin

## 📄 License

ISC (see `BackEnd/package.json`) 

## 🙌 Acknowledgements

- [Google Generative AI](https://ai.google.dev/) for the Gemini model
- [Prism.js](https://prismjs.com/) for syntax highlighting
- [react-simple-code-editor](https://github.com/react-simple-code-editor/react-simple-code-editor) for the editor component
