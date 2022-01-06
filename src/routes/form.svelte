<script>
	import ZInput from '$lib/c/ZInput.svelte';
	import ZRadio from '$lib/c/ZRadio.svelte';
	import { onMount } from 'svelte';
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

	let prefixOptions = ['Ms.', 'Mr.', 'Dr.'];
	let genderOptions = ['F', 'M', 'X'];
	let contactTypes = ['friend', 'family', 'aquaintence'];

	let products = [
		{ product_id: 101, product_name: 'Boots' },
		{ product_id: 202, product_name: 'Shoes' },
		{ product_id: 333, product_name: 'Jeans' }
	];

	onMount(() => {
		// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map#using_map_to_reformat_objects_in_an_array

		prefixOptions = simpleRemap(prefixOptions);
		genderOptions = simpleRemap(genderOptions);
		contactTypes = simpleRemap(contactTypes);

		products = products.map((element) => {
			return {
				id: element.product_id,
				name: element.product_name,
				label: element.product_name,
				value: element.product_id,
			};
		});
	});

	function simpleRemap(itemList) {
		return itemList.map(element => {
			return {
				id: element,
				name: element,
				label: element,
				value: element,
			};
		});
	}

</script>

<main>
	<div>
		<h1>Complex Svelte Form Example, Part 2</h1>

		<h3>Test Form</h3>
		<form on:submit={handleSubmit}>
			<ZRadio
				nameAttr="prefix"
				nameLabel="Prefix"
				itemList={prefixOptions}
				itemValueChecked="n/a"
				errorText={$errors?.prefix}
				{handleChange}></ZRadio>

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

			<ZRadio
				nameAttr="profile.gender"
				nameLabel="Profile Gender"
				itemList={genderOptions}
				itemValueChecked="n/a"
				errorText={$errors.profile?.gender}
				{handleChange}></ZRadio>

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

					<ZRadio
						nameAttr={`contacts[${j}].contacttype`}
						nameLabel="Contact Type"
						itemList={contactTypes}
						itemValueChecked="n/a"
						errorText={$errors.contacts[j]?.contacttype}
						{handleChange}></ZRadio>

					<ZRadio
						nameAttr={`contacts[${j}].product_id`}
						nameLabel="Product"
						itemList={products}
						itemValueChecked="n/a"
						errorText={$errors.contacts[j]?.product_id}
						{handleChange}></ZRadio>

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
	:global(label) {
		display: inline-block;
		width: 200px;
	}
	:global(label.compact) {
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
