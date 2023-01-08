<script>
	import { Configuration, OpenAIApi } from 'openai';
	let response = [];
	let final = [];
	let prompt;

	async function sendPrompt(prompt) {
		const configuration = new Configuration({
			organization: 'org-urcDSTtd23SI1a1QYO4uQUSC',
			apiKey: import.meta.env.VITE_OPENAI_API_KEY
		});
		const openai = new OpenAIApi(configuration);
		try {
			const completion = await openai.createCompletion({
				model: 'text-davinci-003',
				prompt: 'suggest me music similar to: ' + prompt + ' up to 5 results',
				temperature: 0,
				logprobs: null,
				stream: false,
				max_tokens: 150,
				n: 1,
				presence_penalty: 2,
				frequency_penalty: 2
			});
			response = completion.data.choices[0];
			final = [];
			var n = response.text.split('\n');
			for (var x in n) {
				final.push(n[x]);
				if (n[x] == '') {
					final.pop(n[x]);
				}
			}
			console.log(completion);
		} catch (err) {
			//
			console.log(err);
		}
	}
</script>

<section class="container">
	<div class="row text-center">
		<div class="col-12 mt-5">
			<h1>Welcome to the Music Explorer</h1>
			<p>
				This is an experiment using ChatGPT to help with discovering music you might like based on a
				prompt.
			</p>
			<p>
				You can enter a band, group, a song, lyrics, a mood or environment. The more details, the
				better the recommendations.
			</p>
		</div>
		<div class="col-12 my-5">
			<div class="row">
				<form>
					<div class="mb-3">
						<label for="groupInput" class="form-label">Suggest me music like:</label>
						<input
							bind:value={prompt}
							type="text"
							class="form-control"
							id="groupInput"
							placeholder="something chill, thievery corporation, coffeeshop"
						/>
					</div>
					<button type="submit" on:click={sendPrompt(prompt)} class="btn btn-primary"
						>Suggest</button
					>
				</form>
			</div>
		</div>
		<div class="col-12 my-5">
			<h3 class="">
				{#each final as choice}
					<p>{choice}</p>
				{/each}
			</h3>
		</div>
	</div>
</section>
