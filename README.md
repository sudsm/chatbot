const express = require("express");
const axios = require("axios");
const path = require("path");
const app = express();
const PORT = 3000;

app.use(express.json());
app.use(express.static(path.join(__dirname, "public")));

const OPENAI_API_KEY = "your-openai-api-key";

// Chatbot logic
app.post("/chat", async (req, res) => {
    const userMessage = req.body.message;
    
    const prompt = `You are a chatbot for an online clothing store. Answer customer questions about products, sizes, shipping, returns, and promotions. If they need recommendations, suggest items from our collection.`;
    
    try {
        const response = await axios.post("https://api.openai.com/v1/chat/completions", {
            model: "gpt-4o",
            messages: [{ role: "system", content: prompt }, { role: "user", content: userMessage }],
            max_tokens: 100
        }, {
            headers: { "Authorization": `Bearer ${OPENAI_API_KEY}` }
        });

        res.json({ reply: response.data.choices[0].message.content });
    } catch (error) {
        console.error("Error calling OpenAI API:", error);
        res.status(500).json({ error: "Chatbot unavailable. Please try again later." });
    }
});

// Serve HTML page
app.get("/", (req, res) => {
    res.sendFile(path.join(__dirname, "public", "index.html"));
});

app.listen(PORT, () => {
    console.log(`Chatbot website running on http://localhost:${PORT}`);
});
# chatbot
AI MADR CHATBOT
