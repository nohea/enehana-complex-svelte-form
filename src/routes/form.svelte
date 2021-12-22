<script>
	import { createForm } from 'svelte-forms-lib';

	const formProps = {
		initialValues: {
			fullname: '',
			prefix: '',
			profile: {
				address: '',
				gender: ''
			},
			contacts: []
		},
		onSubmit: (values) => {
			console.log('onSubmit (via handleSubmit): ', JSON.stringify(values));
		}
	};

	const { form, errors, state, handleChange, handleSubmit, handleReset } = createForm(formProps);

	const addcontact = () => {
		console.log('addcontact()');
		$form.contacts = $form.contacts.concat({ name: '', email: '', contacttype: '' });
		$errors.contacts = $errors.contacts.concat({ name: '', email: '', contacttype: '' });
	};

	const removecontact = (i) => () => {
		$form.contacts = $form.contacts.filter((u, j) => j !== i);
		$errors.contacts = $errors.contacts.filter((u, j) => j !== i);
	};
</script>

<main>
<div>
	<h1>Complex Svelte Form Example</h1>

	<h4>Test Form</h4>
	<form on:submit={handleSubmit}>
		<div>
			<label for="fullname"> Full Name </label>
			<input
				type="text"
				name="fullname"
				bind:value={$form.fullname}
				class=""
				placeholder="Full Name"
				on:change={handleChange}
				on:blur={handleChange}
			/>
			{#if $errors.fullname}
				<div class="error-text">{$errors.fullname}</div>
			{/if}
		</div>

        <div>
			<label for="profile.address">Profile Address </label>
			<input
				type="text"
				name="profile.address"
				bind:value={$form.profile.address}
				class=""
				placeholder="Profile Address"
				on:change={handleChange}
				on:blur={handleChange}
			/>
			{#if $errors.profile?.address}
				<div class="error-text">{$errors.profile.address}</div>
			{/if}
		</div>

		<input type="submit" name="submit" value="submit button" />
	</form>
</div>

<div>
	<b>$form: </b>
	<pre>{JSON.stringify($form)}</pre>
</div>
<div>
	<b>$errors: </b>
	<pre>{JSON.stringify($errors)}</pre>
</div>

</main>

<style>
    label {
        display: inline-block;
        width: 200px;
    }

	.error-text {
		color: red;
	}
</style>
