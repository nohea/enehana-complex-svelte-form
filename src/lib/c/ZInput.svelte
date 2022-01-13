<script>
    import { getScopedObj } from "$lib/util";
    import { getContext } from "svelte";
    import { key } from "svelte-forms-lib";
    export let name;
    export let label;
    export let bindValue;
    // allows the Form* components to share state with the parent form
    const { form, errors, handleChange } = getContext(key);
</script>
<div>
    <label for={name}>{label}</label>
    <input
        placeholder={label}
        name={name}
        on:change={handleChange}
        on:blur={handleChange}
        bind:value={bindValue}
        {...$$props}
        />
    {#if getScopedObj($errors, name)}
        <div class="form-error">{getScopedObj($errors, name)}</div>
    {/if}
</div>
