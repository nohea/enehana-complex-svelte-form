<script>
    import { getScopedObj } from "$lib/util";
    import { getContext } from "svelte";
    import { key } from "svelte-forms-lib";
    export let nameAttr;
    export let nameLabel;
    export let bindValue;
    // allows the Form* components to share state with the parent form
    const { form, errors, handleChange } = getContext(key);
</script>
<div>
    <label for={nameAttr}>{nameLabel}</label>
    <input
        placeholder={nameLabel}
        name={nameAttr}
        on:change={handleChange}
        on:blur={handleChange}
        bind:value={bindValue}
    />
    {#if getScopedObj($errors, nameAttr)}
        <div class="form-error">{getScopedObj($errors, nameAttr)}</div>
    {/if}
</div>
