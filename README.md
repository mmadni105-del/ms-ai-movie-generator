const express = require("express");
const OpenAI = require("openai");

const app = express();

app.use(express.json());

const client = new OpenAI({
    apiKey: process.env.OPENAI_API_KEY
});

app.get("/", (req, res) => {
    res.send("MS AI MOVIE GENERATOR API is running!");
});

app.post("/generate-story", async (req, res) => {

    try {

        const {
            title,
            genre,
            character,
            monster,
            idea
        } = req.body;

        const prompt = `
Create a complete original cinematic movie story.

Movie Title: ${title}
Genre: ${genre}
Main Character: ${character}
Monster/Creature: ${monster}
Movie Idea: ${idea}

Requirements:
- Make the story highly suspenseful.
- Create strong emotional moments.
- Add unexpected twists.
- Keep the story connected from beginning to end.
- Make it suitable for a cinematic AI movie.
- Do not copy an existing movie.
- Use original characters and events.
- Write the story in English.
- Give the movie a powerful beginning, middle and ending.
`;

        const response = await client.responses.create({
            model: "gpt-6-luna",
            input: prompt
        });

        res.json({
            success: true,
            story: response.output_text
        });

    } catch (error) {

        console.error(error);

        res.status(500).json({
            success: false,
            error: "AI generation failed."
        });

    }

});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
