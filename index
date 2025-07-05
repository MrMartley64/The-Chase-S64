const express = require('express');
const { Configuration, OpenAIApi } = require('openai');
const cors = require('cors');
require('dotenv').config();

const app = express();
app.use(cors());

const openai = new OpenAIApi(new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
}));

app.get('/random-question', async (req, res) => {
  try {
    const prompt = `Generate one multiple-choice trivia question with 3 options (A, B, C), and specify the correct option as a letter. Return it as JSON with this format:
{
  "question": "...",
  "answers": { "A": "...", "B": "...", "C": "..." },
  "correct": "A"
}`;

    const response = await openai.createChatCompletion({
      model: "gpt-4o",
      messages: [{ role: "user", content: prompt }],
      temperature: 0.8
    });

    const content = response.data.choices[0].message.content;
    const match = content.match(/```json\n([\s\S]*?)```/) || content.match(/{[\s\S]*}/);
    const json = JSON.parse(match[1] || match[0]);
    res.json(json);
  } catch (err) {
    console.error("Error:", err);
    res.status(500).json({ error: "Failed to generate trivia question." });
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`API running on port ${PORT}`));
