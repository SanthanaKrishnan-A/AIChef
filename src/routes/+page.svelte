<script>
	import { GoogleGenerativeAI } from '@google/generative-ai';
	let items = '';
	let count = '';
	let output = '';
	let loading = false;

	const genAI = new GoogleGenerativeAI('AIzaSyCWEm-gdR8Z9Mrwnb_UK7Vf_VLihFsVBFA');
	async function getOutput() {
		loading = true;
		output = "";
		const model = genAI.getGenerativeModel({ model: 'gemini-1.5-flash' });
		const prompt = `Your an AI Chef who is going to help with cooking proper Indian Dishes. 
		mostly south indian . lets put like this your going to recommend easy cooking dishes. 
		Get the ingredients they have and number of people they want to cook and recommendt them the most simple and 
		easy food they can make out of that with ingredients with count/grams that should be enough for the number of people they cook,
		also the procedure to cook with duration. also mention the name of the dish with duration to cook.
		your going to help the most bachelors who are going to cook in home. so recommend simple and easy and tasty food with the ingridents they have. also explain the procedures like your doing for a kid
		return with step by step procedure !
			Ingridents : ${items}
			Number of People : ${count}
		The Output should look like :
		Name of the Dish - Duration to make for the count.
		Ingridents :
			list of the things - count/grams
		Procedure :
			things to do for duration or state`;

		try {
			const result = await model.generateContent({
				contents: [{ role: "user", parts: [{ text: prompt }] }]
			});
			output = result.response.text();
		} catch (error) {
			console.error('Error:', error);
			output = 'Oops! Something went wrong. Try again.';
		} finally {
			loading = false;
		}
	}
</script>

<main>
	<h1>🍛 AI Chef – Instant Recipes</h1>
	<p class="subtitle">Get quick and easy recipes based on what you have!</p>

	<form on:submit|preventDefault={getOutput}>
		<div class="input-group">
			<label for="items">🛒 Ingredients You Have:</label>
			<input id="items" bind:value={items} placeholder="e.g., rice, tomato, onion" required />
		</div>

		<div class="input-group">
			<label for="count">👥 Serving Size:</label>
			<input id="count" type="number" bind:value={count} min="1" required />
		</div>

		<button type="submit" class="btn" disabled={loading}>
			{loading ? '⏳ Generating...' : 'Get Recipe'}
		</button>
	</form>

	{#if loading}
		<div class="loader"></div>
	{:else if output}
		<div class="recipe-card">
			<h2>🍽️ Try This Dish:</h2>
			<p>{@html output.replace(/\n/g, '<br>')}</p>
		</div>
	{/if}
	<footer>
		<p>© 2025 AI Chef. Crafted with ❤️ for home cooks.</p>
	</footer>
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');


	:global(body) {
		background: #fffcf2;
		color: #333;
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		padding: 20px;
		margin: 0;
		font-family: 'Poppins', sans-serif;
	}

	main {
		background: #fff;
		padding: 20px;
		border-radius: 12px;
		box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
		max-width: 500px;
		width: 100%;
		text-align: center;
	}
	
	h1 {
		color: #ff6b6b;
		font-size: 24px;
	}

	.subtitle {
		font-size: 14px;
		color: #666;
		margin-bottom: 15px;
	}

	.input-group {
		margin-bottom: 15px;
		text-align: left;
	}

	label {
		display: block;
		margin-bottom: 5px;
		font-weight: 600;
	}

	input {
		width: 100%;
		padding: 10px;
		border: 1px solid #ddd;
		border-radius: 6px;
		font-size: 14px;
	}

	.btn {
		background: #ff6b6b;
		color: white;
		padding: 10px;
		border: none;
		border-radius: 6px;
		cursor: pointer;
		font-size: 16px;
		transition: 0.3s;
		width: 100%;
	}

	.btn:disabled {
		background: #ccc;
		cursor: not-allowed;
	}

	.btn:hover {
		background: #ff5252;
	}

	.loader {
		margin: 20px auto;
		border: 4px solid #ddd;
		border-top: 4px solid #ff6b6b;
		border-radius: 50%;
		width: 40px;
		height: 40px;
		animation: spin 1s linear infinite;
	}

	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}

	.recipe-card {
		background: #fff;
		padding: 15px;
		border-radius: 8px;
		box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.1);
		margin-top: 20px;
		text-align: left;
	}

	.recipe-card h2 {
		color: #ff6b6b;
		font-size: 18px;
		margin-bottom: 10px;
	}
</style>
