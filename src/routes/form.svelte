<script>
	import { createForm } from 'svelte-forms-lib';
	import * as yup from 'yup';

    const validator = yup.object().shape({
        fullname: yup.string().required(),
        prefix: yup.string(),
        profile: yup.object().shape({
            address: yup
                .string()
                .required(),
            gender: yup
                .string()
        }),
        contacts: yup.array().of(
            yup.object().shape({
                contacttype: yup.string(),
                name: yup.string().required(),
                email: yup.string(),
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

	<h3>Test Form</h3>
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

        <h4>Contacts</h4>
        {#each $form.contacts as contact, j}
          <div class="form-group">
            <div>
              <label for={`contacts[${j}].name`}>Name</label>
              <input
                name={`contacts[${j}].name`}
                placeholder="name"
                on:change={handleChange}
                on:blur={handleChange}
                bind:value={$form.contacts[j].name}
              />
              
              {#if $errors?.contacts[j]?.name}
                <div class="error-text">{$errors.contacts[j].name}</div>
              {/if}
            </div>
    
            <div>
                <label for={`contacts[${j}].email`}>Email</label>
                <input
                placeholder="email"
                name={`contacts[${j}].email`}
                on:change={handleChange}
                on:blur={handleChange}
                bind:value={$form.contacts[j].email}
              />
              {#if $errors?.contacts[j]?.email}
                <div class="error-text">{$errors.contacts[j].email}</div>
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

	.error-text {
		color: red;
	}
</style>
