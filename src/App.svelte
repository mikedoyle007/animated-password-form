<script>
// JS Here...

	let strength = 0;
	let validations = [];		// <== reactive state
	let showPassword = false;

	function validatePassword(e) {
		const password = e.target.value;

		// Updates every time the user types in the form
		validations = [
			(password.length > 5),
			(password.search(/[A-Z]/) > -1),	// <-- returns -1 if no capital letters are found
			(password.search(/[0-9]/) > -1),	// <-- returns -1 if no numbers are found
			(password.search(/[$&+,:;=?@#]/) > -1),	// <-- returns -1 if no special chars are found
		];

		// Calculate the strength by adding up these 4 bool vals
		strength = validations.reduce((acc, cur) => acc + cur);
	}

</script>

<style>

	:global(body) {
		color: white;
		background: black;
	}

	form {
		--text-color: #afafaf;
		max-width: 500px;
	}

	.field {
		width: 100%;
		position: relative;
		border-bottom: 2px dashed var(--text-color);
		margin: 4rem auto 1rem;
	}

	.label {
		color: var(--text-color);
		font-size: 1.2rem;
	}

	.input {
		outline: none;
		border: none;
		overflow: hidden;
		margin: 0;
		width: 100%;
		padding: 0.25rem 0;
		background: none;
		color: white;
		font-size: 1.2rem;
		font-weight: bold;
	}

	.input:valid {
		color: yellowgreen;
	}

	.input:invalid {
		color: orangered;
	}

	/* Border Animation */

	.field::after {
		content: "";
		position: relative;
		display: block;
		height: 4px;
		width: 100%;
		background: #d16dff;
		transform: scaleX(0);
		transform-origin: 0%;
		transition: transform 500ms ease;
		top: 2px;
	}

	.field:focus-within {
		border-color: transparent;
	}

	.field:focus-within::after {
		transform: scaleX(1);
	}

	/* Label Animation */

	.label {
		z-index: -1;
		position: absolute;
		transform: translateY(-2rem);
		transform-origin: 0%;
		transition: transform 400ms;
	}

	.field:focus-within .label,
	.input:not(:placeholder-shown) + .label {
		transform: scale(0.8) translateY(-5rem);
	}

	/* Strength Meter */

	.strength {
		display: flex;
		height: 20px;
		width: 100%;
	}

	.bar {
		margin-right: 5px;
		height: 100%;
		width: 25%;
		transition: box-shadow 500ms;
		box-shadow: inset 0px 20px #1f1f1f;
	}

	.bar-show {
		box-shadow: none;
	}

	.bar-1 {
		background: linear-gradient(to right, red, orangered);
	}

	.bar-2 {
		background: linear-gradient(to right, orangered, yellow);
	}

	.bar-3 {
		background: linear-gradient(to right, yellow, yellowgreen);
	}

	.bar-4 {
		background: linear-gradient(to right, yellowgreen, green);
	}

	.toggle-password {
		position: absolute;
		cursor: help;
		font-size: 0.8rem;
		right: 0.25rem;
		bottom: 0.5rem;
	}

</style>

<!-- HTML here... -->
<main>
	<form>

		<div class="field">
			<input type="email" name="email" class="input" placeholder=" " />
			<label for="email" class="label">Email</label>
		</div>

		<div class="field">
			<input type={showPassword ? 'text' : 'password'} class="input" placeholder=" " on:input={validatePassword} />
			<label for="password" class="label">Password</label>

			<span
				class="toggle-password"
				on:mouseenter={() => (showPassword = true)}
				on:mouseleave={() => (showPassword = false)}>
				{showPassword ? '🙈️' : '👀'}
			</span>
		</div>

		<div class="strength">
			<span class="bar bar-1" class:bar-show={strength > 0}></span>
			<span class="bar bar-2" class:bar-show={strength > 1}></span>
			<span class="bar bar-3" class:bar-show={strength > 2}></span>
			<span class="bar bar-4" class:bar-show={strength > 3}></span>
		</div>

		<!-- Validation Error Messages -->
		<ul>
			<li> {validations[0] ? '✅️' : '❌'} must be at least 5 characters</li>
			<li> {validations[1] ? '✅️' : '❌'} must contain a capital letter</li>
			<li> {validations[2] ? '✅️' : '❌'} must contain a number</li>
			<li> {validations[3] ? '✅️' : '❌'} must contain one of $&+,:;=?@#</li>
		</ul>

	</form>
</main>
