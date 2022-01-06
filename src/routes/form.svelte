<script>
	import ZInput from '$lib/c/ZInput.svelte';
	import { createForm } from 'svelte-forms-lib';
	import * as yup from 'yup';

	const validator = yup.object().shape({
		fullname: yup.string().required(),
		prefix: yup.string(),
		profile: yup.object().shape({
			address: yup.string().required(),
			gender: yup.string()
		}),
		contacts: yup.array().of(
			yup.object().shape({
				contacttype: yup.string(),
				product_id: yup.number(),
				name: yup.string().required(),
				email: yup.string()
			})
		)
	});

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
		validationSchema: validator,
		onSubmit: (values) => {
			console.log('onSubmit (via handleSubmit): ', JSON.stringify(values));
			alert(JSON.stringify(values));
		}
	};

	const { form, errors, state, handleChange, handleSubmit, handleReset } = createForm(formProps);

	const addcontact = () => {
		console.log('addcontact()');
		$form.contacts = $form.contacts.concat({ name: '', email: '', contacttype: '', product_id: 0 });
		$errors.contacts = $errors.contacts.concat({
			name: '',
			email: '',
			contacttype: '',
			product_id: 0
		});
	};

	const removecontact = (i) => () => {
		$form.contacts = $form.contacts.filter((u, j) => j !== i);
		$errors.contacts = $errors.contacts.filter((u, j) => j !== i);
	};

	const prefixOptions = ['Ms.', 'Mr.', 'Dr.'];
	const genderOptions = ['F', 'M', 'X'];
	const contactTypes = ['friend', 'family', 'aquaintence'];

	const products = [
		{ product_id: 101, product_name: 'Boots' },
		{ product_id: 202, product_name: 'Shoes' },
		{ product_id: 333, product_name: 'Jeans' }
	];
</script>

<main>
	<div>
		<h1>Complex Svelte Form Example, Part 2</h1>

		<h3>Test Form</h3>
		<form on:submit={handleSubmit}>
			<div>
				<label for="prefix"> Prefix </label>
				{#each prefixOptions as pre, i}
					<label class="compact">
						<input
							id={`prefix-${pre}`}
							name="prefix"
							value={pre}
							type="radio"
							on:change={handleChange}
							on:blur={handleChange}
						/>
						<span> {pre} </span>
					</label>
				{/each}
				{#if $errors.prefix}
					<div class="error-text">{$errors.prefix}</div>
				{/if}
			</div>

			<ZInput
				nameAttr="fullname"
				nameLabel="Full Name"
				bindValue={$form.fullname}
				errorText={$errors?.fullname}
				{handleChange}
			/>

			<ZInput
				nameAttr="profile.address"
				nameLabel="Profile Address"
				bindValue={$form.profile.address}
				errorText={$errors.profile?.address}
				{handleChange}
			/>

			<div>
				<label for="profile.gender"> Profile Gender</label>
				{#each genderOptions as g, i}
					<label class="compact">
						<input
							id={`prefix-${g}`}
							name="profile.gender"
							value={g}
							type="radio"
							on:change={handleChange}
							on:blur={handleChange}
						/>
						<span> {g} </span>
					</label>
				{/each}
				{#if $errors.profile?.gender}
					<div class="error-text">{$errors.profile.gender}</div>
				{/if}
			</div>

			<h4>Contacts</h4>
			{#each $form.contacts as contact, j}
				<div class="form-group">
					<ZInput
						nameAttr={`contacts[${j}].name`}
						nameLabel="Name"
						bindValue={$form.contacts[j].name}
						errorText={$errors?.contacts[j]?.name}
						{handleChange}
					/>

					<ZInput
						nameAttr={`contacts[${j}].email`}
						nameLabel="Email"
						bindValue={$form.contacts[j].email}
						errorText={$errors?.contacts[j]?.email}
						{handleChange}
					/>

					<div>
						<label for={`contacts[${j}].contacttype`}>Contact Type</label>
						{#each contactTypes as ct, i}
							<label class="compact">
								<input
									type="radio"
									id={`contacts[${j}].contacttype-${ct}`}
									name={`contacts[${j}].contacttype`}
									value={ct}
									on:change={handleChange}
									on:blur={handleChange}
								/>
								<span> {ct} </span>
							</label>
						{/each}
						{#if $errors.contacts[j]?.contacttype}
							<div class="error-text">{$errors.contacts[j].contacttype}</div>
						{/if}
					</div>

					<div>
						<label for={`contacts[${j}].product_id`}>Product</label>
						{#each products as p, i}
							<label class="compact">
								<input
									type="radio"
									id={`contacts[${j}].product_id-${p.product_id}`}
									name={`contacts[${j}].product_id`}
									value={p.product_id}
									on:change={handleChange}
									on:blur={handleChange}
								/>
								<span> {p.product_name} [{p.product_id}]</span>
							</label>
						{/each}
						{#if $errors.contacts[j]?.product_id}
							<div class="error-text">{$errors.contacts[j].product_id}</div>
						{/if}
					</div>

					{#if $form.contacts.length === j + 1}
						<button type="button" on:click={removecontact(j)}>[- remove last contact]</button>
					{/if}
				</div>
			{/each}

			{#if $form.contacts}
				<div>
					<button on:click|preventDefault={addcontact}>[+ add contact]</button>
				</div>
			{/if}

			<div>
				<input type="submit" name="submit" value="submit button" />
			</div>
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
	label.compact {
		width: max-content;
	}
	.form-group {
		display: block;
		border-style: solid;
		border-width: 2px;
		border-color: gray;
		width: max-content;
		margin: 4px;
	}
	:global(.error-text) {
		color: red;
	}
</style>
